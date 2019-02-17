# Home Assistant Notification Center

This project will aim to centralize TTS and Device Notifications between all HA Notify Components.

Many thanks also to @lentron & @CCOSTAN  for laying down a lot of the groundwork for this.

I started using the Janet project by @lentron but it didn't meet all of my needs and the creator of the project seemed to have stopped making skills so I decided to take it a step further and create this repo to keep the new project more documented and kept up.

More info to come!

Lovelace Row Config

- badges: []
  cards:
    - entities:
        - entity: automation.notification_new_device_connected
        - entity: automation.notification_update_available
        - entity: automation.notification_start_up
        - entity: automation.notification_shut_down
        - entity: automation.notification_3d_printer_finished
        - entity: automation.notification_battery_alert
        - entity: automation.notification_hdd_temp
        - entity: automation.notification_happy_birthday
        - entity: automation.notification_go_to_bed
        - entity: automation.notification_left_something_open
        - entity: automation.notification_lights_when_sundown
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
