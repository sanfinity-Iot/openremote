**THIS PAGE IS WORK IN PROGRESS AND WE WELCOME YOUR FEEDBACK**

It describes how you can build your own energy management system. Some features are described with two versions:

**NOW:** which describes the current implementation and the way it works now.

**WISH:** the way we foresee it to work. See this as user requirements. And feel to comment on it and share your wishes.

# Introduction

OpenRemote can be used as an Energy Management System (EMS), which can schedule energy consuming devices and storage devices, taking into account your renewable energy producers, agile tariffs from your energy provider and required charge schedules for your electric vehicles. 

In this electricity example, we are connecting a series of consumers: an energy meter, chargers, a series of vehicles; a series of producers: solar panels and a windturbine; and a static battery. Secondly, we are adding the forecasting services for both consumption and production and connecting the agile supplier tariffs. Finally, the optimisation service will control the setpoint power of the battery, as well as the vehicles. The optimisation will either target the highest financial savings or carbon savings, based on your preference.

# Electricity assets and attributes

We assume you have set up the latest version of OpenRemote. If not, check out the [Quick Start](https://github.com/openremote/openremote/blob/master/README.md) first.

If you navigate to 'Assets' and add an asset, using the '+' button. You will see a list of asset types. A subset of these asset types are intended to set up your own EMS. They include the relevant attributes and configuration setting such that the optimisation service can recognise and manage them.

We'll be setting up a series of these assets, step-by-step.

## Optimisation

The Optimisation asset is actually representing the optimisation service. It will take into account all assets which are added as children of this asset. So first add an Optimisation asset at the root of your asset tree on the left and give it a name. We'll [explain later](#optimisation-1) how the optimisation actually works, after adding all the other assets.

## Electricity Producer
main attributes: power and power forecast

### PV Solar
specific attributes

#### Agents 
eg. Solar Edge, 

#### Forecast
Forecast.Solar

### Wind Turbine
specific attributes

#### Agents
eg. 

#### Forecast
OpenWeather

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


