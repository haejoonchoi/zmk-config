# Agent instructions

## Local firmware builds

This repository's build matrix is defined in `build.yaml`; keep local build
commands aligned with it. From the repository root, initialize the west
workspace once and fetch its pinned modules:

```sh
west init -l config
west update
```

Install the Zephyr SDK version required by the pinned ZMK/Zephyr revision and
the Python requirements for that workspace before building. Build each half
into its own directory:

```sh
west build -p -s zmk/app -d build/lily58-left -b nice_nano_v2 -- \
  -DZMK_CONFIG="$PWD/config" \
  -DSHIELD="lily58_left nice_view_adapter nice_view" \
  -DSNIPPET=studio-rpc-usb-uart

west build -p -s zmk/app -d build/lily58-right -b nice_nano_v2 -- \
  -DZMK_CONFIG="$PWD/config" \
  -DSHIELD=lily58_right
```

The firmware files are `build/lily58-left/zephyr/zmk.uf2` and
`build/lily58-right/zephyr/zmk.uf2`. These builds require the matching board,
shield, and snippet definitions from the pinned ZMK modules.
