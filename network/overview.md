---
title: "The network"
description: "Map, API, and how to feed."
---

A network of receivers, open to anyone with an antenna. The aircraft
on the map, and on the wall, are what those receivers heard.

- Map: [flightportrait.com/network](https://flightportrait.com/network)
- API: [data.flightportrait.com](https://data.flightportrait.com)
  (no API key). [Reference](/api/reference)
- Feed: [the Station](/network/feed)
- Source: [github.com/flightportrait/network](https://github.com/flightportrait/network)
  (map, API, feed docs), [station](https://github.com/flightportrait/station),
  [rx](https://github.com/flightportrait/rx),
  [mlatc](https://github.com/flightportrait/mlatc),
  [mlatd](https://github.com/flightportrait/mlatd)

Live positions come from receivers on the network. Routes and
airframe logs come from openly licensed traces. A gap means it was
not heard. Where a cruising aircraft drops out of range, the map can
show where it most likely is for up to 15 minutes, in grey and marked
as estimated; that layer is off until you turn it on.

Where coverage ends, a route stays half known. Those are listed at
[flightportrait.com/network/gaps.html](https://flightportrait.com/network/gaps.html)
and `GET /v1/gaps`; anyone who knows the other end can answer on that
page. Answers are checked against what was observed, corroborated ones
enter the catalog, and a flight's page shows the route with its
provenance (`route_source`). Observation always wins.

[Licenses and privacy](/network/data). [Credits](https://flightportrait.com/network/credits.html).
