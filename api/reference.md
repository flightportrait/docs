---
title: "The public API"
description: "Live and historical flight data. No key, no account."
---

Live and historical flight data, openly licensed. No key, no account,
no tracking. Base URL:

```
https://data.flightportrait.com
```

Rate limits are per IP and generous for humans and small tools; a 429
tells you to slow down. Interactive OpenAPI docs live at
[data.flightportrait.com/docs](https://data.flightportrait.com/docs).
Data is ODbL 1.0: credit "FlightPortrait network feeders" and link
back.

## The live sky

| Endpoint | Returns |
| --- | --- |
| `GET /v1/now` | Aircraft and station counts, one small object |
| `GET /v1/aircraft` | Every aircraft the network hears right now |
| `GET /v1/trace/{hex}` | Recent positions of one aircraft, oldest first |
| `GET /v2/point/{lat}/{lon}/{radius}` | Aircraft within a radius (nautical miles, capped at 250), in the ecosystem's v2 envelope, so tools built for other aggregators work unchanged |

A snapshot older than 60 seconds is served as 503, never as live
data: an outage must never read as an empty sky.

## History and reference

| Endpoint | Returns |
| --- | --- |
| `GET /v1/route/{callsign}` | Origin and destination for a callsign, from the derived routes table |
| `GET /v1/airframe/{hex}` | One aircraft's observed flight log, newest first, about a year deep |
| `GET /v1/airport/{code}` | One airport: observed totals, busiest routes, and the inferred departures board in the airport's local time |
| `GET /v1/refdata/aircraft/{hex}` | Registration, type, operator |
| `GET /v1/refdata/airlines` | The airlines the network observes |
| `GET /v1/refdata/airlines/{icao}` | One airline: fleet, routes, countries |
| `GET /v1/refdata/airlines/{icao}/schedule/{org}/{dst}` | The inferred timetable for one leg: local departure and arrival times per flight number |
| `GET /v1/refdata/types/{designator}` | An aircraft type spelled out |
| `GET /v1/refdata/airports/{ident}` | One airport |

## Stations

| Endpoint | Returns |
| --- | --- |
| `GET /v1/stations` | The public roster: coarse locations only |
| `GET /v1/stations/{uuid}` | Your station's private status. Knowing the full UUID is the key; the server stores only its hash |

## An example

```sh
curl https://data.flightportrait.com/v1/now
```

```json
{"aircraft_count": 7, "aircraft_with_pos": 5,
 "station_count": 1, "generated_at": 1787924061.0}
```

To extend the coverage: [feed the network](/network/feed).
