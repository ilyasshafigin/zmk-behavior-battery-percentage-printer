# ZMK Battery Percentage Printer Behavior

Fork of [badjeff/zmk-behavior-battery-percentage-printer](https://github.com/badjeff/zmk-behavior-battery-percentage-printer), originally based on [alan0ford's behavior_battery_printer.c](https://github.com/alan0ford/zmk-lplancks/blob/GHPilotBatt/boards/shields/lplancks/behavior_battery_printer.c).

## About this fork

This fork keeps peripheral battery reporting support and adds configurable output keycodes.

You can now override which keycodes are used for:

- digits (`0`-`9`)
- space (` `)
- dot (`.`)
- percent (`%`)

This is useful for non-US layouts and custom symbol mappings.

## What it does

Type the battery percentage by triggering behaviors on the sheild's keymap.

To report battery state of peripheral sheild, you can use combo-keys to toggle an utility layer and assign this behavior on a layer, the dongle would report the peripheral battery state in percentage by sending keycodes (e.g. '89% ') to the host.

## Installation

Include this project on your ZMK's west manifest in `config/west.yml`:

```yaml
manifest:
  remotes:
    - name: ilyasshafigin
      url-base: https://github.com/ilyasshafigin
  projects:
    - name: zmk-behavior-battery-percentage-printer
      remote: ilyasshafigin
      revision: main
```

And update `shield.conf` on both *central* and *peripheral* shields.

```conf
CONFIG_ZMK_SPLIT_BLE_CENTRAL_BATTERY_LEVEL_FETCHING=y
```

Now, update your `shield.keymap` adding the behaviors.

```c
#include <behaviors/battery_percentage_printer.dtsi>

/ {
    keymap {
        compatible = "zmk,keymap";
        base {
            bindings = <
                ...
                ...   &bapp   ...   &bapp ...
                    /* left */    /* right */
                ...
            >;
        };
    };
};
```

## Overriding keycode configuration

Default behavior node (`battery_percentage_printer.dtsi`) uses:

```dts
digit-keycodes = <N0 N1 N2 N3 N4 N5 N6 N7 N8 N9>;
space-keycode = <SPACE>;
dot-keycode = <DOT>;
percent-keycode = <PERCENT>;
```

You can override any of these properties in your keymap:

```dts
&bapp {
    digit-keycodes = <N0 N1 N2 N3 N4 N5 N6 N7 N8 N9>;
    space-keycode = <SPACE>;
    dot-keycode = <COMMA>;
    percent-keycode = <PERCENT>;
};
```

Notes:

- `digit-keycodes` is an array of exactly 10 keycodes in `0..9` order.
- Keycode values must be valid ZMK keycode macros available in your build.
- If you do not override a property, the defaults from `battery_percentage_printer.dtsi` are used.
