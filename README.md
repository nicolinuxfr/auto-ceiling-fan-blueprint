# Auto Ceiling Fan Controller

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fauto-ceiling-fan-blueprint%2Fgh-pages%2Fen%2Fceiling_fan.yaml)

**Raw URL (copy-paste):**
```
https://raw.githubusercontent.com/nicolinuxfr/auto-ceiling-fan-blueprint/gh-pages/en/ceiling_fan.yaml
```

This blueprint automatically controls one or more ceiling fans based on the temperature difference (delta) between the ceiling and the ambient temperature. It helps homogenize room temperature by running the fans at the right speed and direction.

## How it works

1. The blueprint calculates a temperature delta between ceiling and ambient sensors.
2. It determines the fan rotation direction:
   - **Season-based (default)**: when no thermostat entity is set, direction is based on the current season, automatically detected from your Home Assistant location (northern hemisphere: November–March = reverse; southern hemisphere: May–September = reverse).
   - **Thermostat-based**: when a climate entity is set, direction is based on the thermostat's current action. When actively heating, the fan rotates in reverse; otherwise it rotates forward.
3. It adjusts fan speed based on configurable thresholds:
   - Delta below **off threshold** (default 1.0°C): fan turns off.
   - Delta in **hysteresis zone** (off → normal threshold): fan stays at **normal speed** if already running, does not start.
   - Delta above **normal threshold** (default 2.0°C): fan runs at **normal speed** (33%).
   - Delta above **fast threshold** (default 4.0°C): fan runs at **fast speed** (67%).
4. Optional **silent mode**: when active (e.g. TV is on), the fans are limited to normal speed only (fast speed is disabled).

## Temperature sources

Select one or more temperature sensors and/or sensor groups. The automation uses `expand()` to automatically resolve group members, then computes min and max from all available values.

- **A sensor group**: the automation expands its members and extracts all values.
- **Multiple individual sensors**: the automation uses all their values directly.
- **A mix of both**: all values are pooled together.

At least 2 distinct temperature values are required. If only one value is available, the automation does not run.

## Configuration options

| Input | Description | Default |
|---|---|---|
| Fan entities | One or more ceiling fans or fan groups to control | — |
| Temperature sensors | Temperature sensors and/or sensor groups | — |
| Thermostat entity | Optional climate entity for direction control; if empty, season is used | — |
| Off threshold | Delta below which fan stops | 1.0°C |
| Normal speed threshold | Delta above which fan starts at normal speed | 2.0°C |
| Fast speed threshold | Delta above which fan runs at fast speed | 4.0°C |
| Normal speed | Fan speed in normal operation | 33% |
| Fast speed | Fan speed at high delta | 67% |
| Silent mode entity | input_boolean or binary_sensor (e.g. TV on) | — |

## Known limitations

- Blueprints do not support minimum selection count validation: the UI requires at least 1 sensor, but the automation needs at least 2 temperature values to compute a delta. If insufficient values are found, the automation simply does not trigger.
- Fan direction is only set on fans that support it (checked via `supported_features`). Fans without direction support are controlled normally for speed and on/off.
