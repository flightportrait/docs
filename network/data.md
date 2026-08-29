---
title: "Data and licensing"
description: "What the network serves, under which licenses, with what privacy floor."
---

## The licenses

| What | License |
| --- | --- |
| The network stack, API, and web tracker | Apache-2.0 |
| The firmware and device protocol | Apache-2.0 |
| Data served by the network | ODbL 1.0 |
| Basemap tiles | OpenFreemap |
| Airports | OurAirports, public domain |
| Aircraft photos | planespotters.net, credited per image |

Both licenses are irrevocable.

## What the network serves

* Live positions, trails, and per-station presence from network
  receivers.
* Derived routes: callsign to origin and destination, computed
  nightly from openly licensed trace archives and published back to
  the commons.
* Per-airframe flight logs: the legs the open trace archives show an
  aircraft flying, going back about a year. Observation only. A quiet
  day means the archives did not see it, nothing more.

## Attribution

Data (c) FlightPortrait network feeders, ODbL 1.0. If you build on
the API, that line and a link satisfy the license.

## The privacy floor

Station locations are published rounded to about 11 km, derived from
coverage, never from an address. Feeder IP addresses are not stored.
A station's full UUID is a capability held by its owner; the server
stores only a hash. These are not settings; they do not vary by
deployment.
