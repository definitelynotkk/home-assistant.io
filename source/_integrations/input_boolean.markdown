---
title: "Input Boolean"
description: "Instructions on how to integrate the Input Boolean integration into Home Assistant."
logo: home-assistant.png
ha_category:
  - Automation
ha_qa_scale: internal
ha_release: 0.11
---

The `input_boolean` integration allows the user to define boolean values that can be controlled via the frontend and can be used within conditions of automation. This can for example be used to disable or enable certain automations.

To enable input booleans in your installation, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
input_boolean:
  notify_home:
    name: Notify when someone arrives home
    initial: off
    icon: mdi:car
```

{% configuration %}
  input_boolean:
    description: Alias for the input. Multiple entries are allowed.
    required: true
    type: map
    keys:
      name:
        description: Friendly name of the input.
        required: false
        type: string
      initial:
        description: Initial value when Home Assistant starts.
        required: false
        type: boolean
        default: false
      icon:
        description: Icon to display in front of the input element in the frontend.
        required: false
        type: icon
{% endconfiguration %}

### Restore State

This integration will automatically restore the state it had prior to Home Assistant stopping as long as your entity does **not** have a set value for `initial`. To disable this feature, set a valid value for `initial`.

## Automation Examples

Here's an example of an automation using the above `input_boolean`. This action will only occur if the switch is on.

```yaml
automation:
  alias: Arriving home
  trigger:
    platform: state
    entity_id: binary_sensor.motion_garage
    to: 'on'
  condition:
    condition: state
    entity_id: input_boolean.notify_home
    state: 'on'
  action:
    service: notify.pushbullet
    data:
      title: ""
      message: "Honey, I'm home!"
```

You can also set or change the status of an `input_boolean` by using `input_boolean.turn_on`, `input_boolean.turn_off` or `input_boolean.toggle` in your automations.

```yaml
    - service: input_boolean.turn_on
      data:
        entity_id: input_boolean.notify_home
```
