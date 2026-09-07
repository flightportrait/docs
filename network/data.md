---
title: "Data and licensing"
description: "What is served, under which licenses."
---

| What | License |
| --- | --- |
| Map, API and feeder docs | Apache-2.0 ([flightportrait/network](https://github.com/flightportrait/network)) |
| Station runtime, radio, MLAT client and server | AGPL-3.0 ([station](https://github.com/flightportrait/station), [rx](https://github.com/flightportrait/rx), [mlatc](https://github.com/flightportrait/mlatc), [mlatd](https://github.com/flightportrait/mlatd)) |
| Firmware and device protocol | Apache-2.0 ([flightportrait/frame](https://github.com/flightportrait/frame)) |
| Data from the API | ODbL 1.0 |
| Basemap tiles | OpenFreemap |
| Airports | OurAirports, public domain |
| Aircraft photos | planespotters.net, in the browser, credited per image |

Airline marks on the map belong to their owners.

## What the API serves

* Live positions, trails, and station presence from network
  receivers.
* Derived routes (callsign to origin and destination) from openly
  licensed trace archives, published back under ODbL.
* Per-airframe flight logs from those archives, about a year.
  A quiet day means the archives did not see it.

## Attribution

Data (c) FlightPortrait network feeders, ODbL 1.0. That line and a
link satisfy the license.

## Stations

Locations are rounded to about 11 km, from coverage. IPs are not
stored. The full key is held by the feeder; the server stores a
hash. [Privacy policy](https://flightportrait.com/network/privacy).
