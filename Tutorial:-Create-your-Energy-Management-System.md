**THIS PAGE IS WORK IN PROGRESS AND A REQUIREMENTS DOCUMENT AT THIS STAGE. NOT ALL FEATURES ARE CURRENTLY INCLUDED IN MAIN BRANCH**

# Introduction

OpenRemote can be used as an Energy Management System (EMS) for your own microgrid. The EMS schedules the power setpoints of energy consuming devices and storage devices, taking into account renewable energy production (including forecasts), energy consumption (including forecasts), agile tariffs from your energy provider and required charge schedules for your electric vehicles. The optimisation routine targets lowest costs or lowest carbon exhaust. Figure 1 gives an overview of all the pieces of an EMS.

In this electricity example, we will connect a series of electricity producers: solar panels and a wind turbine, an electricity consumer: an energy meter, a static battery, and a charger with an electric vehicle. We are adding the forecasting services for both production and consumption and connecting the agile supplier tariffs. Finally, the optimisation service will control the setpoint power of the battery, as well as the vehicle and calculate your savings. 

<kbd>![Overview EMS](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20overview.jpg)</kbd>
_Figure 1. Overview of Energy Management System and all elements._

# Electricity assets and required agents

We assume you have set up the latest version of OpenRemote. If not, check out the [Quick Start](https://github.com/openremote/openremote/blob/master/README.md) first. Also have a look at how to [use the Manager UI](User-Guide%3A-Manager-UI).

If you navigate to 'Assets' and add an asset, using the '+' button. You will see a list of asset and agent types. A subset of these asset types are intended to set up your own EMS. They include the relevant attributes and configuration setting such that the optimisation service can recognise and manage them.

We'll be setting up a series of these assets, step-by-step.

## Optimisation

An optimisation asset represents the optimisation service. It will take into account all assets which are added as children of this asset. So first add an Optimisation asset at the root of your asset tree on the left and give it a name. We'll [explain later](#optimisation-1) how the optimisation actually works, after adding all the other assets.

## Electricity Producers

We are first adding the (renewable) energy production, by adding a PV solar Asset (Your solar) and a Wind turbine Asset (Your wind). Both need to be added as children of the Optimisation Asset. These assets include a series of standard attributes as shown in figure 2 and 3.

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
* Location. You can do this in the 'MODIFY' mode by opening the map modal next to the location attribute and setting the location by double clicking on the map.

Now add an additional configuration item 'Include forecast solar service' to the 'power' attribute of your solar asset (see Figure 5). Once saved the forecast service is running. To see it in action you can go to the 'Insights' page and select the power attribute in a chart. The dotted line will represent the forecasted data. 

Optionally, if you also want to store the forecasted power for comparing it with the actual values, you can also add the configuration item 'Include forecast solar service' to the attribute 'Power Forecast', as well as the configuration items 'Has predicted data points', 'Read only', 'Rule state', 'Set actual value with forecast', and 'Store data points'.

### Wind Turbine

Your wind turbine asset is configured in a similar manner. First connect the 'Power' (note the negative value) and 'Energy Total Export' to an Agent which connects to your wind turbine system. See the PV Solar Asset example above, or check the different [Agent Protocol options in the Wiki](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview). 

#### Forecast service

Similar as for the Solar asset, you need a forecast for the wind power as well. For the wind power forecast to work we require the Agent 'OpenWeather' to be connected, it connects to the [OpenWeather](https://openweathermap.org/api) service. In this agent you have to add the 'appid' of this service as well as set the location attribute. You can register for a [public free account](https://openweathermap.org/appid) to get this key.  
Once the OpenWeather Agent is connected, you have to fill in the following attributes in your wind turbine asset (see also Figure 3):
* Power export max (also called nominal or rated power in kW at the nominal or rated speed)
* Wind speed max (also called cut-off speed, the maximum speed at which a turbine is normally operating; above it will be turned down; in m/s)
* Wind speed min (also called cut-in speed, the minimum speed required to operate the wind turbine; in m/s)
* Wind speed reference (also called nominal or rated speed; in m/s)
* Location. You can do this in the 'MODIFY' mode by opening the map modal next to the location attribute and setting the location by double clicking on the map.

Now add an additional configuration item 'Include forecast wind service' to the 'power' attribute of your wind turbine asset (similar as for your solar asset, see Figure 5). Once saved the forecast service is running. To see it in action you can go to the 'Insights' page and select the power attribute in a chart. The dotted line will represent the forecasted data. 

Optionally, if you also want to store the forecasted power for comparing it with the actual values, you can also add the configuration item 'Include forecast wind service' to the attribute 'Power Forecast', as well as the configuration items 'Has predicted data points', 'Read only', 'Rule state', 'Set actual value with forecast', and 'Store data points'.

## Electricity Consumer

To connect electricity consuming devices. You can connect several energy meters by an 'Electricity Consumer Asset', as child of the Optimisation Asset. Similar as before you should connect the 'Power' and 'Energy Total Import' to your meters by using any of the existing [Agent Protocol options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview).

#### Forecast

For the electricity consuming devices you also need the forecasted power. You can enable this by adding the configuration item 'Include time series forecast'. This service will take an exponential weighted moving average, based on your preferences: the 'Repeat period': day -or- week, the 'Period range (1-7)': and the number of historical periods of the selected 'Repeat period'. When you select 'Repeat period = day' and 'Period range (number of periods) = 7', the forecast service will take into account the historical attribute values at the same time for the previous 7 days and weigh them exponentially. (see [specification](https://github.com/openremote/openremote/issues/526))

## Electricity Battery

One of the devices the optimisation can actively control is a static battery. You can add an 'Electricity Battery Asset' as child of the Optimisation Asset, and again link it up with an API of your battery system using any of the existing [Agent Protocol options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview). To make the optimisation work, you will need to link at least the following attributes:
* Energy level
* Power setpoint

Turn on the attributes 'Supports import' and 'Supports' export. This is an indication for the optimisation that it's allowed to control the power setpoint, both for charging and discharging.

### Battery Simulator

If you don't have a battery but want to simulate it and see what you can achieve in terms of financial or carbon savings, you can use the 'Storage Simulator Agent'. In this tutorial we use that option. 

Add the 'Storage Simulator Agent' first. Than add the configuration item 'Agent link' and link to the 'Storage Simulator Agent', for six attributes:
* Energy level
* Energy level percentage
* Power
* Power setpoint
* Energy import total
* Energy export total

Also set the specifications of the battery by filling in values for the following attributes:
* Energy capacity
* Power export max
* Power export min
* Energy level percentage max
* Energy level percentage min

## Electric Vehicle and a Charger
The charging of vehicles requires at least an 'Electric vehicle asset', as child of the Optimisation Asset. Optionally you will also add a 'Electric charger asset'. 

First let us explain how the optimisation with treat a vehicle. The optimisation will see the battery of the vehicle as a regular storage device which it can charge, and in some cases discharge. In addition you can set an 'Energy level schedule' on the vehicle (this is an optional attribute) to safeguard that your vehicle battery has enough energy at the time you need it.

So to make the optimisation work the following attributes need to be connected for the Electric vehicle asset, using any of the existing [Agent Protocol options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview):
* Energy level
* Power setpoint

There is a series of attributes on the vehicle you need or can use the make the optimisation work. We'll discuss them one-by-one:
* Supports import, required for the optimisation to actively control the setpoint power for charging
* Supports export, required for the optimisation to actively control the setpoint power for discharging. Only to be set when the vehicle and charger support discharging of course.
* Power import max, required to set the maximum charge power the vehicle can handle
* Power export max, required to set the maximum discharge power the vehicle can handle
* Energy level schedule. This sets the hour of the day, for each day of the week, at which the battery needs to be charged at the indicated percentage (see figure 6). It is not required but prevents an empty battery while you need your car to drive somewhere!
* Energy level percentage max. The optimisation will never charge beyond. Without setting it it will assume 100%
* Energy level percentage min. If a vehicle get's connected with a lower level, it will immediately start charging until it reaches the minimum level. If not set it will assume 0%.

In some cases you also want to connect to your charger, and make the vehicle a child of the charger, e.g.:
* In case you can't control the power setpoint on your vehicle. You will need to link the 'Power setpoint' of your charger to your charging system, using any of the existing [Agent Protocol options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview). In addition you can use the 'Flow editor' to link the 'Power setpoint' of your vehicle to the 'Power setpoint' of your charger. The optimisation will now set the 'Power setpoint' of your vehicle, which will then be forwarded to the 'Power setpoint' of your charger.
* In case you want the optimisation to consider both the 'Power import max' of your charger as well of the vehicle, to take the lowest of the two for the 'Power setpoint'. 

<kbd>![Energy schedule JSON format](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Energylevel%20schedule%20format.png)</kbd>

_Figure 6. The Energy schedule is in a JSON format as shown. Each day of the week (seven days) specifies the required energy level percentage for each hour of the day, starting at midnight. So in the above example the vehicle battery needs to be charged to 90% at 8am on each day_

## Electricity Supplier
The optimisation will als. take into account the 'Tariff import' and 'Tariff export' for your electricity. To enable this, first add an 'Electricity supplier asset' as a child of the Optimisation asset. Note that the tariffs are defined as costs. So the 'Tariff import' is normally positive as you pay, while the 'Tariff export' is negative as you earn. You can either set a fixed value for both tariffs, or connect to and API of your supplier, using any of the existing [Agent Protocol options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview). 

In this tutorial we just simulate values by adding a simulator profile. To do that, take the following steps:
* Add a 'Simulator Agent' at the root of your asset tree. 
* For both the 'Tariff export' and 'Tariff import', add the configuration parameter 'Agent link'. 
* In the 'Agent link' select the 'Simulator Agent' 
* Add the Parameter 'Replay Data' to 'Agent link' (see figure 7) 
* Fill in the prices you want to use per time stamp. Note these are costs, so usually positive for import tariffs and negative for export tariffs (see figure 8 for an example).

<kbd>![Replay data within Agent link](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Supplier%20Agen%20link%20Replay%20Data.png)</kbd>

_Figure 7. Selecting the parameter 'Replay data' within the 'Agent link' configuration item_

<kbd>![Supplier tariff export](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Supplier%20Tariff%20Export.png)</kbd>

_Figure 8. An example for the 'Replay data' for the Export tariff you want to use, indicating a tariff per timestamp (seconds). The negative value indicates you are earning money_

# Optimisation

The asset tree of the EMS you have build should now look like the one in figure 8. The optimisation routine can be configured in the optimisation asset:
* Financial weighting (between 0 and 100%). This indicates whether you are optimising on costs and or carbon. We have only explained the agile financial cost tariffs for the supplier, so set this value at 100%. In a similar manner, using carbon tariffs (an optional attribute of the Supplier asset) you can optimise on carbon by setting the 'Financial weighting at 0'.
* Interval size (the interval at which the optimisation runs again, in hours)
* Optimisation disabled (an on/off). This attribute can only be changed in the 'MODIFY' mode, to (temporarily) disable optimisation.

You have now set everything to run your optimisation. Once optimisation is enabled the optimisation will control and schedule all 'Power setpoints' of assets which have the 'Support import' and/or 'Support export' enabled. It will make the following considerations:
* Take into account the net resulting power towards the grid (export) or from the grid (import) as the difference between export and import tariffs influence the resulting costs.
* Take into account the forecasted power, both for electricity consumers and producers, to forecast when there is a net surplus towards the grid or a net shortage of electricity.
* Take into account the future export tariffs, as it is preferred to charge batteries or vehicles at the time of lowest export tariffs. At that time you would be earning the least by selling, so better store energy.
* Take into account the future import tariffs, as it is preferred to discharge batteries or vehicles at the time of highest import tariffs. You would pay the highest tariffs if you buy electricity, so better use your stored energy.
* Take the Energy level schedule of your vehicle into considerations as well as their Energy level percentage min, and always take care that the energy levels are met in time.
* Prioritise the different batteries, in this case your static battery and your vehicle, based on the so called Levelised cost of Storage (LCOS). LCOS is the additional costs of charging or discharging a battery. It reflects the costs of your battery divided by the capacity times the maximum number of charging cycles, so representing an amortisation. This can be implemented by adding the optional attributes 'Tariff import' and 'Tariff export' to your Electric vehicle asset and your Battery. Note that you should keep the 'Tariff import' value for your vehicle at '0' as charging your vehicle is anyhow required to be able to drive, and the optimisation will not introduce extra charge cycles. We haven't included this in our tutorial and recommend to read-up expert articles before applying. 

<kbd>![EMS asset tree](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20-%20Asset%20tree%20and%20optimisation%20asset.png)</kbd>

_Figure 9. The asset tree of your EMS on the left, with the Optimisation asset selected_

So now you have your EMS running. The last item to explain are the two attributes 'Financial saving' and 'Carbon saving'. These two values are reflecting the EXTRA savings you are achieving due to the optimisation using the static battery in a smart way and schedule the charging of your vehicle at the best time. To know your electricity bill you should of course count up the total imported energy times the import tariff, and the total exported energy times the export tariff... 


***

**DISCLAIMER: OPENREMOTE SHALL UNDER NO CIRCUMSTANCE BE LIABLE FOR ANY DAMAGES ARISING OUT OF ILLEGALLY USING UNOFFICIAL, NOT VENDOR AGREED OR APPROVED, API'S**

