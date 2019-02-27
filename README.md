# Home Assistant Notification Center

## About

This project will aim to centralize TTS and Device Notifications between all [HA Notify Components](https://www.home-assistant.io/components/notify/).

Many thanks also to [Lentron](https://github.com/Lentron) & [CCostan](https://github.com/CCOSTAN)  for laying down a lot of the groundwork for this.

I started using the [Janet project](https://community.home-assistant.io/t/janet-the-good-place/38904/2) by Lentron but it didn't meet all of my needs and the creator of the project seemed to have stopped making skills so I decided to take it a step further and create this repo to keep the new project more documented and kept up.

I would like for this project to get improved by the community so that we can make something awesome and easy for everyone.

More info to come!

## Notifications Included

* Start Up
* Shut down
* Reboot
* Update Available
* New Device Connected
* Last Message
* Low Battery Warning

## Additional Abilities

* Change TTS Device Based On Motion Sensor States
* Send Notifications to multiple notify components using only 1 service call
* Send iOS Push Notifications
* Send Images
* Send iOS Sounds


# Configuration
## Lovelace Row Configuration

Enter this config into your **ui-lovelace.yaml** or your **raw config editor**

```
- badges: []
  cards:
    - entities:
        - entity: automation.notification_new_device_connected
        - entity: automation.notification_update_available
        - entity: automation.notification_start_up
        - entity: automation.notification_shut_down
        - entity: automation.notification_battery_alert
        - entity: automation.notification_happy_birthday
        - entity: automation.notification_left_something_open
        - entity: automation.notification_repeat_last_message
      title: Notifications
      type: entities
    - entities:
        - entity: input_boolean.speech_notifications
        - entity: input_boolean.text_notifications
        - entity: input_select.notification_media_player_google
        - entity: input_number.notification_google_volume
        - entity: input_select.notification_media_player_alexa
        - entity: input_number.notification_alexa_volume
        - entity: input_boolean.guest_mode
        - entity: input_boolean.notification_alert_mode
      title: Settings
      type: entities
  icon: 'mdi:speaker'
  path: notifications
  title: Notifications
```

## Notification Center YAML Configuration

1. input_select.notification_media_player_google and alexa **HAVE** to contain the **Friendly Names** of your Media Players.
2. Script speech_processing has to refer to your **Personal TTS Component**.
3. Script text_processing has to refer to your **Personal Notification Component**.
4. Lines that need to be edited in the notification_center.yaml to match your config are **439-446** (Hoping to get this added to the config.yaml soon)

# How To Use

**Service Call For iOS Notifications Would Look Like This**
```
- service: script.speech_engine
  data:
    notify: "ios_joes_iphone"
    call_printer_finished: 1
    title: "3D Printer Report"
    content_type: "jpeg"
    ios_category: "camera"
    camera_entity: "camera.octoprint_cam"
```
**Currently available calls/parameters:**

Call values have to be **value: 1** to trigger them:
```
"call_greeting": 1,
"call_introduction": 1,
"call_update": 1,
"call_okay": 1,
"call_inform": 1,
"call_bye": 1,
"call_shut_down": 1,
"call_new_device": 1,
"call_location_inquiry": 1
"call_rain_warning": 1
```
Parameter values are **string values** or **sensor states**
```
“person”: “Michael”
“CustomMessage”: “This is an extra string.”
“Event”: “some string”
“WeatherRain”: {{ sensor.example.state + unit_of_measurement }}
```
