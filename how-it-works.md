---
title: "How it works"
description: "What the frame does all day, and why nothing on the internet can reach into your home."
---

From your sky to your wall, in one direction only. This page explains
what your frame does all day, and why nothing on the internet,
including us, can reach into your home.

## Your sky, listened to

Every aircraft announces itself as it flies: where it is, how high,
what it is called. All day, our server listens for the ones that cross
your sky.

Those planes become one drawing. You choose how often a new one
appears: every few minutes, every few hours, or twice a day at sunrise
and sunset. The drawing is made on our server, not on the frame. The
frame's only job is to hang it up.

## The frame always speaks first

Most connected devices keep a door open so their maker can push things
to them. A FlightPortrait has no door. The frame wakes, calls out over
an encrypted connection, asks "is there a new drawing for me?", hangs
up, and goes back to sleep. The server can answer; it can never call.

This is not a setting that could be flipped later. It is the shape of
the system: the frame runs no server, keeps no port open, and accepts
no incoming connection of any kind.

## Asleep between drawings

Between drawings the frame sleeps with its radio off: not idle, not
listening, off. It wakes only to collect the next drawing, stays
connected for less than a minute, then sleeps again.

So the schedule you pick is also the battery: ask for a few drawings a
day and it runs for months on one charge, ask for one every few
minutes and it runs for weeks. Either way, most of the time there is
nothing on your network to talk to.

## What travels

When the frame calls, the conversation is small enough to write down
in full.

**The frame sends.** Which drawing it is showing, how full the battery
is, how good the signal is, and which firmware it runs. That is the
whole list. The frame does not know your name, your account or your
address. None of that is ever stored on it.

**The frame receives.** The drawing, when to wake up next, and now and
then a firmware update, which the frame checks byte by byte and
refuses to run unless it matches. One thing no server can change is
which server the frame talks to. Only hands on the frame itself can
set that.

## Your sky, not your address

To know which planes crossed your sky we need a rough centre, and
rough is all we keep. If you use your phone's position during setup,
the app rounds it before it is sent and the server rounds it again
before it is stored. We end up with a rounded point, a place name and
your timezone. That points at a neighbourhood, not at a doorstep.

## Bluetooth for setup, then silence

The one exception to "the frame only calls out" lasts a few minutes,
once: setup. The frame shows a QR code on its own glass, your phone
scans it, and the two talk over an encrypted Bluetooth session to hand
over your Wi-Fi details. When setup ends, Bluetooth switches off and
stays off. A factory reset wipes those details and starts the frame
over as a stranger.

## Don't take our word for it

The frame's [firmware is open source](https://github.com/flightportrait/frame),
and the [protocol it speaks](https://github.com/flightportrait/frame/blob/main/docs/PROTOCOL.md)
is published in full: every request, every field. If you'd rather not
talk to our server at all, you can
[point your frame at your own](/diy/byos). It is a supported,
documented path.
