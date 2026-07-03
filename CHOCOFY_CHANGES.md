# Chocofy changes — corne keymap ported to sweep

Branch: **`sweep-chocofy`**

This documents the work done in `config/sweep.keymap`: the corne keymap
(`zmk-config-corne/config/corne.keymap`) was ported onto the sweep, and what the
new layout **loses** versus the previous sweep keymap.

---

## What changed

### Layers (replaced)

| # | Old sweep | New (corne port) |
|---|---|---|
| 0 | `DEF` (Mac QWERTY) | **`Base`** — corne default layer |
| 1 | `WIN` (Windows QWERTY) | **`Symb`** — corne symbs layer |
| 2 | `NUM` (numbers + nav) | **`Num`** — corne numbrkt layer |
| 3 | `SYM` (symbols) | **`Nav`** — corne nav layer |
| 4 | `FUN` (tri-layer) | **`Func`** — corne function layer |
| 5 | — | **`BT`** — corne control layer |
| 6 | — | **`MOUSE`** — kept from old sweep |

The old `DEF`/`WIN`/`NUM`/`SYM`/`FUN` layers are gone. `MOUSE` is retained
unchanged (layer index moved 5 → 6) so the trackpad still works.

### Tap-hold behaviors (replaced)

| Removed (sweep) | Added (corne) |
|---|---|
| `ht` — home-row hold-tap (balanced) | `hm` — homerow mods (tap-preferred, 175 ms) |
| `ds_z` — tap key / hold = drag-scroll | `hltk` — tap key / hold = layer (tap-preferred, 175 ms) |
| `sel_x` — tap key / hold = mouse click | |

> **Note:** `hm` is defined but **not bound** anywhere, because the corne *Base*
> layer uses plain keys on the home row (no home-row mods) — only the corne's
> Colemak layer (not ported) uses `hm`. See "Lost — home-row mods" below.

### Combos (replaced)

The 5 old **mouse-chord combos** are gone. The 6 **corne combos** were ported,
with `key-positions` remapped from the corne's 6-column matrix to the sweep's
5-column matrix. Every combo uses the **same physical fingers** as on the corne.

| Combo | Keys | Fingers | corne pos → sweep pos |
|---|---|---|---|
| Tab | W + E | left ring + middle (top row) | `<2 3>` → `<1 2>` |
| Esc | S + D | left ring + middle (home row) | `<14 15>` → `<11 12>` |
| LGUI | D + F | left middle + index | `<15 16>` → `<12 13>` |
| RGUI | N + K | right | `<19 20>` → `<16 17>` |
| Backspace | K + O | right | `<20 21>` → `<17 18>` |
| Enter | P + Ctrl-key | bottom row | `<32 33>` → `<27 28>` |

All combos are active on the **Base layer only** (`layers = <BASE>`). The corne
had them on `<0 1>` (Base + Colemak); the sweep has a single alpha layer.

### Thumb row (reduced to middle 4)

The corne has 6 thumb keys per layer; the sweep has **4 thumb keys + 2 encoders
on the outside**. Per layer, only the **middle 4** of the corne's 6 thumb keys
are populated; the 2 outermost thumb positions are `&none`.

The matrix (`boards/shields/sweep/sweep.dtsi`) still defines 6 thumb positions
(30–35), so the keymap keeps 6 thumb cells per layer with the outermost two set
to `&none`. No hardware/dtsi change was made.

### Encoders

Every ported layer uses **left = volume −/+ , right = brightness −/+**
(`&inc_dec_kp C_VOL_DN C_VOL_UP` / `&inc_dec_kp C_BRI_DEC C_BRI_UP`), matching
the sweep's existing setup. The `MOUSE` layer keeps its own encoder bindings
(pointer speed / scroll speed).

### Other

- `&sk { quick-release; }` ported from corne (Base uses `&sk LALT` / `&sk LCTRL`).
- `conditional_layers` (tri-layer) **removed** — corne uses explicit `&mo` only.
- Mouse/pointer behaviors kept for the `MOUSE` layer: `drgscrl`, `ptr_spd`,
  `enc_ptr_spd`, `enc_scrl_spd`, `crq_mode`, `mmv`, `msc`, plus the
  `pointer_processor` / `drag_scroll_processor` input processors.

---

## Adaptations (relocations made to stay functional)

Dropping the outermost thumb keys removed a few critical bindings. These were
relocated to an inner thumb slot rather than lost:

| What | Was on (corne) | Now on (sweep) |
|---|---|---|
| Bluetooth layer access (`&to BT`) | Func outermost-left thumb (dropped) | Func inner-left thumb |
| `&bt BT_CLR` (unpair) | BT outermost-left thumb (dropped) | BT inner thumb |
| `&tog MOUSE` (enter mouse layer) | — (corne has none) | Symb inner-right thumb (added) |

---

## Lost vs the previous sweep keymap

These features/keys existed on the old sweep and are **no longer present**.

### Behaviors / layers

- **Home-row mods.** The old sweep bound `&ht` on every home-row alpha
  (e.g. `&ht LALT S`). The new Base has **plain keys** — no modifiers on hold.
  `hm` is available if you want to re-add them (e.g. `&hm LCTRL A`).
- **Drag-scroll-on-hold** (`ds_z`) — old `Z` held = trackpad drag-scroll. Gone.
- **Click-on-hold** (`sel_x`) — old `X` held = left mouse button (drag-select). Gone.
- **Windows layer** — old `WIN` layer (layer 1) with swapped Ctrl/Win order. Gone
  (corne has a single Base layer; the `&tog` to Colemak in corne Func was
  dropped, so there is no second alpha layer).
- **Tri-layer conditional** — old sweep auto-activated `FUN` when `NUM`+`NAV`
  were both held. Removed (corne doesn't use conditional layers).
- **5 mouse-chord combos** — `E+R` (right click), `R+T` (forward), `D+F` (left
  click), `F+G` (back), `C+V` (middle click). Replaced by the corne's typing
  combos. Clicking now requires the `MOUSE` layer.

### Thumb keys dropped (outermost, per layer)

| Layer | Dropped outermost thumb key(s) |
|---|---|
| Base | `LA(SPACE)` (left), `LG(SPACE)` (right) |
| Num | `RALT` (right) |
| Func | (the original `&to BT` was relocated — see Adaptations) |
| MOUSE | `ptr_spd SPEED_POINTER SPEED_RESET` (left), `ptr_spd SPEED_SCROLL SPEED_RESET` (right) — speed reset to default |

### Other removed bindings

- **`studio_unlock`** — every old layer had a ZMK-Studio unlock thumb key. Gone
  (corne has none). ZMK Studio is still enabled in `config/*.conf`; add a
  `&studio_unlock` binding if you want to use it.
- **`soft_off`** key access — old `FUN` had a soft-off key. Not present on the
  corne Func layer. (Soft-off GPIO wake config in `sweep.dtsi` is unchanged.)
- **`default_report`** — old `SYM` had this custom behavior on a key. The
  `behaviors/report.dtsi` include is retained but the behavior is now unused.

---

## Quick "where is it now"

| You want… | Where it is |
|---|---|
| Symbols | hold **Symb** thumb (Base inner-left) |
| Numbers / brackets | hold **Num** thumb (Base inner-left, `&hltk NAV SPACE` neighbor) |
| Navigation / arrows | hold **Nav** (`&hltk NAV SPACE` on Base) |
| Function keys / media / BT | **Func** layer → F-keys, volume, brightness; **BT** via `&to BT` |
| Mouse / trackpad | `&tog MOUSE` on the **Symb** layer |
| Esc / Tab / Backspace / Enter | combos on Base (see table above) |
