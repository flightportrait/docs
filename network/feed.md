---
title: "Feed the network"
description: "One line in a receiver you already run, or the Station on a Raspberry Pi."
---

Feeding is not exclusive. Our own stations feed adsb.lol, adsb.fi and
airplanes.live alongside this network.

## Already running a receiver

readsb, in `NET_OPTIONS` of `/etc/default/readsb`:

```
--net-connector feed.flightportrait.com,30004,beast_reduce_plus_out,uuid=YOUR-KEY
```

ultrafeeder, one entry in `ULTRAFEEDER_CONFIG`:

```
adsb,feed.flightportrait.com,30004,beast_reduce_plus_out,uuid=YOUR-KEY;mlat,feed.flightportrait.com,31090,uuid=YOUR-KEY
```

MLAT with readsb: run mlat-client against `feed.flightportrait.com:31090`
with the same key. dump1090-fa cannot connect out; a readsb in
`--net-only` mode beside it carries the line. The adsb.im image takes
the ultrafeeder entry on its Expert page under "Ultrafeeder extra
args".

Generate a key with `cat /proc/sys/kernel/random/uuid` and keep it. It
is the station identity and the key to its status page. We store a
hash, not the key.

## Starting from nothing

A Raspberry Pi (3B or newer, or a Zero 2 W), an RTL-SDR dongle, a
1090 MHz antenna: about US$100, no soldering. Flash Raspberry Pi OS
Lite, log in, run one line:

```sh
curl -fsSL https://flightportrait.com/station/install.sh | sh
```

The installer says what it does before each step. It downloads the
Station runtime ([stationd](https://github.com/flightportrait/station)),
the radio ([rx](https://github.com/flightportrait/rx)) and the MLAT
client ([mlatc](https://github.com/flightportrait/mlatc)), installs a
service, and prints the address of a setup page: name the station, put
the antenna on the map, tick the networks to feed. The same page then
shows how the station is doing.

On a machine that already runs a receiver the installer stops first:
`--add` prints the line above and installs nothing; `--replace`
imports the existing feeds and keys and takes over.

[The join page](https://flightportrait.com/network/?mode=join)
issues a key and watches until the station is heard.

## What is public

The roster shows a generated id, online status, and a location
rounded to about 11 km from coverage. Not an address. Feeder IPs
are not stored. [Privacy policy](https://flightportrait.com/network/privacy).

[Feeder terms](https://flightportrait.com/network/terms): you keep
your data; the aggregate is published under ODbL. Stop whenever you
like.
