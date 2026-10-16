---
layout: post
permalink: /usbaudio-ptt-test.html
title: "Test your CM108 PTT in 15 seconds: recorder.exe -t"
subtitle: "Download, double-click test.cmd, watch PTT follow the audio"
author: jumbo5566
categories: [pro, radio]
tags: [cm108, ptt, sa818, ch340, radio, windows]
banner:
  video: ""
  loop: true
  volume: 0.8
  start_at: 8.5
  image: "/assets/images/usbaudio/test-waveform.png"
  opacity: 0.618
  background: "#000"
---

You wired a CM108 USB sound card to your radio for PTT, and now you need one answer: **does audio actually key the transmitter?** `recorder.exe -t` answers it in about fifteen seconds, with no talking and no terminal skills.

![Waveform of the 5 second test recording, peak at 0.800](/assets/images/usbaudio/test-waveform.png)

## Download

**[USBaudio_test.rar](/assets/downloads/USBaudio_test.rar)** — 363 KB, Windows 10 / 11, 64-bit. Open with 7-Zip or WinRAR.

- `recorder.exe` — the program. Single file, no installer, no DLLs
- `test.cmd` — one line: `recorder.exe -t`
- `README.en.md` — the full manual: PTT logic, SA818 / SR-FRS programming, wiring, troubleshooting

## Run it

1. Extract the archive to any folder
2. Double-click **`test.cmd`**

That is all. If Windows shows a warning because the build is unsigned, choose Run anyway.

The program records 5 seconds from the USB microphone, raises the level to 80 % of full scale, saves it as `test.wav`, plays it back over the USB speaker for 5 seconds — and PTT follows that playback volume exactly as it does in normal use. Silence at the input is not a mystery: it prints hints about microphone level, the Windows microphone privacy setting and the mic wiring.

While it runs you get one live line:

```
[  3.2s] TX vol [########------]  35%  PTT:ON  'q' quit
```

`TX vol` is the sound card's playback level, measured 10 times a second. `PTT:ON` means GPIO3 is asserted and the radio should be transmitting. Press `q` to stop at any time — PTT is released on exit, so the radio can't be left stuck in transmit.

Raising the recording to 80 % is what makes this test trustworthy. PTT keys above 5 % volume, so a quiet microphone can sit under the threshold all day and make working hardware look broken. After normalization the same audio always keys the radio, and a failure means a real fault. The waveform above is a real run: 4.98 s, 44.1 kHz, peak exactly **0.800**.

## What it is

USBaudio drives a CM108-family USB sound card (`VID_0D8C&PID_0012`) and a CH340 serial adapter (`VID_1A86&PID_7523`). It keys the radio from the playback volume, and programs SA818 / SR-FRS modules over serial.

- **GPIO3** → radio PTT. Keys above 5 % volume, holds 3 seconds so gaps between digital-mode bursts don't unkey it
- **IO4** → the blue COS LED: a 5 second pulse at startup
- Speaker output → radio mic input; CH340 → SA818 / SR-FRS at 9600 8N1

## Other modes

- `recorder.exe` — auto: PTT mode if the CM108 is found, frequency setup if only the CH340 is there, help if neither
- `recorder.exe -f` — set frequency, tone, squelch, volume, bandwidth. SA818 and SR-FRS are detected by themselves; type `88.5` and the CTCSS code is worked out for you
- `recorder.exe -r` — record from the USB microphone until you press `q`, saves a WAV and draws the waveform
- `recorder.exe -h` — all options

## If something is wrong

- **`[PTT] No HID device found`** — another program holds the CM108 (SDR software is the usual suspect), or the card has a different VID/PID
- **Recording is silence** — Windows microphone level, microphone privacy setting, then the mic wiring on the board
- **No sound on playback** — the USB speaker endpoint is disabled in Windows
- **`+DMOSETGROUP:1`** in `-f` mode — frequency outside the module's band, or a tone format it dislikes

---

For amateur radio use, as-is. The CM108 PTT code is a Windows port of the open-source cm108-ptt (Hamlib-derived) implementation; SR-FRS follows the `srfrs.py` reference by Fred (W6BSD) / BG7IYN.
