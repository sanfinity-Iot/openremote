The Manager UI is the dashboard which gives you access to OpenRemote, and allows you to configure, monitor, and control your IoT platform. We'll explain the main features of the Manager UI, sometimes referring to examples from the [online Demo](https://openremote.io/demo/), for which you only have access in 'read' mode. To have full 'admin' access to all functionality you will first need to [install OpenRemote](https://github.com/openremote/openremote/blob/master/README.md). If you prefer watching a video, rather than reading, check out the [Introduction Videos](https://youtu.be/K28CQMKr-rQ).

# OpenRemote Manager

To access the Manager you will first need to login with the correct credentials (admin/secret for your local installation). Note that our account management and identity service includes features like a 'forgot password' flow. See Keycloak documentation for more details.  

If you open the application you will get four main views: `Map` (Figure 1), `Assets` (Figure 2), `Rules` (Figure 6-8), and `Insights` (Figure 9). We'll discuss all views as well as the additional settings and account pages.

## Map

The `Map` page will show your map (see the custom deployment wiki to [change your map](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment)). You can move, zoom, tilt, and pan the map. On the map all your assets are shown which have a location as well as the configuration item `show on dashboard` selected. Assets can both have static or dynamic locations (eg. a car, boat or plane). Once you select an asset a panel will appear, summarising the attributes of the asset and their current values. The `Asset details` button in this panel will bring you to the respective Asset page.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Map.png)
_Figure 1. The Map view, here with the Demo Smart City, showing the map with different attributes across the City_

## Assets

The Asset view gives you access to the respective Assets and related attributes. You will see the assets and the tree structure on the left. On the right you will see the Asset page, which contains a panel with all Attributes, a Location panel (if any) and a History panel (if any data is stored).

The Attribute panel will give an overview of all attributes and their units. For those attributes which can be changed via the UI, and for which you have the 'write' role as a user, you can type a value and press 'enter' or press the `send` arrow on the right (e.g. in below example the Carbon dioxide value is assumed to be a manual input)). Other attribute values could be coming from sensors or are automatically controlled by rules.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Mnager%20-%20Asset%20Environment.png)
_Figure 2. An asset of the type 'environment'_

### Creating an asset

With the correct permissions (not in the Demo environment), you can create a new asset on the Assets page by clicking the `+` in the header of the asset tree. This will open a modal that shows the available asset types. When you select one you will see the attributes and optional attributes. The latter ones can be added by selecting them. Also decide whether it needs a parent, before you click `Add`. With the asset added, you will see the asset (and asset page) appear in the asset tree on the left. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Create%20Asset-Parent.png)
_Figure 3. Creating an asset of the type 'environment'_

Often you will want to add or modify the attributes to define how they are used in the system, e.g. get values through agent link, make the attribute available in rules, or format its value. We'll explain that next.

### Adding an attribute

On the Assets page enable `Edit asset` and click `+ Add attribute` at the bottom of the list of attributes. In the modal you can choose attributes already associated with the asset type of the asset, or you can create a `custom attribute`. The custom attributes should have a `camelCase` name, which will automatically be translated to Sentence case with spaces. You can create attributes of prepared attribute types that are known by the system.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Edit%20%26%20Add%20attribute%20(2).png)
_Figure 4. The Asset view in 'Edit mode' (left) and adding an attribute (right)_

### Configure attributes

For any attribute, while in `Edit asset` mode, you can can expand each attribute which gives you the option to add configuration items or change existing ones. You can use configuration items for example to decide whether you want to store historical data and for how long, allow data to be used in rules, or whether a location should be displayed on the Map. See the page [Assets, Agents and Attributes](https://github.com/openremote/openremote/wiki/User-Guide%3A-Assets%2C-Agents-and-Attributes) for all available configuration options.

### Creating an agent

Agents are a specific type of asset, which are used to connect to external sensors, actuators, gateways or services, using protocols. They are added in the same manner as assets by clicking the `+` in the header of the asset tree. This will open a modal that shows the available agent types. You will see generic ones: HTTP, Websocket, TCP/IP, UDP and SNMP; as well as more specific ones like Z-wave, KNX or Velbus.
Once you create an Agent, the agent page will display the relevant attributes, required to establish an actual connection to the external world.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Add%20Agent%20(2).png)
_Figure 5. Creating an Agent, using the HTTP Agent (left) creates the Agent asset page (right)_

Note that you can also connect to OpenRemote through the Manager APIs, see the [paragraph: Manager APIs](#manager-apis).

### Linking agents and assets

Next you want to link data coming in through your agents with the attributes of your created assets. Some Agents have auto discovery (e.g. Z-wave) or configuration files (e.g. KNX and Velbus) and assets ar automatically generated. For others it's a manual step. See the example for connecting to [OpenWeatherMap via an HTTP Agent](https://github.com/openremote/openremote/wiki/User-Guide%3A-HTTP-Agent).

## Rules

The Rules view (see figures 6-8) allows you to build three types of rules:
* WHEN-THEN: allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute.
* FLOW: allows for processing attributes and converting them into new attributes. 
* GROOVY: programming any advanced logic, using attributes in the system.
All rules have the option to use a time scheduler, which allows for have rules activated on a time schedule with a recurrence.

### WHEN-THEN Rules

WHEN-THEN rules are intended to define lefthand-side conditions for attributes which trigger a righthand-side action for another attribute. The righthand side can also handle e-mails or push notifications (using the OpenRemote consoles). 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20LHS%20%26%20RHS%20(2).png)
_Figure 6. A WHEN-THEN Rules example, which shows how, on the lefthand-side an asset attribute condition can be selected, while on the righthand-side the action is selected._

The frequency on which rules trigger as well as a timer schedule can be set. See the examples below. 

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20Schedules%20(2).png)
_Figure 7. The Rules trigger frequency (left) as well as time scheduler (right) allows for defining when rules are triggered and active_

See the [WHEN-THEN Rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Use-When-Then-Rules) wiki for more details. 

### Flow Rules

Flow rules can be used to link and process attributes. In the visual editor you can use `Input` (blue), `Processor` (green), and `Output` nodes, and wire them up. See the wiki [Flow Rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Use-Flow-Rules) for more details. The same scheduler as for WHEN-THEN rules is available for flow rules.

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Flow%20%26%20Groovy%20(2).png)
_Figure 8. Flow rules to process data (left) and Groovy rules for programming more advanced logic (right)_

### Groovy Rules

Groovy rules are intended for more advanced processing and automation (see an example in figure 8, right).

### Global versus Realm Rules

As an admin user of the system which can access all realms, you have the option to select 'Global' versus 'Realm' rules. Global rules allow you to use WHEN-THEN and Groovy rules, across realms. Realm rules are part of a single realm and can only access attributes within that realm.

## Insights

The Insights view (only visible in the desktop version, see figure 4) allows you to create a single page report:
* Chart: allows you to select multiple attributes in the system and compare them (vertical comparison). By adding a second period you can also compare attributes for different time periods
* Attribute panel: allows for picking individual attributes, eg. KPIs and see there current performance over a period, as well as their relative or absolute change. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Insights.png)
_Figure 9. The Insights view of the Demo Smart City, showing the soil temperature at Leuven Haven and a few random attribute panels._

## Settings and accounts

### Interconnect Manager instances

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Manager%20Interconnect%20(2).png)
_Figure 10. Several OpenRemote instances can be interconnected, e.g. connecting multiple instances on edge gateways to one central cloud hosted instance. The Manager Interconnect page, used at the edge instances creates the keys (left) which are used on the central instance by adding Edge gateway Assets (right)._

### Languages

### Logs

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Logs.png)
_Figure 11. The Logs page to evaluate system behaviour._

### Account, Users, and Roles

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Account%20(2).png)
_Figure 12. The account page which allows setting e.g. contact details (left) and reset passwords (right)._

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Users%20(2).png)
_Figure 13. Creating users for a selected Realm and assigning roles (left). Also service users are created here (right)_

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Roles.png)
_Figure 14. Roles can be defined which can be linked to individual users_

### Realms

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Realms.png)
_Figure 15. Realms can be created to manage multiple independent projects within one OpenRemote instance_

## Manager APIs

### Service users

### MQTT, Websocket and HTTP

# See Also
- [Customising map and styling](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment)
- [Next 'Get Started' step: Working on the UI](Developer-Guide%3A-Working-on-the-UI)
- [Get Started](https://openremote.io/get-started-iot-platform/)
- [OpenRemote Wiki](https://github.com/openremote/openremote/wiki)