---
title: "Bring your own server"
description: "Point a stock frame at any server that speaks three endpoints."
---

Keep the frame, replace the cloud. This is a supported, documented
path, not a hack: a stock frame can be re-pointed at any server that
implements three HTTPS endpoints, with no toolchain and no reflash.

## How the frame changes servers

The server base URL is set during pairing, or later through the
long-press re-provision flow, always by hands on the frame itself. No
server can ever change which server a frame talks to; there is no
remote path to that setting. Clear the URL and the frame returns to
the factory default.

## What your server must speak

Three endpoints under `/device/v1/`, published field by field in
[PROTOCOL.md](https://github.com/flightportrait/frame/blob/main/docs/PROTOCOL.md):

| Endpoint | What it does |
| --- | --- |
| `POST /device/v1/setup` | The frame introduces itself once and receives its credentials |
| `GET /device/v1/display` | The frame asks for the current drawing and its next wake time |
| `POST /device/v1/log` | The frame reports battery, signal, and any errors |

The drawing itself is a raw panel image: exactly 960,000 bytes, 1200
by 1600 portrait, two pixels per byte in the six-ink palette, verified
by the sha256 the display response carries. The format is specified
byte by byte in the same document.

## The reference server

[`examples/byos_server.py`](https://github.com/flightportrait/frame/blob/main/examples/byos_server.py)
is a complete server in plain stdlib Python. No framework, no
dependencies. Point a frame at it and it will set up, poll, download,
and display whatever panel image you serve. Start there, replace the
image it serves, and you have a frame that shows anything you can
render to 960,000 bytes.

## What you give up

Our renderer stays on our server: the daily poster of your sky is
what you buy when you buy a frame. A frame on your own server
displays whatever you draw for it instead. The hardware, the battery
life, and the privacy properties are identical either way.
