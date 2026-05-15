# ZMK Sandbender

ZMK firmware configuration for the sandbender wireless split keyboard.

## Structure

```
zmk-sandbender/
├── build.yaml               — GitHub Actions build matrix (5 targets)
├── config/
│   ├── west.yml             — Zephyr/West dependencies
│   └── shared.conf          — Global settings (BT power, sleep, encoders)
└── boards/shields/
    ├── sandbender_b_v0/     — Previous sandbender version (B v0)
    └── urchin/              — Reference design (Duccio Breschi), fully implemented keymap
```

## Shields

### sandbender_b_v0
Previous sandbender version. Structure is good but keymap is incomplete — only the DEFAULT layer is active, layers 1-5 are commented out. Use urchin keymap as reference when porting.

- MCU: nice!nano v2, Col2row diode direction
- Matrix: 4 rows × 12 columns per half (not matching c_sw_34_v0 — needs new shield)
- Left rows: pins 6,7,8,9 | cols: pins 19,18,15,14,16,10
- Right rows: pins 15,14,16,10 | cols: pins 4,5,6,7,8,9
- Encoder: pins 20/21 (left), 2/3 (right) — present but disabled
- Display: nice!view via SPI

### urchin
Reference implementation. All 6 layers fully implemented. Use as the keymap template for new shields.

Layers: DEFAULT, L_MODS, R_MODS, R_NUM, L_SPEC, BT_CTL

## Global Config (shared.conf)

- BT TX power: +8 dBm
- Deep sleep: enabled, 30-minute idle timeout
- EC11 encoder support: enabled globally

## Planned: c_sw_34_v0 Shield

New shield needed for the current sandbender design (`ergogen/boards/sandbender/c_sw_34_v0.yaml`).

Target architecture: **dongle pattern** (3 shields)
- `sandbender_left` — peripheral, BLE only, no USB
- `sandbender_right` — peripheral, BLE only, no USB
- `sandbender_dongle` — central, USB HID, no key matrix

See `../research/dongle_pattern.md` for full implementation notes.

### c_sw_34_v0 MCU Pin Assignments (nice!nano)
- Cols 0-5: P4, P5, P6, P7, P8, P9
- Rows 0-3: P15, P14, P16, P10
- Scroll wheel encoder (A/B/C): TBD — pending GPIO audit
- Cirque trackpad SPI (SCK/MOSI/MISO/CS/DR): TBD — pending GPIO audit

## ZMK Source Reference

A full ZMK upstream clone lives at `../zmk/` for reference only. How to use it (local builds, module development, etc.) is TBD — decision deferred until board designs are further along.
