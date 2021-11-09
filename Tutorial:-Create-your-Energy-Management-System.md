**THIS PAGE IS WORK IN PROGRESS AND MERELY A REQUIREMENTS DOCUMENT**

It describes how you can build your own energy management system. Some features are described with as **WISH**, meaning they are not implemented yet.

# Introduction

OpenRemote can be used as an Energy Management System (EMS) of your own microgrid. The EMS can schedule the power setpoints of energy consuming devices and storage devices, taking into account your renewable energy producers, agile tariffs from your energy provider and required charge schedules for your electric vehicles. Using an optimisation routine, the scheduling is targeting lowest costs or lowest carbon exhaust.

In this electricity example, we will connect a series of consumers: an energy meter, chargers, a series of vehicles; a series of producers: solar panels and a windturbine; and a static battery. Secondly, we are adding the forecasting services for both consumption and production and connecting the agile supplier tariffs. Finally, the optimisation service will control the setpoint power of the battery, as well as the vehicles. 

![Overview EMS](https://github.com/openremote/Documentation/blob/master/manuscript/figures/EMS%20overview.png)
_Figure 1. Overview of Energy Management System and all elements._

# Electricity assets and required agents

We assume you have set up the latest version of OpenRemote. If not, check out the [Quick Start](https://github.com/openremote/openremote/blob/master/README.md) first. Also have a look at how to [use the Manager UI](User-Guide%3A-Manager-UI).

If you navigate to 'Assets' and add an asset, using the '+' button. You will see a list of asset and agent types. A subset of these asset types are intended to set up your own EMS. They include the relevant attributes and configuration setting such that the optimisation service can recognise and manage them.

We'll be setting up a series of these assets, step-by-step.

## Optimisation

First add an Optimisation asset is actually representing the optimisation service. It will take into account all assets which are added as children of this asset. So first add an Optimisation asset at the root of your asset tree on the left and give it a name. We'll [explain later](#optimisation-1) how the optimisation actually works, after adding all the other assets.

## Electricity Producer
structure with main attributes: power and power forecast

### PV Solar
specific attributes
set location

#### Forecast service

**NOW:** API credentials for Forecast.Solar need to be set in provisioning code before deploying OpenRemote. Add a config item 'Include in forecast solar service' in 'power' attribute.

**WISH:** Set up a connection to Forecast.Solar using the Forecast.solar Agent. Next, when the config item 'Include in forecast solar service' is added to the 'power' attribute of a PV Solar asset, the predicted data points will be added to this attribute, using the location of the PV solar asset.

### Wind Turbine
specific attributes
set location

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


