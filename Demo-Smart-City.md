This page describes a typical Smart City Application, which is part of the demo environment and can be a starting point for cities to benefit from a digital era, in which both citizens and infrastructure are digitally connected. 

The demo illustrates how the integration of several vertical solutions in a central platform, benefits a City as well as it Citizens:
* creating new applications requiring the (combination of) vertical solutions, using rules based algorithms as well creating dedicated front-end applications
* active monitoring of all sensors, and actors via healthchecks, for maintenance purposes
* exposing the data as assets and attributes, in a standardised manner via APIs, including the right acces rights, either 'restricted' to certain users and roles, or 'public'.

The basic set-up is part of the main branch (See [Get Started](https://openremote.io/get-started-manager/)). You can take it as a starting point for developing your own application. 

We have described the application here only for the [Technical Manager](#application-for-technical-manager). You can create applications for the City User, and the City Service Manager, similar as described in [Demo Smart Building](Demo-Smart-Building).

# Application for Technical Manager

This example is available as part of the main branch which you will need to set up first. Follow the [Get Started](https://openremote.io/get-started-manager/) or go to the [Developers page](https://openremote.io/developers/). 

When all Docker containers are ready, you can access the OpenRemote Manager and API with a webbrowser (if you are using Docker Toolbox replace localhost with 192.168.99.100):

OpenRemote Manager: https://localhost
Username: admin
Password: secret

In the Asset structure on the left you will see Tenant B - Smart City. It includes a Service Agent (Simulator) and three Area's (A, B, C).

# Crowd management

## Integration of camera

* Describe required attributes
* Rules: Alerts total crowd, and crowd growth

## Integration of sound

* Describe required attributes
* Rules: Alert sound level, sound type

## A new application

* Alerts based on a combination crowd, crowd changes, or sounds
* Notifications or light

# Environment

## Integration of your air quality sensors

* Describe required attributes
* Rules: Alerts on Ozon

## A new application

* Alerts on combining crowd, environment and notifications

# Lighting

## Integration of your lighting solution

* Describe required attributes
* Rules:

## A new application

* Lighting based on pollution, sound, or crowd conditions

# Traffic management

## Integration of traffic

* Describe required attributes
* Rules:

## Integration of traffic lights

* Describe required attributes

## A new application

* Optimise traffic flows

# Health status and maintenance

# Publishing live data

## Make your City smart

* Integrate UI components in your existing City website
* Use rules for actions or notifications

## Enabling third party applications 

* Make assets public- or restricted and expose the APIs

# How to get started

If you want to get this demo running, follow the [Get Started](https://openremote.io/get-started-manager/) or go to the [Developers page](https://openremote.io/developers/).

# See Also
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)