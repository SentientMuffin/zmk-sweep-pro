# Sweep Pro — Quickstart & Keymap Reference

A visual reference for the **Sweep Pro** ZMK firmware: every layer diagrammed,
plus where the e-ink display, trackpad, and rotary encoders live and how to tune them.

All behavior is in **`config/sweep.keymap`**. Hardware wiring is in
**`boards/shields/sweep/`**. Compile-time toggles are in **`config/*.conf`**.

---

## The keyboard at a glance

| | |
|---|---|
| **Board** | nice!nano (nRF52840), split BLE (left = central, right = peripheral) |
| **Keys** | 36 total — 3 rows × 10 (split 5 \| 5) + 1 thumb row × 6 (split 3 \| 3) |
| **Encoders** | 2 × ALPS EC11 (one per half) |
| **Trackpad** | Cirque Pinnacle II (right half, I²C) |
| **Display** | SSD1680 e-ink, 152×152 (left half, SPI) |
| **Layers** | 6 — `DEF` `WIN` `NUM` `SYM` `FUN` `MSE` |

### Build variants (`build.yaml`)

| Artifact | Shields | What it includes |
|---|---|---|
| `sweep_left` | `sweep_left` | left half, bare |
| `sweep_left_display` | `sweep_left` + `sweep_left_display_hw` + `sweep_display` | left half **+ e-ink** |
| `sweep_right` | `sweep_right` | right half, bare |
| `sweep_right_trackpad` | `sweep_right` + `sweep_right_trackpad` | right half **+ trackpad** |
| `settings_reset` | `settings_reset` | BLE unpair helper |

---

## How to read these diagrams

Each layer is drawn as two 5-key halves with a gap (where the split sits),
plus a centered 3-key thumb cluster per side.

### Cell glyphs

| Glyph | Meaning |
|---|---|
| `·` | transparent — falls through to the active base layer |
| `Q`, `,`, `⏎` | plain keypress (`&kp`) |
| `S⌥` `D⌃` `F⌘` | **home-row mod** — tap the letter, hold for the modifier (`⌥` Alt `⌃` Ctrl `⌘` Cmd) |
| `Z‖` | tap `Z`, hold = **drag-scroll** (the trackpad scrolls) |
| `X●` | tap `X`, hold = **left mouse click** (so you can drag-select) |
| `⇥SYM` `⇥NUM` | **layer-tap** — tap = `Tab`, hold = that layer |
| `⊕MSE` `⊕WIN` | **toggle** that layer on/off |
| `Std` | ZMK Studio unlock (live keymap editing over USB) |
| `Rpt` | custom report behavior (`&default_report`) |

### Media / system glyphs

`🔇` mute · `Vol+`/`Vol−` volume · `Bri+`/`Bri−` brightness ·
`⏮` `⏯` `⏭` prev / play-pause / next · `⏻` soft-off · `Out⊕` toggle USB↔BT output ·
`BT0`–`BT4` select bluetooth slot · `BTclr` clear pairings

### Navigation glyphs

`← ↑ ↓ →` arrows · `Hm`/`End` Home/End · `⇞`/`⇟` Page Up/Down · `⎋` Esc · `⇥` Tab · `⏎` Enter

### Mouse-layer glyphs (MSE only)

| Glyph | Meaning |
|---|---|
| `MB4` `MB5` | mouse back / forward buttons |
| `Mclk` `Lclk` `Rclk` | middle / left / right click |
| `Sc← ↓ ↑ →` | trackpad **scroll** in that direction |
| `Mv← ↓ ↑ →` | trackpad **move** (accelerated cursor) in that direction |
| `Pf−` `Pf+` | pointer speed **fine** down / up |
| `Sf−` `Sf+` | scroll speed **fine** down / up |
| `P=0` `S=0` | reset pointer / scroll speed to default |
| `Mod⊕` | toggle Cirque trackpad mode (absolute ⇄ relative) |

---

## Layers

### Layer DEF — default (Mac)
QWERTY with Mac-order home-row mods. This is the power-on layer.

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│  Q   │  W   │  E   │  R   │  T   │   │  Y   │  U   │  I   │  O   │  P   │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  A   │  S⌥  │  D⌃  │  F⌘  │  G   │   │  H   │  J⌘  │  K⌃  │  L⌥  │ Bsp  │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  Z‖  │  X●  │  C   │  V   │  B   │   │  N   │  M   │  ,   │  .   │  ⏎   │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
              ┌──────┬──────┬──────┐   ┌──────┬──────┬──────┐
              │  🔇  │ ⇥SYM │  ⇧   │   │  ␣   │ ⇥NUM │ Std  │
              └──────┴──────┴──────┘   └──────┴──────┴──────┘
```
**Encoders** — left: Vol− / Vol+ · right: Bri− / Bri+

### Layer WIN — Windows
Identical to DEF, but home-row mods use Windows order (Ctrl ⇄ Win swapped):
`D⌘` `F⌃` on the left, `J⌃` `K⌘` on the right. Toggle to it from the FUN layer (`⊕WIN`).

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│  Q   │  W   │  E   │  R   │  T   │   │  Y   │  U   │  I   │  O   │  P   │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  A   │  S⌥  │  D⌘  │  F⌃  │  G   │   │  H   │  J⌃  │  K⌘  │  L⌥  │ Bsp  │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  Z‖  │  X●  │  C   │  V   │  B   │   │  N   │  M   │  ,   │  .   │  ⏎   │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
              ┌──────┬──────┬──────┐   ┌──────┬──────┬──────┐
              │  🔇  │ ⇥SYM │  ⇧   │   │  ␣   │ ⇥NUM │ Std  │
              └──────┴──────┴──────┘   └──────┴──────┴──────┘
```
**Encoders** — left: Vol− / Vol+ · right: Bri− / Bri+

### Layer NUM — numbers & navigation
Hold the **right thumb `⇥NUM`** to enter. Left side = editing keys, right side = a full nav cluster.

```
┌─────┬─────┬─────┬─────┬─────┐   ┌─────┬─────┬─────┬─────┬─────┐
│  1  │  2  │  3  │  4  │  5  │   │  6  │  7  │  8  │  9  │  0  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│ Del │ Cps │ PSc │ Ins │  ·  │   │  ←  │  ↓  │  ↑  │  →  │  ·  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  ·  │  ·  │  ·  │  ·  │  ·  │   │ Hm  │  ⇟  │  ⇞  │ End │  ·  │
└─────┴─────┴─────┴─────┴─────┘   └─────┴─────┴─────┴─────┴─────┘
            ┌─────┬─────┬─────┐   ┌─────┬─────┬─────┐
            │  🔇 │  ·  │  ⎋  │   │  ·  │  ·  │ Std │
            └─────┴─────┴─────┘   └─────┴─────┴─────┘
```
**Encoders** — left: Vol− / Vol+ · right: Bri− / Bri+

### Layer SYM — symbols & punctuation
Hold the **left thumb `⇥SYM`** to enter. The `Rpt` key invokes the custom report behavior.

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│  !   │  @   │  #   │  $   │  %   │   │  ^   │  &   │  *   │  `   │  ~   │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  “   │  [   │  {   │  (   │ Rpt  │   │  /   │  −   │  =   │  :   │  ;   │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  ‘   │  ]   │  }   │  )   │  ·   │   │  \   │  _   │  +   │  |   │  ?   │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
              ┌──────┬──────┬──────┐   ┌──────┬──────┬──────┐
              │  🔇  │  ·   │  ·   │   │ ⊕MSE │  ·   │ Std  │
              └──────┴──────┴──────┘   └──────┴──────┴──────┘
```
**Encoders** — left: Vol− / Vol+ · right: Bri− / Bri+

### Layer FUN — function keys, bluetooth & system
**Auto-activated** when both `⇥SYM` and `⇥NUM` are held (tri-layer conditional).
Bluetooth slots, output toggle, media transport, and soft-off live here.

```
┌───────┬───────┬───────┬───────┬───────┐   ┌───────┬───────┬───────┬───────┬───────┐
│  F1   │  F2   │  F3   │  F4   │  BT0  │   │  BT2  │ Out⊕  │ ⊕WIN  │   🔇  │   ⏮   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  F5   │  F6   │  F7   │  F8   │  BT1  │   │  BT3  │ Bri+  │  Std  │ Vol+  │   ⏯   │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  F9   │  F10  │  F11  │  F12  │ BTclr │   │  BT4  │ Bri−  │   ⏻   │ Vol−  │   ⏭   │
└───────┴───────┴──────┴──────┴───────┘   └───────┴───────┴───────┴───────┴───────┘
                ┌───────┬───────┬───────┐   ┌───────┬───────┬───────┐
                │   🔇  │   ·   │   ·   │   │   ·   │   ·   │  Std  │
                └───────┴───────┴───────┘   └───────┴───────┴───────┘
```
**Encoders** — left: Vol− / Vol+ · right: Bri− / Bri+

### Layer MSE — mouse / trackpad
**Toggled** with `⊕MSE` (on the SYM thumb) or the `⊕MSE` keys at the layer's outer edges.
Right half drives the trackpad (scroll + move); left half tunes pointer/scroll speed.

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│ ⊕MSE │ MB4  │ Mclk │ MB5  │  ·   │   │ Sc←  │ Sc↓  │ Sc↑  │ Sc→  │ ⊕MSE │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  ·   │  ·   │  ·   │  ·   │  ·   │   │ Mv←  │ Mv↓  │ Mv↑  │ Mv→  │  ·   │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│ Pf−  │ Pf+  │ Sf−  │ Sf+  │ Mod⊕ │   │  ·   │ MB4  │ Mclk │ MB5  │  ·   │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
              ┌──────┬──────┬──────┐   ┌──────┬──────┬──────┐
              │ P=0  │  ␣   │  ⇧   │   │ Lclk │ Rclk │ S=0  │
              └──────┴──────┴──────┘   └──────┴──────┴──────┘
```
**Encoders** — left: pointer speed − / + · right: scroll speed − / +

---

## Combos (mouse chords)

Two-key chords active on **DEF, WIN, and MSE** layers (25 ms timeout, only after
125 ms idle). These give you a full mouse without entering the MSE layer.

| Press together | Result | |
|---|---|---|
| `E` + `R` | Right click | `Rclk` |
| `R` + `T` | Forward button | `MB5` |
| `D` + `F` | Left click | `Lclk` |
| `F` + `G` | Back button | `MB4` |
| `C` + `V` | Middle click | `Mclk` |

---

## Encoders & trackpad at a glance

### Rotary encoders (2 × EC11)
- **Wiring & enable** — defined in `boards/shields/sweep/sweep.dtsi`
  (`left_encoder`, `right_encoder`, both on `gpio0 11` / `gpio1 0`, pull-up),
  enabled per half in `sweep_left.overlay` / `sweep_right.overlay`.
- **Resolution** — `triggers-per-rotation`: left = **15**, right = **45** (in `sweep.keymap`, `&sensors` block).
- **Action per layer** — each layer's `sensor-bindings` line. Most layers =
  volume (left) + brightness (right); the MSE layer remaps them to pointer/scroll speed.

### Trackpad (Cirque Pinnacle II)
- **Wiring & feel** — `boards/shields/sweep/sweep_right_trackpad.overlay`:
  I²C `0x2a`, absolute mode, sensitivity `2x`, plus tap/drag, edge-motion, and
  right-edge scroll tuning.
- **Pointer acceleration** — two input processors in `sweep.keymap`:
  `pointer_processor` (adaptive accel curve, 10–400%) and `drag_scroll_processor`.
- **Driver config** — `config/sweep_right_trackpad.conf` (I²C, 4 KB input stack);
  the **left** half (`config/sweep_left.conf`) actually processes pointer events
  over the split and sends HID reports.

### E-ink display (SSD1680, 152×152)
- **Wiring & timings** — `boards/shields/sweep/sweep_left_display_hw.overlay`
  (SPI0, full + partial refresh LUTs).
- **Driver / fonts** — `config/sweep_left_display_hw.conf` (SSD16XX, LVGL 1-bit,
  Montserrat 12/14/16, custom trackpad-status widget).
- **Graphics** — come from the external `zmk-vfx-sweep-pro-display` module.

---

## Where to change what

| Want to change… | Edit this |
|---|---|
| Which key does what | `config/sweep.keymap` → that layer's `bindings = < … >` |
| Layer order / which is default | top `#define` block (`MAC` `WIN` `RIG` `LEF` `TRI` `MOUSE`) |
| Encoder direction / pins | `boards/shields/sweep/sweep.dtsi` (`a-gpios` / `b-gpios`) |
| Encoder resolution | `&sensors { … triggers-per-rotation }` in `sweep.keymap` |
| What each encoder does | each layer's `sensor-bindings = < … >` |
| Trackpad sensitivity / tap / scroll zone | `boards/shields/sweep/sweep_right_trackpad.overlay` |
| Pointer speed / acceleration curve | `pointer_processor` / `drag_scroll_processor` in `sweep.keymap` |
| E-ink wiring | `boards/shields/sweep/sweep_left_display_hw.overlay` |
| E-ink fonts / refresh | `config/sweep_left_display_hw.conf` |
| BLE power / sleep / battery / USB | `config/sweep.conf` |
| Which features a build includes | `build.yaml` (shield combos) |
| Custom external modules / versions | `config/west.yml` |

> The diagrams in this file are generated, not hand-aligned. If you change the
> keymap, regenerate them — see the formatting tooling alongside this repo.
