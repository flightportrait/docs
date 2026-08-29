---
title: "The public API"
description: "Live and historical flight data. No API key."
---

Base URL:

```
https://data.flightportrait.com
```

No API key, no account. Rate limits are per IP; a 429 means slow
down. Interactive docs:
[data.flightportrait.com/docs](https://data.flightportrait.com/docs).

Data is ODbL 1.0. Credit "FlightPortrait network feeders" and link
back. [Licenses and privacy](/network/data).

## Live sky

| Endpoint | Returns |
| --- | --- |
| `GET /v1/now` | Aircraft and station counts |
| `GET /v1/aircraft` | Aircraft the network hears right now |
| `GET /v1/trace/{hex}` | Recent positions of one aircraft, oldest first |
| `GET /v2/point/{lat}/{lon}/{radius}` | Aircraft within a radius (nautical miles, capped at 250), in the usual v2 envelope |

A snapshot older than 60 seconds returns 503.

## History and reference

| Endpoint | Returns |
| --- | --- |
| `GET /v1/route/{callsign}` | Origin and destination |
| `GET /v1/airframe/{hex}` | Observed flight log, newest first, about a year |
| `GET /v1/airport/{code}` | Totals, busiest routes, inferred board in local time |
| `GET /v1/refdata/aircraft/{hex}` | Registration, type, operator |
| `GET /v1/refdata/airlines` | Airlines the network observes |
| `GET /v1/refdata/airlines/{icao}` | One airline: fleet, routes, countries |
| `GET /v1/refdata/airlines/{icao}/schedule/{org}/{dst}` | Inferred timetable for one leg, local times |
| `GET /v1/refdata/types/{designator}` | Type name |
| `GET /v1/refdata/airports/{ident}` | One airport |

## Stations

| Endpoint | Returns |
| --- | --- |
| `GET /v1/stations` | Public roster, coarse locations |
| `GET /v1/stations/{uuid}` | Station status. The full UUID is the key; the server stores a hash |

## Example

```sh
curl https://data.flightportrait.com/v1/now
```

```json
{"aircraft_count": 7, "aircraft_with_pos": 5,
 "station_count": 1, "generated_at": 1787924061.0}
```

[Feed the network](/network/feed).
