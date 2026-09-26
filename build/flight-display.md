---
title: "Build a flight display"
description: "The nearest flight overhead, on a 2.9 inch e-paper by your window."
---

An ESP32 asks the network what is flying nearest to you every 30
seconds and draws it on a small e-paper: callsign, route, type,
registration, altitude, speed and distance. Between changes the panel
holds the picture with no power at all. One evening and eight wires;
no soldering if your board comes with its pins fitted.

> Photo to come: the finished display on a windowsill.

## Parts

| Part | Notes |
| --- | --- |
| ESP32 development board | Any ESP32-WROOM-32 "DevKit" board with a USB port |
| 2.9 inch black and white e-paper module, 296 by 128 | SSD1680 driver, SPI, with its own breakout board. Waveshare 2.9 inch V2 or WeAct 2.9 inch |
| 8 female-to-female jumper wires | Or a breadboard and male wires |
| USB cable and a phone charger | Micro-USB or USB-C, whichever your board takes |

The code does not care which e-paper brand you pick as long as the
driver chip is the SSD1680; the one line that names it is marked in
the sketch.

## Wiring

| E-paper pin | ESP32 pin |
| --- | --- |
| VCC | 3V3 |
| GND | GND |
| DIN (or SDA) | GPIO 23 |
| CLK (or SCL) | GPIO 18 |
| CS | GPIO 5 |
| DC | GPIO 17 |
| RST (or RES) | GPIO 16 |
| BUSY | GPIO 4 |

Some newer Waveshare modules have a ninth pin, PWR: connect it to 3V3
as well. Power the panel from 3V3, never from 5V.

> Photo to come: the wiring, close up.

## Software

1. Install the [Arduino IDE](https://www.arduino.cc/en/software).
2. In Boards Manager, install **esp32** by Espressif Systems.
3. In Library Manager, install **GxEPD2** by Jean-Marc Zingg (it pulls
   in Adafruit GFX Library) and **ArduinoJson** by Benoit Blanchon,
   version 7.
4. Choose the board **ESP32 Dev Module** and your USB port.
5. Paste the sketch below, fill in the four settings at the top, and
   upload.

The first picture says "Connecting". Within a minute it shows the
nearest aircraft, or "A quiet sky" when nothing is airborne within
your radius. It redraws only when something changes.

## The sketch

We compiled it against esp32 3.3.10, GxEPD2 1.6.9 and ArduinoJson
7.4.2. It uses 90 percent of the board's default program space, which
is mostly the secure connection.

```cpp
// Flight display: the nearest aircraft overhead, on a 2.9 inch e-paper.
// ESP32 + 296 x 128 black and white e-paper (SSD1680), free data from
// https://data.flightportrait.com. No key, no account.
//
// Data (c) FlightPortrait network feeders and credited sources, ODbL 1.0.
//
// Libraries (Arduino Library Manager): GxEPD2, Adafruit GFX Library,
// ArduinoJson 7.

#include <WiFi.h>
#include <WiFiClientSecure.h>
#include <HTTPClient.h>
#include <ArduinoJson.h>
#include <GxEPD2_BW.h>
#include <Fonts/FreeSansBold18pt7b.h>
#include <Fonts/FreeSans12pt7b.h>
#include <Fonts/FreeSans9pt7b.h>

// ---- Your settings -------------------------------------------------------
const char* WIFI_SSID = "your-network";
const char* WIFI_PASS = "your-password";
const float LAT = 1.3521;        // your position, degrees
const float LON = 103.8198;
const int RADIUS_NM = 15;        // how far to look, nautical miles (max 250)
const uint32_t POLL_MS = 30000;  // 30 s; the API allows one call every 2 s
// Say who you are, so a misbehaving client can be told apart from you.
const char* USER_AGENT = "flight-display/1.0 (+https://github.com/you)";
// --------------------------------------------------------------------------

const char* API = "https://data.flightportrait.com";

// Waveshare 2.9 inch V2. For a WeAct 2.9 inch module use GxEPD2_290_BS;
// for a Good Display GDEY029T94 use GxEPD2_290_GDEY029T94.
#define PANEL GxEPD2_290_T94_V2
// CS, DC, RST, BUSY. SCK is GPIO 18 and DIN (MOSI) GPIO 23, the ESP32's
// default SPI pins.
GxEPD2_BW<PANEL, PANEL::HEIGHT> display(PANEL(5, 17, 16, 4));

struct Flight {
  String callsign;  // SIA606
  String type;      // A359
  String reg;       // 9V-SHM
  bool named;       // broadcasts a callsign, so it has a flight to look up
  long altitude;    // feet
  float speed;      // knots
  float distance;   // nautical miles
};

String shownKey;           // what the panel shows now; redraw only on change
String routeFor, route;    // route cache: one lookup per new callsign
uint32_t waitUntil = 0;    // set by a 429's Retry-After

// GET a path and parse the JSON body through a filter, so only the
// fields we use are kept in memory. Returns the HTTP status (or -1).
int getJson(const String& path, JsonDocument& doc, JsonDocument& filter) {
  WiFiClientSecure tls;
  // The data is public and read-only, so this sketch skips certificate
  // checks. Pin a root certificate with tls.setCACert() if you need to.
  tls.setInsecure();
  HTTPClient http;
  http.useHTTP10(true);  // a plain body we can stream into the parser
  http.setUserAgent(USER_AGENT);
  const char* keep[] = {"Retry-After"};
  http.collectHeaders(keep, 1);
  if (!http.begin(tls, String(API) + path)) return -1;
  int status = http.GET();
  if (status == 429) {
    int seconds = http.header("Retry-After").toInt();
    waitUntil = millis() + (seconds > 0 ? seconds : 60) * 1000UL;
  } else if (status == 200) {
    DeserializationError err = deserializeJson(
        doc, http.getStream(), DeserializationOption::Filter(filter));
    if (err) status = -1;
  }
  http.end();
  return status;
}

// The nearest airborne aircraft. The API sorts by distance already.
int nearest(Flight& out) {
  JsonDocument filter;
  JsonObject f = filter["ac"].add<JsonObject>();
  for (const char* k : {"flight", "t", "r", "hex", "alt_baro", "gs", "dst"})
    f[k] = true;

  JsonDocument doc;
  char path[64];
  snprintf(path, sizeof path, "/v2/point/%.4f/%.4f/%d", LAT, LON, RADIUS_NM);
  int status = getJson(path, doc, filter);
  if (status != 200) return status;

  for (JsonObject ac : doc["ac"].as<JsonArray>()) {
    if (!ac["alt_baro"].is<long>()) continue;  // "ground": parked or taxiing
    out.named = ac["flight"].is<const char*>();
    out.callsign = ac["flight"] | ac["r"] | ac["hex"] | "";
    out.type = ac["t"] | "";
    out.reg = ac["r"] | "";
    out.altitude = ac["alt_baro"];
    out.speed = ac["gs"] | 0.0f;
    out.distance = ac["dst"] | 0.0f;
    return 200;
  }
  return 204;  // a quiet sky
}

// Where a flight goes, from its observed history: "SIN > ICN". The
// derived route when there is one, else the leg it flies most often.
String routeText(JsonDocument& doc) {
  String text;
  for (const char* code : doc["route"].as<JsonArray>()) {
    if (text.length()) text += " > ";
    text += code;
  }
  if (!text.length() && doc["legs"][0]["org"].is<const char*>()) {
    text = String(doc["legs"][0]["org"].as<const char*>()) + " > " +
           doc["legs"][0]["dst"].as<const char*>();
  }
  return text;
}

String routeOf(const String& callsign) {
  if (callsign == routeFor) return route;
  JsonDocument filter;
  filter["route"] = true;
  filter["legs"][0]["org"] = true;
  filter["legs"][0]["dst"] = true;
  JsonDocument doc;
  int status = getJson("/v1/flights/" + callsign, doc, filter);
  if (status == 429 || status < 200 || status >= 500) return "";  // retry later
  routeFor = callsign;
  route = routeText(doc);
  return route;
}

void draw(const Flight* f, const String& line2) {
  display.setRotation(1);  // landscape, 296 x 128
  display.setTextColor(GxEPD_BLACK);
  display.setFullWindow();
  display.firstPage();
  do {
    display.fillScreen(GxEPD_WHITE);
    if (f) {
      display.setFont(&FreeSansBold18pt7b);
      display.setCursor(6, 32);
      display.print(f->callsign);
      display.setFont(&FreeSans9pt7b);
      String away = String(f->distance, 1) + " nm";
      int16_t x, y;
      uint16_t w, h;
      display.getTextBounds(away, 0, 0, &x, &y, &w, &h);
      display.setCursor(290 - w, 32);
      display.print(away);
      display.setFont(&FreeSans12pt7b);
      display.setCursor(6, 60);
      display.print(line2);
      display.setFont(&FreeSans9pt7b);
      display.setCursor(6, 84);
      display.print(f->type + "  " + f->reg);
      display.setCursor(6, 106);
      display.print(String(f->altitude) + " ft   " + String(f->speed, 0) + " kt");
    } else {
      display.setFont(&FreeSans12pt7b);
      display.setCursor(6, 60);
      display.print(line2);
    }
    display.setFont(nullptr);  // the built-in 6 x 8 font
    display.setCursor(6, 118);
    display.print("Data: FlightPortrait network feeders, ODbL");
  } while (display.nextPage());
  display.hibernate();
}

void show(const Flight* f, const String& line2) {
  String key = f ? f->callsign + line2 + f->altitude / 1000 : line2;
  if (key == shownKey) return;  // e-paper keeps the last picture for free
  shownKey = key;
  draw(f, line2);
}

void setup() {
  Serial.begin(115200);
  display.init(115200, true, 2, false);
  show(nullptr, "Connecting");
  WiFi.mode(WIFI_STA);
  WiFi.begin(WIFI_SSID, WIFI_PASS);
  while (WiFi.status() != WL_CONNECTED) delay(250);
}

void loop() {
  if ((int32_t)(millis() - waitUntil) < 0) {
    delay(1000);
    return;
  }
  if (WiFi.status() != WL_CONNECTED) {
    WiFi.reconnect();
    delay(5000);
    return;
  }
  Flight f;
  int status = nearest(f);
  Serial.printf("point: %d\n", status);
  if (status == 200) {
    String r = f.named ? routeOf(f.callsign) : String();
    show(&f, r.length() ? r : String("Route unknown"));
  } else if (status == 204) {
    show(nullptr, "A quiet sky");
  }
  // Anything else (503 while the network catches up, a dropped
  // connection): keep the last picture and try again next round.
  delay(POLL_MS);
}
```

## How it works

* `/v2/point/{lat}/{lon}/{radius}` returns every aircraft within the
  radius, nearest first. The sketch keeps the first one that is not on
  the ground. A filter throws away the fields it does not draw, so the
  answer fits comfortably in memory.
* `/v1/flights/{callsign}` gives the route the network has observed
  for that callsign, or the leg it flies most often. The sketch asks
  once per new callsign, not on every poll.
* A 429 carries `Retry-After`, and the sketch waits that long. A 503
  means the live picture is briefly stale; the panel keeps what it
  shows and the next round tries again.
* The e-paper redraws only when the flight, its route or its altitude
  (to the nearest thousand feet) changes.

The footer credits the data, as the licence asks:
[Attribution](/build/api#attribution).

## If something is off

| What you see | Try |
| --- | --- |
| A blank or garbled panel | The wrong driver class. Change `PANEL` to `GxEPD2_290_BS` (WeAct) or `GxEPD2_290_GDEY029T94`, and check VCC is on 3V3 |
| "Connecting" forever | The SSID or password, or a 5 GHz network: the ESP32 speaks 2.4 GHz only |
| "A quiet sky" all day | No feeder hears your area yet, or the radius is small. Check the [live map](https://flightportrait.com/network), widen `RADIUS_NM`, or [feed the network](/network/feed) |
| "Route unknown" | The network has not observed that callsign's route yet. It fills in as more flights are heard |

## Take it further

* A bigger panel: GxEPD2 drives most SPI e-paper sizes; change the
  class and the coordinates.
* Deep sleep between polls with `esp_deep_sleep()` and a battery.
* Show the airline's name from `/v1/airlines/{icao}`, the first three
  letters of the callsign, cached for a day.

Made one? [Share your build](/build/share).
