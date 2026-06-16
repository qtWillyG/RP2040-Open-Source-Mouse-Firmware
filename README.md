# RP2040 Custom USB HID Mouse

A USB HID mouse for the Raspberry Pi Pico (RP2040) built with the
[Adafruit TinyUSB](https://github.com/adafruit/Adafruit_TinyUSB_Arduino) library.

It demonstrates how to override the default USB descriptors (Vendor ID,
Product ID, Manufacturer and Product strings) and switch between multiple
device-identity profiles on boot, plus basic HID mouse movement and clicks.

## Features

- 🪪 **Custom USB descriptors** — set your own VID/PID and Manufacturer/Product strings
- 🗂️ **Profile table** — define multiple device identities in a struct array and pick one on boot
- 🔀 **Boot-time selection** — choose a profile by constant, or from a DIP switch / jumper on GPIO
- 🖱️ **HID mouse basics** — cursor movement and left-click to verify enumeration works

## Hardware

- Raspberry Pi Pico (RP2040)
- USB cable
- *(optional)* a DIP switch / jumpers on GP14 / GP15 for hardware profile selection

## Build & Flash

1. Install the **Arduino IDE** and the
   [Earle Philhower arduino-pico](https://github.com/earlephilhower/arduino-pico) core.
2. Install the **Adafruit TinyUSB Library** via the Library Manager.
3. In **Tools**:
   - **Board:** `Raspberry Pi Pico`
   - **USB Stack:** `Adafruit TinyUSB`
4. **Sketch → Export Compiled Binary** to produce a `.uf2`.
5. Hold **BOOTSEL**, plug in the Pico, and drag the `.uf2` onto the `RPI-RP2` drive.

## Configuration

Edit the `profiles[]` table to define your own device identities:

```cpp
const MouseProfile profiles[] = {
  { "Custom A", 0xCAFE, 0x4001, "MyLab", "Custom HID Mouse A" },
  // ...
};
Set activeProfile to the index you want, or uncomment
selectProfileFromHardware() to read it from GPIO.

Use your own IDs. The VID/PIDs in this repo are placeholder example values.
For real / published projects use a properly allocated Vendor ID
(e.g. via pid.codes for hobby work).

Usage
Once flashed, the cursor automatically traces a small square every 2 seconds
as a proof-of-life. The core HID helpers are:

mouseMove(dx, dy);   // move the cursor by a relative amount
mouseLeftClick();    // press and release the left button
Notes
Once flashed, the board enumerates as an HID mouse, so re-flashing usually
needs the BOOTSEL button.
Descriptor overrides must be applied before USB enumeration — the sketch
handles re-enumeration automatically.
License
MIT — see LICENSE.

Two reminders:
- Save it as **`README.md`** (exact name) — that's the only filename GitHub renders as the repo homepage; `read.md` won't show up.
- Swap or remove the **License** section if you're not using MIT.
Want a combined README that covers both the RP2040 and RP2350 boards in one repo instead of two separate ones?
