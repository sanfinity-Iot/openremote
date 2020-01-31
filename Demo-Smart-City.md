This page describes a typical Smart City Application, which is part of the demo environment and can be a starting point for cities to benefit from a digital era, in which both citizens and infrastructure are digitally connected. 

The demo illustrates how the integration of several vertical solutions in a central platform, benefits a City as well as it Citizens:
* creating new applications requiring the (combination of) vertical solutions, using rules based algorithms as well creating dedicated front-end applications
* active monitoring of all sensors, and actors via healthchecks, for maintenance purposes
* exposing the data as assets and attributes, in a standardised manner via APIs, including the right acces rights, either 'restricted' to certain users and roles, or 'public'.

There are two applications which are part of the manager, once you have set it up yourself (See [Get Started](https://openremote.io/get-started-manager/)): the Application for Technical manager (for configuration of the project) and the Smart City demo, which giving full control over the application. We'll describe both with a simple example.

For convenience you can have a look at the online version of the [online Demo Smart City](https://demo.openremote.io/main/?realm=smartcity) using the credentials (smartCity / smartCity).

# Application for Technical Manager

The application for the Technical Manager requires you to set up the demo environment locally first. Follow the [Get Started](https://openremote.io/get-started-manager/). Next, you can access both the Application for Technical Manager via your browser (if you are using Docker Toolbox replace localhost with 192.168.99.100):

Application for Technical Manager: https://localhost (Username: admin, Password: secret)

If you open the Application for the Technical Manager, have a look at Smart City - Smart City. It includes a Service Agent (Simulator) and three Area's (1, 2, 3). If you use the 'Assets' view and select one of the Area's, you will notice that you see several devices (People Counter, Microphone, Environment, Light). The Service Agent (Simulator) can be used to change the simulated sensor values for Area 1, an additionally trigger some Alerts. 

We'll describe the set-up of all the devices (Assets) and the sensor and control parameters (attributes). We have organised them along a couple of application domains and have given a few examples of how a new application can be build by combining several attributes (Figure 1). In addition rules examples are given which can be used to automate your controls or generate alerts. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Smart%20City%20Environment%20Asset.png)
_Figure 1. The Asset view for the Smart City Realm, showing the attributes in for the Environment Asset in Area 1_

## Service Agent (simulator)

Before you read on, the service agent, can be used to simulate some values to the sensors (as you haven't connected real sensors yet, see figure 2). To do that select the attribute 'citySimulator' and open it up via the arrow on the right. Type in values and don't forget to press the 'Write simulator state' button.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Simulator.png)
_Figure 2. The Service Agent (Simulator) for the Smart City Realm, showing the attributes you can write to to simulate some of the sensor values_

## Crowd management

### Integration of camera

* Describe required attributes
* Rules: Alerts total crowd, and crowd growth

### Integration of sound

* Describe required attributes
* Rules: Alert sound level, sound type

### A new application

* Alerts based on a combination crowd, crowd changes, or sounds
* Notifications or light

## Environment

### Integration of air quality sensors

* Describe required attributes
* Rules: Alerts on Ozon

### A new application

* Alerts on combining crowd, environment and notifications

## Lighting

### Integration of a lighting solution

* Describe required attributes
* Rules:

### A new application

* Lighting based on pollution, sound, or crowd conditions

## Traffic management

### Integration of traffic

* Describe required attributes
* Rules:

### Integration of traffic lights

* Describe required attributes

### A new application

* Optimise traffic flows

## Health status and maintenance

## Publishing live data

### Make your City smart

* Integrate UI components in your existing City website
* Use rules for actions or notifications

### Enabling third party applications 

* Make assets public- or restricted and expose the APIs

## More functionality outside the scope of a simple demo

Next to the described example you will see that the Technical Manager, includes a Map view, Rules view, and Notification view. In addition you see a System view with Logging and the option to create and manage users. We refer to the [Wiki Documentation](https://github.com/openremote/openremote/wiki) if you want to know more about the available functionality (visible and invisible).

# Demo Smart City

To view an example application we have added the Demo Smart City https://localhost/main/?realm=smartcity/ (Username: smartCity, Password: smartCity), which gives control over all areas. It's build using the OpenRemote UI components (for Developers see [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Working-on-the-UI)).

If you just want to see how it looks before actually installing OpenRemote, see the [online Demo Smart City](https://demo.openremote.io/main/?realm=smartcity) using the credentials (smartCity / smartCity).

If you open the application on desktop you will get three main views: Map, Assets, and Rules. On mobile you will only see Map, and Assets.

## Map and Assets

Both the Map and Asset view give you access to the respective Assets and related attributes. You can see and control them, but also have the option to look at the historical data via graphs or event lists.

## Rules

The Rules view (only visible in the desktop version) allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute. This rules UI is translated into a Rules Object Model (one of the four rule types) in the Technical Manager. 

The Rules view is not fully functional yet in the demo, we apologise for that. However, it's work in progress we don't want to withhold, and get your feedback.

# See Also
- [Demo Smart Building](Demo-Smart-Building)
- [Next 'Get Started' step: Working on the UI](Developer-Guide%3A-Working-on-the-UI)
- [Get Started](https://openremote.io/get-started-manager/)