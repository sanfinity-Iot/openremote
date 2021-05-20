The Manager UI is the dashboard which gives you access to OpenRemote, and allows you to configure, monitor, and control your IoT platform. We'll explain the main features of the Manager UI, sometimes referring to examples from the [online Demo](https://openremote.io/demo/), for which you only have access in 'read' mode. To have full 'admin' access to all functionality you will first need to [install OpenRemote](https://github.com/openremote/openremote/blob/master/README.md). If you prefer watching a video, rather than reading, check out the [Introduction Video](https://youtu.be/K28CQMKr-rQ).

# OpenRemote Manager

Login...

If you open the application you will get four main views: Map (Figure 1), Assets (Figure 2), Rules (Figure 3), and Insights. On mobile you will only see Map, Assets and Insights.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Login%20(2).png)
_Figure 1. Login page (left) with option to reset password by selecting 'Forgot Password'_

## Map

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Map.png)
_Figure 2. The Map view, here with the Demo Smart City, showing the map with different attributes across the City_

## Assets

Both the Map and Asset view give you access to the respective Assets and related attributes. You can see and control them, but also have the option to look at the historical data via graphs or event lists.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Building%20Asset.png)
![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Assets%20(2).png)
_Figure 3. The Asset view of a building-type asset (top), the modal used to create an environment-type asset (bottom left), and the resulting environment-type asset (bottom right)_

If you want to add assets and link them to an existing system or dataservice, first install OpenRemote locally (see [Quick Start)](https://github.com/openremote/openremote/blob/master/README.md)) and have a look at the video [Integrating with OpenRemote](https://youtu.be/mx9amWaItn0). 

### Creating an asset
With the correct permissions, you can create a new asset on the `Assets` page by clicking the `+` in the header of the asset tree. This will open a modal that shows the available asset types. Select one and click `Add`. With the asset added, you can write its attributes (if not read-only). Often you will want to modify the attributes to define how they are used in the system, e.g. get values through agent link, make the attribute available in rules, or format its value.

### Adding an attribute
On the Assets page enable 'Edit mode' and click `+ Add attribute` at the bottom of the list of attributes. In the modal you can choose attributes already associated with the asset type of the asset, or you can create a `custom attribute`. The custom attributes should have a `camelCase` name, which will automatically be translated to Sentence case with spaces. You can create attributes of prepared attribute types that are known by the system.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Edit%20%26%20Add%20attribute%20(2).png)
_Figure 4. The Asset view in 'Edit mode' (left) and adding an attribute (right)_

### Configure attributes

Link to [Assets, Agents and Attributes](https://github.com/openremote/openremote/wiki/User-Guide%3A-Assets%2C-Agents-and-Attributes) 

### Creating an agent

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Add%20Agent%20(2).png)
_Figure 5. Creating an Agent, using the HTTP Agent (left) creates the Agent asset page (right)_

### Linking agents and assets

## Rules

The Rules view (only visible in the desktop version, see figure 3) allows you to build three types of rules:
* WHEN-THEN: allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute.
* FLOW: allows for processing attributes and converting them into new attributes. 
* GROOVY: programming any advanced logic, using attributes in the system.
All rules have the option to use a time scheduler, which allows for have rules activated on a time schedule with a recurrence.

### WHEN-THEN Rules

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20LHS%20%26%20RHS%20(2).png)
_Figure 6. A WHEN-THEN Rules example, which shows how, on the lefthand-side an asset attribute condition can be selected, while on the righthand-side the action is selected._

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20Schedules%20(2).png)
_Figure 7. The Rules trigger frequency (left) as well as time scheduler (right) allows for defining when rules are triggered and active_

### Flow Rules

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Flow%20%26%20Groovy%20(2).png)
_Figure 8. Flow rules to process data (left) and Groovy rules for programming more advanced logic (right)_

### Groovy Rules

### Global versus Realm Rules

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

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Edit%20Account.png)
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