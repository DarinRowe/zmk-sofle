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

## Upstream synchronization and build

This branch merges upstream `a741725193/zmk-sofle` through `cad147725279b0635a3d512ac99f6668bfec3704` (July 23, 2026). It replaces the earlier Zephyr 4 board migration with upstream's `nice_nano_v2` controller and `eyelash_sofle_left` / `eyelash_sofle_right` shields. The hardware definitions and GPIOs match that upstream commit. All five Cornix key arrays and four combinations are unchanged by this synchronization.

The upstream Dya Studio integration is retained: its ZMK `v0.3-branch+dya` fork and modules for runtime sensor rotation, Bluetooth management, battery history, settings RPC and runtime input processing. Module revisions are pinned in `config/west.yml` to the current commits of upstream's configured refs. The upstream hardware module is also pinned to the merged commit, and the build workflow is pinned to the official ZMK v0.3.0 commit used by upstream. The old Zephyr 4 board files are removed rather than mixed with the shield architecture.

The single left encoder now uses runtime sensor rotation behaviors with the same default volume and scroll actions as before. Each layer has exactly one sensor binding, matching the hardware definition. Runtime settings saved by a Studio client can override these defaults. The battery-history module is present but remains disabled in the upstream shield configuration.

The matrix builds `eyelash_sofle_studio_left.uf2`, `eyelash_sofle_right.uf2` and `settings_reset_nice_nano_v2.uf2`. The settings-reset image erases settings; it is not normal firmware and is excluded from the normal left/right download package. A previous firmware package is kept separately for rollback. Compilation checks configuration and linking; physical matrix wiring, encoder direction, hold-tap feel, Bluetooth and Dya Studio operations need testing on the keyboard.

If a previous keymap was saved in ZMK Studio, it may override the firmware defaults. Record the old layout before using the client's restore-stock/default-settings action to adopt this layout. Do not erase Bluetooth settings merely to change keys. Dya Studio-specific runtime configuration may also need to be restored through its own client.
