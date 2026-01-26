# ZMK Configuration for Terence0 Keyboard

This directory contains the ZMK firmware configuration for the Terence0 split keyboard using a 3-device setup with nice!nano v2 controllers.

## Architecture

This is a **dongle-based wireless split keyboard** with three nice!nano controllers:
- **Left peripheral**: Wireless peripheral for left half
- **Right peripheral**: Wireless peripheral for right half  
- **Central dongle**: Receives from both peripherals and connects to your computer via USB/Bluetooth

## Features

- **34-key split layout** (5x3 + 2 thumb keys per side)
- **Fully wireless** - both halves communicate to central dongle via BLE
- **Home row mods** for ergonomic modifier access
- **6 layers**: Default (QWERTY), Numbers, Symbols, Function, Navigation, Bluetooth
- **Combos**: Caps Lock (F+J), Layer Lock (thumb keys)
- **Low power consumption** with sleep mode after 15 minutes

## Building Firmware

### Option 1: GitHub Actions (Recommended)

1. Fork the [ZMK firmware repository](https://github.com/zmkfirmware/zmk)
2. Copy the contents of the `config/` folder to `config/` in your fork
3. Copy `build.yaml` to the root of your fork
4. Push to GitHub - firmware will build automatically
5. Download the `.uf2` files from the Actions tab

### Option 2: Local Build

1. Set up the ZMK development environment: https://zmk.dev/docs/development/setup
2. Navigate to your ZMK directory
3. Build the firmware for all three devices:
   ```bash
   # Left peripheral
   west build -d build/left -b nice_nano_v2 -- -DSHIELD=terence0_left -DZMK_CONFIG="/path/to/myKeyboard/myKeyboardV1/zmk/config"
   
   # Right peripheral
   west build -d build/right -b nice_nano_v2 -- -DSHIELD=terence0_right -DZMK_CONFIG="/path/to/myKeyboard/myKeyboardV1/zmk/config"
   
   # Central dongle
   west build -d build/dongle -b nice_nano_v2 -- -DSHIELD=terence0_dongle -DZMK_CONFIG="/path/to/myKeyboard/myKeyboardV1/zmk/config"
   ```
4. Flash the `.uf2` files to the appropriate nice!nanos

## Flashing

For each nice!nano:
1. Connect via USB
2. Double-tap the reset button to enter bootloader mode
3. Drag and drop the appropriate `.uf2` file:
   - `terence0_left-nice_nano_v2-zmk.uf2` → Left half controller
   - `terence0_right-nice_nano_v2-zmk.uf2` → Right half controller
   - `terence0_dongle-nice_nano_v2-zmk.uf2` → Central dongle
4. The controller will reboot automatically

Flash all three devices before proceeding to pairing.

## Hardware Setup

- **Left half**: Install nice!nano with battery
- **Right half**: Install nice!nano with battery
- **Central dongle**: Install nice!nano (can be in a separate case or attached to one half)
- Both peripherals are completely wireless and battery-powered
- The dongle connects to your computer via USB or Bluetooth

## Pairing

1. Flash all three nice!nanos with their respective firmware
2. Power on the central dongle first (via USB or battery)
3. Power on the left peripheral - it should automatically pair with the dongle
4. Power on the right peripheral - it should automatically pair with the dongle
5. Both halves should now be functional
6. Connect the dongle to your computer:
   - Via USB: Plug it in
   - Via Bluetooth: Use the Bluetooth layer to select a profile and pair with your device

## Customizing

Edit [config/terence0.keymap](config/terence0.keymap) to customize:
- Key positions
- Layer behaviors
- Combos
- Home row mod timings

After making changes, rebuild and reflash the firmware.

## Layer Overview

- **Layer 0 (Default)**: QWERTY with home row mods
- **Layer 1 (Numbers)**: Number row, basic symbols
- **Layer 2 (Symbols)**: Additional symbols and punctuation
- **Layer 3 (Function)**: F-keys and navigation (Home, End, PgUp, PgDn)
- **Layer 4 (Navigation)**: Arrow keys
- **Layer 5 (Bluetooth)**: BT profile selection and clearing

## Troubleshooting

- **Peripherals won't pair with dongle**: 
  - Ensure the dongle is powered on first
  - Try resetting all three devices
  - Clear Bluetooth bonds and re-pair
- **One half not responding**: 
  - Check that the peripheral is powered on
  - Verify the peripheral paired successfully (check dongle's LED behavior)
  - Try re-flashing that peripheral
- **Keys not working**: Verify pin assignments in overlay files match your wiring
- **High power consumption on peripherals**: Disable USB logging in `terence0.conf`
- **Can't pair dongle with computer**: Use BT_CLR key in layer 5 to clear bonds
- **Keys not working**: Check pin assignments in overlay files match your wiring

## Resources

- [ZMK Documentation](https://zmk.dev/)
- [ZMK Discord](https://zmk.dev/community/discord/invite)
- [Keycodes Reference](https://zmk.dev/docs/codes)
