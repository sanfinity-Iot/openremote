Connect to a [Velbus](https://www.velbus.eu/) network using either of the following implementations:

* Direct RS232/Serial using `VMBRSUSB` or `VMB1USB` - [Velbus Serial Agent](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/velbus/VelbusSerialAgent.java)
* TCP/IP using [VelServ](https://github.com/jeroends/velserv) or similar - [Velbus TCP Agent](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/velbus/VelbusTcpAgent.java)


## Agent configuration
The following describes the Velbus agent supported configuration attributes:

### TCP
| Attribute | Description | Value type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `host` | TCP server hostname or IP address | [Hostname or IP address](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueType.java#L153) | Y |
| `port` | TCP server port | [Port number](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueType.java#L148) | Y |

### Serial
| Attribute | Description | Value type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `serialPort` | Serial port | Text | Y |


### TCP & Serial
| Attribute | Description | Value type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `timeInjectionInterval` | Time injection interval (s) - as Velbus doesn't have RTC or support daylight saving time so this should be set to about 1hr | [Positive Integer](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueType.java#L83) | Y |

## Agent link
For attributes linked to a Velbus agent, the following describes the supported agent link fields which are in addition to the standard [Agent Link](./User-Guide:-Agent-Overview#agent-links) fields:

| Field | Description | Value type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `deviceAddress` | The Velbus module address to link to | Integer (1-254) | Y |
| `deviceValueLink` | The Velbus module value to link to | Text (see below) | Y |

***

### Hardware compatibility table

'x' indicates a colour option that does not affect the capability of the module.

Modules are queried for their module type during initialisation, non-compatible modules will **not work in any way**.

<table>
  <thead>
<tr>
<th>Module Type</th>
    <th>Firmware</th>
    <th>Date tested</th> 
    <th>Notes</th>
  </tr>

  </thead>
  <tbody>
  
  <tr>
    <td>VMBGPOx</td>
    <td>1436</td>
    <td>Aug 2015</td>
    <td>Don't send too many commands too quickly, leave reasonable delays between commands. </td>
  </tr>



  <tr>
    <td>VMBGPODx</td>
    <td>1436</td>
    <td>Aug 2015</td>
    <td>Don't send too many commands too quickly, leave reasonable delays between commands. </td> 
  </tr>


<tr>
    <td>VMBGP4x</td>
    <td></td>
    <td>Oct 2016</td>
    <td>Functions as expected</td> 
  </tr>

<tr>
    <td>VMBGP4PIRx</td>
    <td></td>
    <td>Oct 2016</td>
    <td>Functions as expected</td> 
  </tr>

<tr>
    <td>VMBGP2x</td>
    <td>1349</td>
    <td>Aug 2015</td>
    <td>All commands function as expected</td> 
  </tr>

<tr>
    <td>VMBGP1x</td>
    <td>1347</td>
    <td>Aug 2015</td>
    <td>All commands function as expected</td> 
  </tr>

<tr>
    <td>VMBGPOTCx</td>
    <td></td>
    <td>Aug 2015</td>
    <td>Not available for testing but all commands should function as expected</td> 
  </tr>

<tr>
    <td>VMB8PBU</td>
    <td>1350</td>
    <td>July 2015</td>
    <td>All commands function as expected</td> 
  </tr>

<tr>
    <td>VMB1TS</td>
    <td></td>
    <td>Jan 2017</td>
    <td>All commands function as expected</td> 
  </tr>

<tr>
    <td>VMB7IN</td>
    <td>1439</td>
    <td>July 2015</td>
    <td>All commands function as expected, Custom sensors seem to work better for energy values</td> 
  </tr>

<tr>
    <td>VMBPIROx</td>
    <td></td>
    <td>Oct 2016</td>
    <td>All commands function as expected</td> 
  </tr>


<tr>
    <td>VMBPIRC</td>
    <td></td>
    <td>Oct 2016</td>
    <td>Functions as expected</td> 
  </tr>

<tr>
    <td>VMBPIRM</td>
    <td></td>
    <td>Oct 2016</td>
    <td>All commands function as expected</td> 
  </tr>

<tr>
    <td>VMB2BLE</td>
    <td>1325</td>
    <td>July 2015</td>
    <td>All commands function as expected</td> 
  </tr>

<tr>
    <td>VMB1BLS</td>
    <td></td>
    <td>Aug 2015</td>
    <td>Not available for testing but assured they perform as single channel version of VMB2BLE</td> 
  </tr>

<tr>
    <td>VMBDMI</td>
    <td>1201</td>
    <td>July 2015</td>
    <td>All commands function as expected,  except ON command, use Dimmer_level:100 instead, <However, this causes an Off & Memory toggle when used with a switch and slider combo. (Both methods were used on the same panel during testing) Switch will work perfectly on first load, but once a slider has been moved, then the switch (with commands OFF and Dimmer_Level:100) will only toggle between Off and Memory</td> 
  </tr>

<tr>
    <td>VMBDMI-R</td>
    <td></td>
    <td>Aug 2015</td>
    <td>All commands function as expected</td> 
  </tr>


<tr>
    <td>VMB4DC</td>
    <td>1327</td>
    <td>July 2015</td>
    <td>All commands function as expected,  except ON command, use Dimmer_level:100 instead, <However, this causes an Off & Memory toggle when used with a switch and slider combo. (Both methods were used on the same panel during testing) Switch will work perfectly on first load, but once a slider has been moved, then the switch (with commands OFF and Dimmer_Level:100) will only toggle between Off and Memory</td> 
  </tr>



<tr>
<td>VMB4RYLD</td>
<td>1327</td>
<td>July 2015</td>
<td>All commands function as expected</td>
</tr>

<tr>
<td>VMB4RYNO</td>
<td>1327</td>
<td>July 2015</td>
<td>All commands function as expected</td>
</tr>


<tr>
<td>VMB1RYNO</td>
<td>1325</td>
<td>July 2015</td>
<td>All commands function as expected</td>
</tr>


<tr>
<td>VMB1RYNOS</td>
<td>1422</td>
<td>July 2015</td>
<td>All commands function as expected</td>
</tr>

<tr>
<td>VMBMETEO</td>
<td></td>
<td>Jan 2017</td>
<td>All commands function as expected</td>
</tr>

</tbody>

</table>

### Device value link
The following describes the supported values for the `deviceValueLink` Agent Link field, supported values are grouped by device function, please refer to VelbusLink or Velbus documentation to understand what functions each device type supports.


#### Relay Channel
Parameter `X` must be replaced with a channel number (1-5).

| Value | Description | Read | Write |
| ------------- | ------------- | ------------- | ------------- |
| `CHX` | Channel state | Text (`OFF`, `ON`, `INTERMITTENT`) | Text (`OFF`, `ON`, `INTERMITTENT`) |
| `CHX_SETTING` | Channel setting | Text (`NORMAL`, `INHIBITED`, `FORCED`, `LOCKED`) | Text (`NORMAL`, `INHIBITED`, `FORCED`, `LOCKED`) |
| `CHX_LOCKED` | Channel lock (forced off) state | Boolean | Boolean |
| `CHX_INHIBITED` | Channel inhibit state | Boolean | Boolean |
| `CHX_FORCED` | Channel forced on state | Boolean | Boolean |
| `CHX_LED` | Channel LED state | Text (`OFF`, `ON`, `SLOW`, `FAST`, `VERYFAST`) |  |
| `CHX_ON` | Set channel on for specified duration (s) |  | Integer (0 = Off) |
| `CHX_INTERMITTENT` | Set channel intermittent for specified duration (s) |  | Integer (0 = Cancel) |
| `CHX_LOCK` | Set channel locked for specified duration (s) |  | Integer (0 = Cancel) |
| `CHX_FORCE_OFF` | Set channel locked for specified duration (s) |  | Integer (0 = Cancel) |
| `CHX_FORCE_ON` | Set channel forced on for specified duration (s) |  | Integer (0 = Cancel) |
| `CHX_INHIBIT` | Set channel inhibit for specified duration (s) |  | Integer (0 = Cancel) |

#### Input Channel (Push Button)
Parameter `X` must be replaced with a channel number (1-32) depending on module type and configuration.

| Value | Description | Read | Write |
| ------------- | ------------- | ------------- | ------------- |
| `CHX` | Channel state | Text (`RELEASED`, `PRESSED`, `LONG_PRESSED`) | Text (`RELEASED`, `PRESSED`, `LONG_PRESSED`) |
| `CHX_LED` | Channel LED state | Text (`OFF`, `ON`, `SLOW`, `FAST`, `VERYFAST`) | Text (`OFF`, `ON`, `SLOW`, `FAST`, `VERYFAST`) |
| `CHX_LOCKED` | Channel lock state | Boolean | Boolean |
| `CHX_ENABLED` | Channel enabled state | Boolean | Boolean |
| `CHX_INVERTED` | Channel inverted state | Boolean | Boolean |
| `CHX_LOCK` | Set channel locked for specified duration (s) |  | Integer (0 = Cancel) |


#### Temperature Sensor
Some modules support temperature but don't have thermostat (e.g. `VMBMETEO`)

| Value | Description | Read | Write |
| ------------- | ------------- | ------------- | ------------- |
| `TEMP_CURRENT` | Current temperature (°C) | Decimal |  |
| `TEMP_MIN` | Minimum temperature (°C) | Decimal |  |
| `TEMP_MAX` | Maximum temperature (°C) | Decimal |  |


#### Thermostat

| Value | Description | Read | Write |
| ------------- | ------------- | ------------- | ------------- |
| `HEATER` | Heater state | Text (`RELEASED`, `PRESSED`) |  |
| `COOLER` | Heater state | Text (`RELEASED`, `PRESSED`) |  |
| `PUMP` | Heater state | Text (`RELEASED`, `PRESSED`) |  |
| `BOOST` | Heater state | Text (`RELEASED`, `PRESSED`) |  |
| `TEMP_ALARM1` | Alarm 1 state | Text (`RELEASED`, `PRESSED`) |  |
| `TEMP_ALARM2` | Alarm 2 state | Text (`RELEASED`, `PRESSED`) |  |
| `TEMP_ALARM3` | Alarm 3 state | Text (`RELEASED`, `PRESSED`) |  |
| `TEMP_ALARM4` | Alarm 4 state | Text (`RELEASED`, `PRESSED`) |  |
| `TEMP_STATE` | Thermostat state | Text (`DISABLED`, `MANUAL`, `TIMER`, `NORMAL`) | Text (`DISABLED`, `MANUAL`, `TIMER`, `NORMAL`) |
| `TEMP_MODE` | Thermostat mode | Text (`COOL_COMFORT`, `COOL_DAY`, `COOL_NIGHT`, `COOL_SAFE`, `HEAT_COMFORT`, `HEAT_DAY`, `HEAT_NIGHT`, `HEAT_SAFE`) | Text (`COOL_COMFORT`, `COOL_DAY`, `COOL_NIGHT`, `COOL_SAFE`, `HEAT_COMFORT`, `HEAT_DAY`, `HEAT_NIGHT`, `HEAT_SAFE`) |
| `TEMP_MODE_COOL_COMFORT_MINS` | Set mode to cool comfort for specified duration (mins) |  | Integer |
| `TEMP_MODE_COOL_DAY_MINS` | Set mode to cool day for specified duration (mins) |  | Integer |
| `TEMP_MODE_COOL_NIGHT_MINS` | Set mode to cool night for specified duration (mins) |  | Integer |
| `TEMP_MODE_COOL_SAFE_MINS` | Set mode to cool safe for specified duration (mins) |  | Integer |
| `TEMP_MODE_HEAT_COMFORT_MINS` | Set mode to heat comfort for specified duration (mins) |  | Integer |
| `TEMP_MODE_HEAT_DAY_MINS` | Set mode to heat day for specified duration (mins) |  | Integer |
| `TEMP_MODE_HEAT_NIGHT_MINS` | Set mode to heat night for specified duration (mins) |  | Integer |
| `TEMP_MODE_COOL_SAFE_MINS` | Set mode to heat safe for specified duration (mins) |  | Integer |
| `TEMP_TARGET_CURRENT` | Set current target temperature (°C) |  | Decimal |
| `TEMP_TARGET_COOL_COMFORT` | Set cool comfort target temperature (°C) |  | Decimal |
| `TEMP_TARGET_COOL_DAY` | Set cool day target temperature (°C) |  | Decimal |
| `TEMP_TARGET_COOL_NIGHT` | Set cool night target temperature (°C) |  | Decimal |
| `TEMP_TARGET_COOL_SAFE` | Set cool safe target temperature (°C) |  | Decimal |
| `TEMP_TARGET_CURRENT` | Set current target temperature (°C) |  | Decimal |
| `TEMP_TARGET_HEAT_COMFORT` | Set heat comfort target temperature (°C) |  | Decimal |
| `TEMP_TARGET_HEAT_DAY` | Set heat day target temperature (°C) |  | Decimal |
| `TEMP_TARGET_HEAT_NIGHT` | Set heat night target temperature (°C) |  | Decimal |
| `TEMP_TARGET_HEAT_SAFE` | Set heat safe target temperature (°C) |  | Decimal |

#### Blind Channel
Parameter `X` must be replaced with a channel number (1-2) depending on module type.

| Value | Description | Read | Write |
| ------------- | ------------- | ------------- | ------------- |
| `CHX` | Channel state | Text (`UP`, `DOWN`, `HALT`) | Text (`UP`, `DOWN`, `HALT`) |
| `CHX_SETTING` | Channel setting | Text (`NORMAL`, `INHIBITED`, `INHIBITED_DOWN`, `INHIBITED_UP`, `FORCED_DOWN`, `FORCED_UP`, `LOCKED`) | Text (`NORMAL`, `INHIBITED`, `INHIBITED_DOWN`, `INHIBITED_UP`, `FORCED_DOWN`, `FORCED_UP`, `LOCKED`) |
| `CHX_LED_UP` | Channel LED state | Text (`OFF`, `ON`, `SLOW`, `FAST`, `VERYFAST`) | Text (`OFF`, `ON`, `SLOW`, `FAST`, `VERYFAST`) |
| `CHX_LED_DOWN` | Channel LED state | Text (`OFF`, `ON`, `SLOW`, `FAST`, `VERYFAST`) | Text (`OFF`, `ON`, `SLOW`, `FAST`, `VERYFAST`) |
| `CHX_INHIBITED` | Channel inhibited state | Boolean | Boolean |
| `CHX_INHIBITED_UP` | Channel inhibited up state | Boolean | Boolean |
| `CHX_INHIBITED_DOWN` | Channel inhibited down state | Boolean | Boolean |
| `CHX_FORCED_UP` | Channel forced up state | Boolean | Boolean |
| `CHX_FORCED_DOWN` | Channel forced down state | Boolean | Boolean |
| `CHX_LOCKED` | Channel lock state | Boolean | Boolean |
| `CHX_POSITION` | Channel position (%) | Integer | Integer |
| `CHX_UP` | Set channel up for specified duration (s) |  | Integer (0 = Cancel, -1 = Indefinitely) |
| `CHX_DOWN` | Set channel down for specified duration (s) |  | Integer (0 = Cancel, -1 = Indefinitely) |
| `CHX_INHIBIT` | Set channel inhibit for specified duration (s) |  | Integer (0 = Cancel, -1 = Indefinitely) |
| `CHX_INHIBIT_UP` | Set channel inhibit up for specified duration (s) |  | Integer (0 = Cancel, -1 = Indefinitely) |
| `CHX_INHIBIT_DOWN` | Set channel inhibit down for specified duration (s) |  | Integer (0 = Cancel, -1 = Indefinitely) |
| `CHX_FORCE_UP` | Set channel force up for specified duration (s) |  | Integer (0 = Cancel, -1 = Indefinitely) |
| `CHX_FORCE_DOWN` | Set channel force down for specified duration (s) |  | Integer (0 = Cancel, -1 = Indefinitely) |
| `CHX_LOCK` | Set channel locked for specified duration (s) |  | Integer (0 = Cancel) |

#### Counter

#### OLED



#### Analog Input
Parameter `X` must be replaced with a channel number (1-4).

#### Analog Output
Parameter `X` must be replaced with a channel number (1-4).







### Relay Read Commands (x4) 


<table>
  <thead>
<tr>
<th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Returns</th>
<th>Notes</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>STATUS</td>
    <td>Channel Number (e.g. 1)</td>
    <td>Read channel status</td>
    <td>ON, OFF, N/A</td>
    <td></td>
  </tr>


<tr>
<td>SETTING_STATUS</td>
<td>Channel number (e.g. 1)</td>
<td>Read channel setting</td>
<td>NORMAL, INHIBITED, FORCED, DISABLED, N/A</td>
<td></td>
</tr>

<tr>
<td>LED_STATUS</td>
<td>Channel number (e.g. 1)</td>
<td>Read channel LED status</td>
<td>OFF, ON, SLOW, FAST, VERYFAST, N/A</td>
<td>Fast indicated a short press timer is active, Slow indicates a Long press timer is active</td>
</tr>

<tr>
<td>LOCK_STATUS</td>
<td>Channel number (e.g. 1)</td>
<td>Read channel lock status</td>
<td>ON, OFF, N/A</td>
<td></td>
</tr>
</tbody>

</table>


### Relay Write Commands (x4)

<table>
  <thead>
<tr>
<th>Command</th>
    <th>Value</th>
    <th>Description</th> 
  </tr>

  </thead>
  <tbody>


<tr>
<td>ON</td>
<td>Channel number and optional duration in seconds separated by ':' (e.g. 1:60)</td>
<td>Turn channel on for the specified period of time in seconds (if no time specified then it is turned on permanently)</td>
</tr>

<tr>
<td>OFF</td>
<td>Channel number (e.g. 1)</td>
<td>Turn channel off</td> 
</tr>


<tr>
<td>LOCK</td>
<td>Channel number and optional lock duration in seconds separated by ':' (e.g. 1:3600)</td>
<td>Turn channel off and prevent it from being switched on for the specified period of time, if no duration is provided then it is permanently disabled</td>
</tr>

<tr>
<td>UNLOCK</td>
<td>Channel number (e.g. 1)</td>
<td>Enable channel</td>
</tr>


</tbody>

</table>


***


## Input Devices


> This group of commands covers all types of Velbus input device.
> Not all commands work with all input devices


### Input Read Commands (x15)



<table>
  <thead>
<tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Returns</th>
    <th>Notes</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>STATUS</td>
    <td>Channel number or temperature channel name (e.g. 1 or HEATER)</td>
    <td>Read channel status; possible channel numbers are 1-32 (depending on module) and possible temperature channel names are (HEATER, BOOST, PUMP, COOLER, ALARM1, ALARM2, ALARM3, ALARM4)</td>
    <td>RELEASED, PRESSED, LONG_PRESSED, N/A</td>
    <td>Use Sensor Type "Custom" as the return is a text string, use the returned text as a values in a custom sensor against sensor state names "on" & "off" to use with a switch. All input modules support channel status,  but only Glass Panels (VMBGPxxxx) support temperature related status queries</td>
 </tr>

 <tr>
    <td>LOCK_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel lock status, 'On' meaning the channel is locked, 'Off' meaning the channel is unlocked.</td>
    <td>ON, OFF, N/A</td>
    <td></td>
</tr>


<tr>
    <td>LED_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel LED status</td>
    <td>OFF, ON, SLOW, FAST, VERYFAST, N/A</td>
    <td>This is a read command that queries a particular section of the module's memory and is dependant on certain conditions. IF the feedback LED  in question is working in 'normal' mode (See VelbusLink LED Feedback tab in Configure Module), this read command will initially return N/A, simply because the LED in question (within the module memory map) has not been sent a direct command, but the actual LED will be reflecting the state of any 'actions' it has been configured with.</td>
</tr>

</tbody>

</table>



> LED status 
> Once OpenRemote has sent a direct LED command, it will return that status, the actual LED will also continue to change state according to it's VelbusLink configuration, but the returned state will always be whatever the last direct LED command was that was sent to it from OpenRemote.
> 
> 
> This could be useful if you wanted to use a 'Spare' button within a Velbus module to store a status, for use elsewhere in your Openremote project.
> 
> IF the LED in question is in Feedback mode, the actual LED will completely ignore any state command that OpenRemote sends it, however the module will return the state that the last command sent to that LED.
> Changing the LED mode to Monitoring will enable it to respond to commands from OpenRemote.




<table>
  <thead>


  <tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Returns</th>
    <th>Notes</th>
    <th>Compatible Modules</th>
  </tr>

  </thead>

  <tbody>

  <tr>
    <td>ALARM_STATUS</td>
    <td>Alarm number (e.g. 1)</td>
    <td>Read wake and bedtime alarm status (Enabled or Disabled)</td>
    <td>ON, OFF, N/A</td>
    <td></td>
    <td></td>
</tr>


<tr>
    <td>ALARM_TYPE_STATUS</td>
    <td>Alarm number ( 1 or 2)</td>
    <td>Read alarm type status (e.g. master (global) or local)</td>
    <td>LOCAL, MASTER, N/A</td>
    <td>This read command is only for display / reminder purposes to show that any changes to an alarm time will affect all modules in the Velbus Network (or segment if multiple networks are configured) that share the master / global alarm.</td>
    <td></td>
</tr>

<tr>
    <td>ALARM_TIME_STATUS</td>
    <td>Alarm type (Options are:  1:WAKE, 1:BED, 2:WAKE & 2:BED)</td>
    <td>Read specified alarm set time</td>
    <td>string date in HH:mm format (e.g. 20:00)</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>SUNRISE_STATUS</td>
    <td></td>
    <td>Read sunrise status</td>
    <td>ON, OFF, N/A</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>SUNSET_STATUS</td>
    <td></td>
    <td>Read sunset status</td>
    <td>ON, OFF, N/A</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>PROGRAM_STATUS</td>
    <td></td>
    <td>Read active program</td>
    <td>NONE(0), SUMMER(1), WINTER(2), HOLIDAY(3), N/A</td>
    <td></td>
    <td></td>
</tr>


<tr>
    <td>TEMP_STATUS</td>
    <td></td>
    <td>Read current temperature without units</td>
    <td>double with resolution of 0.1oC (e.g. 19.3) , N/A</td>
    <td>0.1° resolution only displayed when using Custom sensors, Level sensors will display whole values </td>
    <td>Only for use with modules with temperature capabilities.</td>

</tr>


<tr>
    <td>TEMP_TARGET_STATUS</td>
    <td>Blank for current target temperature or optionally the temperature mode to read (HEAT_COMFORT, HEAT_DAY, HEAT_NIGHT, HEAT_SAFE, COOL_SAFE, COOL_NIGHT, COOL_DAY & COOL_COMFORT)</td>
    <td>Read either current or specified temperature mode target temperature without units</td>
    <td>double with resolution of 0.5oC (e.g. 19.5) , N/A</td>
    <td></td>
    <td>Only for modules with thermostat functionality</td>
</tr>


<tr>
    <td>TEMP_STATE_STATUS</td>
    <td></td>
    <td>Read current state of the temperature module; normal = temperature program steps execute when specified, disabled = thermostat disabled, manual = program steps ignored and module is permanently in current mode, timer = program steps ignored until current sleep timer expires</td>
    <td>NORMAL. DISABLED, MANUAL, TIMER, N/A</td>
    <td></td>
    <td>Only for modules with thermostat functionality</td>
</tr>


</tbody>

</table>

> 
> TEMP_STATE_STATUS is affected by:-
> 
> 'TEMP_MODE', when used with a time value sets this status to 'Timer'
> 
> 'LOCK' Command, changes the status to 'Disabled'
> 
> 'UNLOCK' command releases the 'Disabled' mode, but will not release from a 'Timer' state, to release a 'Timer' state you must send the thermostat another 'TEMP_MODE' command, without a time value.
> 
> Setting a Temp_Mode with -1 as time value will but the thermostat into Manual Mode.
> 
> Curiously, when the Thermostat is in the 'Disabled' state, it also sets TEMP_MODE_STATUS to 'Heat_Safe' mode.
> 


<table>
  <thead>
<tr>
<th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Returns</th>
    <th>Notes</th>
    <th>Compatible Modules</th>
  </tr>

  </thead>
  <tbody>


  <tr>
    <td>TEMP_MODE_STATUS</td>
    <td></td>
    <td>Read active temperature mode</td>
    <td>COOL_COMFORT, COOL_DAY, COOL_NIGHT, COOL_SAFE, HEAT_COMFORT, HEAT_DAY, HEAT_NIGHT, HEAT_SAFE, N/A</td>
    <td></td>
    <td>Only for modules with thermostat functionality</td>
</tr>

<tr>
    <td>COUNTER_STATUS</td>
    <td>Channel number or function (wind, rain or light) : optional multipler (e.g. 1 or 2:0.536 or wind:0.12345)</td>
    <td>Read counter channel cumulative value</td>
    <td>Counter value to 2DP (e.g. 1234.56)</td>
    <td></td>
    <td>Only relevant to counter or energy monitors and METEO</td>
</tr>


<tr>
    <td>COUNTER_INSTANT_STATUS</td>
    <td>Channel number or function (wind, rain or light) : optional multipler (e.g. 1 or 2:0.536 or wind:0.12345</td>
    <td>Read counter channel instantaneous value</td>
    <td>Instantaneous value to 2DP (e.g. 1234.56)</td>
    <td></td>
    <td>Only relevant to counter or energy monitors and METEO</td>
 </tr>

</tbody>

</table>



### Input Write Commands (x16)




<table>
  <thead>
<tr>
<th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Notes</th>
    <th>Compatible Modules</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>TIME_UPDATE</td>
    <td></td>
    <td>Sends a global time & date update to the Velbus network.</td>
    <td>This is the only command that does not require a Module base address (but it does require a random address between 1 and 253), as it is a global command, it also requires a Network ID. To update all networks you are required to create a macro to fire separate commands into each Velbus network ID.</td>
    <td>All modules that hold the current time</td>
</tr>

<tr>
    <td>PRESS</td>
    <td>Channel number (e.g. 1 to 8, or 32 for GPO)</td>
    <td>Simulate a push button press on the specified channel</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>LONG_PRESS</td>
    <td>Channel number (e.g. 1 to 8, or 32 for GPO)</td>
    <td>Simulate a push button long press on the specified channel</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>RELEASE</td>
    <td>Channel number (e.g. 1 to 8, or 32 for GPO)</td>
    <td>Simulate a push button release on the specified channel</td>
    <td></td>
    <td></td>
</tr>

</tbody>

</table>


> The three commands above are needed separately in order to fully support the Velbus protocol. So in order for your Velbus system to perform as expected you shouldn't just use a Press command within a user interface if you are trying to mimic a button press. You should create a macro for each button you want to mimic, for standard presses, or long presses. The macro should contain the button press command, a delay (25ms is normal for a button press) and a release command. There is a button widget operational change currently being planned (July 2015) which will facilitate a command when pressed and a command when released. This will mean that 'press and hold' dimming will be possible with Long_Press & Release commands, or momentary relay closures with the Press & Release combination.


<table>
  <thead>
<tr>
<th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Returns</th>
    <th>Notes</th>
    <th>Compatible Modules</th>
  </tr>

  </thead>
  <tbody>


  <tr>
    <td>LOCK</td>
    <td>Channel number and optional lock duration in seconds separated by ':' (e.g. 1:3600)</td>
    <td>Lock channel and prevent it from being activated for the specified period of time, if no duration is provided then it is permanently disabled; if channel is max channel number + 1 (e.g. 9 or 33) then the thermostat is locked (if supported)</td>
    <td></td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>UNLOCK</td>
    <td>Channel number (e.g. 1 to 8, or 32 for GPO)</td>
    <td>Unlock channel; if channel is max channel number + 1 (e.g. 9 or 33) then the thermostat is unlocked (if supported)</td>
    <td></td>
    <td></td>
    <td></td>
</tr>


<tr>
    <td>LED</td>
    <td>Channel number and LED state separated by ':' (e.g. 1:OFF)</td>
    <td>Set the LED state for the specified channel; possible LED states are OFF, ON, SLOW, FAST, VERYFAST</td>
    <td>This command only affects the LED state if the LED mode has been set to MONITOR, otherwise the set state is only stored in the module's memory.</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>TEMP_TARGET</td>
    <td>COOL_SAFE, COOL_NIGHT, COOL_DAY, COOL_ COMFORT, HEAT_SAFE, HEAT_NIGHT, HEAT_DAY, HEAT_COMFORT,CURRENT, separated by ':' (e.g. COOL_COMFORT:20.0) [IF USED WITH A SLIDER THEN THE NEW TARGET TEMPERATURE WILL BE TAKEN FROM THE SLIDER VALUE]</td>
    <td>Sets the target temperature for the specified temperature mode; possible temperature modes are all values returned from TEMP_MODE_STATUS and additionally CURRENT which changes the current target temperature.</td>
     <td>Changes to mode target temperatures are permanent except for CURRENT which is only valid until the active temperature mode changes.<td>
    <td>Only modules with thermostat functionality </td>
</tr>

<tr>
    <td>TEMP_TARGET_RELATIVE</td>
    <td>Temperature mode to set and new relative target temperature separated by ':' (e.g. COOL_COMFORT:-0.5)</td>
    <td>Sets the target temperature relative to the current value for the specified temperature mode; possible temperature modes are all values returned from TEMP_MODE_STATUS and additionally CURRENT which changes the current target temperature.</td>
    <td>Changes to mode target temperatures are permanent except for CURRENT which is only valid until the active temperature mode changes<td>
    <td>Only modules with thermostat functionality </td>
</tr>

<tr>
    <td>TEMP_MODE</td>
    <td>Temperature mode to set and optional duration to set it for in minutes separated by ':' (e.g. COOL_SAFE:20160)</td>
    <td>Sets the current temperature mode, if no duration is specified then the change is only effective until the next time a program step changes the mode, if a duration value of -1 is used then the change is permanent</td>
    <td>Program steps will be disabled i.e. manual mode, otherwise program steps to change mode will be ignored for the duration specified ; possible temperature modes are all values returned from TEMP_MODE_STATUS</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>ALARM</td>
    <td>Alarm number, alarm type and new status separated by ':' (e.g. 1:MASTER:ON)</td>
    <td>Set alarm status; possible alarm type values are MASTER or LOCAL; possible alarm number values are 1 or 2; possible status values are ON or OFF</td>
     <td>Master alarm changes can be sent to any module as it will affect all modules on the bus that are in master alarm mode</td>
    <td></td>
    <td></td>
</tr>

<tr>
    <td>ALARM_TIME</td>
    <td>Alarm number, alarm time type and new time in minutes past midnight separated by ':' (e.g.1:WAKE:340)</td>
    <td>Sets the time for the specified alarm; possible values for alarm time type are WAKE or BED; possible values for alarm number are 1 or 2; possible values for time are 0-1439</td>
     <td>Master alarm changes can be sent to any module as it will affect all modules on the bus that are in master alarm mode.   This is a dynamic command that will accept values from a slider</td>
    <td></td>
    <td></td>
</tr>

</tbody>

</table>

> 
> The ALARM_TIME command will alter either the LOCAL or MASTER alarm time depending on which of the two modes this alarm is set to (it is not possible to specify the alarm type as there is no way of reading the time for both alarm types, as the time is the same, with only a bit set for master or local)
> 
> Sliders can inject values into this command.
> A range sensor is required with a minimum value of 0 and a maximum value of 1439
> 
> Likewise, reduced ranges can be set to limit the user's options.
> 

Formula for alarm times in minutes:

Given that 24 hours have 1439 unique minutes, the easiest way to work out the alarm time in minutes is to use a simple spreadsheet that works out;
(Hour x 60 minutes) + minutes past the hour


<table>
  <thead>
<tr>
    <th></th>
    <th>A</th>
    <th>B</th> 
    <th>Expected Result</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>1</td>
    <td>Whole minutes in a day</td>
    <td>1440</td>
    <td>1440</td> 
  </tr>


  <tr>
    <td>2</td>
    <td>Required alarm time</td>
    <td>07:30</td>
    <td>07:30</td> 
  </tr>

  <tr>
    <td>3</td>
    <td>Alarm time in minutes</td>
    <td>=B1*B2</td>
    <td>450</td> 
  </tr>


  <tr>
    <td>4</td>
    <td>Check hour</td>
    <td>=Rounddown(B3/60,0)</td>
    <td>7</td>
  </tr>


  <tr>
    <td>5</td>
    <td>Check minutes</td>
    <td>=B3-(B4*60)</td>
    <td>30</td> 
  </tr>



</tbody>

</table>


<table>
  <thead>
<tr>
    <th>24 Hr time</th>
    <th>Time in mins</th>
    <th></th> 
    <th>24 Hr time</th>
    <th>Time in mins</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>00:00</td>
    <td>0</td>
    <td> </td>
    <td>20:47</td>
    <td>1247</td> 
  </tr>

  <tr>
    <td>06:30</td>
    <td>390</td>
    <td> </td>
    <td>21:15</td>
    <td>1275</td> 
  </tr>

  <tr>
    <td>08:30</td>
    <td>510</td>
    <td> </td>
    <td>22:30</td>
    <td>1350</td> 
  </tr>


  <tr>
    <td>19:00</td>
    <td>1140</td>
    <td> </td>
    <td>23:59</td>
    <td>1439</td> 
  </tr>

</tbody>

</table>

<table>
  <thead>
<tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Notes</th>
    <th>Compatible modules</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>ALARM_TIME_RELATIVE</td>
    <td>Alarm number, alarm time type and change of time in minutes relative to current time separated by ':' (e.g. 1:wake:-5 or 2 : bed : 120)</td>
<td>Sets the time for the specified alarm relative to the current value; possible values for the alarm number are 1 or 2; possible values for alarm time type are 'wake' or 'bed'</td>
    <td>This will alter the LOCAL or MASTER alarm time depending on which of the two modes this alarm is set to (it is not possible to set different times for Local or Master as the time is the same for each alarm, with just a bit set for master or local)</td>
<td></td>
</tr>


</tbody>

</table>


> It is best practise to use VelbusLink to define Master alarm groups, then only incorporate alarm time changes for a single Master alarm module in an OpenRemote controller.



> Dynamic time setting can be achieved by creating a grid which displays the alarm time in the middle and a selection of incrementing and decrementing ALARM_TIME_RELATIVE commands. I.E. +1 min, -5 min, +60 min, +180 min (3 hours)



<table>
  <thead>
<tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Notes</th>
    <th>Compatible modules</th>
  </tr>

  </thead>
  <tbody>


  <tr>
    <td>MEMO_TEXT</td>
    <td>String to use as memo text (maximum of 63 characters) and optional duration to display in seconds separated by ':' (e.g. TEST MESSAGE:5</td>
    <td>Sets the memo text on an OLED touch panel for the specified period of time; if no duration is provided then a default of 5 seconds is used. This is a dynamic command that well accept a text string from a Drule, so can be used to display information on an OLED panel from other sources</td>
    <td>If you must send a string longer than 63 characters, you will have to create a macro to send each memo_text command. Each single MEMO_TEXT string will scroll until the next command is sent, or the Velbus protocol in OpenRemote times out and sends the clear command, which is a completely blank MEMO_TEXT string</td>
    <td>Only OLED variants of glass panels. VMBGPOxxx</td>
</tr>

<tr>
    <td>LANGUAGE</td>
    <td>ENGLISH, FRANCAIS, NEDERLANDS, ESPANOL, DEUTSCH, ITALIANO</td>
    <td>Sets the system display language of the OLED panel (Used when the panel is in programming mode, normally done by pressing the lower middle divider when it is displaying either, Clock, Temperatures or Energy</td>
    <td></td>
    <td>Only OLED variants of glass panels. VMBGPOxxx</td>
</tr>

<tr>
    <td>PROGRAM</td>
    <td>NONE or 0, SUMMER or 1, WINTER or 2, HOLIDAY or 3</td>
    <td>Used to set the active program group within each module</td>
    <td>Remember that program groups are unique to each module. However, you can create a macro if you want all (or selections of) modules to change</td>
    <td></td>
</tr>

</tbody>

</table>


***


## Dimmers and 0-10v controller

Tested with;

VMBDMI

VMB4DC

VMBDMI-R


### Dimmer Read Commands (x5)


<table>
  <thead>
<tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Returns</th>
    <th>Notes</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel status</td>
    <td>ON, OFF, N/A</td>
    <td>Not all dimmer modules support this command</td>
</tr>


<tr>
    <td>DIMMER_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read dimmer channel level value</td>
    <td>Dimmer value percentage (0-100)</td>
    <td></td>
</tr>


<tr>
    <td>SETTING_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel setting</td>
    <td>NORMAL, INHIBITED, FORCED, DISABLED, N/A</td>
    <td></td>
</tr>


<tr>
    <td>LED_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel LED status</td>
    <td>OFF, ON, SLOW, FAST, VERYFAST, N/A</td>
    <td>OFF & ON are self explanatory, SLOW means that there is a timer in play, FAST means that the dimmer is Dimming. The LED will return ON at any stage if it is not changing the dim level or OFF.</td>
</tr>

<tr>
    <td>LOCK_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel lock status</td>
    <td>ON, OFF, N/A</td>
    <td></td>
</tr>


</tbody>

</table>

### Dimmer Write Commands (x6)


<table>
  <thead>
<tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th> 
    <th>Notes</th>
  </tr>

  </thead>
  <tbody>



<tr>
    <td>OFF</td>
    <td>Channel number (e.g. 1)</td>
    <td>Turn channel off</td>
    <td></td>
</tr>

<tr>
    <td>DIMMER_LEVEL</td>
    <td>Channel number, dim level to set as percentage (0-100) and optional duration in seconds to reach this level separated by ':' (e.g. 1:50:5) [IF USED WITH A SLIDER THEN THE DIM LEVEL WILL BE TAKEN FROM THE SLIDER VALUE]</td>
    <td>Set channel dim level to the specified value in the specified time in seconds (if no time specified then a default of 1 second is used)</td>
    <td>Try to avoid using this command with a button as it may start to behave like a memory On function (we are looking into why. July 2015)</td>
</tr>

<tr>
    <td>HALT</td>
    <td>Channel number (e.g. 1)</td>
    <td>Stop channel dimming if in progress</td>
    <td></td>
</tr>

<tr>
    <td>LOCK</td>
    <td>Channel number and optional lock duration in seconds separated by ':' (e.g. 1:3600)</td>
    <td>Turn channel off and prevent it from being switched on for the specified period of time, if no duration is provided then it is permanently disabled</td>
    <td></td>
</tr>

<tr>
    <td>UNLOCK</td>
    <td>Channel number (e.g. 1)</td>
    <td>Enable channel</td>
    <td></td>
</tr>

</tbody>

</table>


***


## Blinds

### Blinds Read Commands (x4)




<table>
  <thead>
<tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th>
	<th>Returns</th>
    <th>Notes</th>
  </tr>

  </thead>
  <tbody>

<tr>
    <td>STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read blind channel status</td>
    <td>HALT, UP, DOWN, N/A</td>
    <td></td>
</tr>


<tr>
    <td>POSITION_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read blind channel position status</td>
    <td>0-100, N/A</td>
    <td></td>
</tr>

<tr>
    <td>SETTING_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel setting</td>
    <td>NORMAL, INHIBITED, FORCED, DISABLED, N/A</td>
    <td></td>
</tr>

<tr>
    <td>LOCK_STATUS</td>
    <td>Channel number (e.g. 1)</td>
    <td>Read channel lock status</td>
    <td>ON, OFF, N/A</td>
    <td></td>
</tr>

</tbody>

</table>


### Blinds Write Commands (x6)


<table>
  <thead>
<tr>
    <th>Command</th>
    <th>Value</th>
    <th>Description</th>
    <th>Notes</th>
  </tr>

  </thead>
  <tbody>

  <tr>
    <td>BLIND_POSITION</td>
    <td>Channel number and optional position value (e.g. 1:100)  [IF USED WITH A SLIDER THEN THE POSITION WILL BE TAKEN FROM THE SLIDER VALUE]</td>
    <td>Set blind channel position, accepts value between 0-100</td>
    <td></td>
</tr>



<tr>
    <td>UP</td>
    <td>Channel number and optional duration to keep up relay active separated by ':' (e.g. 1:10)</td>
    <td>Move blind channel up for optional duration; if no time is specified then the default timeout is used</td>
    <td></td>
</tr>


<tr>
    <td>DOWN</td>
    <td>Channel number and optional duration to keep down relay active separated by ':' (e.g. 1:10)</td>
    <td>Move blind channel down for optional duration; if no time is specified then the default timeout is used</td>
    <td></td>
</tr>

<tr>
    <td>HALT</td>
    <td>Channel number (e.g. 1)</td>
    <td>Halt the specified blind channel (i.e. turn off both up and down relays)</td>
    <td></td>
</tr>

<tr>
    <td>LOCK</td>
    <td>Channel number and optional lock duration in seconds separated by ':' (e.g. 1:3600)</td>
    <td>Turn channel off and prevent it from being switched on for the specified period of time, if no duration is provided then it is permanently disabled</td>
    <td></td>
</tr>


<tr>
    <td>UNLOCK</td>
    <td>Channel number (e.g. 1)</td>
    <td>Enable channel</td>
    <td></td>
</tr>


</tbody>

</table>
