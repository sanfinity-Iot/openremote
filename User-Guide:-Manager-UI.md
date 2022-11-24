The Manager UI is the dashboard which gives you access to OpenRemote, and allows you to configure, monitor, and control your IoT platform. You can access it at `https://localhost` for the master realm, or at `https://localhost/manager/?realm=yourrealm`, for a specific realm. We'll explain the main features of the Manager UI, sometimes referring to examples from the [online Demo](https://openremote.io/demo/), for which you only have access in 'read' mode. To have full 'admin' access to all functionality you will first need to [install OpenRemote](https://github.com/openremote/openremote/blob/master/README.md). If you prefer watching a video, rather than reading, check out the [Introduction Videos](https://youtu.be/K28CQMKr-rQ).

To access the Manager you will first need to login with the correct credentials (admin/secret for your local installation). Note that our account management and identity service includes features like a 'forgot password' flow. See [Realms](#realms), [Users and Roles](#users-and-roles) for more details.  

If you open the application you will get four main pages: [Map](#map), [Assets](#assets), [Rules](#rules), and [Insights](#insights). In addition there are a series of [Settings](#settings-and-access) to interconnect managers, change language, edit your account, and create users, roles, or realms. Also the service users for the [Manager APIs](#manager-apis) can be set here.

# Map

The `Map` page will show your map (see the [custom deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment) wiki if you like to change your map). You can pan, zoom, and tilt the map. On the map all assets are shown which have a location as well the configuration item `show on dashboard` set. Assets can both have static or dynamic locations (eg. a car, boat or plane). You will see the direction an asset is facing or moving if the asset includes an attribute called 'direction'. When selecting an asset, a panel will show its attributes and values. The `Asset details` button in this panel will bring you to the respective Asset page.

As part of the [custom deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment) you can also configure assets to change their colour based on an attribute value (number, boolean, or string) and show a label with or without units. 

<kbd>![OpenRemote Map](https://user-images.githubusercontent.com/11444149/174818710-a9799894-d4fe-4828-9f5d-3763f8d48f06.png)</kbd>
_Figure 1. The Map view, here with the Demo Smart City, showing the map with different assets across the city. The ship is also showing its direction_

# Assets

The `Assets` page lets you view and modify assets and their attributes. You will see the asset tree structure on the left, and the details of the selected asset on the right. The asset page contains an Info panel, an Attributes panel, a Location panel (if any) and a History panel.

The Info and Attribute panels will give an overview of all attributes and their value. These can hold meta-, sensor-, or control-data. For attributes of which the value can be changed via the UI, and for which you have the 'write' role as a user, you can type a value and press 'enter' or press the `send` arrow on the right (e.g. in below example the 'Manufacturer', 'Model' and 'Notes' attributes are manual inputs). Other attribute values could be live readings from sensors or are automatically updated by Rules.

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Ozone%20Asset.png)</kbd>
_Figure 2. An asset of the type 'environment'_

## Create an asset

With the correct permissions (not available in the demo, you'll need your own [installation](https://github.com/openremote/openremote/blob/master/README.md)), you can create a new asset on the Assets page by clicking the `+` in the header of the asset tree. This will open a modal that shows the available asset types. When you select one you will see the attributes and optional attributes. Optional attributes can be added by selecting them in this modal. You can set or change its parent by selecting the upper right pencil and selecting an asset in the asset tree. Click `Add` to create the asset.

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Create%20Asset-Parent.png)</kbd>
_Figure 3. Creating an asset of the type 'environment' with the Building selected as parent_

Often you will want to add or configure attributes to define how they are used in the system, e.g. get values through agent link, make the attribute available in rules, or format its value. We'll explain how that works next.

## Add an attribute

On the Assets page enable `Edit asset` (on the top middle of the page) which changes the view to a list of all the assets attributes. Click `+ Add attribute` at the bottom of the list of attributes. In the modal you can choose optional attributes already associated with the asset type of the asset, or you can create a `custom attribute`. The custom attributes should be given a `camelCase` name, which will automatically be translated to Sentence case with spaces. You can create attributes of prepared value types that are known by the system.

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Add%20attribute.png)</kbd>
_Figure 4. The Asset view in 'Edit mode' while adding an attribute_

## Configure attributes

While in `Edit asset` mode you can expand each attribute, which gives you the option to add configuration items or change existing ones. You can use configuration items for example to decide whether you want to store historical data and for how long, allow data to be used in rules, or whether a location should be displayed on the Map. The most used configuration items are:

| Configuration items | Description |
| :--- | :--- |
| `Access restricted read` | Restricted users can read if they have read attribute access to the asset |
| `Access restricted write` | Restricted users can write if they have write attribute access to the asset |
| `Data points max age days` | Time period for data to be stored |
| `Label` | Adds a friendly name, replacing the default name |
| `Read only` | Data can not be filled via UI, only by agents or rules. |
| `Rule state` | Adds attribute as option to select in rules, on lefthand side |
| `Rule reset immediate` | Allows rule to retrigger immediately. Can be useful for event based data |
| `Show on dashboard` | Used in combination with 'location' will display asset on the Map view |
| `Store data points` | Stores data points in the database, default for one month |
| `Units` | Adds a unit to the attribute value, see [composition and options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Assets%2C-Agents-and-Attributes#attribute-descriptor) |

See the wiki page [explaining all available configuration item options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Assets%2C-Agents-and-Attributes#asset-model). Don't forget to save the asset after making changes.

## Create an agent

Agents are a specific type of asset used to connect to external sensors, actuators, gateways, or services using protocols. They are added in the same manner as assets by clicking the `+` in the header of the asset tree. This will open a modal that shows the available agent types at the top of the list. You will see the generic ones: [HTTP](https://github.com/openremote/openremote/wiki/User-Guide%3A-HTTP-Agent), [Websocket](https://github.com/openremote/openremote/wiki/User-Guide%3A-Websocket-Agent), [MQTT](https://github.com/openremote/openremote/wiki/User-Guide%3A-MQTT-Agent), [TCP](https://github.com/openremote/openremote/wiki/User-Guide%3A-TCP-Agent), [UDP](https://github.com/openremote/openremote/wiki/User-Guide%3A-UDP-Agent) and [SNMP](https://github.com/openremote/openremote/wiki/User-Guide%3A-SNMP-Agent); as well as more specific ones like [Z-wave](https://github.com/openremote/openremote/wiki/User-Guide%3A-Z-Wave-Agent), [KNX](https://github.com/openremote/openremote/wiki/User-Guide%3A-KNX-Agent) or [Velbus](https://github.com/openremote/openremote/wiki/User-Guide:-Velbus-Agent-(TCP-IP-or-Socket)).
Once you create an Agent, the agent page will display the relevant attributes, required to establish an actual connection to the external world.

Some Agents have auto discovery (e.g. Z-wave) or use configuration files (e.g. KNX and Velbus). The Agent page will show a discovery button or a file selector. Once set correctly the Agent will also create an additional asset/attribute structure for all discovered or configured assets. 

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Create%20Agent.png)</kbd>
<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Agent%20Page.png)</kbd>
_Figure 5. Creating an Agent, using the HTTP Agent (top) creates the Agent asset page (bottom)_

Note that you can also connect to OpenRemote through the Manager APIs without using the UI, see the [paragraph: Manager APIs](#manager-apis).

## Link agents and assets

If the Agent doesn't support discovery or configuration files, you will manually need to link data coming in through your agents to the attributes of your assets. We use [configuration items](https://github.com/openremote/openremote/wiki/User-Guide%3A-Assets%2C-Agents-and-Attributes) on attributes such as `Agent link` and `Attribute link` for that. See the tutorial for connecting to [OpenWeatherMap via an HTTP Agent to a Weather asset](https://github.com/openremote/openremote/wiki/Tutorial%3A-Open-Weather-API-using-HTTP-Agent).

## Filtering assets and agents

In the asset tree on the left, you can filter assets by typing their name. The advanced filter also allows selecting an asset type, typing the attribute name (without spaces) and filter on a value.

<kbd>![Filtering assets](https://user-images.githubusercontent.com/11444149/174821811-4cb80c13-0416-4681-a1af-3ef9d622a73f.png)</kbd>
_Figure 6. Asset filtering by typing the asset name (left) or by using the advanced filter, selecting an asset type, typing the attribute name (without spaces) and filter on an attribute value_

## Grouping assets and group control

In the asset tree you can multi-select assets and collectively add them as children to a group, just by dragging them onto another asset. You can use it for example to add a series of lights as children to a room or building. Additionally, by adding [this Control Groovy rule](https://github.com/openremote/openremote/blob/master/test/src/test/resources/org/openremote/test/rules/ChildAssetControl.groovy) for each group asset, you can control all these child assets at the same time, just by creating the corresponding attributes (same name and type) in the parent asset. So in case of the light example: by creating an 'onOff' (boolean) attribute in the room or building asset you can control all lights at the same time. Note that you create a [Control Groovy rule](https://github.com/openremote/openremote/blob/master/test/src/test/resources/org/openremote/test/rules/ChildAssetControl.groovy) for each parent asset and adjust the parentAssetID to the ID of the parent asset. You can find the ID in the URL of the asset page. Additionally, don't forget to add the configuration item 'Rule state' to the attribute of both the parent and child assets.

# Rules

The Rules page (only available on desktop screen sizes) allows you to build three types of rules:
* [WHEN-THEN Rules](#when-then-rules): When certain conditions created with asset attributes are met, then trigger an action for another attribute.
* [FLOW](#flow-rules): Process attributes and convert them into new attributes with a simple drag-and-drop interface. 
* [GROOVY](#groovy-rules): programming any advanced logic, using attributes in the system.
All rules can be set to only be active during a (recurring) event set with the scheduler.

Note that you need to add the configuration item 'Rule state' to both the attributes used on the left-hand-side and right-hand-side of the rule, to make the rule actually fire an action.

## When-Then Rules

When-Then rules use conditions set for attributes to trigger an action for another attribute. The actions do not only include controlling assets, but can also be used to send e-mails or push notifications to mobile apps (using the OpenRemote consoles). 

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20(2-2).png)</kbd>
_Figure 7. A When-Then example, which shows how, on the When-side an asset type can be selected, while on the Then-side the action, in this case a push notification, is defined._

The frequency on which rules trigger as well as a timer schedule can be set. 
The rule frequency, a dropdown on the upper right of each 'Then' panel defines how frequent a rule can trigger. For example 'Always' means every time the  condition is triggered, but only after the condition has been false (so not continuously on being true).
The scheduler (right next to the name field of the rule) lets you set an occurrence period as well as repeat that occurrence.

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20scheduling%20(2-2).png)</kbd>
_Figure 8. The Rules trigger frequency (right) as well as time scheduler (left) defines when rules are triggered and active. The scheduled example sets the rule to be active until June 20 2022, only on weekdays._

See the [When-Then rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Use-When-Then-Rules) wiki for more details. 

## Flow Rules

Flow rules can be used to fill (new) attributes with processed other attributes. In the visual editor you can use `Input` (blue), `Processor` (green), and `Output` (purple) nodes, and wire them up. See the wiki [Flow Rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Use-Flow-Rules) for more details.

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20Flow.png)</kbd>
_Figure 9. Flow rules to process data_

## Groovy Rules

Groovy rules are intended for more advanced processing and automation (see an example in figure 10). For more information see [Groovy Rules](https://github.com/openremote/openremote/wiki/User-Guide%3A-Create-Rules-with-Groovy-Editor).

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Rules%20Groovy.png)</kbd>
_Figure 10. Groovy rules for more advanced processing, logic or automation_

## Global versus Realm Rules

As an admin user of the system who can access all realms, you have the option to select 'Global' versus 'Realm' rules. Global rules allow you to use When-Then and Groovy rules that can access assets across realms. Realm users can only use Realm rules which are a part of their realm and can only access attributes within that realm.

# Insights

The Insights page (see figure 11) allows you to create multiple dashboards. You can:
* Share dashboards with other users or keep your dashboard private. 
* Define the dashboard behaviour for different screen sizes and optimise the design for a specific screen.

<kbd>![](https://user-images.githubusercontent.com/11444149/203108105-563f030c-3def-46f2-be1f-41c440e83bb7.png)</kbd>
_Figure 11. The Insights view of the Demo Smart City, showing a dashboard with the three different types of widgets._

# Settings and access

Admin users of the 'Master' realm see the Realm selector on the top right to switch between Realms. Next to that the dots give you access to a series of general settings as well as account access related settings. We will explain these here:

## Manager interconnect

You can link multiple instances of OpenRemote (as Edge Gateways) to a single Central instance of OpenRemote. The Gateways will be visible in the Central manager to create a single point of remote access and data collection. The Edge Gateways function each connect to their local assets and run independently with their own rules.
See the [Edge Gateway documentation](https://github.com/openremote/openremote/wiki/User-Guide%3A-Edge-Gateway) for more details.
 
<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Interconnect%20(2).png)</kbd>
_Figure 12. Several OpenRemote instances can be interconnected, e.g. connecting multiple instances on edge gateways to one central cloud hosted instance. The Manager Interconnect page, used at the edge instances (left) uses the keys which are created on the central instance by adding Edge gateway Assets (right)._

## Languages

OpenRemote currently supports 8 languages: English, German, French, Spanish, Portuguese, Italian, Chinese, and Dutch.

## Logs

The logs page shows information, warnings and errors of the different activities of OpenRemote. You can use it to understand the behaviour of the whole platform or debug issues, e.g. errors connecting agent with device.

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Logs.png)</kbd>
_Figure 13. The Logs page to evaluate system behaviour._

## Account

On the Account page you can (re)set you personal information or password. You can also monitor your past sessions or enable 2FA via an Authenticator (currently disabled).

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Account%20(2).png)</kbd>
_Figure 14. The account page with contact details (left) and reset passwords (right)._

## Users and Roles

If you have the correct access rights (role or permissions) for it you can also create new Users for the selected realm and set the roles of a user. 

You can also give a user restricted access to one or more assets. Note that you additionally have to set a configuration item 'Access restricted user read/write' on each attribute you want to give the user access to ([see 'Configure attributes'](https://github.com/openremote/openremote/wiki/User-Guide:-Manager-UI#configure-attributes)). 

An example of using 'restricted access' is when you are connecting up an apartment complex and you want each tenant only to have access to the assets of his or her apartment. By leaving out the configuration item 'Access restricted user read/write' for some attributes, you can hide these from the tenant.

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20restricted%20user.png)</kbd>
_Figure 15. Creating users for a selected realm and assigning roles_

Same for Roles, with the correct permissions, you can create and edit roles. These roles define which parts of the system a user is allowed to Read or Write, e.g. system settings, assets, attributes, map, or rules. Also see the userguide: [Realms, users and roles](https://github.com/openremote/openremote/wiki/User-Guide%3A-Realms%2C-users-and-roles).

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Roles.png)</kbd>
_Figure 16. Roles are made of a set of permissions_

## Realms

Only the Master realm Admin user can create `Realms` by accessing the master realm `https://youradress/manager`. Realms are separated projects which can be used for individual users or customers of your platform. Individual Realms can be reached at `https://youradress/manager/?realm=realmname`. Note that the Master realm Admin user should have created a new user in this realm first, before the individual Realm can be accessed.

You can create a realm by adding a `realmname` name (single word, lower case letters), and a `Friendly name`. You can (temporarily) disable realms, which blocks access for any user.    

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Realms.png)</kbd>
_Figure 17. Realms can be created to manage multiple independent projects within one OpenRemote instance_

Also see the userguide: [Realms, users and roles](https://github.com/openremote/openremote/wiki/User-Guide%3A-Realms%2C-users-and-roles).

## Auto provisioning of devices

If you are an OEM, developing and producing your own hardware, you can provision your devices and OpenRemote to automatically have your devices connecting, once they get online. Using certificates (we currently support X.509) your devices will register and automatically generate and connect to an asset of a defined type in the OpenRemote Manager (see figure 18). For details, check out the wiki about ['Auto provisioning'](https://github.com/openremote/openremote/wiki/User-Guide%3A-Auto-Provisioning).

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Auto%20provisioning%20.png)</kbd>
_Figure 18. Auto provisioning of devices_

# Manager APIs

The Manager APIs let you interact with OpenRemote without using the UI. This can be used to e.g synchronize attribute data with external clients, accessing configurations, or creating new assets. To authenticate you'll need to create a service user first on the Users page. We have three types of APIs to choose from: HTTP, MQTT, and Websocket. 

## Service users

Service users can be created on the `Users` page, and selecting `Add user` in the `Service user` panel (see figure 18). The `Username` (ClientID) can be set using letters, dashes, and numbers, while the `Secret` will be generated automatically once saved. Note that you also select the role(s).

<kbd>![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Manager%20-%20Service%20Users.png)</kbd>
_Figure 18. Creating service users, with Username, Secret and Roles for a selected Realm_

## HTTP, MQTT, and Websocket

The Manager API is compose of three APIs: HTTP, MQTT, and Websocket: 
* HTTP API is the traditional request response API with live documentation available via Swagger UI (see `https://youraddress/swagger/`) or you can look at the [demo environment swagger](https://demo.openremote.app/swagger/).
* MQTT is a publish-subscribe API which allows connecting to our MQTT broker
* Websocket API is a publish-subscribe API that is event based.

More information on these APIs regarding formats and authentication can be found in the wiki for [Manager APIs](https://github.com/openremote/openremote/wiki/User-Guide%3A-Manager-APIs)

# See Also
- [Custom Deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment)
- [Setting up an IDE](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Setting-up-an-IDE)
- [Working on the UI](Developer-Guide%3A-Working-on-the-UI)