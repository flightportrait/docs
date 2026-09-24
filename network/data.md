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
| Aircraft, registrations and build years | Mictronics aircraft database via tar1090-db, ODC-By 1.0; FAA registry, public domain |
| Occurrence reports (Canada) | Transport Canada CADORS, Open Government Licence – Canada |
| Flight history and routes | our feeders and the adsb.lol archive, ODbL 1.0 |
| Airports | OurAirports, public domain; time zones from timezone-boundary-builder, ODbL 1.0 |
| Airline names | Virtual Radar Server standing data, CC0 1.0 |
| Ocean tracks and routes (estimates) | FAA, public domain; UK AIP (NATS) and AIP Ireland |
| Basemap tiles | OpenFreeMap, OpenMapTiles, OpenStreetMap |
| Night lights | NASA Black Marble, public domain |
| Aircraft photos | planespotters.net, in the browser, credited per image |

Every source, with its credit line:
[flightportrait.com/network/credits.html](https://flightportrait.com/network/credits.html).
Airline marks on the map belong to their owners.

## What the API serves

* Live positions, trails, and station presence from network
  receivers.
* Derived routes (callsign to origin and destination) from openly
  licensed trace archives, published back under ODbL.
* Per-airframe flight logs from those archives, about a year.
  A quiet day means the archives did not see it.
* Each airframe's lifetime record: registrations over time, serial,
  build year, owners where a registry publishes them, and notable
  events, each with its source.
* Estimated positions (`/v1/estimated`): cruising aircraft the network
  stopped hearing, placed for up to 15 minutes along their route and
  marked `estimated`. Never mixed into the observed data.

## Attribution

Data (c) FlightPortrait network feeders, ODbL 1.0. That line and a
link satisfy the license. Republishing the data carries the sources'
credits with it (the table above, and the credits page).

## Stations

Locations are rounded to about 11 km, from coverage. IPs are not
stored. The full key is held by the feeder; the server stores a
hash. [Privacy policy](https://flightportrait.com/network/privacy).
