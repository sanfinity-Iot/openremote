Agents can be put into the following categories:

* Specialised agents (Velbus, Z-Wave, KNX, etc.)
* Generic agents (HTTP, TCP, UDP, WS, MQTT, etc.)

## Specialised agents
Specialised agents are ones that understand the message structure of the underlying devices/service and therefore generally require much less configuration in order to link attributes to them.

## Generic agents
Generic agents on the other hand understand nothing about the underlying devices/service and therefore generally require more configuration in order to link attributes to them. This gives a lot of flexibility in terms of what devices/services you can communicate with and the `agentLink` configuration options make it possible to easily configure generic inbound/outbound value processing (convert data type, insert value into bigger message payload etc.).

## Agent linked attributes
Regular assets are connected to agents by adding an `agentLink` configuration item to the attributes that need connecting [Agent link](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/asset/agent/AgentLink.java)

## Generic Agent Link Configuration
Whilst the following are mostly applicable to the generic protocols, it is possible to use any of these configuration options on any agent linked attribute in order to do basic value processing before the value is passed to the agent for processing (can be useful if you want an attribute with a different value type to what the linked agent expects).


## Dynamic values
When writing to linked attributes it can be desirable to use insert the written value into a bigger payload before sending to the protocol; the dynamic value placeholder `{$value}` makes this possible and every occurrence within the bigger payload is replaced by the value written to the linked attribute.


## Response value filtering
When a generic protocol returns a payload it can be desirable to filter that payload to extract a specific piece of information that should actually be written to the linked attribute; that is the purpose of the `Value filters` configuration item for protocol linked attributes, it takes an array of [Value filters](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueFilter.java) as serialised `JSON Objects` e.g.:

```
[
    {
        "type": "json",
        "path": [
             "response",
             "body"
        ]
    },
    {
        "type": "regex",
        "pattern": ".*(\\d)$",
        "matchGroup": 1
    }
]
```

The payload returned from the server is passed through each filter in the order specified in the `JSON Array`.