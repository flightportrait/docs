---
title: "Feed the network"
description: "The Station: a Raspberry Pi, a dongle, one line."
---

The Station is our own feeder software: a radio, an MLAT client, and
a status page, on a Raspberry Pi. It feeds FlightPortrait and any
other network you tick. Feeding is not exclusive; our own stations
feed adsb.lol, adsb.fi and airplanes.live too.

## What you need

- A Raspberry Pi 3B or newer, or a Zero 2 W, with its power supply.
- A USB receiver for 1090 MHz. One with a built-in filter, like the
  FlightAware Pro Stick Plus, hears furthest; any RTL-SDR dongle works.
- A 1090 MHz antenna and a 32 GB high-endurance microSD card.

About US$100, no soldering. Where the antenna goes matters more than
anything on the list: as high as you can, by a window or outside, with
open sky around it.

## Install

1. In [Raspberry Pi Imager](https://www.raspberrypi.com/software/),
   choose your Pi, then **Raspberry Pi OS Lite (64-bit)**, under
   Raspberry Pi OS (other). The Station runs on 64-bit only. In the
   settings, name it `station`, set a username and password, add your
   Wi-Fi and turn on SSH. Write the card, put it in the Pi, plug in the
   receiver and the power.
2. A few minutes later, from a computer on the same Wi-Fi (Terminal on
   a Mac, PowerShell on Windows), log in and run the installer:

   ```sh
   ssh username@station.local
   curl -fsSL https://flightportrait.com/station/install.sh | sh
   ```

3. On your phone, on the same Wi-Fi, open `station.local:8654` (or the
   numeric address the installer printed).

The installer says what it does before each step. The setup page asks
three things: name the station,
put the antenna on the map, tick the networks to feed. The same page
then shows how the station is doing: aircraft now, message rate,
today against yesterday, MLAT sync per network. When something is
wrong it says what, in one sentence, and what to do about it.

The station key is shown once at setup. Keep it: it is the station's
identity and the key to its status page. We store a hash, not the key.
[The join page](https://flightportrait.com/network/?mode=join) watches
until the station is heard.

## Already running a receiver

The installer notices readsb, dump1090-fa, PiAware, FR24, an adsb.im
image or ultrafeeder, and asks before touching anything. Two doors:

- `--replace`: the Station takes over the dongle, imports the feeds and
  keys you had, and asks before stopping anything.
- `--add`: nothing is installed; it prints the one line that makes what
  you run feed FlightPortrait, with a key.

That line, for readsb's `NET_OPTIONS` (then restart readsb):

```
--net-connector feed.flightportrait.com,30004,beast_reduce_plus_out,uuid=YOUR-KEY
```

with mlat-client beside it for MLAT:

```
mlat-client --input-type dump1090 --input-connect localhost:30005 --server feed.flightportrait.com:31090 --lat LAT --lon LON --alt ALT --user NAME --uuid YOUR-KEY
```

dump1090-fa only listens and cannot connect out; a small readsb beside
it (`--net-only`, reading its Beast port 30005) carries the same line.

and for `ULTRAFEEDER_CONFIG`, or the adsb.im Expert page:

```
adsb,feed.flightportrait.com,30004,beast_reduce_plus_out,uuid=YOUR-KEY;mlat,feed.flightportrait.com,31090,uuid=YOUR-KEY
```

## What is public

The roster shows a generated id, online status, and a location
rounded to about 11 km from coverage. Not an address. Feeder IPs
are not stored. [Privacy policy](https://flightportrait.com/network/privacy).

[Feeder terms](https://flightportrait.com/network/terms): you keep
your data; the aggregate is published under ODbL. Stop whenever you
like.

## Source

[station](https://github.com/flightportrait/station) (the runtime and
the installer), [rx](https://github.com/flightportrait/rx) (the radio),
[mlatc](https://github.com/flightportrait/mlatc) (the MLAT client),
[mlatd](https://github.com/flightportrait/mlatd) (the MLAT server).
All AGPL-3.0.
