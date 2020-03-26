This page describes a typical Smart City Application, which is part of the demo environment and can be a starting point for cities to benefit from a digital era, in which both citizens and infrastructure are digitally connected. 

The demo illustrates how the integration of several vertical solutions in a central platform, benefits a City as well as it Citizens:
* creating new applications requiring the (combination of) vertical solutions, using rules based algorithms as well creating dedicated front-end applications
* active monitoring of all sensors, and actors via healthchecks, for maintenance purposes
* exposing the data as assets and attributes, in a standardised manner via APIs, including the right acces rights, either 'restricted' to certain users and roles, or 'public'.

There are two applications which are part of the manager, once you have set it up yourself (See [Get Started](https://openremote.io/get-started-manager/)): the Application for Technical manager (for configuration of the project) and the Smart City demo, which giving full control over the application. We'll describe both with a simple example.

For convenience you can have a look at the online version of the [online Demo Smart City](https://demo.openremote.io/main/?realm=smartcity) using the credentials (smartCity / smartCity).

# Application for Technical Manager

The application for the Technical Manager requires you to set up the demo environment locally first. Follow the [Get Started](https://openremote.io/get-started-manager/). Next, you can access both the Application for Technical Manager via your browser (if you are using Docker Toolbox replace localhost with 192.168.99.100):

Application for Technical Manager: https://localhost:8080 (Username: admin, Password: secret)

If you open the Application for the Technical Manager, you will see three Realms on the left: Master, Smart Building, and Smart City (Figure 1). The [Demo Smart City](#demo-smart-city) refers to the Realm: Smart City. It includes three area with different assets (lights, environment sensors, microphones and sound alerts, people counter) each with different attributes. If you use the 'Assets' view in the Technical Manager, you will notice that you see exactly the same attributes as visible in the [Demo Smart City](#demo-smart-city). The Service Agent (Simulator) in Smart City can be used to change the simulated sensor values for the different areas. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Smart%20City%20Environment%20Asset.png)
_Figure 1. The Asset view for the Smart City Realm, showing the attributes in for the Environment Asset in Area 1_

## Areas, assets and attributes

As you navigate through the 'Assets' view (Figure 1) you will notice that each area contains several assets and attributes. There are environmental sensors, microphones, and people counters, and there is lighting control. For those attributes you can control (eg. a light), you can change values and save it by pressing the 'Write' button. For adding sensor values you can use the simulator. If you added the weatherAPI, following the [example for connecting your data](User-Guide%3A-Connecting-to-a-HTTP-API) you also see an attribute 'outsideTemp' with the current outdoor temperature, as part of the Smart City Asset.

## Service Agent (simulator)

Before you read on, the service agent, can be used to simulate some values to the sensors (as you haven't connected real sensors yet, see figure 2). To do that select the 'Service Agent (Simulator)' and open it up via the arrow on the right. Type in values and don't forget to press the 'Write simulator state' button.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Simulator.png)
_Figure 2. The Service Agent (Simulator) for the Smart City Realm, showing the attributes you can write to to simulate some of the sensor values_

Next to the described example you will see that the Technical Manager, includes a Map view, Rules view, and Notification view. In addition you see a System view with Logging and the option to create and manage users. We refer to the [Wiki Documentation](https://github.com/openremote/openremote/wiki) if you want to know more about the available functionality (visible and invisible).

# Demo Smart City

To view an example application we have added the Demo Smart City https://localhost:8080/main/?realm=smartcity/ (Username: smartCity, Password: smartCity), which gives control over all areas. It's build using the OpenRemote UI components (for Developers see [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Working-on-the-UI)) and supports 5 languages.

If you just want to see how it looks before actually installing OpenRemote, see the [online Demo Smart City](https://demo.openremote.io/main/?realm=smartcity) using the credentials (smartCity / smartCity) and select your preferred language at the top right.

If you open the application on desktop you will get three main views: Map (Figure 3), Assets (Figure 4), and Rules (Figure 5). On mobile you will only see Map, and Assets.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Map.png)
_Figure 3. The Map view of the Demo Smart City, showing the map with different attributes across the City_

## Map and Assets

Both the Map and Asset view give you access to the respective Assets and related attributes. You can see and control them, but also have the option to look at the historical data via graphs or event lists.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Assets.png)
_Figure 4. The Asset view of the Demo Smart City, showing the environment sensor with the different attributes_

## Rules

The Rules view (only visible in the desktop version, see figure 5) allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute. This rules UI is translated into a Rules Object Model (one of the four rule types) in the Technical Manager. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Rules.png)
_Figure 5. The Rules view of the Demo Smart City, showing showing an example which turns on a light and send an e-mail when the Ozone levels are increasing_

## Health status and maintenance

For monitoring of your system a health check is included.

## Publishing live data

To expose information from the platform we have several options: using UI components or bundled webpackages, set alerts and notifications using the rules, or using the public- or restricted APIs.

See the documentation on our wiki for more details.

# See Also
- [Demo Smart Building](Demo-Smart-Building)
- [Next 'Get Started' step: Working on the UI](Developer-Guide%3A-Working-on-the-UI)
- [Get Started](https://openremote.io/get-started-manager/)
- [OpenRemote Wiki](https://github.com/openremote/openremote/wiki)