| Supported targets | ESP32 | ESP32-C2 | ESP32-C3 | ESP32-C6 | ESP32-H2 | ESP32-S2 | ESP32-S3 |
| ----------------- | ----- | -------- | -------- | -------- | -------- | -------- | -------- |

# ESP-MQTT SSL example with PSK verification

This example connects to a local broker configured to PSK authentication.

## How to use the example

### Hardware required

This example can be executed on any ESP32 board, the only required interface is WiFi to connect to a MQTT
broker with preconfigured PSK verification method.

#### Mosquitto broker settings

```
$ sudo dnf install -y mosquitto
$ sudo systemctl enable mosquitto.service
$ sudo systemctl start mosquitto.service
$ systemctl status mosquitto.service
$ sudo nano /etc/mosquitto/mosquitto.conf
$ sudo cat /etc/mosquitto/mosquitto.conf
listener 8883
psk_hint hint
psk_file /etc/mosquitto/psk
allow_anonymous true
$ sudo nano /etc/mosquitto/psk
$ sudo cat /etc/mosquitto/psk
hint:BAD123
$ sudo systemctl restart mosquitto.service
```

Note that the PSK file has to contain pairs of hints and keys. Keys are stored as text hexadecimal values in PSK file, while the example code stores key as plain binary
as required by MQTT API. (See the example source for details: `"BAD123" -> 0xBA, 0xD1, 0x23`)

### Configure the project

- Run `idf.py menuconfig`
- Configure Wi-Fi or Ethernet under "Example Connection Configuration" menu.
- Add your broker IP to `app_main.c`:
```
#define EXAMPLE_BROKER_URI "mqtts://192.168.2.157"
```

### Build and flash

For more information refer to the root [README.md](../README.md).

```
$ idf.py build
$ idf.py flash
```

## Example output

```
I (5647) MQTTS_EXAMPLE: [APP] Free memory: 234728 bytes
I (5657) MQTTS_EXAMPLE: Other event id:7
I (5667) main_task: Returned from app_main()
I (6117) MQTTS_EXAMPLE: MQTT_EVENT_CONNECTED
I (6117) MQTTS_EXAMPLE: sent subscribe successful, msg_id=32194
I (6127) MQTTS_EXAMPLE: sent publish successful, msg_id=0
I (6137) MQTTS_EXAMPLE: MQTT_EVENT_SUBSCRIBED, msg_id=32194
I (6137) MQTTS_EXAMPLE: MQTT_EVENT_DATA
TOPIC=esp32/sensors/temp
DATA=data
I (755127) MQTTS_EXAMPLE: MQTT_EVENT_DATA
TOPIC=esp32/sensors/temp
DATA=other_data
```
```
$ mosquitto_pub -h localhost -p 8883 --psk BAD123 --psk-identity hint -t "esp32/sensors/temp" -m "other_data" -q 1
```
```
$ mosquitto_sub -h localhost -p 8883 --psk BAD123 --psk-identity hint -t "esp32/sensors/temp" -v

esp32/sensors/temp data
```
