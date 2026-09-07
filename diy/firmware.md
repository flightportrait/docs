---
title: "The firmware"
description: "The complete source of what a FlightPortrait runs."
---

The complete source of what a FlightPortrait runs lives at
[flightportrait/frame](https://github.com/flightportrait/frame),
Apache-2.0.

## What it is

An ESP-IDF application for the ESP32-S3. The whole life of the frame
is one loop: wake, poll the server over HTTPS, download a drawing if
there is one, push it to the panel, deep sleep until the server's
suggested wake time. Add BLE provisioning for first setup, byte-level
verification of every download, exponential backoff when the network
misbehaves, and an error log the frame reports on its next call.

There is no other behavior to find. The frame runs no server, opens no
port, and accepts no incoming connection.

## The hardware it targets

* The FlightPortrait frame: an ESP32-S3 driver board of our own design
* 13.3 inch E Ink Spectra 6 panel, 1200 by 1600, portrait
* Six inks: black, white, yellow, red, blue, green

## Map of the repo

* `docs/PROTOCOL.md`: the device API contract and the byte-level
  panel format. The only place the protocol is documented.
* `main/`: the shippable firmware. `nvs_schema.h` is the complete
  list of what a frame remembers.
* `examples/byos_server.py`: a reference server in stdlib Python.
* `first-flash/`: the box-opening sequence and its failure tree.
* `arduino-sd-demo/`: a no-network demo that blits a rendered panel
  image from microSD, useful for exercising a bare panel.
* `tests/`: contract tests that compile with plain `cc`.

## Building

```sh
cd frame
idf.py set-target esp32s3
idf.py build
```

The espressif/idf Docker image works when you would rather not
install the toolchain.
