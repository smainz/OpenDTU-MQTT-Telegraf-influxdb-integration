# MQTT Topics

The base topic, as configured in the web GUI is prepended to all follwing topics.
`[dtuserial]` will be replaced with the serial number of the DTU.
`[serial]` will be replaced with the serial number of the inverter.
`[1-N]` willbe replaced with the number of the output phase (for AC values ) or input string (for DC values).

## General topics

| Topic(new)           | Topic (old)                             | R / W | Description                                          | Value / Unit               |
| -------------------- | --------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| [dtuserial]/ip       | dtu/ip                                  | R     | IP address of OpenDTU                                | IP address                 |
| [dtuserial]/hostname | dtu/hostname                            | R     | Current hostname of the dtu (as set in web GUI)      | string                     |
| [dtuserial]/rssi     | dtu/rssi                                | R     | WiFi network quality                                 | db value                   |
| [dtuserial]/status   | dtu/status                              | R     | Indicates whether OpenDTU network is reachable       | online /  offline          |
| [dtuserial]/uptime   | dtu/uptime                              | R     | Time in seconds since startup                        | seconds                    |
| [dtuserial]/git_hash |                                         | R     | git hash of the current software version             | string                     |

### Mapping to InfluxDB

```
measurement = dtu
dtu = [dtuserial]
field = hostname | ip | rssi | status | uptime
value = xxxx
```

## Totals (AC and DC) per DTU

Enabled inverter means, that only inverters with "Poll inverter data" enabled are considered.

| Topic(new)                 | Topic (old)                       | R / W | Description                                          | Value / Unit               |
| -------------------------- | --------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| [dtuserial]/ac/power       | ac/power                          | R     | Sum of AC active power of all enabled inverters      | W                          |
| [dtuserial]/ac/yieldtotal  | ac/yieldtotal                     | R     | Sum of energy converted to AC since reset watt hours of all enabled inverters | Kilo watt hours (kWh) |
| [dtuserial]/ac/yieldday    | ac/yieldday                       | R     | Sum of energy converted to AC per day in watt hours of all enabled inverters  | Watt hours (Wh)
| [dtuserial]/ac/is_valid    | ac/is_valid                       | R     | Indicator whether all enabled inverters where reachable | 0 or 1                  |
| [dtuserial]/dc/power       | dc/power                          | R     | Sum of DC power of all enabled inverters             | Watt (W)                   |
| [dtuserial]/dc/irradiation | dc/irradiation                    | R     | Produced power of all enabled inverter stripes with defined irradiation settings divided by sum of all enabled inverters irradiation | % |
| [dtuserial]/dc/is_valid    | dc/is_valid                       | R     | Indicator whether all enabled inverters where reachable | 0 or 1                  |

### Mapping to InfluxDB

```
measurement = dtu_totals
dtu = [dtuserial]
channel = AC | DC
field  = power | yieldtotal | yieldday | is_valid | irradiation
value=xxxx
```

## Inverter specific topics

| Topic(new)                                               | Topic (old)                             | R / W | Description                                          | Value / Unit               |
| -------------------------------------------------------- | --------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| [dtuserial]/inverters/[serial]/name                      | [serial]/name                           | R     | Name of the inverter as configured in web GUI        | String                           |
| [dtuserial]/inverters/[serial]/devinfo/bootloaderversion | [serial]/device/bootloaderversion       | R     | Bootloader version of the inverter                   |                            |
| [dtuserial]/inverters/[serial]/devinfo/fwbuildversion    | [serial]/device/fwbuildversion          | R     | Firmware version of the inverter                     |                            |
| [dtuserial]/inverters/[serial]/devinfo/fwbuilddatetime   | [serial]/device/fwbuilddatetime         | R     | Build date / time of inverter firmware               |                            |
| [dtuserial]/inverters/[serial]/devinfo/hwpartnumber      | [serial]/device/hwpartnumber            | R     | Hardware part number of the inverter                 |                            |
| [dtuserial]/inverters/[serial]/devinfo/hwversion         | [serial]/device/hwversion               | R     | Hardware version of the inverter                     |                            |
| [dtuserial]/inverters/[serial]/devinfo/maxpower          |                                         | R     |                                                      |                            |
|                                                          | [serial]/status/reachable               | R     | Indicates whether the inverter is reachable          | 0 or 1                     |
| [dtuserial]/inverters/[serial]/producing                 | [serial]/status/producing               | R     | Indicates whether the inverter is producing AC power | 0 or 1                     |
| [dtuserial]/inverters/[serial]/last_update               | [serial]/status/last_update             | R     | Unix timestamp of last inverter statistics udpate    | seconds since JAN 01 1970 (UTC) |
| [dtuserial]/inverters/[serial]/temperature               | [serial]/0/temperature                  | R     | Temperature of inverter in degree celsius            | Degree Celsius (°C)        |
| [dtuserial]/inverters/[serial]/limit_relative            | [serial]/status/limit_relative          | R     | Current applied production limit of the inverter     | % of total possible output |
| [dtuserial]/inverters/[serial]/limit_absolute            | [serial]/status/limit_absolute          | R     | Current applied production limit of the inverter     | Watt (W)                   |
| [dtuserial]/inverters/[serial]/uptime                    |                                         | R     |                                                      |                            |
| [dtuserial]/inverters/[serial]/status                    |                                         | R     |                                                      |                            |

### Mapping to InfluxDB

```
measurement = inverter
dtu = [dtuserial]
inverter = [serial]
field = name | bootloaderversion | fwbuildversion | fwbuilddatetime | hwpartnumber | hwversion | maxpower | producing | last_update | temperature | limit_relative | limit_absolute | uptime | status
value = xxxx
```

## AC channel total per inverter

| Topic(new)                                   | Topic (old)                  | R / W | Description                                          | Value / Unit               |
| -------------------------------------------- | ---------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| [dtuserial]/inverters/[serial]/ac/current    | [serial]/0/current           | R     | AC current in ampere                                 | Ampere (A)                 |
| [dtuserial]/inverters/[serial]/ac/frequency  | [serial]/0/frequency         | R     | AC frequency in hertz                                | Hertz (Hz)                 |
| [dtuserial]/inverters/[serial]/ac/yieldday   | [serial]/0/yieldday          | R     | Energy converted to AC per day in watt hours         | Watt hours (Wh)            |
| [dtuserial]/inverters/[serial]/ac/yieldday | [serial]/0/yieldtotal        | R     | Energy converted to AC since reset watt hours        | Kilo watt hours (kWh)      |

### Mapping to InfluxDB

```
measurement = inverter_totals
dtu = [dtuserial]
inverter = [serial]
channel = AC
field = current | frequency | yieldday | yieldday
value = xxxx
```

## AC channel per inverter and phase

| Topic(new)                                               | Topic (old)                  | R / W | Description                                     | Value / Unit               |
| -------------------------------------------------------- | ---------------------------- | ----- | ----------------------------------------------- | -------------------------- |
| [dtuserial]/inverters/[serial]/phase/[1-N]/current       | [serial]/0/current           | R     | AC current in ampere                            | Ampere (A)                 |
|                                                          | [serial]/0/efficiency        | R     | Ratio AC Power over DC Power in percent         | %                          |
|                                                          | [serial]/0/frequency         | R     | AC frequency in hertz                           | Hertz (Hz)                 |
| [dtuserial]/inverters/[serial]/phase/[1-N]/power         | [serial]/0/power             | R     | AC active power in watts                        | Watt (W)                   |
| [dtuserial]/inverters/[serial]/phase/[1-N]/powerfactor   | [serial]/0/powerfactor       | R     | Power factor in percent                         | %                          |
| [dtuserial]/inverters/[serial]/phase/[1-N]/reactivepower | [serial]/0/reactivepower     | R     | AC reactive power in VAr                        | VAr                        |
| [dtuserial]/inverters/[serial]/phase/[1-N]/voltage       | [serial]/0/voltage           | R     | AC voltage in volt                              | Volt (V)                   |
| [dtuserial]/inverters/[serial]/phase/[1-N]/yieldday      | [serial]/0/yieldday          | R     | Energy converted to AC per day in watt hours    | Watt hours (Wh)            |
| [dtuserial]/inverters/[serial]/phase/[1-N]/yieldtotal    | [serial]/0/yieldtotal        | R     | Energy converted to AC since reset watt hours   | Kilo watt hours (kWh)      |
| [dtuserial]/inverters/[serial]/phase/[1-N]/name          |                              | R     |                                                 |

### Mapping to InfluxDB

```
measurement  = inverter_details
dtu = [dtuserial]
inverter = [serial]
channel = AC
phase = 1-N
field = current | power | powerfactor | reactivepower | voltage | yieldday | yieldday
value=xxxx
```

## DC input channel topics per inverter

Totals:

| Topic(new)                                    | Topic (old)                  | R / W | Description            | Value / Unit 
| --------------------------------------------- | ---------------------------- | ----- | ---------------------- | ------------------------ |
| [dtuserial]/inverters/[serial]/dc/power       | [serial]/0/powerdc           | R     | DC power in watts      | Watt (W)                 |
| [dtuserial]/inverters/[serial]/dc/current     |                              | R     | DC current             | Ampere (A)               |
| [dtuserial]/inverters/[serial]/dc/irradiation |                              | R     |                        |                          |

### Mapping to InfluxDB

```
measurement = inverter_totals
dtu = [dtuserial]
inverter = [serial]
channel = DC
field = power | current | irradiation
value = xxxx
```

## DC input channel topics per inverter per string

| Topic(new)                                              | Topic (old)                    | R / W | Description                                          | Value / Unit               |
| ------------------------------------------------------- | ------------------------------ | ----- | ---------------------------------------------------- | -------------------------- |
| [dtuserial]/inverters/[serial]/string/[1-N]/current     | [serial]/[1-4]/current         | R     | DC current of specific input in ampere               | Ampere (A)                 |
| [dtuserial]/inverters/[serial]/string/[1-N]/name        | [serial]/[1-4]/name            | R     | Name of the DC input channel as configured in web GUI|                            |
| [dtuserial]/inverters/[serial]/string/[1-N]/irradiation | [serial]/[1-4]/irradiation     | R     | Ratio DC Power over set maximum power (in web GUI)   | %                          |
| [dtuserial]/inverters/[serial]/string/[1-N]/power       | [serial]/[1-4]/power           | R     | DC power of specific input in watt                   | Watt (W)                   |
| [dtuserial]/inverters/[serial]/string/[1-N]/voltage     | [serial]/[1-4]/voltage         | R     | DC voltage of specific input in volt                 | Volt (V)                   |
| [dtuserial]/inverters/[serial]/string/[1-N]/yieldday    | [serial]/[1-4]/yieldday        | R     | Energy converted to AC per day on specific input     | Watt hours (Wh)            |
| [dtuserial]/inverters/[serial]/string/[1-N]/yieldtotal  | [serial]/[1-4]/yieldtotal      | R     | Energy converted to AC since reset on specific input | Kilo watt hours (kWh)      |
| [dtuserial]/inverters/[serial]/string/[1-N]/maxpower    |                                | R     |                                                      |                            |

### Mapping to InfluxDB

```
measurement = inverter_details
dtu = [dtuserial]
inverter = [serial]
channel = DC
string = 1-N
field = current | name | irridiation | power | voltage | yieldday | yieldtotal
value = xxxx
```

### Topics to set values

| Topic(new)                                                      | Topic                                     | R / W | Description                                          | Value / Unit               |
| --------------------------------------------------------------- | ----------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| [dtuserial]/inverters/[serial]/cmd/limit_persistent_relative    | [serial]/cmd/limit_persistent_relative    | W     | Set the inverter limit as a percentage of total production capability. The  value will survive the night without power. The updated value will show up in the web GUI and limit_relative topic immediatly. | %                          |
| [dtuserial]/inverters/[serial]/cmd/limit_persistent_absolute    | [serial]/cmd/limit_persistent_absolute    | W     | Set the inverter limit as a absolute value. The  value will survive the night without power. The updated value will set immediatly within the inverter but show up in the web GUI and limit_relative topic after around 4 minutes. If you are using a already known inverter (known Hardware ID), the updated value will show up within a few seconds. | Watt (W)                   |
| [dtuserial]/inverters/[serial]/cmd/limit_nonpersistent_relative | [serial]/cmd/limit_nonpersistent_relative | W     | Set the inverter limit as a percentage of total production capability. The  value will reset to the last persistent value at night without power. The updated value will show up in the web GUI and limit_relative topic immediatly. The value must be published non-retained, otherwise it will be ignored! | %                          |
| [dtuserial]/inverters/[serial]/cmd/limit_nonpersistent_absolute | [serial]/cmd/limit_nonpersistent_absolute | W     | Set the inverter limit as a absolute value. The  value will reset to the last persistent value at night without power. The updated value will set immediatly within the inverter but show up in the web GUI and limit_relative topic after around 4 minutes. If you are using a already known inverter (known Hardware ID), the updated value will show up within a few seconds. The value must be published non-retained, otherwise it will be ignored! | Watt (W)                   |
| [dtuserial]/inverters/[serial]/cmd/power                        | [serial]/cmd/power                        | W     | Turn the inverter on (1) or off (0)                 | 0 or 1                     |
| [dtuserial]/inverters/[serial]/cmd/restart                      | [serial]/cmd/restart                      | W     | Restarts the inverters (also resets YieldDay)       | 1                          |

Not mapped to InfluxDB
