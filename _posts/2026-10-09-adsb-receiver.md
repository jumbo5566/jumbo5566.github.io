---
layout: post
title: "ADS-B Aircraft Receiver: see the planes over your house"
subtitle: "Receive-only ADS-B board, 3.3V, 115200 serial"
author: jumbo5566
categories: [pro, adsb]
tags: [adsb, receiver, mavlink, antenna]
banner:
  video: ""
  loop: true
  volume: 0.8
  start_at: 8.5
  image: "/assets/images/adsb/ADSB.png"
  opacity: 0.618
  background: "#000"
---

Airliners broadcast their own identity and position, over and over, in the clear. A palm-sized board can pick that up and hand it to your computer over a serial cable — callsign, latitude, longitude, altitude, ground speed, heading, climb rate. No internet, no subscription, no monthly fee: the signal comes from the aircraft.

![Live aircraft map captured with the receiver](/assets/images/adsb/ADSB.png)

## What it is

A receive-only ADS-B receiver board. It listens on the aviation broadcast band, decodes what the aircraft around you are already transmitting, and delivers live aircraft information over a standard serial port to a laptop, Raspberry Pi, microcontroller or drone ground station. It never transmits, so it interferes with nothing.

## What you will see

- **Callsign / registration** — you know exactly which flight it is
- **Latitude and longitude** — plotted live on the map
- **Pressure altitude** — how high it is right now
- **Ground speed** and **heading**
- **Climb / descent rate**
- **Signal strength** — tells you whether the antenna is well placed
- **Many aircraft at once**, with stale targets cleared automatically

## Key features

- **No internet, no subscription** — works offline, zero running costs
- **Two mainstream output formats** — one for drone ground stations (Mission Planner and the MAVLink ecosystem), one for open-source flight-map tools; switch with a single command
- **Tunable sensitivity** — boost it to reach farther, dial it back near an airport to cut noise; takes effect in seconds
- **Self-tuning on power-up** — adapts to different antennas and RF modules
- **Settings survive power-off** — one command returns to factory defaults
- **Firmware update over one cable** — the same serial port, saved settings preserved
- **Plug and play** — four wires to a USB-to-serial adapter; powered at **3.3V**, draws about **10 mA**

## Four steps to a live map

1. **Wire it up** — `3.3V → VCC`, `GND → GND`, `TX → RX`, `RX → TX`. Use 3.3V only; on a CH340 breakout the `VCC` pin is 5V from USB, so take 3.3V from the adapter's regulator pin or a separate supply.
2. **Fit the antenna** — as high and open as you can (window, balcony, roof), away from metal and large glass.
3. **Open the software** — Mission Planner, a flight-map tool, or the browser serial terminal.
4. **Watch the aircraft** — icons appear on the map; click one for callsign, altitude, speed and heading.

## Tools

- **Web serial terminal** — `Web-serial-at.html` in the repository runs in Chrome / Edge / Opera over `http://localhost`, with the module's AT commands as one-click buttons. Set 115200 8N1, press CONNECT, send `AT`, get `OK`.
- **Python viewer** — `python mavlink_decoder_win.py COM25 115200` prints every decoded aircraft and logs each record to a `jsonl` file.
- **Repository** — [jumbo5566/adsb-module](https://github.com/jumbo5566/adsb-module)
- **Product page** — [ADS-B Receiver](/adsb.html)

## Specifications (user view)

- Power supply: **3.3V only** (5V not supported)
- Working current: approx. **10 mA**
- Interface: standard serial port, 115200 default, 9600 selectable
- Receive band: ADS-B aviation broadcast band (1090 MHz), with matching antenna
- Mode: receive only, no transmission

This device receives flight information that aircraft broadcast publicly, for learning, research, hobby use and derivative projects. Use it only in ways that comply with your local regulations.
