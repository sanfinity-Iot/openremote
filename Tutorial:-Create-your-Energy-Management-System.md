**THIS PAGE IS WORK IN PROGRESS AND WE WELCOME YOUR FEEDBACK**

It describes how you can build your own energy management system. Some features are described with two versions:

**NOW:** which describes the current implementation and the way it works now.

**WISH:** the way we foresee it to work. See this as user requirements. And feel to comment on it and share your wishes.


# Introduction

OpenRemote can be used as an Energy Management System (EMS), which can schedule energy consuming devices and storage devices, taking into account your renewable energy producers, agile tariffs from your energy provider and required charge schedules for your electric vehicles. 

In this electricity example, an optimisation service will control the setpoint power of the connected batteries and electric vehicles, considering all connected 'consumers' and 'producers', their forecasted values, as well as
In this tutorial we explain how to set it up.

An optimisation service will control
Purpose of optimisation: overall financial savings or carbon savings

Describe asset structure within the optimisation group

# Electricity assets and attributes

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


