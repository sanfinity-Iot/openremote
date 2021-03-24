# WIP

# What is an Asset?
## Assets
An asset can be anything that is connected to your system in some way. An asset is always attached to a realm, so data is not shared between realms. OpenRemote stores your current asset status in its database, and manages it in rules and flows. This collectively is called the context of your OpenRemote system.

An asset might be an actual external service or an installed device at some location. There are also special assets such as an Agent, which links external services and devices with other assets.

You can use the asset hierarchy to model an organizational structure. E.g. A city has buildings, which has floors, which have presence sensors.

Each asset can have multiple attributes that hold a value. An attribute can be configured to: retreive its value from an agent, store its data points, be used in rules, and much more. You can read how to use them [below](#how-to-create-an-asset-attribute-and-its-configuration-items)

## Asset types
Assets that are used often in the system can be defined as an Asset Type with has several specified attributes.
This bring the following benefits:
- Consistent use of asset attributes: they will always be of the same name and type;
- Configure which Asset Types can be added and used in rules through the [manager config](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment);
- An icon + colour to identify them easily.

## Agents
An agent is a subset of an Asset that is associated with a Protocol. A protocol instance is responsible for connecting devices and services to the OpenRemote context broker, and describes the communication between them. The agent itself holds the configuration parameters of this protocol instance. Each agent will provide an instance of its protocol.

# How to create an asset, attribute and its configuration items

## Creating an asset
This is done by setting their configuration items. In the Manager UI you can do this by going to the asset on the Assets page, enabling the 'Edit mode', and selecting the attribute you can to configure.   stored, and each attribute can be configured to be used in a certain way.

## Adding an attribute
At the bottom of the attribute list you can click `+ Add attribute` to bring up a modal. In that modal you can select attributes already associated with the asset type of the asset, or you can create a `custom attribute`.
The custom attribute can be of on of the following attribute types:

## Attribute configuration items
Base set for assets:

**Protocol/Service**
* Agent link: Links the attribute to an agent, connecting it to a sensor and/or actuator with required configuration properties encapsulated in the concrete protocol specific org.openremote.model.asset.agent.AgentLink.
* Attribute link: Links the attribute to another attribute, so an attribute event on the attribute triggers the same attribute event on the linked attribute.

**Access permission**
* Access public read: Marks the attribute as readable by unauthenticated public clients.
* Access public write: Marks the attribute as writable by unauthenticated public clients.
* Access restricted read: Marks the attribute as readable by restricted clients and therefore users who are linked to the asset.
* Access restricted write: Marks the attribute as writable by restricted clients and therefore users who are linked to the asset.
* Read only: Marks the attribute as read-only for non-superuser clients. South-bound AttributeEvents by regular or restricted users are ignored. North-bound AttributeEvents made by protocols and rules engine are possible.

**Data point**
* Store data points: Can be set to false to prevent attribute values being stored in time series database; otherwise any attribute which also has an AGENT_LINK meta item or STORE_DATA_POINTS is set to true, will be stored.
* Data points max age: How long to store attribute values in time series database (data older than this will be automatically purged)
* Has predicted data points: Could possibly have predicted data points

**Rule**
* Rule event expires: Set maximum lifetime of AssetState temporary facts in rules, for example "PT1H30M5S". The rules engine will remove temporary AssetState facts if they are older than this value (using event source/value timestamp, not event processing time). The default expiration for asset events can be configured with environment variable RULE_EVENT_EXPIRES.
* Rule event: Should attribute writes be processed by the rules engines as temporary facts. When an attribute is updated, the change will be inserted as a new AssetState temporary fact in rules engines. These facts expire automatically after a defined time, see RULE_EVENT_EXPIRES. If you want to match (multiple) {}s for the same attribute over time, to evaluate the change history of an attribute, add this meta item.
* Rule state:Can be set to false to exclude an attribute update from being processed by the rules engines as {} facts, otherwise any attribute that also has an #AGENT_LINK} meta item or #RULE_STATE} is true, will be processed with a lifecycle that reflects the state of the asset attribute. Each attribute will have one fact at all times in rules memory. These state facts are kept in sync with asset changes: When the attribute is updated, the fact will be updated (replaced). If you want evaluate the change history of an attribute, you typically need to combine this with #RULE_EVENT}.

**Formatting/Display**
* Label: A human-friendly string that can be displayed in UI instead of the raw attribute name.
* Units: Indicates the units associated with the value, there's some special handling for Boolean and Date values but otherwise the value type should be numeric. Units are intended for UI usage and should support internationalisation, custom unit types can be composed e.g. ["kilo", "metre", "per", "hour"] => "km/h" see org.openremote.model.Constants for well known units that UIs should support as a minimum. Currencies get special handling and should be represented using the upper case 3 letter currency code as defined in ISO 4217
* Format: ValueFormat to be applied when converting the associated Attribute to string representation.
* Contraints: ValueConstraints to be applied to the Attribute value; these override any constraints defined on any of the descriptors associated with the attribute. (AllowedValues, Future, FutureOrPresent, Max, Min, NotBlank, NotEmpty, NotNull, Past, PastOrPresent, Pattern, Size).
```{
"constraints": [
  {
     "type": "max",
     "max": 999999
  }
]}```

* Secret: Marks the value as secret and indicates that clients should display this in a concealed manner (e.g. password input with optional show)
* Multiline: Indicates that any input should support multiline text entry
* Show on dashboard: If there is a dashboard, some kind of attribute overview, should this attribute be shown.
* Momentary: Indicates that the button input should send the true/on/pressed/closed value when pressed; and then send the false/off/released/open value when released.

For agent specific configuration items check their wiki page.