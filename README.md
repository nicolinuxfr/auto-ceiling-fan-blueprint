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
   - **Without AC mode entity**: direction is based on the current season (northern hemisphere — November–March = reverse for heating, April–October = forward for cooling).
   - **With AC mode entity**: direction is controlled by an `input_select` entity.
3. It adjusts fan speed based on configurable thresholds:
   - Delta below **stop threshold** (default 0.8°C): fan turns off.
   - Delta in **hysteresis zone** (stop → start threshold): fan runs at **low speed** (16%).
   - Delta above **start threshold** (default 2.0°C): fan runs at **medium speed** (33%).
   - Delta above **high speed threshold** (default 5.0°C): fan runs at **high speed** (67%).
4. Optional **silent mode**: when active (e.g. TV is on), the start threshold is raised to avoid noise.

## Temperature sources (choose one)

- **Thermometer group** (recommended): a Home Assistant `group.xxx` containing temperature sensors. The automation calculates min and max itself.
- **Two separate sensors**: provide a ceiling sensor and an ambient sensor directly.

If both are configured, the group takes priority.

## Configuration options

| Input | Description | Default |
|---|---|---|
| Fan entity | The ceiling fan to control | — |
| Temperature group | A group of temperature sensors (group.xxx) | — |
| Ceiling sensor | Temperature near the ceiling | — |
| Ambient sensor | Temperature at room level | — |
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

- The seasonal direction detection assumes a **northern hemisphere** location. Southern hemisphere users should configure an AC mode `input_select` entity instead.
- Blueprints do not support mutually exclusive optional inputs: both the group and the two-sensor options are shown, but only one should be filled (the group takes priority).
- Fan direction changes may not be supported by all fan integrations when the fan is off.
