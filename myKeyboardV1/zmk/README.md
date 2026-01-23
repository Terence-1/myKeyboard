# ZMK Configuration for Dofe Keyboard

This directory contains the ZMK firmware configuration for the Dofe split keyboard using nice!nano v2 controllers.

## Features

- **34-key split layout** (5x3 + 2 thumb keys per side)
- **Home row mods** for ergonomic modifier access
- **5 layers**: Default (QWERTY), Numbers, Symbols, Function, Navigation
- **Bluetooth layer** for pairing management
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
3. Build the firmware:
   ```bash
   west build -d build/left -b nice_nano_v2 -- -DSHIELD=dofe_left -DZMK_CONFIG="/path/to/myKeyboard/myKeyboardV1/zmk/config"
   west build -d build/right -b nice_nano_v2 -- -DSHIELD=dofe_right -DZMK_CONFIG="/path/to/myKeyboard/myKeyboardV1/zmk/config"
   ```
4. Flash the `.uf2` files to your nice!nanos

## Flashing

1. Connect the nice!nano via USB
2. Double-tap the reset button to enter bootloader mode
3. Drag and drop the appropriate `.uf2` file to the mounted drive
4. The controller will reboot automatically

Flash the left side first, then the right side.

## Pairing

1. Flash both halves
2. Power on the left half (it acts as the central/master)
3. Power on the right half
4. The halves should pair automatically
5. Put the keyboard in pairing mode by accessing the Bluetooth layer (hold both symbol layer keys)
6. Select a profile (0-4) to pair with your device

## Customizing

Edit [config/dofe.keymap](config/dofe.keymap) to customize:
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

- **Halves won't pair**: Try clearing Bluetooth bonds on both halves
- **Keys not working**: Check pin assignments in overlay files match your wiring
- **High power consumption**: Disable USB logging in `dofe.conf`

## Resources

- [ZMK Documentation](https://zmk.dev/)
- [ZMK Discord](https://zmk.dev/community/discord/invite)
- [Keycodes Reference](https://zmk.dev/docs/codes)
