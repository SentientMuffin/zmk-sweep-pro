# Sweep Pro — Quickstart & Keymap Reference

A visual reference for the **Sweep Pro** ZMK firmware (chocofy layout):
every layer diagrammed, plus where the e-ink display, trackpad, and rotary encoders live.

All behavior is in **`config/sweep.keymap`**. Hardware wiring is in
**`boards/shields/sweep/`**. Compile-time toggles are in **`config/*.conf`**.

> **Chocofy layout note:** the keymap here is ported from a Corne config.
> See **`CHOCOFY_CHANGES.md`** for what changed and what was lost versus the
> original Sweep keymap. Notably: **no home-row mods on Base** (plain keys).

---

## The keyboard at a glance

| | |
|---|---|
| **Board** | nice!nano (nRF52840), split BLE (left = central, right = peripheral) |
| **Keys** | 34 — 3 rows × 10 (split 5 \| 5) + **4 thumb keys** (2 per side) |
| **Encoders** | 2 × ALPS EC11 — **one outside each thumb cluster** (shown as `↻`) |
| **Trackpad** | Cirque Pinnacle II (right half, I²C) |
| **Display** | SSD1680 e-ink, 152×152 (left half, SPI) |
| **Layers** | 7 — `Base` `Symb` `Num` `Nav` `Func` `BT` `Mouse` |

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

Each layer is two 5-key halves with a gap (the split), plus a centered
**2+2 thumb cluster flanked by the two encoders** (`↻`).

Empty cells are unused (`&none`). The thumb diagram always shows both encoders
even when a layer leaves a thumb blank.

### Modifier glyphs

| Glyph | Meaning |
|---|---|
| `⇧` `⌃` `⌥` `⌘` | Shift / Ctrl / Alt / Cmd (plain `&kp`) |
| `s⌥` `s⌃` | **sticky** Alt / Ctrl (`&sk`) — tap, then the next keypress is modified |
| `⇧⌘` `⌃⌥⌘` | chorded mods (`LS(LGUI)` = Shift+Cmd, `LC(LA(LGUI))` = Ctrl+Alt+Cmd) |
| `⌘Z` `⌘X` … | Cmd+key combos (`LG(Z)` etc.) |

> **No home-row mods** — every alpha is a plain key. Mods only appear as
> dedicated keys or sticky keys, never on hold of a letter.

### Layer & action glyphs

| Glyph | Meaning |
|---|---|
| `Num` `Sym` `Func` | **momentary layer** (`&mo`) — hold the key to activate |
| `␣/N` | **layer-tap** (`&hltk`) — tap = Space (`␣`), hold = Nav |
| `⊙MSE` | **toggle** Mouse layer on/off (`&tog`) |
| `→BT` `→Def` | **switch-to** layer (`&to`) |
| `↻` | rotary encoder (outside each thumb cluster) |
| `BT0`–`BT4` | select bluetooth slot · `BTclr` clear pairings |

### Navigation / media glyphs

`← ↑ ↓ →` arrows · `⇥` Tab · `⏎` Enter · `Bsp` Backspace · `Cps` Caps ·
`🔇` mute · `Vol+`/`Vol−` volume · `Bri+`/`Bri−` brightness ·
`F1`–`F12` function keys

### Mouse-layer glyphs (Mouse only)

| Glyph | Meaning |
|---|---|
| `MB4` `MB5` | mouse back / forward buttons |
| `Mclk` `Lclk` `Rclk` | middle / left / right click |
| `Sc← ↓ ↑ →` | trackpad **scroll** in that direction |
| `Mv← ↓ ↑ →` | trackpad **move** (accelerated cursor) |
| `Pf−` `Pf+` | pointer speed **fine** down / up |
| `Sf−` `Sf+` | scroll speed **fine** down / up |
| `Mod⊙` | toggle Cirque trackpad mode (absolute ⇄ relative) |

---

## Layers

### Layer Base — default (QWERTY, no home-row mods)
The power-on layer. Plain alphas; sticky Alt (`s⌥`) and sticky Ctrl (`s⌃`) on the
outer top/bottom-right. Thumbs reach Num / Nav (left) and Shift / Sym (right).

```
┌─────┬─────┬─────┬─────┬─────┐   ┌─────┬─────┬─────┬─────┬─────┐
│  Q  │  W  │  E  │  R  │  T  │   │  Y  │  U  │  I  │ s⌥  │ Bsp │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  A  │  S  │  D  │  F  │  G  │   │  H  │  N  │  K  │  O  │  L  │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│  Z  │  X  │  C  │  V  │  B  │   │  J  │  M  │  P  │ s⌃  │ Cps │
└─────┴─────┴─────┴─────┴─────┘   └─────┴─────┴─────┴─────┴─────┘
         ┌─────┐   ┌─────┬─────┐   ┌─────┬─────┐   ┌─────┐
         │  ↻  │   │ Num │ ␣/N │   │  ⇧  │ Sym │   │  ↻  │
         └─────┘   └─────┴─────┘   └─────┴─────┘   └─────┘
```
**Encoders** — Vol−/+ · Bri−/+

### Layer Symb — symbols & punctuation  *(hold Sym thumb)*

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│  ~   │  @   │  +   │  ^   │      │   │      │  $   │  =   │  :   │      │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  /   │  !   │  ,   │  .   │  *   │   │  #   │  “   │  ‘   │  |   │  ;   │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  \   │  _   │  −   │  &   │      │   │      │  %   │  ?   │  `   │      │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
            ┌──────┐   ┌──────┬──────┐   ┌──────┬──────┐   ┌──────┐
            │  ↻   │   │ Func │  ␣   │   │ ⊙MSE │      │   │  ↻   │
            └──────┘   └──────┴──────┘   └──────┴──────┘   └──────┘
```
**Encoders** — Vol−/+ · Bri−/+

### Layer Num — numbers & brackets  *(hold Num thumb)*

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│  +   │  1   │  2   │  3   │  ,   │   │      │  <   │  >   │  =   │      │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  0   │  4   │  5   │  6   │  .   │   │  {   │  (   │  )   │  }   │  ⌥N  │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  ⇧⌘  │  7   │  8   │  9   │  −   │   │      │  [   │  ]   │      │      │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
            ┌──────┐   ┌──────┬──────┐   ┌──────┬──────┐   ┌──────┐
            │  ↻   │   │      │      │   │ Func │      │   │  ↻   │
            └──────┘   └──────┴──────┘   └──────┴──────┘   └──────┘
```
**Encoders** — Vol−/+ · Bri−/+

### Layer Nav — navigation & window management  *(hold `␣/N` thumb)*
Left side = app/window shortcuts (undo/cut/copy/paste, app switcher). Right side = arrow cluster.

```
┌─────┬─────┬─────┬─────┬─────┐   ┌─────┬─────┬─────┬─────┬─────┐
│  ~  │  ⇥  │ Bsp │  ⏎  │  ⌃  │   │     │     │ ⌥␣  │     │     │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│ ⌥␣  │ ⌘␣  │  ⇧  │  ⌘  │ ⌃⌥⌘ │   │  ←  │  ↓  │  ↑  │  →  │     │
├─────┼─────┼─────┼─────┼─────┤   ├─────┼─────┼─────┼─────┼─────┤
│ ⌘Z  │ ⌘X  │ ⌘C  │ ⌘V  │  ⌥  │   │     │     │     │     │     │
└─────┴─────┴─────┴─────┴─────┘   └─────┴─────┴─────┴─────┴─────┘
         ┌─────┐   ┌─────┬─────┐   ┌─────┬─────┐   ┌─────┐
         │  ↻  │   │     │     │   │  ⇥  │     │   │  ↻  │
         └─────┘   └─────┴─────┘   └─────┴─────┘   └─────┘
```
**Encoders** — Vol−/+ · Bri−/+

### Layer Func — function keys & media  *(hold Func from Symb/Num)*
F-keys on the left, media transport on the bottom-right. Reach Bluetooth via `→BT`.

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│  F1  │  F2  │  F3  │  F4  │      │   │      │      │      │      │      │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  F5  │  F6  │  F7  │  F8  │      │   │      │      │      │      │      │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│  F9  │ F10  │ F11  │ F12  │      │   │  🔇   │ Vol− │ Vol+ │      │      │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
            ┌──────┐   ┌──────┬──────┐   ┌──────┬──────┐   ┌──────┐
            │  ↻   │   │ →BT  │      │   │      │      │   │  ↻   │
            └──────┘   └──────┴──────┘   └──────┴──────┘   └──────┘
```
**Encoders** — Vol−/+ · Bri−/+

### Layer BT — bluetooth slots & unpair  *(→BT from Func)*
Select a bluetooth slot (0–4), clear pairings (`BTclr`), or return to Base (`→Def`).

```
┌───────┬───────┬───────┬───────┬───────┐   ┌───────┬───────┬───────┬───────┬───────┐
│       │       │       │       │       │   │       │       │       │       │       │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│  BT0  │  BT1  │  BT2  │  BT3  │  BT4  │   │       │       │       │       │       │
├───────┼───────┼───────┼───────┼───────┤   ├───────┼───────┼───────┼───────┼───────┤
│       │       │       │       │       │   │       │       │       │       │       │
└───────┴───────┴───────┴───────┴───────┘   └───────┴───────┴───────┴───────┴───────┘
              ┌───────┐   ┌───────┬───────┐   ┌───────┬───────┐   ┌───────┐
              │   ↻   │   │ →Def  │ BTclr │   │ →Def  │       │   │   ↻   │
              └───────┘   └───────┴───────┘   └───────┴───────┘   └───────┘
```
**Encoders** — Vol−/+ · Bri−/+

### Layer Mouse — trackpad & mouse  *(⊙MSE toggle)*
Right half drives the trackpad (scroll + move); left half tunes pointer/scroll speed.
Toggle back off with either `⊙MSE` key.

```
┌──────┬──────┬──────┬──────┬──────┐   ┌──────┬──────┬──────┬──────┬──────┐
│ ⊙MSE │ MB4  │ Mclk │ MB5  │      │   │ Sc←  │ Sc↓  │ Sc↑  │ Sc→  │ ⊙MSE │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│      │      │      │      │      │   │ Mv←  │ Mv↓  │ Mv↑  │ Mv→  │      │
├──────┼──────┼──────┼──────┼──────┤   ├──────┼──────┼──────┼──────┼──────┤
│ Pf−  │ Pf+  │ Sf−  │ Sf+  │ Mod⊙ │   │      │ MB4  │ Mclk │ MB5  │      │
└──────┴──────┴──────┴──────┴──────┘   └──────┴──────┴──────┴──────┴──────┘
            ┌──────┐   ┌──────┬──────┐   ┌──────┬──────┐   ┌──────┐
            │  ↻   │   │  ␣   │  ⇧   │   │ Lclk │ Rclk │   │  ↻   │
            └──────┘   └──────┴──────┘   └──────┴──────┘   └──────┘
```
**Encoders** — pointer speed −/+ · scroll speed −/+

---

## Combos (Base layer only)

Two-key chords on the **Base** layer (50 ms timeout). These give you Tab, Esc,
modifiers, Backspace, and Enter without a dedicated key.

| Press together | Result | Fingers |
|---|---|---|
| `W` + `E` | `⇥` Tab | left ring + middle (top row) |
| `S` + `D` | `⎋` Esc | left ring + middle (home row) |
| `D` + `F` | `⌘` Left Cmd | left middle + index |
| `N` + `K` | `⌘` Right Cmd | right |
| `K` + `O` | `Bsp` Backspace | right |
| `P` + Ctrl-key | `⏎` Enter | bottom row |

---

## Encoders, trackpad & display at a glance

### Rotary encoders (2 × EC11)
- **Wiring & enable** — `boards/shields/sweep/sweep.dtsi` (`left_encoder`,
  `right_encoder`, pull-up), enabled per half in `sweep_left.overlay` /
  `sweep_right.overlay`.
- **Resolution** — `triggers-per-rotation`: left = **15**, right = **45**
  (`sweep.keymap`, `&sensors` block).
- **Action per layer** — each layer's `sensor-bindings` line. All corne-ported
  layers = volume (left) + brightness (right); **Mouse** layer = pointer/scroll speed.

### Trackpad (Cirque Pinnacle II)
- **Wiring & feel** — `boards/shields/sweep/sweep_right_trackpad.overlay`:
  I²C `0x2a`, absolute mode, sensitivity `2x`, tap/drag, edge-motion, right-edge scroll.
- **Pointer acceleration** — two input processors in `sweep.keymap`:
  `pointer_processor` (adaptive accel curve, 10–400%) and `drag_scroll_processor`.
- **Driver config** — `config/sweep_right_trackpad.conf` (I²C, 4 KB input stack);
  the **left** half (`config/sweep_left.conf`) processes pointer events over the split.

### E-ink display (SSD1680, 152×152)
- **Wiring & timings** — `boards/shields/sweep/sweep_left_display_hw.overlay` (SPI0).
- **Driver / fonts** — `config/sweep_left_display_hw.conf` (SSD16XX, LVGL 1-bit).
- **Graphics** — from the external `zmk-vfx-sweep-pro-display` module.

---

## Where to change what

| Want to change… | Edit this |
|---|---|
| Which key does what | `config/sweep.keymap` → that layer's `bindings = < … >` |
| Layer order / which is default | top `#define` block (`BASE SYMB NUM NAV FUNC BT MOUSE`) |
| Add home-row mods | bind `&hm MOD KEY` on Base alphas (behavior already defined) |
| Encoder direction / pins | `boards/shields/sweep/sweep.dtsi` (`a-gpios` / `b-gpios`) |
| Encoder resolution | `&sensors { … triggers-per-rotation }` in `sweep.keymap` |
| What each encoder does | each layer's `sensor-bindings = < … >` |
| Trackpad sensitivity / tap / scroll | `boards/shields/sweep/sweep_right_trackpad.overlay` |
| Pointer speed / acceleration curve | `pointer_processor` / `drag_scroll_processor` in `sweep.keymap` |
| E-ink wiring / fonts | `sweep_left_display_hw.overlay` / `sweep_left_display_hw.conf` |
| BLE power / sleep / battery / USB | `config/sweep.conf` |
| Which features a build includes | `build.yaml` (shield combos) |

> The diagrams here are generated, not hand-aligned. If you change the keymap,
> regenerate them from the formatter tooling alongside this repo. See
> `CHOCOFY_CHANGES.md` for what this layout dropped vs the original Sweep config.
