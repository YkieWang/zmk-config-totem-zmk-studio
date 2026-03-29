# AGENTS.md

This file provides guidance to codeflicker when working with code in this repository.

## WHY: Purpose and Goals

ZMK firmware configuration for the Totem split ergonomic keyboard (38 keys, Seeeduino XIAO BLE). Includes ZMK Studio support for wireless keymap editing via USB/UART RPC on the left half.

## WHAT: Technical Stack

- **Firmware**: ZMK v0.3 (Zephyr-based)
- **MCU**: Seeeduino XIAO BLE (nRF52840)
- **Build**: GitHub Actions via `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3`
- **Key files**: `config/totem.keymap` (custom layout), `config/west.yml` (dependencies), `build.yaml` (build matrix)
- **Hardware defs**: `boards/shields/totem/` — `.dtsi`, `.overlay`, `Kconfig.*`

## HOW: Core Development Workflow

```bash
# Firmware builds automatically on push via GitHub Actions
# To build locally (requires ZMK dev environment):
west build -b seeeduino_xiao_ble -- -DSHIELD=totem_left -DCONFIG_ZMK_STUDIO=y

# Flash via UF2 drag-and-drop after build
```

## Progressive Disclosure

For detailed information, consult these documents as needed:

- `docs/agent/architecture.md` - Shield structure, keymap layers, build matrix details

**When working on a task, first determine which documentation is relevant, then read only those files.**
