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
not heard.

Where coverage ends, a route stays half known. Those are listed at
[flightportrait.com/network/gaps.html](https://flightportrait.com/network/gaps.html)
and `GET /v1/gaps`; anyone who knows the other end can answer with
`POST /v1/contributions`. Answers are checked against what was
observed and reviewed before a flight's page shows them, with their
provenance (`route_source`). Observation always wins.

[Licenses and privacy](/network/data).
