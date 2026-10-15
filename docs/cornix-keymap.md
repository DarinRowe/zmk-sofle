# Cornix-inspired Sofle layout

This layout is migrated from `cornix 2.vil` for the Eyelash Sofle with no center arrow-key column. Board slots 6, 19, 32, 45 and 58 are intentionally inert on every layer; no feature depends on them. The existing board definition exposes one left encoder. No second encoder or GPIO wiring is added.

## Layers and access

| Layer | Access from Base | Contents |
| --- | --- | --- |
| 0 Base | Default | QWERTY; Enter / held Right Alt; quote / held Right Shift |
| 1 Shortcuts | Hold right thumb Backslash | Command shortcuts, I/J/K/L = Up/Left/Down/Right, H/N = left/right brace |
| 2 Mouse | Hold right thumb Mouse key | I/J/K/L move; U/O = left/right click; Y/H = scroll up/down; M/,/./Slash = the four chords below |
| 3 Symbols | Hold Space or left thumb Symbols key | Number row on Q–P, shifted symbols on A–semicolon, keypad minus/plus/equal on Z/X/C, braces on V/B, brackets on N/M |
| 4 System | Hold the extra top-left Grave key (tap for backtick) | Number keys 1–5 select Bluetooth profiles 0–4; Tab clears the current profile; A/S select USB/BLE output; Z and comma reset; C and slash enter bootloader; dot held for 2 s powers off |

Left thumb keys, from outside toward the center: Control, Command, Alt, Symbols, Escape. Right thumb keys, from the center outward: Backspace, Space/Symbols, Backslash/Shortcuts, Mouse, Right Control. The redundant second left Command and the unused Cornix right thumb slot are omitted to fit the Sofle.

The extra physical number row remains available on Base, provides F1–F10 on Shortcuts, and inherits through Mouse and Symbols. The pre-existing ZMK hold-tap defaults are retained (200 ms; mod-tap hold-preferred and layer-tap tap-preferred). Vial timing settings are not imported as ZMK settings.

## Combinations and mouse controls

Combinations are restricted to Base, with a 50 ms simultaneous press window:

| Keys | Output |
| --- | --- |
| J + K | Command + Option + Left |
| K + L | Command + Option + Right |
| U + I | Control + Left |
| I + O | Control + Right |

These are the complete chords represented by Vial macros M0–M3, expressed as ZMK key presses so modifiers are released with the behavior. Application effects depend on macOS and application shortcut settings. Existing extra browser-back/forward combinations are removed to match the source.

On Mouse, Caps Lock, left Shift and left Control select Bluetooth profiles 0, 1 and 2. The Vial source calls these USER00–USER02 without defining their firmware behavior; Bluetooth selection is an explicit Sofle adaptation based on its previous Mouse layer, not a decoded meaning of the Vial USER keycodes.

The left encoder changes volume on Base, Symbols and System, and scrolls on Shortcuts and Mouse. Its click mutes except where the source disables it on Shortcuts. Cornix’s independent second encoder and right encoder button cannot be reproduced by a single encoder; right-click remains available on Mouse + O. Numbers and shifted symbols assume the host uses a US keyboard layout.

## Build compatibility

This branch includes the Zephyr 4 board migration and encoder compatibility fix from PR #3. The local board module is used instead of fetching a duplicate copy from upstream. ZMK and its build workflow are pinned to `8a44ef109ae301ce084b32dd5fe25200da9e402f`, the ZMK revision available during the successful April 3, 2026 build, to keep the migration separate from subsequent ZMK changes. The final CI build is the validation of this combination; this historical pin alone is not proof of a successful new build.

The matrix builds the right firmware, the left Studio firmware and a left settings-reset image. The settings-reset image erases settings; it is not the normal left firmware. Firmware compilation checks configuration and linking, but physical matrix wiring, encoder direction, timing feel and Bluetooth behavior require testing on the keyboard.

If a previous keymap was saved in ZMK Studio, it may override the firmware defaults. First inspect/export the Studio layout and use Studio’s reset-to-defaults action when adopting this layout; avoid erasing Bluetooth settings just to change keys.
