---
title: "Build"
description: "What you can make with FlightPortrait's data, firmware and protocol."
---

Every aircraft on the map was heard by an antenna on somebody's roof.
That data is free to build on. The frame's firmware and protocol are
open. The drawings are ours, and the frame is where they live.

## Three layers

| Layer | What you get | Terms |
| --- | --- | --- |
| The data | Live aircraft, flight history, routes, airports, airlines, search | Free, no key, ODbL 1.0 |
| The frame | Firmware, device protocol, a reference server | Open source, Apache-2.0 |
| The art | Our renderer and the catalogue's drawings | Licensed, coming |

### The data

One HTTPS call tells you what is flying over a point, nearest first.
Two more tell you where a flight goes and find anything a person
might type. [API quickstart](/build/api).

The smallest useful thing to make with it is a desk display that
shows the nearest flight overhead: an ESP32, a 2.9 inch e-paper,
eight wires. [Build a flight display](/build/flight-display).

### The frame

Point a stock frame at your own server, or build a frame from parts
and run the same firmware.
[Bring your own server](/diy/byos) and
[build your own frame](/diy/byod).

### The art

A key for builders who make their own device and want our renderer
and the catalogue's drawings on it, without buying a frame. It is not
available yet, and its terms and price are not set. If you want one,
write to hello@flightportrait.com with what you are building; it
helps us shape it.

## Builds

What people made: displays, dashboards, integrations, frames from
parts. [The gallery](https://flightportrait.com/build).
[Share yours](/build/share).

## Feed the network

Everything above gets better with every antenna. A Raspberry Pi, a
USB receiver and a 1090 MHz antenna, about US$100, no soldering.
[Feed the network](/network/feed).
