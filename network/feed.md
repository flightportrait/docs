---
title: "Feed the network"
description: "The Station: a Raspberry Pi, a dongle, one line."
---

The Station is our own feeder software: a radio, an MLAT client, and
a status page, on a Raspberry Pi. It feeds FlightPortrait and any
other network you tick. Feeding is not exclusive; our own stations
feed adsb.lol, adsb.fi and airplanes.live too.

## What you need

A Raspberry Pi (3B or newer, or a Zero 2 W), an RTL-SDR dongle, and
a 1090 MHz antenna. About US$100, no soldering.

## Install

Flash Raspberry Pi OS Lite with the official imager, log in, run one
line:

```sh
curl -fsSL https://flightportrait.com/station/install.sh | sh
```

The installer says what it does before each step, then prints the
address of the setup page. Open it on your phone: name the station,
put the antenna on the map, tick the networks to feed. The same page
then shows how the station is doing: aircraft now, message rate,
today against yesterday, MLAT sync per network. When something is
wrong it says what, in one sentence, and what to do about it.

The station key is shown once at setup. Keep it: it is the station's
identity and the key to its status page. We store a hash, not the key.
[The join page](https://flightportrait.com/network/?mode=join) watches
until the station is heard.

## Already running a receiver

The installer notices readsb, PiAware, FR24 or ultrafeeder and stops.
Two doors:

- `--replace`: the Station takes over the dongle, imports the feeds and
  keys you had, and asks before stopping anything.
- `--add`: nothing is installed; it prints the one line that makes what
  you run feed FlightPortrait, with a key.

That line, for readsb's `NET_OPTIONS`:

```
--net-connector feed.flightportrait.com,30004,beast_reduce_plus_out,uuid=YOUR-KEY
```

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
