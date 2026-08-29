---
title: "Feed the network"
description: "One line in a feeder config; never exclusive."
---

Feeding takes one line in the config of a receiver you may already
run, and it is never exclusive: our own stations feed adsb.lol,
adsb.fi and airplanes.live alongside.

## If you already feed other networks

Add one connector line to your feeder config:

```
adsb,feed.flightportrait.com,30004,beast_reduce_plus_out,uuid=YOUR-UUID
```

Generate a UUID with `cat /proc/sys/kernel/random/uuid` and keep it:
it is your station's identity and your private key to its status page.

## If you are starting from zero

A receiver is a US$40 RTL-SDR dongle with a 1090 MHz antenna and any
computer that runs Docker, a Raspberry Pi included.
[The join page](https://flightportrait.com/network/join.html)
generates your station identity in the browser and watches until your
antenna is heard, and its setup brief can be handed to whatever gets
your receiver configured, a person or an agent.

## What feeders get

The API, a private station status page (knowing your full UUID is the
key; we store only its hash), and a place on the map. Station
locations are shown rounded to about 11 km, computed from coverage,
never from an address. Feeder IPs are not stored.

## The terms, in short

Your data stays yours; you license the network to aggregate and
republish it under ODbL. Stop feeding whenever you like. The full
text is at
[flightportrait.com/network/terms](https://flightportrait.com/network/terms).
