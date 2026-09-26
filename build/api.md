---
title: "API quickstart"
description: "Three calls: what flies over a point, one flight, search."
---

Base URL: `https://data.flightportrait.com`. No key, no account, open
to any origin, so it works from a browser page too.

## The three calls

| Call | What it answers |
| --- | --- |
| `GET /v2/point/{lat}/{lon}/{radius}` | Aircraft within `radius` nautical miles of a point, nearest first. Radius caps at 250. |
| `GET /v1/flights/{callsign}` | One flight: its route, the times it usually keeps, the airframes that fly it. |
| `GET /v1/search?q=` | Anything a person might type: a flight, a route, an airport, an airline, a registration. |

Every other endpoint is in the [API reference](/api/reference).

## curl

Aircraft within 15 nautical miles of the middle of Singapore:

```sh
curl https://data.flightportrait.com/v2/point/1.3521/103.8198/15
```

```json
{"ac": [{"hex": "7805dc", "flight": "CKK288", "t": "B77L", "r": "B-2076",
         "lat": 1.387344, "lon": 104.007336, "alt_baro": 7225, "gs": 287.2,
         "track": 340.05, "dst": 11.5, ...}, ...],
 "now": 1790407220.35, "total": 2}
```

`ac` speaks readsb's field names: `t` is the ICAO type, `r` the
registration, `alt_baro` feet (or `"ground"`), `gs` knots, `dst` the
distance in nautical miles. `flight` is absent when the aircraft does
not broadcast a callsign.

One flight:

```sh
curl https://data.flightportrait.com/v1/flights/SIA322
```

```json
{"callsign": "SIA322", "route": ["SIN", "LHR"], "route_source": "observed",
 "legs": [{"org": "SIN", "dst": "LHR", "dep": "23:00", ...}], ...}
```

Times in `legs` are local at the origin airport. A callsign the network
never heard returns 404 with `"error": "not_observed"`.

Search:

```sh
curl -L "https://data.flightportrait.com/v1/search?q=SIN%20LHR"
```

```json
{"q": "SIN LHR", "results": [
  {"kind": "flight", "id": "BAW16", "label": "BAW16",
   "detail": "SIN → LHR · 48 flights", "score": 46.8}, ...]}
```

`-L` because a query in any other spelling may be redirected to its
canonical form (trimmed, uppercase). `id` is what to open next: a hex
for `/v1/airframes/`, a callsign for `/v1/flights/`, a code for
`/v1/airports/` or `/v1/airlines/`.

## Python

Standard library only.

```python
import json
import urllib.parse
import urllib.request

API = "https://data.flightportrait.com"
HEADERS = {"User-Agent": "my-sky-script/1.0 (you@example.com)"}


def get(path):
    request = urllib.request.Request(API + path, headers=HEADERS)
    with urllib.request.urlopen(request, timeout=10) as response:
        return json.load(response)


# What is flying within 15 nautical miles of a point, nearest first
sky = get("/v2/point/1.3521/103.8198/15")
for ac in sky["ac"]:
    print(ac.get("flight", ac["hex"]), ac.get("t"), ac.get("alt_baro"), f'{ac["dst"]} nm')

# One flight: where it goes, when it usually leaves
flight = get("/v1/flights/SIA322")
print(flight["route"], flight["legs"][0]["dep"])

# Search, as a person would type it
found = get("/v1/search?" + urllib.parse.urlencode({"q": "SIN LHR"}))
for result in found["results"]:
    print(result["kind"], result["id"], result["label"], result["detail"])
```

```text
AIQ356 A21N 11150 11.1 nm
CKK288 B77L 4425 12.6 nm
MXD158 B38M 38000 14.0 nm
['SIN', 'LHR'] 23:00
flight BAW16 BAW16 SIN → LHR · 48 flights
flight SIA306 SIA306 SIN → LHR · 47 flights
```

## JavaScript

Node 18 or newer as an `.mjs` file, or a browser page.

```js
const API = "https://data.flightportrait.com";

async function get(path) {
  const response = await fetch(API + path);
  if (!response.ok) throw new Error(`${response.status} ${await response.text()}`);
  return response.json();
}

// What is flying within 15 nautical miles of a point, nearest first
const sky = await get("/v2/point/1.3521/103.8198/15");
for (const ac of sky.ac) {
  console.log(ac.flight ?? ac.hex, ac.t, ac.alt_baro, `${ac.dst} nm`);
}

// One flight: where it goes, when it usually leaves
const flight = await get("/v1/flights/SIA322");
console.log(flight.route, flight.legs[0].dep);

// Search, as a person would type it
const found = await get("/v1/search?q=" + encodeURIComponent("SIN LHR"));
for (const r of found.results) console.log(r.kind, r.id, r.label, r.detail);
```

## ESP32 (Arduino)

The nearest airborne aircraft, printed to the serial monitor. Needs
the esp32 board package and ArduinoJson 7.

```cpp
#include <WiFi.h>
#include <WiFiClientSecure.h>
#include <HTTPClient.h>
#include <ArduinoJson.h>

const char* WIFI_SSID = "your-network";
const char* WIFI_PASS = "your-password";

void setup() {
  Serial.begin(115200);
  WiFi.begin(WIFI_SSID, WIFI_PASS);
  while (WiFi.status() != WL_CONNECTED) delay(250);
}

void loop() {
  WiFiClientSecure tls;
  tls.setInsecure();  // public, read-only data; pin a root CA if you need to
  HTTPClient http;
  http.useHTTP10(true);  // a plain body the parser can stream
  http.setUserAgent("my-esp32/1.0 (you@example.com)");
  http.begin(tls, "https://data.flightportrait.com/v2/point/1.3521/103.8198/15");
  if (http.GET() == 200) {
    JsonDocument filter;  // keep only what we print
    JsonObject f = filter["ac"].add<JsonObject>();
    f["flight"] = f["t"] = f["alt_baro"] = f["dst"] = true;
    JsonDocument doc;
    deserializeJson(doc, http.getStream(), DeserializationOption::Filter(filter));
    for (JsonObject ac : doc["ac"].as<JsonArray>()) {
      if (!ac["alt_baro"].is<long>()) continue;  // "ground"
      Serial.printf("%s %s %ld ft %.1f nm\n", ac["flight"] | "?", ac["t"] | "?",
                    ac["alt_baro"].as<long>(), ac["dst"].as<float>());
      break;
    }
  }
  http.end();
  delay(30000);
}
```

For a panel instead of a serial monitor:
[Build a flight display](/build/flight-display).

## Attribution

Show this line wherever the data appears, with a link to
`https://flightportrait.com/network`:

```text
Data (c) FlightPortrait network feeders and credited sources, ODbL 1.0
```

On a display too small for the whole line, put "Data: FlightPortrait
network feeders, ODbL" on the screen and the full line in your
project's README or about page. Republishing the data itself carries the sources' credits
with it. [Data and licensing](/network/data).

## Being a good neighbour

* Limits are per IP over 600 seconds, per call: `/v2/point` 300
  (one call every 2 seconds), `/v1/flights` 120, `/v1/search` 600.
  A 429 carries `Retry-After` in seconds. Wait that long.
* The live answer changes every few seconds and the edge caches it
  for 5, so polling faster than that returns the same body. A display
  on a desk is well served by one call every 30 to 60 seconds.
* History and reference answers change nightly. Cache them for an
  hour or more, and look a flight up once, when its callsign first
  appears, not on every poll.
* A 503 means the live picture is more than 60 seconds old. Keep what
  you last showed and try again; the API never pretends the sky is
  empty.
* Send a `User-Agent` that names your project and a way to reach you.
  If something you made misbehaves, we would rather write to you than
  block an address.

Heavy users get more from the network by feeding it:
[Feed the network](/network/feed).
