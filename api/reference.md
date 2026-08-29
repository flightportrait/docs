---
title: "API"
description: "Live and historical flight data. No API key."
---

Base URL: `https://data.flightportrait.com`

No API key, no account. Rate limits are per IP over a 600 second window. 429 means wait.

Data is ODbL 1.0. Credit "FlightPortrait network feeders" and link back. [Licenses](/network/data). [Terms](https://flightportrait.com/network/terms).

```sh
curl https://data.flightportrait.com/v1/now
```

```json
{"aircraft_count": 7, "aircraft_with_pos": 5,
 "station_count": 1, "generated_at": 1787924061.0}
```

A live snapshot older than 60 seconds returns 503.

Spec: [openapi.json](/openapi.json).

[Feed the network](/network/feed).
