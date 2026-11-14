# esphome-econet — multi-instance fork

A clone of [esphome-econet/esphome-econet](https://github.com/esphome-econet/esphome-econet)
carrying what the water-heater controller board needs: two heaters, each on its own RS485 bus,
in one ESPHome node. Why it exists and what depends on it:
`~/projects/memory/projects/econet_controller_pcb.md`. The board itself is `~/projects/pcb/econet_controller`
(pin map: `docs/pinmap.md`).

**On GitHub as `jpurcell001/esphome-econet`** (`origin`, since 2026-11-14); `upstream` is fetch-only
(push `DISABLED`). The board's ESPHome config in
`~/projects/home_assistant/esphome/packages/econet_controller_board.yaml` still points at this
clone by local path (`../../forks/esphome-econet`), so builds run on a Mac with `~/projects/forks/`.

## Branches

- **`dev/multi-instance`**: v3.8.2 plus the changes below. Compile-tested only (2026-10-21),
  since no board exists yet.
- **`ha-prod`** (later): whatever a board actually runs, tagged `v3.8.2+multi.<n>`. A change
  reaches it only once it's deployed.
- **`fix/multi-conf`** (later, an upstream PR candidate): `MULTI_CONF` alone, on upstream `main`.
  Opening a PR is the owner's call.
- **`main`**, once the fork exists, mirrors upstream; never commit to it.

## The changes

1. `components/econet/__init__.py`: `MULTI_CONF = True`. The C++ has no shared mutable state,
   so this is the only component change.
2. `econet_heatpump_water_heater_instance.yaml`: **generated, do not edit.** It is
   `econet_heatpump_water_heater.yaml` merged and reduced to one heater, with every id prefixed
   `${p}_` and every entity tagged `device_id: ${device}`. Regenerate it with
   `tools/gen_instance_package.py` (the command is in its docstring) after any upstream bump, and
   diff the result.
