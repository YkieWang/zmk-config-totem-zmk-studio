# Architecture

## Project Structure

```
zmk-config-totem-zmk-studio/
├── build.yaml                    # GitHub Actions build matrix (3 variants)
├── config/
│   ├── west.yml                  # ZMK v0.3 dependency manifest
│   ├── totem.conf                # Firmware feature flags
│   └── totem.keymap              # Active custom keymap (edit this)
├── boards/shields/totem/
│   ├── totem.dtsi                # Hardware matrix (10 cols × 4 rows, 38 keys)
│   ├── totem_left.overlay        # Left half GPIO: D4,D5,D10,D9,D8
│   ├── totem_right.overlay       # Right half GPIO: D8,D9,D10,D5,D4 (reversed, col-offset=5)
│   ├── totem-layouts.dtsi        # Physical key coordinates for ZMK Studio UI
│   ├── totem.zmk.yml             # Shield metadata
│   ├── Kconfig.defconfig         # Left=CENTRAL, both halves enable SPLIT
│   └── Kconfig.shield            # Shield selection config
└── zephyr/module.yml             # Sets board_root to repo root
```

## Build Matrix (build.yaml)

| Shield | ZMK Studio | Notes |
|---|---|---|
| `totem_left` | Yes (`-DCONFIG_ZMK_STUDIO=y`) | Central half, USB/UART RPC snippet |
| `totem_right` | No | Peripheral half |
| `settings_reset` | No | Factory reset firmware |

## Keymap Layers (config/totem.keymap)

| Layer | Index | Contents |
|---|---|---|
| BASE | 0 | Colemak-inspired alpha, mod-taps, layer-taps |
| NAV | 1 | Arrows, symbols, brackets, Bluetooth controls |
| SYM | 2 | F1-F12, Bluetooth controls, `&gif` macro |
| ADJ | 3 | Numpad (right side) |

**Special behaviors:**
- `&mt` (mod-tap): tap = key, hold = modifier
- `&lt` (layer-tap): tap = key, hold = activate layer
- Combo: positions 0+1 → ESC
- Macro `&gif`: sends `Shift+2` then `gif` (types @gif)

## Split Architecture

- Left half = **CENTRAL** (USB/BLE host-facing, runs ZMK Studio)
- Right half = **PERIPHERAL** (BLE-only, connects to left)
- Both use Seeeduino XIAO BLE (nRF52840)

## Disabled Features (totem.conf)

- RGB underglow (commented out)
- OLED display (commented out)
- USB logging (`CONFIG_ZMK_USB_LOGGING=n`)
