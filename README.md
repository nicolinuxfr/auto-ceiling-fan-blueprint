# Auto Ceiling Fan Controller

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fauto-ceiling-fan-blueprint%2Fgh-pages%2Fen%2Fceiling_fan.yaml)

**Raw URL (copy-paste):**
```
https://raw.githubusercontent.com/nicolinuxfr/auto-ceiling-fan-blueprint/gh-pages/en/ceiling_fan.yaml
```

This blueprint automatically controls a ceiling fan based on the temperature difference (delta) between the ceiling and the ambient temperature. It helps homogenize room temperature by running the fan at the right speed and direction.

## How it works

1. The blueprint calculates a temperature delta between ceiling and ambient sensors.
2. It determines the fan direction:
   - **Without AC mode entity**: direction is based on the current season, automatically detected from your Home Assistant location (northern hemisphere: November–March = reverse; southern hemisphere: May–September = reverse).
   - **With AC mode entity**: direction is controlled by an `input_select` entity.
3. It adjusts fan speed based on configurable thresholds:
   - Delta below **stop threshold** (default 0.8°C): fan turns off.
   - Delta in **hysteresis zone** (stop → start threshold): fan runs at **low speed** (16%).
   - Delta above **start threshold** (default 2.0°C): fan runs at **medium speed** (33%).
   - Delta above **high speed threshold** (default 5.0°C): fan runs at **high speed** (67%).
4. Optional **silent mode**: when active (e.g. TV is on), the start threshold is raised to avoid noise.

## Temperature sources

Select one or more temperature sensors and/or sensor groups. The automation uses `expand()` to automatically resolve group members, then computes min and max from all available values.

- **A sensor group**: the automation expands its members and extracts all values.
- **Multiple individual sensors**: the automation uses all their values directly.
- **A mix of both**: all values are pooled together.

At least 2 distinct temperature values are required. If only one value is available, the automation does not run.

## Configuration options

| Input | Description | Default |
|---|---|---|
| Fan entity | The ceiling fan to control | — |
| Temperature sensors | Temperature sensors and/or sensor groups | — |
| AC mode entity | Optional input_select for heating/cooling/off | — |
| Heating state | State value for heating mode | `Chauffage` |
| Cooling state | State value for cooling mode | `Climatisation` |
| Off state | State value for off mode | `Éteint` |
| Stop threshold | Delta below which fan stops | 0.8°C |
| Normal start threshold | Delta above which fan starts (normal mode) | 2.0°C |
| Silent start threshold | Delta above which fan starts (silent mode) | 3.5°C |
| High speed threshold | Delta above which fan runs at high speed | 5.0°C |
| Low speed | Fan speed in hysteresis zone | 16% |
| Medium speed | Fan speed in normal operation | 33% |
| High speed | Fan speed at high delta | 67% |
| Silent mode entity | input_boolean or binary_sensor (e.g. TV on) | — |

## Known limitations

- Blueprints do not support minimum selection count validation: the UI requires at least 1 sensor, but the automation needs at least 2 temperature values to compute a delta. If insufficient values are found, the automation simply does not trigger.
- Fan direction changes may not be supported by all fan integrations when the fan is off.
