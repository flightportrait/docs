---
title: "An open flight tracker"
description: "A live tracker built on community receivers and openly licensed data."
---

The FlightPortrait Network is an open source alternative to
FlightRadar24: a live flight tracker built on community receivers and
openly licensed data. Everything is in
[one repository](https://github.com/flightportrait/network), from the
antenna's Beast port to the map in your browser.

Our instance runs at
[flightportrait.com/network](https://flightportrait.com/network). It
is the biggest instance, not the only possible one.

## What it does

* A live map: aircraft as top-down silhouettes, sized by type, with
  trails, search, and callsign labels.
* A detail card per flight: photo, type spelled out, route with
  cities, altitude, speed, heading.
* A page per airframe: its observed flight log going back a year,
  rotations, quiet periods stated as quiet periods.
* Airline pages: fleets, destinations, route frequencies, derived
  departure boards.
* An open API serving all of it. Rate-limited, no key, no account.

## Observation, not inference

Everything shown is observation. Positions come from receivers, routes
and schedules are derived from what aircraft actually flew, and a gap
means the network did not hear it. There is no commercial schedule
feed and no inference dressed up as fact.

## The licenses

Apache-2.0 for the code, ODbL for the data, both irrevocable.
