This page describes a typical Smart City Application, which is part of the demo environment and can be a starting point for cities to benefit from a digital era, in which both citizens and infrastructure are digitally connected. 

The demo illustrates how the integration of several vertical solutions in a central platform, benefits a City as well as it Citizens:
* creating new applications requiring the (combination of) vertical solutions, using rules based algorithms as well creating dedicated front-end applications
* active monitoring of all sensors, and actors via healthchecks, for maintenance purposes
* exposing the data as assets and attributes, in a standardised manner via APIs, including the right acces rights, either 'restricted' to certain users and roles, or 'public'.

The basic set-up is part of the main branch (See [Get Started](https://openremote.io/get-started-manager/)). You can take it as a starting point for developing your own application. 

We have described the application here only for the [Technical Manager](#application-for-technical-manager). You can create applications for the City User, and the City Service Manager, similar as described in [Demo Smart Building](Demo-Smart-Building).

# Application for Technical Manager

The application for the Technical Manager requires you to set up the demo environment locally first. Follow the [Get Started](https://openremote.io/get-started-manager/). Next, you can access both the Application for Technical Manager via your browser (if you are using Docker Toolbox replace localhost with 192.168.99.100):

Application for Technical Manager: https://localhost (Username: admin, Password: secret)

If you open the Application for the Technical Manager, have a look at Tenant B - Smart City. It includes a Service Agent (Simulator) and three Area's (1, 2, 3). If you use the 'Assets' view and select one of the Area's, you will notice that you see several devices (Camera, Microphone, Environment, Light). The Service Agent (Simulator) can be used to change the simulated sensor values for Area 1, an additionally trigger some Alerts. 

We'll describe the set-up of all the devices (Assets) and the sensor and control parameters (attributes). We have organised them along a couple of application domains and have given a few examples of how a new application can be build by combining several attributes. In addition rules examples are given which can be used to automate your controls or generate alerts. 

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

# See Also
- [Demo Smart Building](Demo-Smart-Building)
- [Get Started](https://openremote.io/get-started-manager/)