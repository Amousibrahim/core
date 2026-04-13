I've been running into the same Tibber integration issues in Home Assistant, so I tried a small workaround that seems to help a bit. It just checks if the sensor goes unavailable or unknown for a few minutes, and then reloads the integration. I also added a cooldown so it doesn't keep spamming reloads if the API is down for a while.

```yaml
alias: "System: Tibber Auto-Recovery"
description: >
  Reload Tibber if sensor becomes unavailable or unknown for 5 minutes.
mode: single
trigger:
  - platform: state
    entity_id: sensor.electricity_price
    to: "unavailable"
    for:
      minutes: 5
  - platform: state
    entity_id: sensor.electricity_price
    to: "unknown"
    for:
      minutes: 5
action:
  - service: homeassistant.reload_config_entry
    target:
      entity_id: sensor.electricity_price
  - delay:
      hours: 1
```

Not a real fix obviously, just something that makes it a bit more stable until the root issue is sorted.
