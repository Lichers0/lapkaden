# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a ZMK firmware project for the Wellum36 split mechanical keyboard. The keyboard uses the nice!nano v2 controller and has a 36-key layout (3x5+3 keys per half).

## Architecture

### Key Components

1. **Shield Definition** (`boards/shields/wellum36/`)
   - Device tree files define the physical keyboard layout and GPIO pin connections
   - Currently configured for direct pin connection (not matrix) - each key has its own GPIO pin
   - Split keyboard with separate left/right overlays

2. **Keymap** (`config/wellum36.keymap`)
   - 8 layers defined: DEF, GAM, GFN, SYM, NAV, NUM, ALT, CMD
   - Custom behaviors for sticky modifiers and tab/window switching
   - Conditional layer activation (NAV + SYM = NUM)

3. **Configuration** (`config/wellum36.conf`)
   - Bluetooth enabled, RGB/display disabled
   - Sleep mode configured (15 min idle timeout)
   - Power management enabled

## Build Commands

```bash
# Build firmware for both halves
west build -p -d build/left -b nice_nano_v2 -- -DSHIELD=wellum36_left
west build -p -d build/right -b nice_nano_v2 -- -DSHIELD=wellum36_right

# Flash firmware (after build)
west flash -d build/left
west flash -d build/right
```

## Development Notes

### Pin Connections
- Left half uses pins: 0-10, 14-21 on pro_micro
- Right half uses pins: 0-10, 14-21 on pro_micro  
- All pins configured as GPIO_ACTIVE_LOW with internal pull-ups

### Switching Between Direct Pin and Matrix
The project has commented-out matrix configuration. To switch:
1. Comment out the direct pin configuration in `wellum36.dtsi` and overlays
2. Uncomment the matrix configuration sections
3. Adjust the keymap transform accordingly

### West Manifest
Project uses ZMK's main branch through West workspace management (`config/west.yml`)