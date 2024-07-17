# ESPHome configurations

In contrast to whats seems to be true with most ESPHome users, I do not use Home Assistant and try to keep these configurations platform agnostic. Using MQTT as primary communication bus.

## MiFlora Flower Sensors

Compatible with the **hhccjcy01** based Xiaomi Flora Care sensors. 

The `ble-proxy.yaml` config is based on the [GL.iNet GL-S10](https://www.gl-inet.com/products/gl-s10/ "GL-Inet GL-S10") ESP32 device with ethernet. The initial plan was to run [OpenMQTTGateway](https://github.com/1technophile/OpenMQTTGateway "OpenMQTTGateway") on this device. Unfortunately they do not support (see [#1903](https://github.com/1technophile/OpenMQTTGateway/issues/1903 "#1903")) this device, and their own product, which is very similar, was out of stock. Therefore I was looking for a elegant way to support multiple sensors in ESPHome.

To add sensors, you can easily add another package entry (with a unique name)

```yaml
  miflora-X: !include
    file: sensors/hhccjcy01.yaml
    vars:
      flower_care_mac: "AA:BB:CC:DD:EE:FF"
      flower_care_id_prefix: dracaena_fragrans
      flower_care_friendly_name: Dracaena Fragrans
```

## Watermeter

This config is inspired by the [huizebruin/s0tool](https://github.com/huizebruin/s0tool/ "huizebruin/s0tool") tool. It does not require an Home Assistant connection to maintain the current meter count, and depends on MQTT for its operation.

The hardware is based on a **LJ18A3-8-Z** inductive proximity sensor. Combined with a **Wemos D1 Mini v4.0.0**, it fits nicely in the following case on a Sensus 620 meter: [Enclosure for Sensus 620 watermeter by CaptNemo](https://www.printables.com/en/model/370548-enclosure-for-esp32-and-inductieve-proximity-senso)

You can set the current meter count by sending it to the `watermeter/set_water_usage` topic. Example:
```
~ # mosquitto_pub -t watermeter/set_water_usage -m 151.792
```
