---
title: "API"
description: "Live and historical flight data. No API key."
---

Base URL: `https://data.flightportrait.com`

No API key, no account. Rate limits are per IP over a 600 second window; a 429 carries `Retry-After` in seconds. Wait that long.

Data is ODbL 1.0. Credit "FlightPortrait network feeders" and link back. [Licenses](/network/data). [Terms](https://flightportrait.com/network/terms).

```sh
curl https://data.flightportrait.com/v1/now
```

```json
{"aircraft_count": 7, "aircraft_with_pos": 5,
 "station_count": 1, "generated_at": 1787924061.0}
```

## The rules of the road

* A live snapshot older than 60 seconds returns 503, never an empty sky. A missing history artifact returns 503 too, never a 404 that would claim the aircraft was not seen.
* Every error is `{"error": "<code>", "detail": "<text>"}`. The codes: `not_found`, `not_observed`, `invalid_request`, `rate_limited`, `stale_snapshot`, `artifact_unavailable`.
* Operations marked `stable` in the spec only ever gain fields. Operations marked `map` exist for [the map](https://flightportrait.com/network) and can change with it.
* `/v1/aircraft` and `/v2/point` speak readsb's wire dialect (`hex`, `t`, `r`, `gs`). Everything else uses full words (`reg`, `type`, `org`, `dst`).
* Everything is observation. Gaps mean the network's sources did not hear it, nothing more.

## A polite client

Poll `/v1/aircraft` every 10 seconds or slower; the edge caches for 10 seconds, so polling faster returns the same body. History and reference responses cache for an hour. Budgets are per route and generous for any client that is not scraping.

Spec: [openapi.json](/openapi.json).

[Feed the network](/network/feed).
