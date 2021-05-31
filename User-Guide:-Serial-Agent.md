Connect to a Serial Server.

## Agent configuration
The following describes the supported agent configuration attributes:

| Attribute | Description | Value type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `host` | TCP server hostname or IP address | [Hostname or IP address](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueType.java#L153) | Y |
| `port` | TCP server port | [Port number](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/value/ValueType.java#L148) | Y |

As this is a generic IO Agent the optional attributes described in the [Generic Agent Overview](https://github.com/openremote/openremote/wiki/User-Guide:-Agent-Overview#generic-agents-io-agents) can also be used.

## Agent link
For attributes linked to this agent, the `Default` [Agent Link](./User-Guide:-Agent-Overview#agent-links) should be used with a `type` value of `Default`.
