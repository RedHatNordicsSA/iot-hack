# Zigbee2MQTT instructions

We have few devices what to connect over zigbee radio connection. The list contains the info page and sample status or command messages from mqtt bus.


# Devices

## Ikea Trådfri

* [IR motion sensor E1745](https://www.zigbee2mqtt.io/devices/E1525_E1745.html)
    example output:
    ```
    zigbee2mqtt/0x680ae2fffe3487ce {"requested_brightness_level":254,"requested_brightness_percent":100,"linkquality":115,"occupancy":true}
    zigbee2mqtt/0x680ae2fffe3487ce {"requested_brightness_level":254,"requested_brightness_percent":100,"linkquality":115,"occupancy":false}
    ```
* [Light bulb LED1545G12](https://www.zigbee2mqtt.io/devices/LED1545G12.html)
    example commands
    ```
    mosquitto_pub -t zigbee2mqtt/0x90fd9ffffede0f32/set -m '{ "state": "ON"}'
    mosquitto_pub -t zigbee2mqtt/0x90fd9ffffede0f32/set -m '{ "state": "OFF"}'
    mosquitto_pub -t zigbee2mqtt/0x90fd9ffffede0f32/set -m '{ "brightness": "10"}' # 0-255
    ```
    example status
    ```
    zigbee2mqtt/0x90fd9ffffede0f32 {"state":"OFF","brightness":125,"update_available":true}
    ```

## Xiaomi

* [Temperature/Humidity sensor](https://www.zigbee2mqtt.io/devices/WSDCGQ11LM.html)
    example output:
    ```
    zigbee2mqtt/0x00158d00040862ed {"battery":100,"voltage":3005,"temperature":20.71,"humidity":22.98,"pressure":986.4,"linkquality":160}
    ```
