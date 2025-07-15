# ha-homekit-valve-duration

This is a fork of the Home Assistant core component [`homekit`](https://github.com/home-assistant/core/tree/dev/homeassistant/components/homekit)

Based on the idea of https://github.com/f-perna/homekit-valve

## Features

- **Set default duration** in HomeKit, synced with a state in Home Assistant
- **Display remaining time** based on an end date, to be provided by a sensor in Home Assistant

<img width="300" alt="IMG_4092" src="https://github.com/user-attachments/assets/1cf044f6-910e-4e11-95a8-8e108e705ebe" />
<img width="300" alt="IMG_4093" src="https://github.com/user-attachments/assets/7855e5c1-1539-4200-b85d-50a6464bb3f8" />

## Supported entities

The linked entities can be added to a
- **Switch** of type `faucet`, `shower`, `sprinkler`, or `valve`
- **Valve**

## Linked entities

To add support for the default run time and remaining duration characteristics, you will have to link entities from Home Assistant:

Entity Config |Type|Description
-|-|-|
linked_valve_duration|`input_number`|This entity can be updated in Home Assistant, or via HomeKit. The state is synced in both directions.
linked_valve_end_time|`sensor`|A sensor with `device_class` `timestamp`, for calculating the remaining time. State must be managed in Home Assistant, value will not be updated by HomeKit. The correct end time should be set as soon as possible after the switch or valve is activated. If not provided immediately, the default duration set in `linked_valve_duration` will be used as initial countdown.

## Helper entities

If you do not yet have the entities available, for linking them to your valve, you can create helpers.

**Default run time** for `linked_valve_duration`:
1. Create an `input_number` helper entity 

**End time** for `linked_valve_end_time`:
1. Create an `input_datetime` helper entity, that is updated when the valve switch is turned on in Home Assistant (via a turn_on action or automation)
2. Create a `sensor` template (`device_class` timestamp), that provides a state based on the `input_datetime` entity helper you already created 

> You can create and configure the helper entities in `Settings -> Devices & Services -> Helpers`

## Example

```
homekit:
  name: "HASS Bridge"
  port: 21064
  mode: bridge
  filter:
    include_entities:
      - switch.sprinkler
  entity_config:
    switch.sprinkler
      type: sprinkler
      linked_valve_duration: input_number.sprinkler_default_run_time
      linked_valve_end_time: sensor.sprinkler_end_time
```
## Setup

> Use at your own risk! This is a POC and intended for testing purposes and developers who want to contribute to the Home Assistant core implementation.

To test this POC, you may 
1. Copy the `homekit` folder from this repository into your `<config>/custom_components` folder in Home Assistant
2. Adjust your HomeKit configuration (see example)
3. Restart Home Assistant

This will load this fork instead of the built in homekit integration. Turn on debug logging for troubleshooting.
