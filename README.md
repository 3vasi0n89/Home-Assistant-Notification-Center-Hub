# Home Assistant Notification Center

## About

This project will aim to centralize TTS and Device Notifications between all [Home Assistant Notify Components](https://www.home-assistant.io/components/notify/).

Many thanks to [Lentron](https://github.com/Lentron) & [CCostan](https://github.com/CCOSTAN) for laying down some of the groundwork for this!

I started using the [Janet project](https://community.home-assistant.io/t/janet-the-good-place/38904/2) by Lentron but it didn't meet all of my needs and the creator of the project seemed to have stopped making the "**skills**" so I decided to take it a step further and create this repo to keep the new project more documented and kept up.

I update my own [Home Assistant Config](https://github.com/3vasi0n89/home-assistant-config-files) on at least a weekly basis and will do my best to keep this project as up to date as possible.

It would be nice for this project to get improved by the community so that we can make something awesome and easy for everyone.
With that being said there are a few ground rules for automations.

1. No profanity or sexual language (Let's keep this family friendly)
2. All automations must be labeled with the starting prefix notification_

There are a few things that still need to be tweaked in this project and I am looking for some help with some of the coding if anyone would like to chip in.

**More updates to come!**

## Notifications Currently Included

* Start Up
* Shut down
* Reboot
* Update Available
* New Device Connected
* Last Message
* Low Battery Warning

## Additional Abilities

* Send Notifications to multiple notify components using only 1 service call
* Actionable Notifications (Currently only supported by iOS but Android will be in the near future)
* Send Images
* Send Sounds

## Bonus Tip

If you have a bunch of motion sensors in your house then you can use the bonus automation that will set the output of the TTS to the device in the room that the last tripped motion detector was set off.


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

## Notification_Center_Config.YAML Configuration

The packages have been split up so that I should be able to update the notification_center.yaml without messing up anybodys settings.

input_select.notification_media_player_google and alexa **Must** contain the **Friendly Names** of your Media Players.

## Set Up Notify Groups

Follow this page [here](https://www.home-assistant.io/components/notify.group/) for instructions on how to set up notify groups in Home Assistant for all of your text notifications.

# How To Use

![service_call][logo]

[logo]: https://github.com/3vasi0n89/home-assistant-config-files/blob/master/www/images/screenshot4.jpg "Service Call"

**Service call for a standard notification would look like this:** (Note. This call will only send a **TTS** notification.)
```
- service: script.speech_engine
  data:
    call_greeting: 1
    call_welcome_home: 1
    call_inside_weather: 1
```

**Additionally if you want to send both a TTS and Text notification then call like this:** (Note. The notify variable is used for text notifications. The value for the variable must match the service part of your notify.entity_id, e.g. mine is notify.ios_joes_iphone so I use ios_joes_iphone)
```
- service: script.speech_engine
  data:
  call_greeting: 1
  call_welcome_home: 1
  call_inside_weather: 1
  notify: "ios_joes_iphone"
  person: "Joe"
```

**Service call for an iOS notification with an image attachment would look like this:**

![service_call2][logo2]

[logo2]: https://github.com/3vasi0n89/home-assistant-config-files/blob/master/www/images/screentshot1.jpg "Service Call2"

![service_call3][logo3]

[logo3]: https://github.com/3vasi0n89/home-assistant-config-files/blob/master/www/images/screenshot2.jpg "Service Call3"
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

**Service call to use the person, mode, or other extra parameters using sensor states need to use *data_template* and would look like this:**
```
- service: script.speech_engine
  data:
    call_greeting: 1
    call_welcome_home: 1
    call_inside_weather: 1
    notify: "ios_family"
  data_template:
    person: >-
      {{ trigger.event.data.name }}
```

**Current available calls:** (Note. Call values have to be a value of: **1** to trigger them. The person, mode, and location parameters are just here to show you which calls can use them)
```
call_announcement: 1
call_start_up: 1
call_greeting: 1 (person)
call_mode_enabled: 1 (mode)
call_mode_disabled: 1 (mode)
call_guest_mode_on: 1
call_guest_mode_off: 1
call_alarm_clock: 1 (person)
call_welcome_home: 1 (person)
call_welcome_home2: 1 (person)
call_work_arrive: 1 (person)
call_work_left: 1 (person)
call_okay: 1
call_location_inquiry: 1 (person) (location)
call_rain_warning: 1
call_leaving: 1
call_inside_weather: 1
call_outside_weather: 1
call_dark_outside: 1
call_dark_inside: 1
call_lock_check: 1 (person)
call_door_open: 1 (person)
call_garage_check: 1
call_window_check: 1
call_light_check: 1
call_responsibilities: 1
call_printer_finished: 1
call_hot_inside: 1
call_doorbell: 1
call_shut_down: 1
call_update: 1
call_inspirational_quote: 1
call_new_device: 1
call_bed_time: 1
call_closing: 1
```

**Current available extra parameters:** (Note. Parameter values are **String Values** or **Sensor States**)
```
“person”: “Michael”
"mode": "Vacation"
“CustomMessage”: “This is an extra string.”
“Event”: “some string”
“WeatherRain”: {{ sensor.example.state + unit_of_measurement }}
```
