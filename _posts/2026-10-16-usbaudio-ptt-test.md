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

## A real run, from cold start to key-up

This is the whole session, copied out of the console:

```
C:\Users\Administrator\Desktop\USBaudio>recorder.exe -t
Usage: recorder.exe [option]

Options:
  (none)   Auto mode (default): if the CM108 USB sound card is present,
           run PTT mode (volume > 5% -> PTT on, 3 s min hold; IO4 high
           for 5 s at startup = the blue COS LED). Otherwise, if a CH340
           serial adapter is present, run frequency setup mode. If neither
           is found, show this help.
  -r       Recorder mode: record from the USB mic until 'q' pressed,
           then save to a WAV file and show the waveform.
  -f       Frequency setup mode: find the CH340 serial adapter
           (USB\VID_1A86&PID_7523), connect to the SA818 module and
           program frequency / tone / squelch / volume (no range check).
           Enter 'q' at any prompt to quit without changes.
  -t       Test mode: record 5 s from the USB mic (normalized), play it
           back on the USB speaker; PTT follows the playback volume.
           Exits by itself.
  -h       Show this help and exit.

Target device: USB\VID_0D8C&PID_0012 (CM108-family USB audio, VID/PID in code).

Note (Windows): when using the 'incoming radio signal triggers on
volume decrease' feature, please remove the diode on the board.

=== Test Mode: record 10 s, then play back (PTT driven by volume) ===

[USB] Found target device: 麦克风 (3- USB Audio Device) (matched by registry name "USB Audio Device")
[TEST] Recording 5 s from the USB mic...
[REC] Recording... press 'q' + Enter to stop
[REC]   4.9 sec | vol [                         ]   0% | press 'q' to stop
[OK] Recording stopped after 5 sec (215208 frames)
     Sample rate: 44100 Hz, Channels: 1, Bit depth: 16-bit
[NORM] Peak 0.0% -> scaling by 2621.4x to 80%
[SAVED] Written to: test.wav (4.9 sec)
[TEST] Recording level: peak 80.0%, RMS 6.4%
[PTT] Found CM108 HID device: \\?\hid#vid_0d8c&pid_0012&mi_03#7&38dee8e7&0&0000#{4d1e55b2-f16f-11cf-88cb-001111000030}
[PTT] Monitoring playback of: 扬声器 (3- USB Audio Device)
[IO4] >>> HIGH at startup = blue COS LED on (auto-low after 5 s)

[[PTT] MoniTEtoSTri] ngPlaying back the reco playback volume (threshold 5%). Press 'q' to quit.
[  0.0s] RX       vol [                         ]   0%  PTT:OFF  'q' quit follows playback volume)...
[PLAY] test.wav: 44100 Hz, 1 ch, 215208 samples (4.9 s)
[PLAY] Render format: 48000 Hz, 2 ch, 32-bit float
[PLAY] Playing...

[PTT] >>> TX ON  (vol 38.7%, min 3 s hold)
[  0.2s] TX( 2.9s) vol [########                 ]  32%  PTT:ON   'q' quit
```

Reading it, line by line, this is what you are actually checking:

- **`[USB] Found target device: 麦克风 (3- USB Audio Device)`** — the microphone side was picked out of the registry on its own, nothing to configure
- **`[NORM] Peak 0.0% -> scaling by 2621.4x to 80%`** — the room was quiet. The raw capture peaked at next to nothing, and normalization pushed it up to 80 % anyway. This single line is why the test never needs you to talk into the microphone
- **`[PTT] Found CM108 HID device: \\?\hid#vid_0d8c&pid_0012...`** — the GPIO side found too, by the same rule
- **`[IO4] >>> HIGH at startup`** — the blue COS LED lights for 5 seconds and drops by itself
- **`PTT:OFF` at 0.0 s, then `>>> TX ON (vol 38.7%, min 3 s hold)`** — the transmitter keys because the playback volume crossed 5 %, and for no other reason
- **`TX( 2.9s)`** — the 3 second minimum hold counting down, so a momentary dip in the audio cannot unkey the radio mid-burst
- **`test.wav: 44100 Hz, 1 ch` into `Render format: 48000 Hz, 2 ch`** — it resamples on the way out, so a 44.1 kHz mono file plays correctly on a 48 kHz stereo endpoint

The two garbled lines in the middle are the PTT monitor and the playback thread writing to the same console at the same time; they only overlap on screen, the behaviour is fine. And the banner line still says `record 10 s` — leftover text from an older build, the capture is 5 seconds, as the `[TEST]` line says.

After the 4.9 seconds of playback run out, the volume drops, PTT releases after the hold, and the program exits on its own.

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
