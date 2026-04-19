# 🦆 Duck Buddy — A Wearable Flock Safety Detector Plushie

*A conceptual art project for school. Not a product. Just a little duck who needs you.*

---

## What Is This?

Duck Buddy is a wearable plushie that detects Flock Safety surveillance cameras nearby and lets you know — by playing a random audio clip and shivering.

The idea came from wanting to honor the work done by the open source surveillance detection community in a way that felt human, tactile, and a little absurd. A helpless rubber duck who can sense danger but can't do anything about it except shake and quack. The user has to get them both to safety.

It is art. It is also functional. It is definitely a duck.

---

## The Concept

Flock Safety cameras are networked surveillance devices deployed across cities, often without public awareness. They capture license plates, log movements, and feed data to law enforcement databases. Projects like **Flock You** (see below) have built open source tools to detect their presence using WiFi and Bluetooth signals the cameras emit.

Duck Buddy takes that detection capability and puts it inside a plushie you can carry or wear. When the duck detects a Flock device nearby:

- It plays a **random audio clip** from an onboard speaker
- It **shivers** via a haptic motor

There is also an **attract mode button** — press it and the duck performs its detection response without needing a real target nearby. This was added so people could understand what the duck does without having to stand next to a surveillance camera to demonstrate it.

The goal was never to build a product. It was to make something that asks: *what if the thing warning you about surveillance was small, cute, and completely dependent on you?*

---

## Hardware

| Component | Detail |
|---|---|
| Microcontroller | Seeed XIAO ESP32-C3 |
| Audio | DFMini Player + speaker |
| Haptic | ERM motor via MOSFET (100Ω gate resistor, flyback diode) |
| Button | Momentary, active LOW on GPIO9 |
| Power | USB or LiPo |

### Pin Assignments

| Function | GPIO | Notes |
|---|---|---|
| DFMini RX | GPIO4 | via 1kΩ resistor |
| DFMini TX | GPIO5 | direct |
| Haptic motor | GPIO10 | MOSFET gate |
| Attract button | GPIO9 | active LOW, internal pull-up |

### SD Card

FAT32 formatted. Audio files named `001.mp3` through `025.mp3` in root directory.

---

## How It Works

The firmware (built on the Flock You codebase) runs WiFi in promiscuous mode and continuously scans BLE advertisements, looking for:

- Known Flock Safety SSIDs and MAC prefixes
- Penguin and Pigvision surveillance device signatures
- Raven gunshot detector BLE service UUIDs (SoundThinking/ShotSpotter)

When a match is found, the duck plays a random track and triggers the haptic motor. It continues a heartbeat pulse every 10 seconds as long as the device remains in range.

The attract mode button sets a flag that the main loop picks up — it plays a track and runs the haptic, then returns to scanning. One press, one response.

For more detail on the detection methodology, the original projects are the right place to look.

---

## Standing On The Shoulders Of Ducks

This project would not exist without the following people and their work:

### colonelpanichacks — Flock You (Original)
The original Flock You firmware. WiFi promiscuous mode detection, BLE scanning, MAC prefix matching, JSON output, the Flask dashboard — all of it originated here. Duck Buddy is built directly on this foundation.
👉 https://github.com/colonelpanichacks/flock-you/tree/main

### Storby42 — Flock You C3 OLED
The ESP32-C3 port with OLED support that made it practical to run this on the XIAO form factor. The C3-specific adaptations in this project reference this work.
👉 https://github.com/Storby42/flock-you-C3-OLED

### DeFlock
Crowdsourced ALPR camera location database and detection research. MAC prefix and SSID pattern databases draw from their work.
👉 https://deflock.me — https://github.com/FoggedLens/deflock

### GainSec
Published the `raven_configurations.json` dataset containing verified BLE service UUIDs from SoundThinking/ShotSpotter Raven acoustic gunshot detection devices across firmware versions 1.1.7, 1.2.0, and 1.3.1. The Raven detection in this firmware would not be possible without their research.
👉 https://github.com/GainSec

---

## Legal and Ethical Note

This is a school art project exploring surveillance, privacy, and the emotional language of technology. It is not a product. It is not for sale. Passive WiFi and BLE monitoring laws vary by jurisdiction... know yours. This project does not connect to, interfere with, or transmit to any device it detects.

---

## License

Educational and research use. Please respect the licenses of the upstream projects credited above.

---

*The duck cannot save itself. That's the point.*
