The Manager UI is the dashboard which gives you access to OpenRemote, and allows you to configure, monitor, and control your IoT platform. We'll explain the main features of the Manager UI, sometimes referring to examples from the [online Demo](https://openremote.io/demo/), for which you only have access in 'read' mode. To have full 'admin' access to all functionality you will first need to [install OpenRemote](https://github.com/openremote/openremote/blob/master/README.md). If you prefer watching a video, rather than reading, check out the [Introduction Videos](https://youtu.be/K28CQMKr-rQ).

To access the Manager you will first need to login with the correct credentials (admin/secret for your local installation). Note that our account management and identity service includes features like a 'forgot password' flow. See [Realms, Users and Roles](https://github.com/openremote/openremote/wiki/User-Guide%3A-Realms%2C-users-and-roles) for more details.  

If you open the application you will get four main pages: [Map](#map), [Assets](#assets), [Rules](#rules), and [Insights](#insights). In addition there are a series of [Settings](#settings-and-access) to interconnect managers, change language, edit your account, and create users, roles, or realms. Also the service users for the [Manager APIs](#manager-apis) can be set here.

# Map

The `Map` page will show your map (see the custom deployment wiki if you like to [change your map](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment)). You can move, zoom, tilt, and pan the map. On the map all assets are shown which have a location as well a configuration item `show on dashboard` selected. Assets can both have static or dynamic locations (eg. a car, boat or plane). Once you select an asset a panel will appear, summarising the attributes of that asset and their current values. The `Asset details` button in this panel will bring you to the respective Asset page.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Demo%20Map.png)
_Figure 1. The Map view, here with the Demo Smart City, showing the map with different assets across the City_

# Assets

The `Asset` view gives you access to the respective assets and related attributes. You will see an asset tree structure on the left. On the right you will see the asset page of the selected asset. The asset page contains a Notes panel, an Attributes panel, a Location panel (if any) and a History panel.

The Notes and Attribute panels will give an overview of all attributes and their units. These can be meta-, sensor-, or control-data. For those attributes which can be changed via the UI, and for which you have the 'write' role as a user, you can type a value and press 'enter' or press the `send` arrow on the right (e.g. in below example the 'Manufacturer', 'Model' and 'Notes' attributes are manual inputs). Other attribute values could be live readings from sensors or are automatically updated by Rules.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Ozone%20Asset.png)
_Figure 2. An asset of the type 'environment'_

## Create an asset

With the correct admin permissions (you'll need your own [installation](https://github.com/openremote/openremote/blob/master/README.md)), you can create a new asset on the Assets page by clicking the `+` in the header of the asset tree. This will open a modal that shows the available asset types when scrolling down in the list. When you select one you will see the attributes and optional attributes. Optional attributes can be added by selecting them in this modal. Also decide whether it needs a parent by selecting the upper right pencil and selecting an asset in the asset tree. Click `Add` to create and add the asset. You will see the asset (and asset page) appear in the asset tree on the left. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Create%20Asset-Parent.png)
_Figure 3. Creating an asset of the type 'environment' with the Building selected as parent_

Often you will want to add or configure the attributes to define how they are used in the system, e.g. get values through agent link, make the attribute available in rules, or format its value. We'll explain how that works next.

## Add an attribute

On the Assets page enable `Edit asset` (on the top middle of the page) which changes the view to a complete list of all available attributes. Click `+ Add attribute` at the bottom of the list of attributes. In the modal you can choose optional attributes already associated with the asset type of the asset, or you can create a `custom attribute`. The custom attributes should have a `camelCase` name, which will automatically be translated to Sentence case with spaces. You can create attributes of prepared value types that are known by the system.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Add%20attribute.png)
_Figure 4. The Asset view in 'Edit mode' while adding an attribute_

## Configure attributes

For each attribute, while in `Edit asset` mode, you can expand it, which gives you the option to add configuration items or change existing ones. You can use configuration items for example to decide whether you want to store historical data and for how long, allow data to be used in rules, or whether a location should be displayed on the Map. See the wiki page [explaining all available configuration item options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Assets%2C-Agents-and-Attributes).

## Create an agent

Agents are a specific type of asset, which are used to connect to external sensors, actuators, gateways or services, using protocols. They are added in the same manner as assets by clicking the `+` in the header of the asset tree. This will open a modal that shows the available agent types at the top of the list. You will see the generic ones: [HTTP](https://github.com/openremote/openremote/wiki/User-Guide%3A-HTTP-Agent), [Websocket](https://github.com/openremote/openremote/wiki/User-Guide%3A-Websocket-Agent), TCP/IP, [UDP](https://github.com/openremote/openremote/wiki/User-Guide%3A-UDP-Agent) and [SNMP](https://github.com/openremote/openremote/wiki/User-Guide%3A-SNMP-Agent); as well as more specific ones like [Z-wave](https://github.com/openremote/openremote/wiki/User-Guide%3A-Z-Wave-Agent), [KNX](https://github.com/openremote/openremote/wiki/User-Guide%3A-KNX-Agent) or [Velbus](https://github.com/openremote/openremote/wiki/User-Guide:-Velbus-Agent-(TCP-IP-or-Socket)).
Once you create an Agent, the agent page will display the relevant attributes, required to establish an actual connection to the external world.

Some Agents have auto discovery (e.g. Z-wave) or use configuration files (e.g. KNX and Velbus). The Agent page will show a discovery button or a file selector. Once set correctly the Agent will also create an additional asset/attribute structure for all discovered or configured assets. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Create%20Agent.png)
![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Agent%20Page.png)
_Figure 5. Creating an Agent, using the HTTP Agent (top) creates the Agent asset page (bottom)_

Note that you can also connect to OpenRemote through the Manager APIs without using the UI, see the [paragraph: Manager APIs](#manager-apis).

## Link agents and assets

If the Agent doesn't support discovery or configuration files, you will manually need to link data coming in through your agents with the attributes of your created assets. We use attributes such as `Asset link` and `Agent link` for that. See the example for connecting to [OpenWeatherMap via an HTTP Agent to a Weather asset](https://github.com/openremote/openremote/wiki/User-Guide%3A-HTTP-Agent).

# Rules

The Rules view (see figures 6-9) allows you to build three types of rules:
* [WHEN-THEN Rules](#when-then-rules): allows you to set Left Hand Side conditions of available attributes, and trigger a Right Hand Side action for another attribute.
* [FLOW](#flow-rules): allows for processing attributes and converting them into new attributes. 
* [GROOVY](#groovy-rules): programming any advanced logic, using attributes in the system.
All rules have the option to use a time scheduler, which allows for have rules activated on a time schedule with a recurrence.

## WHEN-THEN Rules

WHEN-THEN rules are intended to define lefthand-side conditions for attributes which trigger a righthand-side action for another attribute. The righthand side can also handle e-mails or push notifications (using the OpenRemote consoles). 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20(2-2).png)
_Figure 6. A WHEN-THEN Rules example, which shows how, on the lefthand-side an asset type can be selected, while on the righthand-side the action, in this case a push notification, is defined._

The frequency on which rules trigger as well as a timer schedule can be set. 
The rule frequency, a dropdown on the upper right of each 'THEN' panel defines how frequent a rule can trigger. For example 'Always' means every time the lefthand side condition is triggered, but only after condition is unmet.
The Timer schedule (right next to the title field of the rule) allows you to set an occurrence period as well as repeat that occurrence. The below example sets the rule to be active until June 20, only on weekdays.

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20scheduling%20(2-2).png)
_Figure 7. The Rules trigger frequency (right) as well as time scheduler (left) allows for defining when rules are triggered and active. The scheduled example sets the rule to be active until June 20 2022, only on weekdays._

See the [WHEN-THEN Rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Use-When-Then-Rules) wiki for more details. 

## Flow Rules

Flow rules can be used to fill new attributes with processed other attributes. In the visual editor you can use `Input` (blue), `Processor` (green), and `Output` (purple) nodes, and wire them up. See the wiki [Flow Rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Use-Flow-Rules) for more details. The same scheduler as for WHEN-THEN rules is available for flow rules.

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20Flow.png)
_Figure 8. Flow rules to process data_

## Groovy Rules

Groovy rules are intended for more advanced processing and automation (see an example in figure 9). For more information see [Groovy Rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Create-Rules-with-Groovy-Editor).

![Manager Rules Scheduler](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20Groovy.png)
_Figure 9. Groovy rules for more advanced processing, logic or automation_

## Global versus Realm Rules

As an admin user of the system which can access all realms, you have the option to select 'Global' versus 'Realm' rules. Global rules allow you to use WHEN-THEN and Groovy rules, across realms. Realm users can only use Realm rules which are are part of their realm and can only access attributes within that realm.

# Insights

The Insights view (only visible in the desktop version, (see figure 10) allows you to create a single page report:
* Chart: allows you to select multiple attributes in the system and compare them (vertical comparison). By adding a second period you can also compare attributes for different time periods
* Attribute panel: allows for picking individual attributes, eg. KPIs and see there current performance over a period, as well as their relative or absolute change. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Smart%20City%20-%20Insights.png)
_Figure 10. The Insights view of the Demo Smart City, showing the soil temperature at Leuven Haven and a few random attribute panels._

# Settings and access

On the upper right of the Manager you see a Realm selector which allows switching between Realms (if you are an Admin user). Next to that the `:` dots give you access to a series of general settings as well as account access related settings. We will explain these here.

## Manager interconnect

You can link multiple instances of OpenRemote (as Edge Gateways) to a single Central instance of OpenRemote. To do that you first create a `Gateway Asset` on the Central instance of OpenRemote, which generates a Client ID and Secret. Next, use the `Manager Interconnect` on the Edge Gateway instance and fill in `Hostname`, `Realm` (of the Central instance), and the generated `Client ID` and `Client Secret`. See the [Edge Gateway documentation](https://github.com/openremote/openremote/wiki/User-Guide%3A-Edge-Gateway) for more details.
 
![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Interconnect%20(2).png)
_Figure 11. Several OpenRemote instances can be interconnected, e.g. connecting multiple instances on edge gateways to one central cloud hosted instance. The Manager Interconnect page, used at the edge instances (left) uses the keys which are created on the central instance by adding Edge gateway Assets (right)._

## Languages

OpenRemote currently supports 5 languages: English, German, Spanish, French and Dutch.

## Logs

The logs views shares information, warnings and errors of the different activities of OpenRemote. You can use it to understand the behaviour of the whole platform or debug issues, e.g. when you are seeing error with protocols or rules.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Logs.png)
_Figure 12. The Logs page to evaluate system behaviour._

## Account, Users, and Roles

The account page brings you to a few pages where you can (re)set you personal information or password. You can also monitor your past sessions or enable TFA via an Authenticator (currently disabled).

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Account%20(2).png)
_Figure 13. The account page which allows setting e.g. contact details (left) and reset passwords (right)._

If you have the correct access rights (role) for it you can also create new Users for the selected realm and decide which roles they will get assigned.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Users.png)
_Figure 14. Creating users for a selected Realm and assigning roles_

Same for the Roles. If you have the correct access rights you are allowed to create, adjust and enable roles. These roles define which parts of the system you are allowed to Read or Write to, e.g. system settings, assets, attributes, map, or rules. 

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Roles.png)
_Figure 15. Roles can be defined which can be linked to individual users_

## Realms

Only the Master realm Admin user can create `Realms` by accessing the master realm https://youradress/manager. Realms are separated projects which can be used for individual users or customers of your platform. Individual Realms can be reached at https://youradress/manager/?realm=realmname.

You can create a realm by adding a `Realm` name (single word, small letters), and a `Friendly name`. You can (temporarily) disable realms, which blocks access for any user.   

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Realms.png)
_Figure 16. Realms can be created to manage multiple independent projects within one OpenRemote instance_

# Manager APIs

OpenRemote also allows for interacting with the platform without using the UI. This can be used to synchronise attribute data with external clients, but also for a wider range of functions, e.g. accessing configurations, or creating new assets. To be able to authenticate you'll need to create a service user first. Next you have three types of APIs to choose from: HTTP, Websocket and MQTT. 

## Service users

Service users can be created by selecting the `Users` option, and selecting `Add user` in the `Service user` panel (see figure 17). The `Username` (ClientID) can be filled in by yourself using one string of letters, dashes, and numbers, while the `Secret` will be generated automatically once saved. Note that you also select the role(s), as these will define the access right for the Manager API.

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Service%20Users.png)
_Figure 17. Creating service users, with ClientID/Username, Secret and Roles for a selected Realm_

## HTTP, Websocket, and MQTT

The Manager API is compose of three APIs: HTTP, Websocket and MQTT: 
* HTTP API is the traditional request response API with live documentation available via Swagger UI (see https://youraddress/swagger/) or you can look at the [demo environment](https://demo.openremote.io/swagger/).
* Websocket API is a publish subscribe API that is event based.
* MQTT is another publish subscribe API which allows connecting to our MQTT broker

More information on these APIs regarding formats and authentication can be found in the wiki for [Manager API](https://github.com/openremote/openremote/wiki/User-Guide%3A-Manager-APIs)

# See Also
- [Custom Deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment)
- [Setting up an IDE](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Setting-up-an-IDE)
- [Working on the UI](Developer-Guide%3A-Working-on-the-UI)