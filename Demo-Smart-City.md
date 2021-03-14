The Manager UI functionality is illustrated in the Smart City demo with a few imaginary applications in a single realm. It is part of the [online demo](https://openremote.io/demo/). It illustrates how the integration of several vertical solutions in a central platform, benefits a Smart City and what you can build yourself, once installed locally.

In an OpenRemote system you can have multiple realms for your projects or customers, with the 'Master' realm for the system manager (for configuration the project and remote management of multiple realms). Online you will only get 'view' access to the 'Smart City' project realm. If you have installed OpenRemote locally (see [Quick Start](https://github.com/openremote/openremote/blob/master/README.md)), you will have an empty installation, including the 'Master' realm and full admin access. 

# Online Demo Smart City

To view a few example applications we have added the 'Smart City' project realm to the [online demo](https://openremote.io/demo/), which gives control over all areas. It's build using the OpenRemote UI components (for Developers see [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Working-on-the-UI)).

If you open the application you will get four main views: Map (Figure 1), Assets (Figure 2), Rules (Figure 3), and Insights. On mobile you will only see Map, Assets and Insights.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Map.png)
_Figure 1. The Map view of the Demo Smart City, showing the map with different attributes across the City_

## Map and Assets

Both the Map and Asset view give you access to the respective Assets and related attributes. You can see and control them, but also have the option to look at the historical data via graphs or event lists.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Assets.png)
_Figure 2. The Asset view of the Demo Smart City, showing an EV charger with the different attributes_

If you want to add assets and link them to an existing system or dataservice, first install OpenRemote locally (see [Quick Start)](https://github.com/openremote/openremote/blob/master/README.md)) and have a look at the video [Integrating with OpenRemote](https://youtu.be/mx9amWaItn0). 

## Rules

The Rules view (only visible in the desktop version, see figure 3) allows you to build three types of rules:
* WHEN-THEN: allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute.
* FLOW: allows for processing attributes and converting them into new attributes. 
* GROOVY: programming any advanced logic, using attributes in the system.
All rules have the option to use a time scheduler, which allows for have rules activated on a time schedule with a recurrence.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Rules.png)
_Figure 3a. The Rules view of the Demo Smart City, showing an imaginary example which sends an e-mail when the Ozone levels are increasing while water levels are too high._

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20scheduler.png)
_Figure 3b. The Rules Scheduler allows for activating rules, based on a recurring time schedule_

## Insights

The Insights view (only visible in the desktop version, see figure 4) allows you to create a single page report:
* Chart: allows you to select multiple attributes in the system and compare them (vertical comparison). By adding a second period you can also compare attributes for different time periods
* Attribute panel: allows for picking individual attributes, eg. KPIs and see there current performance over a period, as well as their relative or absolute change. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Insights.png)
_Figure 4. The Insights view of the Demo Smart City, showing the soil temperature at Leuven Haven and a few random attribute panels._

# Application for System Manager

The application for the System Manager requires you to set up OpenRemote locally first. Follow the [Quick Start](https://github.com/openremote/openremote/blob/master/README.md). Next, you can access the Application for System Manager via your browser: https://localhost/manager/?realm=master/ (Username: admin, Password: secret)

If you open the Application for the System Manager, you will see only see the 'Master' realm in the upper right realm-picker. The [Demo Smart City](#demo-smart-city) refers to the 'Smart City' as we have added that realm in the online demo for you. 

To see what functionality you have available as System Manager, watch the Introduction Video: [Integrating with OpenRemote](https://youtu.be/mx9amWaItn0). 

## Groovy rules and Global rules

In addition to the WHEN-THEN and FLOW rules, when logged into the master, locally with the admin privileges, there is a third type 'GROOVY' which allows you to program more complex logic.

You will also notice that you can switch between realm rules and global rules. The global rules allow you to define rules which can use attributes across the different realms, as opposed to the realm rules which will only work within the selected realm. 

See the documentation on our wiki for more details.

# See Also
- [Next 'Get Started' step: Working on the UI](Developer-Guide%3A-Working-on-the-UI)
- [Get Started](https://openremote.io/get-started-iot-platform/)
- [OpenRemote Wiki](https://github.com/openremote/openremote/wiki)