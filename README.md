<div align="center">

<img src="docs/images/banner.svg" alt="Rocket Lamp – an ESPHome smart upgrade for the IKEA AFTONSPARV rocket lamp" width="100%">

<br>

[![IKEA AFTONSPARV](https://img.shields.io/badge/IKEA-AFTONSPARV%20hack-0058A3?style=for-the-badge&logo=ikea&logoColor=FFDB00)](https://www.ikea.com/gb/en/p/aftonsparv-led-table-lamp-rocket-multicolour-00555231/)
[![ESPHome](https://img.shields.io/badge/ESPHome-2026.9%2B-000000?style=for-the-badge&logo=esphome&logoColor=white)](https://esphome.io)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-ready-41BDF5?style=for-the-badge&logo=homeassistant&logoColor=white)](https://www.home-assistant.io)
[![ESP32-C3](https://img.shields.io/badge/ESP32--C3-Super%20Mini-E7352C?style=for-the-badge&logo=espressif&logoColor=white)](https://www.espressif.com/en/products/socs/esp32-c3)

**Give IKEA's AFTONSPARV rocket lamp a new brain.**<br>
Swap the stock electronics for an ESP32-C3 and a WS2812 LED ring, and the kids' rocket gets smooth fades,
rainbow & candle effects, a nightlight, stepless dimming and full Home Assistant control.
The firmware installs as an ESPHome package straight from GitHub.

[Features](#-features) •
[Demo](#-demo) •
[The mod](#-the-hardware-mod) •
[Install](#-firmware-installation) •
[Controls](#-button-controls) •
[Customise](#-customisation) •
[Troubleshooting](#-troubleshooting)

</div>

---

> [!CAUTION]
> This is an unofficial hobby project and is **not affiliated with or endorsed by IKEA**. Opening and
> modifying the lamp voids its warranty, and the modified lamp no longer carries IKEA's safety testing for
> children's products. The mod only touches the **low-voltage (24 V DC) side** – never open the plug-in
> adapter. Insulate all connections, fix everything firmly in place, and build it at your own risk.

## ✨ Features

### Stock vs. modded

| | IKEA AFTONSPARV (stock) | 🚀 Rocket Lamp (modded) |
| --- | :---: | :---: |
| Colours | 7 colours | 9 modes + any colour from Home Assistant |
| Colour cycling | Automatic colour switching | 🌈 Smooth rainbow + 🕯️ flickering candle |
| Button | Pick a colour or auto mode | Click / double / triple click + hold gestures |
| Smart home | – | 🏠 Native Home Assistant light entity |
| Updates | – | 📡 Encrypted over-the-air updates |

### Highlights

|     |     |
| --- | --- |
| 🌈 **9 lighting modes** | Rainbow, Candle, Warm White, Red, Blue, Green, Yellow, Purple and Pink |
| 🎞️ **Smooth everything** | Fades on power on/off and cross-fades between modes, no hard cuts |
| 🔘 **One-button control** | Click, double click, triple click and hold – no app needed |
| 🌙 **Nightlight & dimming** | Warm 1 % glow when held while off, stepless dimming when held while on |
| 🔌 **Keeps the IKEA adapter** | Runs from the lamp's original 24 V power supply through a tiny buck converter, with a built-in LED output cap so it never overloads |
| 🏠 **Home Assistant native** | Auto-discovered light entity with every effect and a colour picker |
| 📦 **Remote package** | Pull the firmware from GitHub, keep only Wi-Fi + key in your own YAML |
| 🔐 **Secure by default** | Encrypted API *and* encrypted OTA updates with a single key |

## 🎬 Demo

https://github.com/user-attachments/assets/cf82f476-0f0d-4c87-8bef-dffe335c4a18

<p align="center">
  <sub>Video not playing? <a href="docs/images/demo.mp4">▶️ Open the demo video</a></sub>
</p>

## 🔧 The hardware mod

The lamp's original LED board and controller come out. In their place go an **ESP32-C3 Super Mini**,
a **37 mm, 12-LED WS2812 ring** and a **new push button**. The **original IKEA adapter stays** – a
tiny **CR-4030L** buck converter in the base turns its 24 V into the 5 V the new electronics need.

### Bill of materials

| Qty | Part | Notes |
| :-: | ---- | ----- |
| 1 | **IKEA AFTONSPARV LED table lamp, rocket** | e.g. article 005.552.31 (UK) – the number varies by country |
| 1 | IKEA **ICPSW24-3-3** adapter | Comes with the lamp: 24 V DC, max 125 mA / 3 W |
| 1 | **[CR-4030L buck converter, 6–32 V → 5 V](https://www.aliexpress.com/item/1005005505907937.html)** | Tiny fixed-output step-down module – pick the **6V-32V to 5V** version, which handles the 24 V adapter with headroom. Alternatives must accept 24 V input: an MP1584EN works, a Mini-360 (~23 V max) does not |
| 1 | **ESP32-C3 Super Mini** | Any ESP32-C3 board works, see [other boards](#using-a-different-board) |
| 1 | **WS2812 LED ring, 12 LEDs** – [AliExpress](https://www.aliexpress.com/item/1005007659632831.html) or [Adafruit NeoPixel Ring 12](https://www.adafruit.com/product/1643) | **37 mm** outside / **23 mm** inside diameter – choose the *12 bits* (AliExpress) or *12 x RGB LEDs* (Adafruit) version |
| 1 | **Momentary push button** | Normally open, wired to GND |
| – | Thin silicone wire, heat-shrink, hot glue / double-sided foam tape | Keep everything insulated and fixed |

#### 🛒 Where to buy

The parts used in the original build come from AliExpress:

- **LED ring:** [WS2812 RGB LED ring, 37 mm](https://www.aliexpress.com/item/1005007659632831.html) – select the **12 bits** version
- **Buck converter:** [CR-4030L DC-DC step-down module](https://www.aliexpress.com/item/1005005505907937.html) – select the **6V-32V to 5V** version

A good alternative LED ring:

- **LED ring:** [Adafruit NeoPixel Ring – 12 x 5050 RGB LED](https://www.adafruit.com/product/1643) – same size
  (36.8 mm outside / 23.3 mm inside), ships with WS2812B or SK6812 LEDs (same protocol) and is available
  from many local distributors. Select **12 x RGB LEDs** – the RGBW and WWA variants won't work with this firmware.

> [!NOTE]
> Marketplace listings change over time. If a link is dead, search for the part name and check the
> specs above (ring size, LED count, 5 V output, 24 V-capable input).

### Power budget

The stock adapter delivers only **3 W**. After the CR-4030L (roughly 90–95 % efficient) that leaves
about **550 mA at 5 V** for the ESP32-C3 *and* the LEDs. The ESP32-C3 needs ~100–120 mA of that,
and each WS2812 colour channel can draw up to ~20 mA at full power:

| Mode (12 LEDs) | Worst case at 100 % | With the 50 % cap | + ESP32-C3 |
| -------------- | ------------------: | ----------------: | ---------: |
| ⚪ Full white (picked in Home Assistant) | ~720 mA ❌ | ~360 mA | ~480 mA ✅ |
| 🌈 Rainbow (yellow/cyan/magenta peaks) | ~480 mA ⚠️ | ~240 mA | ~360 mA ✅ |
| 🔆 Warm White | ~440 mA ⚠️ | ~220 mA | ~340 mA ✅ |
| 🟡 Solid Yellow | ~410 mA ⚠️ | ~205 mA | ~325 mA ✅ |
| 🟣 Solid Purple | ~400 mA ⚠️ | ~200 mA | ~320 mA ✅ |
| 🌸 Solid Pink | ~390 mA ⚠️ | ~195 mA | ~315 mA ✅ |
| 🕯️ Candle | ~360 mA | ~180 mA | ~300 mA ✅ |
| 🔴 🟢 🔵 Single colour | ~240 mA | ~120 mA | ~240 mA ✅ |

Uncapped, white and the brighter modes would overload the adapter, making the lamp flicker or reboot.
That's why the firmware **caps the LED output at 50 % by default** (`max_brightness_pct`). The cap is
applied to the hardware output, so **100 % in Home Assistant = the safe maximum**, and dimming, fades
and the nightlight all work across the full range. The nightlight brightness is compensated
automatically, so it looks exactly as it would without the cap.

> [!TIP]
> These are worst-case figures; real WS2812s often draw less. If you pick pure white at 100 % in
> Home Assistant and measure the current with a USB meter or multimeter, you can raise the cap as long as the LEDs stay below ~400 mA (leaving headroom for the ESP32-C3):
>
> ```yaml
> substitutions:
>   max_brightness_pct: "60"
> ```
>
> Using a bigger 5 V supply instead of the IKEA adapter? Then you can set it to `"100"`.

The firmware also lowers the Wi-Fi TX power (8.5 dB), which saves a little power as well.

### Wiring

| From | To | Substitution |
| ---- | -- | ------------ |
| IKEA adapter **+24 V** (lamp's DC input) | CR-4030L `VIN` | – |
| IKEA adapter **GND** | CR-4030L `GND` | – |
| CR-4030L `VOUT` (**check it reads 5.0 V first!**) | ESP32-C3 `5V` and LED ring `5V` / `VCC` | – |
| CR-4030L `GND` | ESP32-C3 `GND`, LED ring `GND`, one leg of the button | – |
| ESP32-C3 `GPIO3` | LED ring `DIN` | `led_pin` |
| ESP32-C3 `GPIO0` | Other leg of the button (internal pull-up is enabled) | `button_pin` |

```mermaid
flowchart LR
    ADP(["🔌 IKEA ICPSW24-3-3<br/>24 V DC · 3 W"]) -- "24 V" --> BUCK["⚡ CR-4030L buck<br/>24 V → 5 V"]
    ADP -- "GND" --- BUCK
    BUCK -- "5 V" --> V5
    BUCK -- "GND" --- G
    BUCK -- "5 V" --> LED
    BUCK -- "GND" --- LED
    subgraph MCU["ESP32-C3 Super Mini"]
        direction TB
        V5["5V"]
        G["GND"]
        IO3["GPIO3"]
        IO0["GPIO0"]
    end
    IO3 -- "data" --> LED["💡 WS2812 ring<br/>12 LEDs · Ø 37 mm"]
    IO0 --- BTN["🔘 Push button"]
    BTN --- G
```

### Circuit board

<p align="center">
  <a href="https://app.cirkitdesigner.com/project/2a295333-1f8f-4950-9f45-bd626191a41b" target="_blank">
    <img src="docs/images/pcb.svg" alt="Rocket Lamp circuit diagram" width="100%">
  </a>
</p>

### Build steps

#### 1. Flash first

Install the firmware on the ESP32-C3 over USB *before* it goes into the lamp
(see [Firmware installation](#-firmware-installation)). Later updates happen over Wi-Fi.

#### 2. Open the lamp and remove the stock electronics

Unplug the lamp and unscrew the base – IKEA ships the adapter and fins inside it, so it's designed
to come apart with a screwdriver. Take out the original LED board and controller.

#### 3. Reuse the power connector

Keep the lamp's DC socket for the IKEA adapter and solder a red/black pair of wires to it – it will
feed the step-down regulator. Check the polarity with a multimeter before you connect anything.

<p align="center">
  <img src="docs/images/reusing_power_connector.jpg" alt="The lamp's original DC socket with new red and black wires soldered on" width="420">
</p>

#### 4. Wire the step-down regulator

Solder the DC socket wires to the CR-4030L input, and a 5 V/GND pair from its output to each of the
ESP32-C3 and the LED ring (see the [wiring table](#wiring)). Power it from the IKEA adapter on its own
first and make sure a multimeter reads **5.0 V** on the output – a wrong variant would destroy the
ESP32 and the LEDs.

<p align="center">
  <img src="docs/images/step_down_regulator.jpg" alt="CR-4030L step-down regulator with input and output wires soldered on" width="420">
</p>

#### 5. Wire the LED ring

Solder 5 V, GND and the data wire (to the ring's `DIN`, not `DOUT`) to the 37 mm ring.

<p align="center">
  <img src="docs/images/led_ring.jpg" alt="12-LED WS2812 ring with power and data wires soldered on" width="420">
</p>

#### 6. Mount the LED ring under the lens

The stock acrylic lens has 12 domes arranged in a circle – line up each LED of the ring with one dome
and fix the ring in place.

<p align="center">
  <img src="docs/images/led_ring_on_lens.jpg" alt="LED ring fitted behind the lamp's acrylic lens, each LED under one lens dome" width="320">
</p>

#### 7. Mount the button and the ESP32-C3

Hot-glue the push button and the ESP32-C3 onto small pieces of clear plastic, so they can be fixed
firmly inside the base. The button sits at the front of the base, where it's pressed in the
[demo video](#-demo). Insulate every joint with heat-shrink.

<table>
  <tr>
    <td align="center"><img src="docs/images/button_on_plastic.jpg" alt="Push button hot-glued onto a piece of clear plastic" width="400"><br><sub><b>Button on plastic</b></sub></td>
    <td align="center"><img src="docs/images/esp32_on_plastic.jpg" alt="ESP32-C3 Super Mini hot-glued onto a piece of clear plastic" width="400"><br><sub><b>ESP32-C3 on plastic</b></sub></td>
  </tr>
</table>

#### 8. Assemble

Put the lens with the LED ring back into the lamp, then route the wires inside the base: the DC socket
goes into its original slot and the ESP32-C3 and button fit between the ribs of the base. Keep the
wires clear of the screw posts. Plug in the adapter and test that the lamp joins Wi-Fi and reacts to
the button **before** you close everything up, then attach the fins.

<table>
  <tr>
    <td align="center"><img src="docs/images/assemble_1.jpg" alt="Lamp section with the lens and LED ring fitted, electronics still outside" width="260"><br><sub><b>Lens and LED ring fitted</b></sub></td>
    <td align="center"><img src="docs/images/assemble_2.jpg" alt="Inside of the base with the DC socket, ESP32-C3, button and wiring in place" width="260"><br><sub><b>Electronics in the base</b></sub></td>
    <td align="center"><img src="docs/images/assemble_3.jpg" alt="Base closed up with the red fins attached" width="340"><br><sub><b>Fins on</b></sub></td>
  </tr>
</table>

> [!IMPORTANT]
> Never connect USB to the ESP32-C3 while the lamp is powered from the IKEA adapter. On many Super Mini
> boards the `5V` pin is wired straight to USB `VBUS`, so the two supplies would fight. For USB
> flashing or logs, unplug the adapter first – or just use OTA updates.

## 🚀 Firmware installation

The firmware is published as an [ESPHome remote package](https://esphome.io/components/packages/#remotegit-packages).
Your own device config only contains what is personal to you – Wi-Fi credentials and an encryption
key – and pulls everything else from this repository.

> [!IMPORTANT]
> Requires **ESPHome 2026.9.0 or newer** (for encrypted OTA updates).

### 1. Create your device config

Create `rocket-lamp.yaml` in your ESPHome config folder (or copy [`example.yaml`](example.yaml)):

```yaml
substitutions:
  name: rocket-lamp
  friendly_name: Rocket Lamp

packages:
  rocket_lamp: github://nfjodor/ikea-rocket-lamp/rocket-lamp.yaml@main

api:
  encryption:
    key: !secret api_encryption_key

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

### 2. Add your secrets

Add these to your `secrets.yaml` (see [`secrets.example.yaml`](secrets.example.yaml)):

```yaml
wifi_ssid: "your-wifi"
wifi_password: "your-wifi-password"
api_encryption_key: "..."   # openssl rand -base64 32
```

> [!NOTE]
> Remote packages can't read `!secret` themselves, which is why the secrets live in your device
> config. The same API key also encrypts OTA updates – there is no separate OTA password.

### 3. Flash it

<details open>
<summary><b>🏠 ESPHome Device Builder (Home Assistant add-on)</b></summary>

1. Open **ESPHome Device Builder** → **+ New device** → **Continue** → **Skip**.
2. Open the new device's YAML, replace it with the config from step 1 and **Save**.
3. Plug the ESP32-C3 into your computer via USB (not yet wired to the IKEA adapter) and choose
   **Install** → **Plug into this computer** (or **Manual download** and flash it with
   [web.esphome.io](https://web.esphome.io)).
4. Once it's built into the lamp, update over Wi-Fi with **Install** → **Wirelessly**.

</details>

<details>
<summary><b>💻 ESPHome CLI</b></summary>

```bash
pip install --upgrade esphome      # 2026.9.0 or newer
esphome run rocket-lamp.yaml       # first time over USB, afterwards over Wi-Fi
esphome logs rocket-lamp.yaml      # watch it work
```

</details>

<details>
<summary><b>📶 Lamp can't join Wi-Fi?</b></summary>

If the credentials are wrong the lamp opens a hotspot called **"Rocket Lamp Fallback"**.
Connect to it and a captive portal lets you pick the right network.

</details>

### 4. Add it to Home Assistant

Home Assistant discovers the lamp automatically: **Settings → Devices & services → ESPHome →
Configure**, then paste your `api_encryption_key`. You get a **Lamp** light entity with brightness,
colour and every effect.

## 🔘 Button controls

| Gesture | When the lamp is **off** | When the lamp is **on** |
| ------- | ------------------------ | ----------------------- |
| **Click** | Fade on with the last brightness and mode | Fade off |
| **Double click** | Turn on in the last used mode | Cross-fade to the next mode |
| **Triple click** | Reset to **Warm White @ 60 %** | Reset to **Warm White @ 60 %** |
| **Hold** (≥ 0.7 s) | 🌙 Warm nightlight at 1 % | 🔅 Dim / brighten while held – direction flips on every new hold |

**Mode cycle (double click):**
🌈 Rainbow → 🕯️ Candle → 🔆 Warm White → 🔴 Red → 🔵 Blue → 🟢 Green → 🟡 Yellow → 🟣 Purple → 🌸 Pink → 🌈 …

**From Home Assistant** you can pick any effect, brightness or colour. Picking a colour while an
effect is running stops the effect so your colour shows, and leaving the nightlight for a mode
brings back the brightness you had before.

> [!TIP]
> A rocket that glows dim red at bedtime and fades off automatically makes a great Home Assistant
> automation for the kids' room.

## 🎨 Customisation

Every value below is a [substitution](https://esphome.io/components/substitutions/) – override it
in the `substitutions:` block of your own device config, no fork required.

<details open>
<summary><b>Device & hardware</b></summary>

| Substitution | Default | Description |
| ------------ | ------- | ----------- |
| `name` | `rocket-lamp` | Hostname (`rocket-lamp.local`) |
| `friendly_name` | `Rocket Lamp` | Name shown in Home Assistant |
| `led_pin` | `GPIO3` | WS2812 data pin |
| `button_pin` | `GPIO0` | Push button pin (to GND) |
| `num_leds` | `12` | Number of LEDs in your ring |
| `led_chipset` | `WS2812` | Any [`esp32_rmt_led_strip`](https://esphome.io/components/light/esp32_rmt_led_strip/) chipset |
| `led_rgb_order` | `GRB` | Colour order of your LEDs |
| `max_brightness_pct` | `50` | LED output cap in %, keeps the 3 W IKEA adapter from overloading – see [power budget](#power-budget) |

</details>

<details>
<summary><b>Button timing</b></summary>

| Substitution | Default | Description |
| ------------ | ------- | ----------- |
| `click_max` | `400ms` | Longest press that still counts as a click |
| `click_gap` | `400ms` | Longest pause between clicks of a double/triple click |
| `long_press` | `700ms` | Hold time that starts nightlight / dimming |

</details>

<details>
<summary><b>Fades & effects</b></summary>

| Substitution | Default | Description |
| ------------ | ------- | ----------- |
| `fade_ms` | `250` | On/off fade in ms |
| `mode_fade_ms` | `250` | Cross-fade between modes in ms |
| `dim_step_ms` | `40` | Interval of the dimming loop in ms |
| `dim_factor` | `1.06` | Brightness multiplier per dimming step |
| `rainbow_speed` | `2` | Rainbow speed – one full cycle takes `65536 / speed` ms |

</details>

<details>
<summary><b>Mode colours</b></summary>

Each colour is `red, green, blue` with values from 0 to 255. The defaults are tuned for WS2812 LEDs,
whose green and blue are much brighter than their red.

| Substitution | Default | Mode |
| ------------ | ------- | ---- |
| `warm_white_color` | `255, 138, 12` | 🔆 Warm White |
| `red_color` | `255, 0, 0` | 🔴 Solid Red |
| `blue_color` | `0, 0, 255` | 🔵 Solid Blue |
| `green_color` | `0, 255, 0` | 🟢 Solid Green |
| `yellow_color` | `255, 180, 0` | 🟡 Solid Yellow |
| `purple_color` | `170, 0, 255` | 🟣 Solid Purple |
| `pink_color` | `255, 40, 120` | 🌸 Solid Pink |
| `nightlight_color` | `255, 102, 0` | 🌙 Nightlight |

The Candle colour is set with `candle_color`, see below. Colours with more of each channel draw more
current – the [power budget](#power-budget) figures assume the defaults.

The nightlight always runs at 1 %, so only the mix of `nightlight_color` matters: `128, 51, 0` looks the
same as `255, 102, 0`. At such a low level each channel only has a few steps, so a channel set much lower
than the others can drop out completely.

</details>

<details>
<summary><b>Candle</b></summary>

| Substitution | Default | Description |
| ------------ | ------- | ----------- |
| `candle_color` | `255, 138, 12` | Red, green, blue (0–255) of the flame |
| `candle_update_interval` | `16ms` | Frame interval – smoothing is applied per frame, so a longer interval also slows the flicker |
| `candle_smoothing` | `0.25` | How far the flame moves towards its next brightness each frame (0–1, higher = jumpier) |
| `candle_flicker_depth` | `0.25` | Max dip of a normal flicker (0–1, 0.25 = up to 25 % darker) |
| `candle_dip_depth` | `0.55` | Max dip of an occasional deep flicker (0–1) |
| `candle_dip_chance` | `0.08` | Chance that the next flicker is a deep one (0–1) |

</details>

**Example** – a second rocket for the kids' room with a slower rainbow, a calmer, more orange candle
and snappier double clicks:

```yaml
substitutions:
  name: kids-room-rocket
  friendly_name: Kids Room Rocket
  rainbow_speed: "1"
  candle_color: "255, 100, 0"
  candle_smoothing: "0.1"
  candle_dip_chance: "0.02"
  click_gap: 300ms
```

### Overriding components

Need more than substitutions? ESPHome merges your YAML on top of the package, and
[`!extend` / `!remove`](https://esphome.io/components/packages/#extend) let you change any
component that has an `id`:

```yaml
light:
  - id: !extend lamp_light
    name: Rocket                # rename the Home Assistant entity
    restore_mode: ALWAYS_OFF    # always start off after a power cut
```

### Using a different board

The firmware is split into small packages, so you can swap the board and keep the lamp logic:

| File | Contents |
| ---- | -------- |
| [`rocket-lamp.yaml`](rocket-lamp.yaml) | Complete firmware: naming, API, OTA, Wi-Fi + the two packages below |
| [`packages/board-esp32-c3-supermini.yaml`](packages/board-esp32-c3-supermini.yaml) | ESP32-C3 Super Mini: chip, framework, Wi-Fi tuning, default pins |
| [`packages/lamp.yaml`](packages/lamp.yaml) | The lamp itself: light, effects, button logic – board independent |

For example, if you'd rather fit a different ESP32 board into the rocket, pull only the lamp logic and
bring your own board setup:

```yaml
substitutions:
  led_pin: GPIO16
  button_pin: GPIO17

packages:
  rocket_lamp:
    url: https://github.com/nfjodor/ikea-rocket-lamp
    ref: main
    files: [packages/lamp.yaml]

esphome:
  name: rocket-lamp
  friendly_name: Rocket Lamp

esp32:
  board: esp32dev
  framework:
    type: esp-idf

logger:
api:
  encryption:
    key: !secret api_encryption_key
ota:
  - platform: esphome
    encryption:
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

> [!WARNING]
> The light uses the `esp32_rmt_led_strip` platform, so the lamp package needs an **ESP32-family**
> chip (ESP32, -C3, -S2, -S3, -C6 …). Bigger boards also draw more power – keep the
> [power budget](#power-budget) in mind.

### Updates & version pinning

- `@main` always follows the latest version. ESPHome caches remote packages for a day – run
  `esphome clean rocket-lamp.yaml` or set `refresh: 0s` (long form below) to pull changes immediately.
- Pin a release tag for reproducible builds:

```yaml
packages:
  rocket_lamp:
    url: https://github.com/nfjodor/ikea-rocket-lamp
    ref: v1.0.0       # tag, branch or commit
    files: [rocket-lamp.yaml]
    refresh: 1d
```

## 🗂️ Repository layout

```text
rocket-lamp/
├── rocket-lamp.yaml                     # 📦 main package – point your config here
├── packages/
│   ├── board-esp32-c3-supermini.yaml    # 🧩 board: chip, Wi-Fi tuning, pins
│   └── lamp.yaml                        # 🧩 lamp: light, effects, button logic
├── example.yaml                         # 📝 device config that pulls from GitHub
├── dev.yaml                             # 🛠️ device config that builds from this checkout
├── secrets.example.yaml                 # 🔑 template for secrets.yaml
└── docs/images/                         # 🖼️ banner, circuit, build photos and demo video
```

## 🛠️ Development

Work on the firmware locally without pushing to GitHub first:

```bash
git clone https://github.com/nfjodor/ikea-rocket-lamp.git
cd rocket-lamp
cp secrets.example.yaml secrets.yaml     # fill in your values
esphome run dev.yaml                     # builds from the local files
```

[`dev.yaml`](dev.yaml) includes [`rocket-lamp.yaml`](rocket-lamp.yaml) with `!include` instead of
the GitHub URL, so every edit is picked up on the next build.

## 🩺 Troubleshooting

| Symptom | Fix |
| ------- | --- |
| Lamp flickers, goes dark or reboots at high brightness (especially white) | The 3 W IKEA adapter is overloaded. If you raised `max_brightness_pct`, lower it again – see [power budget](#power-budget). |
| Lamp isn't as bright as expected | The LED output is capped at 50 % to protect the IKEA adapter. Raise `max_brightness_pct` only after measuring the current, or with a stronger 5 V supply. |
| Nothing lights up after the mod | Check the buck output is really 5.0 V, the LED data wire goes to the ring's `DIN` (not `DOUT`), and all grounds are connected. |
| Buck converter gets hot | Check the output really is 5.0 V and nothing is shorted. If you used a different module, make sure its input is rated for 24 V. The LEDs may also be drawing too much (see above). |
| Wi-Fi drops or Home Assistant shows the lamp as unavailable | The Super Mini antenna is weak, and inside the rocket it sits close to LEDs and wiring. Route the antenna end away from them. Raise `wifi: output_power` only if the power budget allows. |
| `the device did not offer encryption` during OTA | The lamp runs firmware without OTA encryption. Flash once over USB, or see [enabling encryption on an existing device](https://esphome.io/components/ota/esphome/#enabling-encryption-on-an-existing-device). |
| `Secret ... not defined` | Add the missing key to the `secrets.yaml` next to your device config. |
| Colours are swapped (red shows green) | Set `led_rgb_order` to match your LEDs, e.g. `RGB`. |
| Only some LEDs light up | Set `num_leds` to the actual number of LEDs on your ring. |
| Nightlight is completely dark | Some LEDs can't show 1 %, which is why `gamma_correct` is `1.0` and the nightlight brightness is compensated for `max_brightness_pct`. Check the LED supply and ground connections. |
| Package changes don't show up | Remote packages are cached – run `esphome clean <config>.yaml` or use `refresh: 0s`. |

---

<div align="center">

Made with ❤️, ☕ and a lot of blinking LEDs · Powered by [ESPHome](https://esphome.io) &amp; [Home Assistant](https://www.home-assistant.io)

<sub>IKEA and AFTONSPARV are trademarks of Inter IKEA Systems B.V. This project is not affiliated with IKEA.</sub>

</div>
