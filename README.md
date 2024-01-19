# Home Assistant Denon RS232

This is a Denon AVR / Receiver Custom Component for Home Assistant that allows you to control a receiver through RS232. This custom component should support any Denon receiver with a serial port.
As I only have my own device to test (AVR-3805), development focuses specifically on this device's features. 
This implementation is a fork of the original implementation by [bluepixel00](https://github.com/bluepixel00/HomeAssistant_Denon_RS232) and takes inspiration from other Home Assistant media player integrations.

## Setup:
1) If not present create the `custom_components` folder structure under your Home Assistant config directory:
`config/custom_components/denon232/`

2) Place `__init__.py`, `media_player.py`, `manifest.json` and `denon232_receiver.py` in the denon232 folder under the `custom_components` folder.

3) Add configuration details to configuration.yaml located in the config directory:

```
media_player:
  - platform: denon232
    serial_port: socket://IP.of.ser2net.server:portnumber
    name: Receiver
```

The `serial_port` device referenced should be changed to match what is being used in your setup. In this example a USB to serial converter was used on a remote machine and exported through ser2net.

A local USB device can also be used:

```
media_player:
  - platform: denon232
    serial_port: /dev/ttyUSB0
    name: Receiver
```


## Zones
This integration supports multiple zones. Zones 2 and 3 are automagically detected when supported and added as additional `media_player` entities.

## Play tuner preset
When the main receiver source is set to Tuner, the receiver can be set to play radio presets and frequencies through the `media_player.play_media` service.

```
service: media_player.play_media
target:
  entity_id: media_player.receiver
data:
  media_content_id: "A1"
  media_content_type: radio_preset
---
service: media_player.play_media
target:
  entity_id: media_player.receiver
data:
  media_content_id: "009220"
  media_content_type: radio_freq
```
