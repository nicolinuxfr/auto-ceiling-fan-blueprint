# Changelog

## 2026.3.2

- Less executions of the automation :
  - Only at fixed intervall, 10 min by default, but the user can choose anything between 5 and 120 ;
  - Immediately when the control entities (rotation and silent mode) changes or when the automatisation is saved (after a change or blueprint update).

## 2026.3.1

- Change logic of temperature sensors :
  - Only one field to select multiple sensors or a group of sensors
  - Calculate the delta based on the user selection
  - If only one sensor is selected, delta is always 0 and nothing happens (unfortunately, there is no better way, it seems).

## 2026.3

- Initial release of the Auto Ceiling Fan Controller blueprint.
- Automatic fan direction based on season (northern hemisphere) or optional AC mode input_select.
- Two temperature source modes: min_max group or two separate sensors.
- Three configurable speed thresholds (stop / start / high speed).
- Optional silent mode support via input_boolean or binary_sensor.
- French and English localizations.
