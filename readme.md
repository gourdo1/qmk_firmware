# Quantum Mechanical Keyboard Firmware

## Additions in this fork

This fork adds the "unpassive" keymap to the gmmk pro keyboard: qmk_firmware/keyboards/gmmk/pro/rev1/ansi/keymaps/unpassive
It also adds "unpassive" to users: qmp_firmware/users/unpassive
Both of these folders are forks of gourdo1's work

## Flashing firmware:

A tutorial on QMK with installation instructions and more can be found here: https://docs.qmk.fm/  
Install QMK via the tutorial and run the "qmk setup" command  
To set keyboard to gmmk pro: "qmk config user.keyboard=gmmk/pro/rev1/ansi"  
To compile firmware with 4 cpu threads: "qmk compile -km unpassive -j 4"  
To flash:
- Put the keyboard in bootloader mode (Unplug keyboard, then hold down Fn + \ while plugging keyboard in)
  - If you run the flash command below before putting keyboard in bootloader mode, the command will wait until it detects a keyboard in bootloader mode
- "qmk flash -km unpassive"

When changing far right column of keys (above arrow keys), sometimes the old values remain after flashing a new firmware - fn + esc clears the EEPROM which seems to then load the new buttons. Do this after flashing. I believe this is a bug with the base firmware
- This possible bug also appears to occur when changing default bool values found at the bottom of qmk_firmware/users/unpassive/unpassive.c - you must run Fn + Esc for values to take effect

## TODO:
- Update readme to show actual features: qmk_firmware/keyboards/gmmk/pro/rev1/ansi/keymaps/unpassive

[![Current Version](https://img.shields.io/github/tag/qmk/qmk_firmware.svg)](https://github.com/qmk/qmk_firmware/tags)
[![Discord](https://img.shields.io/discord/440868230475677696.svg)](https://discord.gg/Uq7gcHh)
[![Docs Status](https://img.shields.io/badge/docs-ready-orange.svg)](https://docs.qmk.fm)
[![GitHub contributors](https://img.shields.io/github/contributors/qmk/qmk_firmware.svg)](https://github.com/qmk/qmk_firmware/pulse/monthly)
[![GitHub forks](https://img.shields.io/github/forks/qmk/qmk_firmware.svg?style=social&label=Fork)](https://github.com/qmk/qmk_firmware/)

This is a keyboard firmware based on the [tmk\_keyboard firmware](https://github.com/tmk/tmk_keyboard) with some useful features for Atmel AVR and ARM controllers, and more specifically, the [OLKB product line](https://olkb.com), the [ErgoDox EZ](https://ergodox-ez.com) keyboard, and the [Clueboard product line](https://clueboard.co).

## Documentation

* [See the official documentation on docs.qmk.fm](https://docs.qmk.fm)

The docs are powered by [Docsify](https://docsify.js.org/) and hosted on [GitHub](/docs/). They are also viewable offline; see [Previewing the Documentation](https://docs.qmk.fm/#/contributing?id=previewing-the-documentation) for more details.

You can request changes by making a fork and opening a [pull request](https://github.com/qmk/qmk_firmware/pulls), or by clicking the "Edit this page" link at the bottom of any page.

## Supported Keyboards

* [Planck](/keyboards/planck/)
* [Preonic](/keyboards/preonic/)
* [ErgoDox EZ](/keyboards/ergodox_ez/)
* [Clueboard](/keyboards/clueboard/)
* [Cluepad](/keyboards/clueboard/17/)
* [Atreus](/keyboards/atreus/)

The project also includes community support for [lots of other keyboards](/keyboards/).

## Maintainers

QMK is developed and maintained by Jack Humbert of OLKB with contributions from the community, and of course, [Hasu](https://github.com/tmk). The OLKB product firmwares are maintained by [Jack Humbert](https://github.com/jackhumbert), the Ergodox EZ by [ZSA Technology Labs](https://github.com/zsa), the Clueboard by [Zach White](https://github.com/skullydazed), and the Atreus by [Phil Hagelberg](https://github.com/technomancy).

## Official Website

[qmk.fm](https://qmk.fm) is the official website of QMK, where you can find links to this page, the documentation, and the keyboards supported by QMK.
