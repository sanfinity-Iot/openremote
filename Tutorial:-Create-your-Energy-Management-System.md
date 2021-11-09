**THIS PAGE IS WORK IN PROGRESS AND A REQUIREMENTS DOCUMENT AT THIS STAGE**

It describes how you can build your own energy management system. Some features are described with as **WISH**, meaning they are not implemented yet.

# Introduction

OpenRemote can be used as an Energy Management System (EMS) of your own microgrid. The EMS can schedule the power setpoints of energy consuming devices and storage devices, taking into account your renewable energy producers, agile tariffs from your energy provider and required charge schedules for your electric vehicles. Using an optimisation routine, the scheduling is targeting lowest costs or lowest carbon exhaust.

In this electricity example, we will connect a series of consumers: an energy meter, chargers, a series of vehicles; a series of producers: solar panels and a windturbine; and a static battery. Secondly, we are adding the forecasting services for both consumption and production and connecting the agile supplier tariffs. Finally, the optimisation service will control the setpoint power of the battery, as well as the vehicles. 

![Overview EMS](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20overview.jpg)
_Figure 1. Overview of Energy Management System and all elements._

# Electricity assets and required agents

We assume you have set up the latest version of OpenRemote. If not, check out the [Quick Start](https://github.com/openremote/openremote/blob/master/README.md) first. Also have a look at how to [use the Manager UI](User-Guide%3A-Manager-UI).

If you navigate to 'Assets' and add an asset, using the '+' button. You will see a list of asset and agent types. A subset of these asset types are intended to set up your own EMS. They include the relevant attributes and configuration setting such that the optimisation service can recognise and manage them.

We'll be setting up a series of these assets, step-by-step.

## Optimisation

An optimisation asset represents the optimisation service. It will take into account all assets which are added as children of this asset. So first add an Optimisation asset at the root of your asset tree on the left and give it a name. We'll [explain later](#optimisation-1) how the optimisation actually works, after adding all the other assets.

## Electricity Producer

We are first adding the (renewable) energy production, by adding a PV solar Asset (Your solar) and a Wind turbine Asset (Your wind). These assets include a series of standard attributes as shown in figure 2.

![Solar and Wind Asset](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20Your%20Solar%20and%20Wind%20Asset.png)
_Figure 2. The PV Solar asset (Your solar, on the left) and the Wind Turbine Asset (Your wind, on the right) with their respective attributes._

### PV Solar Asset

To configure and connect your PV Solar asset to your own solar system you can use the Agents. Most importantly you need to connect the actual power (the attribute called power) and the energy meter value (the attribute called energy export total). In this example we connect a solar system from Solar Edge. 
First add an HTTP Agent and configure the Base URI and the Request Headers to connect to your Solar Edge system (see figure 3, left). 
Secondly, select your solar asset, select 'Modify' (on the upper right of the asset page) and connect the power attribute of the PV Solar Asset to this Agent. To do this uncollapse the power attribute, add the configuration item 'agent link', and add the required parameters (see figure 3. right for the Solar Edge example). Also add the configuration items 'Rule state' (so you can use the attribute in rules) and 'Read only' (so you can't accidentally write a value in the 'VIEW' mode). Note the +/- sign convention: we use + for consumption and - for production. So in this case, as we are looking at power production, the power values should be negative. 
In a similar manner you can connect the 'Energy export total' attribute to the energy meter value.   

![SolarEdge HTTP Agent and Solar Agent link](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20Solar%20Edge%20Agent%20and%20Agent%20link.png)
_Figure 3. The HTTP Agent, configured to connect to Solar Edge (left) and the connection of the power attribute in your solar asset, using 'Agent link' (right)._

#### Forecast service

You also need the forecasted power for your solar system. Therefore you first ad another Agent 'Forecast.solar', which connects to the [Forecast.solar](https://forecast.solar/) service. In this agent you have to add the 'apikey' of this service. You can register for a [public free account](https://forecast.solar/#accounts) to get this key.  
Once the Forecast.solar Agent is connected, you have to fill in the following attributes in your solar asset (see also Figure 2):
* Panel azimuth (South orientation or a perfect East-West orientation for an East-West panel orientation equals 0 degrees)
* Panel orientation ('SOUTH' means all panels are oriented in the same South direction; 'EAST WEST' means an East-West configuration)
* Panel pitch (horizontal is 0 degrees)
* Power export max (the installed peak capacity in kW)
* Location of the solar asset. You can do this in the 'MODIFY' model by opening the map modal next to the location attribute and setting the location by double clicking on the map.
Now add an additional configuration item 'Include forecast solar service' to the 'power' attribute of your solar asset (see Figure 3). Once saved the forecast service is running. To see it in action you can go to the 'Insights' page and select the power attribute in a chart. The dotted line will represent the forecasted data. 

Optionally, if you also want to store the forecasted power for comparing it with the actual values, you can add another attribute from the optional list, called 'Power forecast'. Add the configuration item 'Include forecast solar service', as well as the configuration items 'Has predicted data points', 'Read only', 'Rule state', and 'Store data points'.

### Wind Turbine

Your wind turbine asset is configured in a similar manner. First connect the 'Power' (note the negative value) and 'Energy Total Export' to an Agent which connects to your wind turbine system. See the PV Solar Asset example above, or check the different [Agent Protocol options in the Wiki](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview). 

#### Forecast service

**NOW:** OpenWeather Agent to be set up, with rule for windpower forecast.

**WISH:** Set up a connection to OpenWeather using the OpenWeather Agent. Next, when the config item 'Include in forecast wind service' is added to the 'power' attribute of a Wind turbine asset, the predicted data points will be added to this attribute, using the location of the Wind turbine asset.

## Electricity Consumer
main attributes: power and power forecast

#### Agents
eg.

#### Forecast
weighted historical moving average

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


