# snips-volume
## About
This script is an addon for https://snips.ai

Snips currently doesn't allow you to change the audio volume of the TTS engine.

This script tries to solve that by listening on the MQTT topics `hermes/sound/setvolume` and `hermes/sound/getvolume`.\
Volume is changed through Alsa.

If a get request is detected, the script will reply on `hermes/sound/volume` with the current volume.\
Usage in an environment with multiple satellites is supported through the siteId.

## Dependencies
 - `paho` - python MQTT
 - `toml` - library to read the snips configuration file
 - `alsaaudio`- library to talk to the alsa audio services

## Installation
copy `snips-volume.py` to _/opt/snips-volume/_\
copy `snips-volume.service` to _/etc/systemd/system/_

```
sudo chmod +x /opt/snips-volume/snips-volume.py
sudo systemctl enable snips-volume.service
sudo systemctl start snips-volume.service
```

## Message Format

### In Messages

#### hermes/sound/setvolume
JSON Payload: 

| Key     | Value                                    |
| ------- | ---------------------------------------- |
| siteId  | Site where the volume should be changed  |
| volume  | The new volume level                     |

#### hermes/sound/getvolume
JSON Payload: 

| Key     | Value                                                   |
| ------- | ------------------------------------------------------- |
| siteId  | Site where you want to request the current volume level |

### Out Messages

#### hermes/sound/volume
JSON Payload: 

| Key     | Value                           |
| ------- | ------------------------------- |
| siteId  | Site for the volume information |
| volume  | The current volume level        |

## Troubleshooting
- If *alsaaudio* fails to install, libasound2-dev might have to be installed through apt-get
- When the script doesn't seem to work, stop the service and run in with\
`sudo python3 /opt/snips-volume/snips-volume.py` from the terminal.\
If the script fails, you will see the errors this way.
