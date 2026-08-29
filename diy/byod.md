---
title: "Build your own frame"
description: "An ESP32-S3, a Spectra 6 panel, and the same open firmware."
---

No frame at all: the firmware is open, the panel is a catalog part,
and the protocol works the same whether we soldered the board or you
did.

## The parts

* An ESP32-S3 board. The firmware targets the Seeed reTerminal E1004
  (a complete enclosure with the panel already attached) and the XIAO
  ESP32-S3 Plus on the EE02 driver board.
* A 13.3 inch E Ink Spectra 6 panel, 1200 by 1600. This is the only
  panel the panel format targets; smaller Spectra 6 sizes need their
  own driver work.
* A battery if you want it on a wall, USB power if you do not.

## The path

1. Clone [flightportrait/frame](https://github.com/flightportrait/frame),
   build with ESP-IDF: `idf.py set-target esp32s3 && idf.py build`.
2. Before involving any network, prove the glass: `arduino-sd-demo/`
   blits a rendered panel image straight from microSD, so a bad panel
   cable never masquerades as a server problem.
3. Flash the firmware, then pair it like any frame. The pairing flow
   accepts your own server URL, so a self-built frame usually goes
   straight to a self-run server:
   [Bring your own server](/diy/byos).
4. `first-flash/` in the repo is the box-opening sequence we use on
   our own hardware, with test images and a one-page failure tree.
   It works the same on yours.

## What to show on it

Anything that renders to 960,000 bytes in six inks. The reference
server ships with the tooling to serve a static image; from there it
is your renderer against the same panel format we use, specified byte
by byte in
[PROTOCOL.md](https://github.com/flightportrait/frame/blob/main/docs/PROTOCOL.md).
