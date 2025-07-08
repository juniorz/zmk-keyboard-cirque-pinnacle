# zmk-keyboard-cirque-pinnacle

Cirque trackpad module for ZMK.

This module provides two shield:
- `cirque_pinnacle_peripheral`: the Cirque trackpad input device.
- `cirque_pinnacle_central`: the input split device (used on the split central).
- `cirque_pinnacle_adapter`: an adapter to provide multiple pin configurations.

## Hardware requirements

- trackpad with the "Pinnacle Touch Controller". Tested on Cirque GlidePoint (TM040040, TM035035) devices.
- I2C adapter. See juniorz/cirque-i2c-pcb for an adapter that's pin-compatible with the nice!view.

## Usage

```yaml
#west.yml
manifest:
  remotes:
    - name: juniorz
      url-base: https://github.com/juniorz
  projects:
    # cirque pinnacle
    - name: zmk-keyboard-cirque-pinnacle
      remote: juniorz
      revision: main
      import: west.yml
```

```yaml
#build.yaml
---
include:
  - board: nice_nano_v2
    shield: corne_left nice_view_adapter nice_view cirque_pinnacle_central
    snippet: studio-rpc-usb-uart

  - board: nice_nano_v2
    shield: corne_right cirque_pinnacle_adapter cirque_pinnacle_trackpad
```

```cirque_pinnacle_trackpad.overlay
// device configuration
&i2c0 {
    glidepoint: glidepoint@2a {
        compatible = "cirque,pinnacle";

        // see: https://github.com/petejohanson/cirque-input-module/blob/main/dts/bindings/input/cirque%2Cpinnacle-common.yaml
        //rotate-90;
        //x-invert;
        //y-invert;
        //sleep;
        //no-secondary-tap;
        //no-taps;
        //sensitivity = 1x | 2x | 3x | 4x;
        //x-axis-z-min = 5;
        //y-axis-z-min= 4;
    };
};
```

## TODO

- [ ] Add pictures
- [ ] Integrate interesting changes from the many forks of petejohanson/cirque-input-module
  - https://github.com/derui/cirque-input-module/tree/feature/round-scroll
  - https://github.com/fbrzozowski/cirque-input-module/tree/feat/inertial-pointer
  - https://github.com/fbrzozowski/cirque-input-module/tree/feat/pointer-acceleration
  - https://github.com/halfdane/cirque-input-module/tree/absolute_mode
  - https://github.com/XinYun2020/cirque-input-module/tree/alice-circular-scroll