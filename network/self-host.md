---
title: "Run your own instance"
description: "The whole tracker from one repository and one compose file."
---

The whole tracker is one repository and one compose file. Two ways to
run it, by whether you own a receiver.

## With a receiver

The full experience: your antenna, your map, your data.

Hardware: an RTL-SDR dongle with a 1090 MHz antenna, on the machine
that runs the stack or feeding it over the network.

1. Clone [flightportrait/network](https://github.com/flightportrait/network).
2. `cp .env.example .env`, set `NETWORK_LAT` and `NETWORK_LON` to your
   antenna, set `COMPOSE_PROFILES=api`.
3. `docker compose up -d`. The aggregator listens for Beast input on
   port 30004; the API serves on 127.0.0.1:8092.
4. Point your receiver's feeder at your host:

   ```
   adsb,YOUR-HOST,30004,beast_reduce_plus_out,uuid=$(cat /proc/sys/kernel/random/uuid)
   ```

5. Serve `web/` from any static server, with one line before its
   scripts: `<script>window.FP_API="http://YOUR-HOST:8092"</script>`

You now have the map, trails, search and the station registry on your
own hardware. `ops/check_network.sh` and `ops/check_api.sh` verify
the chain end to end.

## Without a receiver

Point mode: the API polls a public aggregator for one region instead
of a local readsb. In `.env`:

```
COMPOSE_PROFILES=api
NETWORK_API_SOURCE_MODE=point
NETWORK_API_SOURCE_LAT=51.47
NETWORK_API_SOURCE_LON=-0.45
NETWORK_API_SOURCE_RADIUS_NM=250
```

Then `docker compose up -d api api-db` and serve `web/` as above. No
aggregator container, no hardware. Station features stay dark (there
are no stations); the live map, trails and search work.

Source choice and terms are annotated in `.env.example`. The poll
floor is 60 seconds and enforced in code. Whichever source you choose,
you are its guest.

## Enrichment layers

Routes and airframe logs are served from artifact files in `data/`
(`routes.json.gz`, `legs.db`). Our instance derives them nightly from
openly licensed trace archives and serves the results at
data.flightportrait.com; a self-hosted frontend gets them from there
by default. Absent artifacts simply return 404; the map works without
them.

## What talks to what

```
stations --beast--> aggregator(readsb) <--reads-- api <--fetch-- web
                                 |                  |
                          (hub/edge roles)     postgres (stations)
                                               data/ (artifacts)
```

One inbound port (30004, with a receiver). The API binds loopback;
expose it through your own reverse proxy or tunnel if you want it
public. The web frontend is static files with no server of its own.
