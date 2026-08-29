---
title: "Welcome"
description: "The FlightPortrait frame and network, and where to start."
---

FlightPortrait is two things.

A frame: a battery-powered 13.3 inch color e-ink artwork that draws
the aircraft that flew over your home. Six inks, no backlight, months
on a charge. It hangs on a wall and once or twice a day the sky above
your house becomes a new drawing.

A network: an open source flight tracker built on community
receivers, with a live map, per-airframe flight logs going back a
year, and an open API. Anyone can feed it with a US$40 antenna.
Anyone can run their own copy.

## Where to start

| You want to | Start here |
| --- | --- |
| Understand what the frame does all day | [How it works](/how-it-works) |
| Point a frame at your own server | [Bring your own server](/diy/byos) |
| Build a frame from parts | [Build your own frame](/diy/byod) |
| Watch the live sky | [The network](/network/overview) |
| Feed the network with your antenna | [Feed the network](/network/feed) |
| Run the whole tracker yourself | [Run your own instance](/network/self-host) |
| Use the flight data in your own project | [The public API](/api/reference) |

## The repositories

Everything here is backed by public code.

* [flightportrait/frame](https://github.com/flightportrait/frame): the
  firmware, the device protocol, and a reference server you can run
  with nothing but Python.
* [flightportrait/network](https://github.com/flightportrait/network):
  the aggregation stack, the public API, and the web tracker, from the
  antenna's Beast port to the map in your browser.

Questions these pages do not answer: hello@flightportrait.com. We read
every message.
