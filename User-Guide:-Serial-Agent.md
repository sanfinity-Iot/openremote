Connect to a Serial Server.

## Agent configuration
The following describes the supported agent configuration attributes:

| Attribute | Description | Value type | Required |
| ------------- | ------------- | ------------- | ------------- |
| `serialPort` | Serial port device address (e.g. `COM1` or `/dev/ttyACM0`) | Text | Y |
| `serialBaudrate` | Serial port baudrate | Integer (Default = `38400`) | Y |

As this is a generic IO Agent the optional attributes described in the [Generic Agent Overview](https://github.com/openremote/openremote/wiki/User-Guide:-Agent-Overview#generic-agents-io-agents) can also be used.

## Agent link
For attributes linked to this agent, the `Default` [Agent Link](./User-Guide:-Agent-Overview#agent-links) should be used with a `type` value of `Default`.
