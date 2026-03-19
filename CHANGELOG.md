# Changelog

## 2026.3.8

- Always set the correct direction for the fan (useful since the fan will go back to it's default after electric disconnect).

## 2026.3.7

- (Hopefully) fix the annoying Tuya fans by sending only one command each time instead of two. 

## 2026.3.6

- Only send command to the fan if it's state is different (should help with crappy Tuya devices, ask me how I know…) ;
- Remove the thermostat trigger, because it was too frequent for me : instead, the status will change when the automation updates at user request (15 min by default).

## 2026.3.5

- Silent mode now uses the fast speed threshold, to minimize when it activates the fan. The idea is to switch it off most of the time and to start the fan only when the delta is huge.

## 2026.3.4

- Better fan direction handling :
  - Only for fans that supports it, the automation won't break if you use some without it ;
  - season-based by default (reverse in winter, forward otherwise or vice-versa depending of your hemisphere)
  - climate-based if selected (reverse when heating, forward otherwise).

## 2026.3.3

- Can select multiple fans (or a group of fans) ;
- Simplify the handling of thresholds and speeds : only two now, the quiet mode always use the loweest speed.

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
