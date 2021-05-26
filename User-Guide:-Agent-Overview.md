[Agents](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/asset/agent/Agent.java) are a special type of asset which link external services/devices with your OpenRemote system via protocols; agents can be put into the following categories:

* Specialised agents (Velbus, Z-Wave, KNX, etc.)
* Generic agents (HTTP, TCP, UDP, WS, MQTT, etc.)

## Agent <-> Protocol relationship
Each agent type has a corresponding [Protocol](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/asset/agent/Protocol.java) implementation; the Agent stores the configuration which is then passed to an instance of the Agent's Protocol implementation so there is a one to one relationship.

## Specialised agents
Specialised agents are ones that understand the message structure of the underlying devices/service and therefore generally require much less configuration in order to link attributes to them.

## Generic agents
Generic agents on the other hand understand nothing about the underlying devices/service and therefore generally require more configuration in order to link attributes to them. This gives a lot of flexibility in terms of what devices/services you can communicate with and the `Agent Link` configuration options make it possible to easily configure generic inbound/outbound value processing (convert data type, insert value into bigger message payload etc.), see below for details on these `Agent Link` configuration options.

## Agent links
Regular assets are connected to agents by adding an `Agent Link` configuration item to attributes that need connecting, each agent has its' own `Agent Link` configuration options but below are the options that are common to all and can be found in the [Agent link](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/asset/agent/AgentLink.java) class.

| Field | Description | Value type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `id` | The agent ID that is the target for this agent link | [Asset ID](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueType.java#L134) | Y |
| `valueFilters` | When an agent protocol updates the value of a linked attribute it can be desirable to filter that value to extract a specific piece of information that should actually be written to the linked attribute; this option defines a series of value filters that incoming messages should pass through before being passed to the agent protocol, the incoming message is passed to each filter in array order and the result of one is the input to the next (i.e. they are composite). The available value filters can be found from the known types in the javadoc but availabe types at the time of writing can be found below | [ValueFilter](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueFilter.java) | N |
| `valueConverter` | Defines a value converter map to allow for basic value type conversion; the incoming value will be converted to JSON and if this string matches a key in the converter then the value of that key will be pushed through to the attribute. An example use case is an API that returns `ACTIVE/DISABLED` text but you want to connect this to a Boolean attribute `true/false` | JSON Object | N |
| `writeValueConverter` | Similar to `valueConverter` but for outbound (Attribute -> Agent protocol) messages | JSON Object | N |
| `writeValue` | Text value to be used for outbound messages; can be used with any attribute type in combination with the dynamic placeholder (see below) or can be used with an attribute of type `ExecutionStatus` (i.e. executable attributes) to determine the value sent to the agent protocol when the attribute execution starts | Text | N |
| `messageMatchPredicate` | Used in combination with the `messageMatchFilters`; the predicate is applied to inbound messages (after the `messageMatchFilters` have been applied) and if the predicate matches then the message is said to match the attribute and the attribute will be updated by passing the original message through the value filter(s) and converter | [ValuePredicate](https://github.com/openremote/openremote/blob/a58951f6780176163bad7f58f79ba2a12eb75eb6/model/src/main/java/org/openremote/model/query/filter/ValuePredicate.java) | N |
| `messageMatchFilters` | Used in combination with the `messageMatchPredicate` to allow filtering the inbound message before the match predicate is evaluated | [ValueFilter[]](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueFilter.java) | N |


### Dynamic value injection
When writing to linked attributes it can be desirable to insert the written value into a bigger payload before sending to the agent protocol; the dynamic value placeholder `{$value}` makes this possible and every occurrence within the bigger payload is replaced by the value written to the linked attribute.

### Value filter known types

* [RegexValueFilter](https://github.com/openremote/openremote/blob/a58951f6780176163bad7f58f79ba2a12eb75eb6/model/src/main/java/org/openremote/model/value/RegexValueFilter.jav)
* [SubStringValueFilter](https://github.com/openremote/openremote/blob/a58951f6780176163bad7f58f79ba2a12eb75eb6/model/src/main/java/org/openremote/model/value/SubStringValueFilter.java)
* [JSONPathFilter](https://github.com/openremote/openremote/blob/a58951f6780176163bad7f58f79ba2a12eb75eb6/model/src/main/java/org/openremote/model/value/JsonPathFilter.java)


```
[
    {
        "type": "json",
        "path": "$..events[?(@.attributeState.ref.name == "targetTemperature")].attributeState.value"
    },
    {
        "type": "regex",
        "pattern": ".*(\\d)$",
        "matchGroup": 1
    },
    {
       "type": "substring",
       "beginIndex": 10,
       "endIndex": 15
    }
]
```