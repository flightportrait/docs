---
title: "Welcome"
description: "The FlightPortrait frame and network."
---

FlightPortrait is two things.

A frame: a battery-powered 13.3 inch color e-ink artwork that draws
the aircraft that flew over your home. Six inks, no backlight, months
on a charge. It hangs on a wall and once or twice a day the sky above
your house becomes a new drawing.

A network of feeders, open to anyone with an antenna. The aircraft on
the map, and on the wall, are what those receivers heard.

## Where to start

| You want to | Start here |
| --- | --- |
| How the frame works | [How it works](/how-it-works) |
| Point a frame at your own server | [Bring your own server](/diy/byos) |
| Build a frame from parts | [Build your own frame](/diy/byod) |
| The live map | [The network](/network/overview) |
| Feed with an antenna, or run the Station | [Feed the network](/network/feed) |
| Use the flight data | [The public API](/api/reference) |

## Repositories

* [flightportrait/frame](https://github.com/flightportrait/frame):
  firmware, device protocol, and a reference server in stdlib Python.
* [flightportrait/network](https://github.com/flightportrait/network):
  the map, the API, how to feed, and what we store about a station.
* [flightportrait/station](https://github.com/flightportrait/station),
  [rx](https://github.com/flightportrait/rx),
  [mlatc](https://github.com/flightportrait/mlatc),
  [mlatd](https://github.com/flightportrait/mlatd): the Station feeder
  runtime, its radio, and the MLAT client and server.

Questions these pages do not answer: hello@flightportrait.com.
