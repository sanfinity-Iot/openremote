First of all, the Manager UI functionality is described in the first paragraph [Online Demo Smart City - Read Mode](#online-demo-smart-city---read-mode). 

Secondly, the full OpenRemote Manager with 'Admin' access is explained in [OpenRemote Manager - Admin Mode](#openremote-manager---admin-mode).

Finally, in the third paragraph, we explain [White labelling your own IoT Platform](#white-labelling-your-own-iot-platform).

# Online Demo Smart City - Read Mode

To view a few example applications we have added the 'Smart City' project realm to the [online demo](https://openremote.io/demo/), which gives control over several application areas in 'read only' mode. Alternatively, you can also watch this [Introduction Video](https://youtu.be/K28CQMKr-rQ).

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

# OpenRemote Manager - Admin Mode

The OpenRemote Manager, with full admin access, requires you to set up OpenRemote locally first. Follow the [Quick Start](https://github.com/openremote/openremote/blob/master/README.md). Next, you can access the Application for System Manager via your browser: https://localhost/manager/?realm=master/ (Username: admin, Password: secret)

If you open the OpenRemote Manager, you will see only see the 'Master' realm in the upper right realm-picker. The [Demo Smart City](#demo-smart-city) refers to the 'Smart City' as we have added that realm in the online demo for your convenience. 

In the 'Admin Mode' the following additional functionality is available (or [watch the video](https://youtu.be/mx9amWaItn0)).
* Add Assets of different types, other OpenRemote EdgeGateway instances, or groups
* Add Agents for protocols to connect to real world sensors, actors or data services
* Fully configure Assets and Agents to make it fit your required Asset type or Protocol
* Use Global rules and/or use Groovy rules
* Connect as EdgeGateway to another (e.g.cloud hosted) instance of OpenRemote, using 'Manager Interconnect'.
* Create and manage new Realms
* Manage users and roles per Realm

## Groovy rules and Global rules

In addition to the WHEN-THEN and FLOW rules, when logged into the master, locally with the admin privileges, there is a third type 'GROOVY' which allows you to program more complex logic.

You will also notice that you can switch between realm rules and global rules. The global rules allow you to define rules which can use attributes across the different realms, as opposed to the realm rules which will only work within the selected realm. 

# White labelling your own IoT Platform

You can adjust name, color settings and logo's, change the map and set environment variables e.g. for e-mail. For instructions see [Custom Deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment).

See the documentation on our wiki for more details.

# See Also
- [Custom Deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment)
- [Next 'Get Started' step: Working on the UI](Developer-Guide%3A-Working-on-the-UI)
- [Get Started](https://openremote.io/get-started-iot-platform/)
- [OpenRemote Wiki](https://github.com/openremote/openremote/wiki)