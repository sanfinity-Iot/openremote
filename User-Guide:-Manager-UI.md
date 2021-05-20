The Manager UI is the dashboard which gives you access to OpenRemote, and allows you to configure, monitor, and control your IoT platform. We'll explain the main features of the Manager UI, sometimes referring to examples from the [online Demo](https://openremote.io/demo/), for which you only have access in 'read' mode. To have full 'admin' access to all functionality you will first need to [install OpenRemote](https://github.com/openremote/openremote/blob/master/README.md). If you prefer watching a video, rather than reading, check out the [Introduction Video](https://youtu.be/K28CQMKr-rQ).

# OpenRemote Manager

Login...

If you open the application you will get four main views: Map (Figure 1), Assets (Figure 2), Rules (Figure 3), and Insights. On mobile you will only see Map, Assets and Insights.

## Map

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Map.png)
_Figure 1. The Map view of the Demo Smart City, showing the map with different attributes across the City_

## Assets

Both the Map and Asset view give you access to the respective Assets and related attributes. You can see and control them, but also have the option to look at the historical data via graphs or event lists.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Assets.png)
_Figure 2. The Asset view of the Demo Smart City, showing an EV charger with the different attributes_

If you want to add assets and link them to an existing system or dataservice, first install OpenRemote locally (see [Quick Start)](https://github.com/openremote/openremote/blob/master/README.md)) and have a look at the video [Integrating with OpenRemote](https://youtu.be/mx9amWaItn0). 

### Creating an asset
With the correct permissions, you can create a new asset on the `Assets` page by clicking the `+` in the header of the asset tree. This will open a modal that shows the available asset types. Select one and click `Add`. With the asset added, you can write its attributes (if not read-only). Often you will want to modify the attributes to define how they are used in the system, e.g. get values through agent link, make the attribute available in rules, or format its value.

### Adding an attribute
On the Assets page enable 'Edit mode' and click `+ Add attribute` at the bottom of the list of attributes. In the modal you can choose attributes already associated with the asset type of the asset, or you can create a `custom attribute`. The custom attributes should have a `camelCase` name, which will automatically be translated to Sentence case with spaces. You can create attributes of prepared attribute types that are known by the system.

### Configure attributes

Link to [Assets, Agents and Attributes](https://github.com/openremote/openremote/wiki/User-Guide%3A-Assets%2C-Agents-and-Attributes) 

### Creating an agent

### Linking agents and assets

## Rules

The Rules view (only visible in the desktop version, see figure 3) allows you to build three types of rules:
* WHEN-THEN: allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute.
* FLOW: allows for processing attributes and converting them into new attributes. 
* GROOVY: programming any advanced logic, using attributes in the system.
All rules have the option to use a time scheduler, which allows for have rules activated on a time schedule with a recurrence.

### WHEN-THEN Rules

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Rules.png)
_Figure 3a. The Rules view of the Demo Smart City, showing an imaginary example which sends an e-mail when the Ozone levels are increasing while water levels are too high._

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20scheduler.png)
_Figure 3b. The Rules Scheduler allows for activating rules, based on a recurring time schedule_

### Flow Rules

### Groovy Rules

### Global versus Realm Rules

## Insights

The Insights view (only visible in the desktop version, see figure 4) allows you to create a single page report:
* Chart: allows you to select multiple attributes in the system and compare them (vertical comparison). By adding a second period you can also compare attributes for different time periods
* Attribute panel: allows for picking individual attributes, eg. KPIs and see there current performance over a period, as well as their relative or absolute change. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Insights.png)
_Figure 4. The Insights view of the Demo Smart City, showing the soil temperature at Leuven Haven and a few random attribute panels._

## Settings and accounts

### Interconnect Manager instances

### Languages

### Logs

### Account, Users, and Roles

### Realms




# See Also
- [Customising map and styling](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment)
- [Next 'Get Started' step: Working on the UI](Developer-Guide%3A-Working-on-the-UI)
- [Get Started](https://openremote.io/get-started-iot-platform/)
- [OpenRemote Wiki](https://github.com/openremote/openremote/wiki)