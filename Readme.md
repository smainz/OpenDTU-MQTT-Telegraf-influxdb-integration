# OpenDTU-MQTT-Telegraf-influxdb-integration

This is a telegraf configuration, which collects data sent by an [OpenDTU](https://github.com/tbnobody/OpenDTU) and imports it into an 
[InfluxDB]([https://www.influxdata.com/products/influxdb-overview/)(Version 2.0).

The idea is to transform the [current structure of MQTT topic](https://github.com/tbnobody/OpenDTU/blob/master/docs/MQTT_Topics.md) sent by the OpenDTUto the format which is compatible with the [future structure of the MQTT topics](https://github.com/tbnobody/OpenDTU/discussions/317#discussioncomment-4345649)

Additionally a [Grafana](https://grafana.com) dashboard is provided using the data in the InfluxDB

This is WIP!
