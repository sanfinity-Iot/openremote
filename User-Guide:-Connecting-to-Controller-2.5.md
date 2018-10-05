Controller Protocol give us a simple way to link a OpenRemote Controller 2.5 to a OpenRemote Manager 3.0.

For the purpose of trying out OpenRemote Manager 3.0 we will first explain how you can connect to the online [Home Example](https://github.com/openremote/Documentation/wiki/Example-Home). Secondly, you can link to your existing Controller 2.5. 

TODO:
- describe correct basic auth on controller for Home Example
- what example sensors and commands to use for the Home Example

NEXT PHASE:
- create a CustomerA standard panel which is connected to existing attributes
- explain how to link these attributes to the Home Example commands/sensors

# Declaring Controller Agent
In the following example, you link to the Home Example Demo Controller 2.5 on `http://demo.openremote.com:8688/controller`. However, you can follow the same procedure to link to your own controller by using the correct controller address `http://my.controller:8688/controller` instead.

1. Login to the manager UI (`https://localhost/master` as `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left
3. Set the following:
   * Asset name: `Controller Agent`
   * Parent asset: `CustomerA -> Smart Home` and select 'OK'
   * Asset Type: `Agent`
4. Click `Create asset` at bottom of screen and then click `Edit asset` in the top right
5. Click `Edit asset` and add a new attribute:
   * Name: `controllerConfig`
   * Type: `Controller Client`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows **(do not forget to click on 'Add item' for every item)** : 
   * Controller Base URI: `http://demo.openremote.com:8688/controller`
   * HTTP basic auth username: if a basic auth is defined on the Controller 2.5
   * HTTP basic auth password: if a basic auth is defined on the Controller 2.5
7. Click `Save asset` at bottom of the screen

You now have a Controller protocol to communicate with Home Example Demo Controller 2.5 as long as you have an internet connection and an attribute will be linked.

The attribute status is DISCONNECTED until a Controller 2.5 is really available on the provided base URL.

We can now add a linked attribute in Manager to the new Agent such that we can get sensors status and execute commands.

# Configuring Attributes to use a Controller Agent
For each different case, we will consider a concrete example and how to implement them in the Manager UI.

## Getting Controller 2.5 sensor status
### Starting from an example : 
Value of a temperature sensor (named 'tempSensor') on a device name 'HomeDevice' : 
* Controller 2.5: a single command triggers the read of the value and a sensor uses that command to make the value available.

### Configuration:
1. Select the Customer A -> Smart Home asset in the asset list
2. Click `Edit asset` in top right
3. Add a new attribute:
   * Name: `homeTemp`
   * Type: `Temperature in Celcius`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * Agent protocol link: Controller Agent -> controllerConfig
   * Controller Device name: HomeDevice
   * Controller Sensor name: tempSensor

You have in `homeTemp` the temperature returned by the Controller 2.5

## Execute a Controller 2.5 command
### Starting from an example : 
Send a setpoint command 'tempSetpoint' on device name 'homeDevice'
* Controller 2.5: a single command that sends the temperature setpoint, it takes no parameter. There is no sensor associated with it.

### Configuration:
1. Select the Customer A -> Smart Home asset in the asset list
2. Click `Edit asset` in top right
3. Add a new attribute:
   * Name: `xxx`
   * Type: `xxx`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * Agent protocol link: Controller Agent -> controllerConfig
   * Controller Device name: HomeDevice
   * Controller Command name: xxx

In your Smart Home asset, you have an attribute where you can click on 'Write' button to execute the command on the linked Controller 2.5.

### More information
If there is a value into attribute value when you click on 'Write', the attribute value will be add as parameter into the command request to Controller 2.5.

## Execute a Controller 2.5 multivalue command
### Starting from an example : 
Control a fan with 3 speeds: low, mid, high on a device name 'homeDevice'
* Controller 2.5: a command to read the state of the fan and a sensor 'fanState' associated with it + commands with no parameter to turn fan off 'fanOff', set it to low speed 'fanLow', medium speed 'fanMed' or high speed 'fanHigh'

### Configuration:
1. Select the Customer A -> Smart Home asset in the asset list
2. Click `Edit asset` in top right
3. Add a new attribute:
   * Name: `fanSpeed`
   * Type: `Text`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * Agent protocol link: Controller Agent -> controllerConfig
   * Controller Device name: HomeDevice
   * Controller Sensor name: fanState
   * Controller Commands map: `{ "off": "fanOff", "low": "fanLow", "medium": "fanMed", "high": "fanHigh" }`

In your Smart Home asset, you have an attribute where you can click on 'Write' button to execute the command linked to the value of the given attribute ("off", "low", "medium" or "high").
And the polling system will keep the attribute value updated with the latest status know of the fan.

### More information
If you don't want to have a polling to get the status of the sensor, you can simply don't add the item 'Controller Sensor name'.

## Execute a Controller 2.5 switch with a Manager Toggle/Switch
### Starting from an example : 
Control a light bulb on device name 'HomeDevice'
* Controller 2.5: a command to read the state of the light, a sensor 'lightState' associated with it, a command 'onCmd' to turn light on and a command 'offCmd' to turn light off (with no parameter)

### Configuration:
1. Select the Customer A -> Smart Home asset in the asset list
2. Click `Edit asset` in top right
3. Add a new attribute:
   * Name: `lightSwitch`
   * Type: `On/Off toggle`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * Agent protocol link: Controller Agent -> controllerConfig
   * Controller Device name: HomeDevice
   * Controller Sensor name: lightState
   * Controller Commands map: `{ "true": "onCmd, "false": "offCmd" }`

In your Smart Home asset, you have a switch/toggle attribute you can check or uncheck and click on 'Write' button to execute the command linked to the value of the switch/toggle.
And the polling system will keep the attribute value updated with the latest status know of the fan.

It is the only case where you have to follow a template for commands map item. You must use true/false as attribute name for the two commands map.

## Controller connection
If the connection is lost with a Controller defined in a Agent, the connection will be checked every 5 seconds until the connection is up and running again. All the linked attributes to the disconnected agent will be put on hold until the connection is back to normal.

## Specify another device name for command execution
### Starting from an example : 
Set the volume of an amplifier defined on two different device name on a Controller 'SensorDevice' & 'CmdDevice'
* Controller 2.5:  a command to read the value of the volume and a sensor 'ampVolume' associated with it on device 'SensorDevice' and a command 'setAmpVol' to set the volume taking the level as the argument

### Configuration:
1. Select the Customer A -> Smart Home asset in the asset list
2. Click `Edit asset` in top right
3. Add a new attribute:
   * Name: `ampLevel`
   * Type: `Text`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * Agent protocol link: Controller Agent -> controllerConfig
   * Controller Device name: SensorDevice
   * Controller Sensor name: ampVolume
   * Controller Device name for command: CmdDevice
   * Controller Command name: `setAmpVol`

In your Smart Home asset, you have an attribute where the value is updated by polling request and where you can click on 'Write' button to send the command to set the volume with the value of the attribute in parameter.

'Controller Device name for command' is a way to overwrite in the same attribute the device name for command only.
