# ha-homekit-valve-duration

This is a fork of the Home Assistant core component [`homekit`](https://github.com/home-assistant/core/tree/dev/homeassistant/components/homekit)

## Setup

> Use at your own risk! This is a POC and intended for testing purposes for developers who want to contribute to the Home Assistant core implementation.

To test this POC, you may 
1. Copy the `homekit` foldet into your `<config>/custom_components` folger
2. Adjust your Homekit configuration
3. Restart Home Assistant

## Linked entities

Configuration|Type|Description
-|-|-|
linked_valve_duration|`input_number`|This entity can be updated in Home Assistant, or via HomeKit. The state is synced in both directions.
linked_valve_end_time|`sensor`|A sensor with `device_class` `timestamp`, for calculating the end time. State must be managed in Home Assistant, value will not be updated by HomeKit.

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
