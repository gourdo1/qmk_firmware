# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal fork of QMK firmware (`origin` = `github.com/UnPassive/qmk_firmware`, last merged from upstream `qmk:master` in Sept 2022). The only code actively maintained here is the **`unpassive`** keymap for the **GMMK Pro rev1 ANSI**. Everything else is upstream QMK; avoid modifying it.

Owned files:
- `keyboards/gmmk/pro/rev1/ansi/keymaps/unpassive/`: keymap layers, RGB indicator logic, paddle game, keymap-level `config.h`/`rules.mk`, and the user-facing `readme.md` (feature list and changelog)
- `users/unpassive/`: userspace code shared by the keymap (custom keycodes, `process_record_user`, idle timeout, persistent settings, encoder actions, autocorrect)

The keymap began as a fork of `gourdo1`'s keymap (itself based on `jonavin`'s). Both still exist in the tree (`users/gourdo1`, `keyboards/gmmk/pro/rev1/ansi/keymaps/gourdo1`) and are useful as references. `../GMMK_Pro_Cheatsheet.pdf` (outside the repo) is the printable key reference.

## Build / flash

`qmk` CLI and `arm-none-eabi-gcc` are installed system-wide.

```sh
qmk compile -kb gmmk/pro/rev1/ansi -km unpassive     # or: make gmmk/pro/rev1/ansi:unpassive
qmk flash   -kb gmmk/pro/rev1/ansi -km unpassive     # board must be in bootloader mode
```

Output lands in `.build/` and as `gmmk_pro_rev1_ansi_unpassive.bin` in the repo root (QMK Toolbox can flash it too). No tests cover this keymap; a clean compile is the check. Watch the firmware size reported at the end of the build (`LTO_ENABLE = yes` is set to keep it small).

Autocorrect dictionary: edit `users/unpassive/autocorrect/autocorrection_dict.txt`, then run `python3 make_autocorrection_data.py` from that directory to regenerate `autocorrection_data.h`. The `(small)`/`(large)` files are alternate pre-generated dictionaries.

## Architecture

**Hook layering.** The userspace (`users/unpassive/unpassive.c`) owns the QMK `*_user` hooks (`process_record_user`, `matrix_scan_user`, `keyboard_post_init_user`, `eeconfig_init_user`) and calls weak `*_keymap` variants (`process_record_keymap`, `matrix_scan_keymap`, `keyboard_post_init_keymap`) that the keymap may override. The keymap itself implements `encoder_update_user` and `rgb_matrix_indicators_advanced_user`. In `process_record_user`, the keymap hook runs first, then autocorrect, then the custom keycode handling.

**Layers** are defined by `enum custom_user_layers` in `unpassive.h` (`_BASE`, `_FN1`, `_NUMPADMOUSE`, `_MOUSEKEY`, optional `_GAME`). `_COLEMAK` is **not** in the enum. It is `#define`d in the keymap `config.h` together with `DYNAMIC_KEYMAP_LAYER_COUNT`, and both depend on `GAME_ENABLE`. If you add or remove a layer, update both places, or VIA/dynamic keymap breaks.

**Feature flags** are a two-step pipeline. The keymap `rules.mk` sets `FOO_ENABLE = yes`, and `users/unpassive/rules.mk` turns that into `OPT_DEFS += -DFOO_ENABLE`. The C code then uses `#ifdef`. Adding a new toggle means touching both rules.mk files.

**Persistent runtime settings.** `user_config_t` (a bitfield union in `unpassive.h`) is stored in EEPROM through `eeconfig_read_user`/`eeconfig_update_user`. Each `TG_*` keycode flips one bit and writes it back. Defaults are set in `eeconfig_init_user`. `PRNCONF` (FN+`) types the current state of every toggle into the focused text field. If you add a field, also add its default, a toggle keycode, and a PRNCONF line.

**Custom keycodes** start at `SAFE_RANGE` in `unpassive.h`. Keymap-local keycodes must start from `NEW_SAFE_RANGE`. Because VIA is enabled, changing the order of this enum shifts keycodes that are already saved in VIA keymaps.

**RGB.** `rgb_matrix_map.h` maps physical keys and side LEDs to LED indices (`LED_*` constants and grouped arrays). `rgb_matrix_indicators_advanced_user` in `keymap.c` draws every per-layer, capslock, numlock, and game-mode highlight, and the paddle game rendering (`paddlegame.h`, `GAME_ENABLE`).

## Conventions

- Tabs for indentation in the `unpassive` files (the repo's `.clang-format` targets upstream style; don't reformat these files).
- Record user-visible behavior changes in the keymap `readme.md`. The git history uses plain descriptive commit messages.
