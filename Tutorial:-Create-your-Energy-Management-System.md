**THIS PAGE IS WORK IN PROGRESS AND A REQUIREMENTS DOCUMENT AT THIS STAGE**

It describes how you can build your own energy management system. Some features are described with as **WISH**, meaning they are not implemented yet.

# Introduction

OpenRemote can be used as an Energy Management System (EMS) for your own microgrid. The EMS can schedule the power setpoints of energy consuming devices and storage devices, taking into account your renewable energy producers, agile tariffs from your energy provider and required charge schedules for your electric vehicles. Using an optimisation routine, the scheduling is targeting lowest costs or lowest carbon exhaust.

In this electricity example, we will connect a series of consumers: an energy meter, chargers, a series of vehicles; a series of producers: solar panels and a windturbine; and a static battery. Secondly, we are adding the forecasting services for both consumption and production and connecting the agile supplier tariffs. Finally, the optimisation service will control the setpoint power of the battery, as well as the vehicles. 

<kbd>![Overview EMS](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20overview.jpg)</kbd>
_Figure 1. Overview of Energy Management System and all elements._

# Electricity assets and required agents

We assume you have set up the latest version of OpenRemote. If not, check out the [Quick Start](https://github.com/openremote/openremote/blob/master/README.md) first. Also have a look at how to [use the Manager UI](User-Guide%3A-Manager-UI).

If you navigate to 'Assets' and add an asset, using the '+' button. You will see a list of asset and agent types. A subset of these asset types are intended to set up your own EMS. They include the relevant attributes and configuration setting such that the optimisation service can recognise and manage them.

We'll be setting up a series of these assets, step-by-step.

## Optimisation

An optimisation asset represents the optimisation service. It will take into account all assets which are added as children of this asset. So first add an Optimisation asset at the root of your asset tree on the left and give it a name. We'll [explain later](#optimisation-1) how the optimisation actually works, after adding all the other assets.

## Electricity Producer

We are first adding the (renewable) energy production, by adding a PV solar Asset (Your solar) and a Wind turbine Asset (Your wind). These assets include a series of standard attributes as shown in figure 2 and 3.

<kbd>![Solar Asset](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Solar%20Asset.png)</kbd>
_Figure 2. The PV Solar asset (Your solar) with the respective attributes._

<kbd>![Wind Asset](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Wind%20turbine%20Asset.png)</kbd>
_Figure 3. The Wind Turbine Asset (Your wind) with the respective attributes._

### PV Solar Asset

To configure and connect your PV Solar asset to your own solar system you can use the Agents. Most importantly you need to connect the actual power (the attribute called power) and the energy meter value (the attribute called energy export total). In this example we connect a solar system from Solar Edge. 
First add an HTTP Agent and configure the Base URI and the Request Headers to connect to your Solar Edge system (see figure 4). 

<kbd>![SolarEdge HTTP Agent](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Solar%20Edge%20Agent.png)</kbd>
_Figure 4. The HTTP Agent, configured to connect to Solar Edge._

Secondly, select your solar asset, select 'Modify' (on the upper right of the asset page) and connect the power attribute of the PV Solar Asset to this Agent. To do this uncollapse the power attribute, add the configuration item 'agent link', and add the required parameters (see figure 5). Also add the configuration items 'Rule state' (so you can use the attribute in rules) and 'Read only' (so you can't accidentally write a value in the 'VIEW' mode). Note the +/- sign convention: we use + for consumption and - for production. So in this case, as we are looking at power production, the power values should be negative. 
In a similar manner you can connect the 'Energy export total' attribute to the energy meter value.   

<kbd>![Solar Agent link](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Solar%20Power%20Agent%20Link.png)</kbd>
_Figure 5. The connection of the power attribute in your solar asset, using 'Agent link'._

#### Forecast service

You also need the forecasted power for your solar system. Therefore you first ad another Agent 'Forecast.solar', which connects to the [Forecast.solar](https://forecast.solar/) service. In this agent you have to add the 'apikey' of this service. You can register for a [public free account](https://forecast.solar/#accounts) to get this key.  
Once the Forecast.solar Agent is connected, you have to fill in the following attributes in your solar asset (see also Figure 2):
* Panel azimuth (South orientation or a perfect East-West orientation for an East-West panel orientation equals 0 degrees)
* Panel orientation ('SOUTH' means all panels are oriented in the same South direction; 'EAST WEST' means an East-West configuration)
* Panel pitch (horizontal is 0 degrees)
* Power export max (the installed peak capacity in kW)
* Location of the solar asset. You can do this in the 'MODIFY' mode by opening the map modal next to the location attribute and setting the location by double clicking on the map.

Now add an additional configuration item 'Include forecast solar service' to the 'power' attribute of your solar asset (see Figure 5). Once saved the forecast service is running. To see it in action you can go to the 'Insights' page and select the power attribute in a chart. The dotted line will represent the forecasted data. 

Optionally, if you also want to store the forecasted power for comparing it with the actual values, you can also add the configuration item 'Include forecast solar service' to the attribute 'Power Forecast', as well as the configuration items 'Has predicted data points', 'Read only', 'Rule state', and 'Store data points'.

### Wind Turbine

Your wind turbine asset is configured in a similar manner. First connect the 'Power' (note the negative value) and 'Energy Total Export' to an Agent which connects to your wind turbine system. See the PV Solar Asset example above, or check the different [Agent Protocol options in the Wiki](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview). 

#### Forecast service

Similar as for the Solar asset, you need a forecast for the wind power as well. For the wind power forecast to work we require the Agent 'OpenWeather' to be connected, it connects to the [OpenWeather](https://openweathermap.org/api) service. In this agent you have to add the 'appid' of this service as well as set the location attribute. You can register for a [public free account](https://openweathermap.org/appid) to get this key.  
Once the OpenWeather Agent is connected, you have to fill in the following attributes in your wind turbine asset (see also Figure 3):
* Power export max (also called nominal or rated power in kW at the nominal or rated speed)
* Wind speed max (also called cut-off speed, the maximum speed at which a turbine is normally operating; above it will be turned down; in m/s)
* Wind speed min (also called cut-in speed, the minimum speed required to operate the wind turbine; in m/s)
* Wind speed reference (also called nominal or rated speed; in m/s)
* Location of the wind turbine asset doesn't need to be set in the wind turbine asset. It's set in the 'OpenWeather' Agent.

Now add an additional configuration item 'Include forecast wind service' to the 'power' attribute of your wind turbine asset (similar as for your solar asset, see Figure 5). Once saved the forecast service is running. To see it in action you can go to the 'Insights' page and select the power attribute in a chart. The dotted line will represent the forecasted data. 

Optionally, if you also want to store the forecasted power for comparing it with the actual values, you can also add the configuration item 'Include forecast wind service' to the attribute 'Power Forecast', as well as the configuration items 'Has predicted data points', 'Read only', 'Rule state', and 'Store data points'.

## Electricity Consumer

To connect electricity consuming devices. You can connect several energy meters by using the 'Electricity Consumer Asset'. Similar as before you should connect the 'Power' and 'Energy Total Import' to your meters by using any of the existing [Agent Protocol options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview).

#### Forecast

For the electricity consuming devices you also need the forecasted power. You can enable this by adding the configuration item 'Include time series forecast'. This service will take a weighted moving average, based on your preferences: the 'Repeat period': day -or- week, as well as the 'Time window (number of periods)': the number of periods of the selected 'Repeat period'. When you select 'Repeat period = day' and 'Time window (number of periods) = 21', the forecast service will take into account the historical attribute values at the same time for all days of the previous three weeks.

## Electricity Battery
main attributes 

#### Agents
eg. simulator

#### Forecast
simulator

## Electricity Charger
main attributes: powerMax, energyImportTotal

#### Agents
eg. OCPP, Unicorn

## Electric Vehicle
main attributes: power and setpoint power

#### Agents
brands or aggregators (eg. Masternaut)

## Energy supplier
Using agile tariffs

#### Agents
EPEX tariffs

# Optimisation
enabling

financial weighting and interval size

carbon and financial saving


