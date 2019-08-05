The examples below describe interactively linking asset attributes to Websocket Servers using the [WebsocketClientProtocol](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/websocket/WebsocketClientProtocol.java). The following examples assume that you are running the [Demo docker compose profile](https://github.com/openremote/openremote/wiki/Developer-Guide:-Docker-compose-profiles#demo-docker-composeyml).

# Basic Websocket Server connection

The following example connects to the OpenRemote 3.0 Manager running on `https://localhost/`:

1. Login to the manager UI (`https://localhost/master` `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left
3. Set the following:
   * `Asset name`: `UDP Agent`
   * `Parent asset`: `Tenant A -> Smart Building` and select 'OK'
   * `Asset Type`: `Agent`
4. Click `Create asset` at bottom of screen
5. Click `Edit asset` and add a new attribute:
   * `Name`: `udpClient`
   * `Type`: `UDP Client`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows:
   * `UDP server hostname/IP`: `127.0.0.1`
   * `UDP server port`: `60123`

**Make sure you click `Add item` when creating new attribute configuration items**.

7. Click `Save asset` at the bottom of the screen

You now have a basic UDP client protocol ready to be linked to by asset attributes; the defaults assume that the UDP server sends/receives ASCII text, this can be changed to binary mode by adding one of the following configuration items:

* `Convert to/from HEX string` - Represents `bytes` as a HEX string e.g. `00FA3349`
* `Convert to/from binary string` - Represents `bytes` as a binary string e.g. `01011100101`

**NOTE: The protocol configuration status will show as `CONNECTED` even if the server is not actually reachable this is due to the fact `UDP` has no notion of a connection.**

1. Select the Tenant A -> Smart Building asset in the asset list
2. Click `Edit asset` in the top right
3. Add a new attribute:
   * Name: `udpSend`
   * Type: `Text`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * `Agent protocol link`: `UDP Agent -> udpClient`

**NOTE: At this point you have enough to be able to send data to the UDP server (just write to the attribute to data you wish to send)**

## Executable attributes
It is possible to create executable attributes (i.e. add the `Executable` configuration item) for the UDP protocol provided the `Write value` configuration item is specified for the linked attribute, this should contain the value to send to the server when the attribute is executed.

## Reading responses
It is possible to display responses from the UDP server by using the `Polling interval (ms)` configuration item in combination with the `Write value` configuration item and then based on the interval specified the protocol will send the write value to the server and wait for a response and this response will be passed through to the linked attribute.

**NOTE: Due to the connection-less nature of UDP and the variety of behaviours different servers exhibit it is possible to configure whether or not the server responds to a given payload (this can be configured on the protocol configuration and/or per attribute using the `UDP server always responds` configuration item). This must be correctly configured when using a mix of polling/non-polling linked attributes to ensure the response is correctly matched to the send payload - for polling linked attribute it is assumed that the server responds (not much point polling it otherwise)**

## Dynamic values
It is possible to use the dynamic value placeholder `{$value}` within the `Write value` configuration item and then any value written to the linked attribute will be inserted into the `Write value` replacing each occurrence of the placeholder.

## Response value filtering
This protocol supports response value filtering for more information see [here](https://github.com/openremote/openremote/wiki/User-Guide:-Generic-protocols#response-value-filtering).

## Protocol configuration items
* `UDP server hostname/IP`**required** - Hostname/IP of the UDP server
* `UDP server port`**required** - Port of the UDP server
* `UDP client bind port` - Port that the client should bind to (if not specified then a random ephemeral port will be used)
* `Charset` - The charset to use when converting ASCII to `byte[]` before sending
* `Convert to/from binary string` - Indicates that the server expects/sends raw `bytes` and these will be represented as binary strings e.g. `01001010110`
* `Convert to/from HEX string` - Indicates that the server expects/sends raw `bytes` and these will be represented as HEX strings e.g. `0FABCD20`
* `Response timeout (ms)` - How long should the client wait for a response from the server before possibly re-sending the payload
* `Send retries` - How many times to retry sending if a response is not received within the `Response timeout (ms)` used by linked attributes that expect a response
* `UDP server always responds` - Indicates that the server always responds to any payload

## Linked attribute configuration items
* `Write value` - Indicates the payload to be written to the server (can contain dynamic placeholders)
* `Value filters` - Filters to apply to response payload before updating the linked attribute
* `Response timeout (ms)` - Overrides value set on the protocol
* `Send retries` - Overrides value set on the protocol
* `UDP server always responds` - Overrides value set on the protocol

# See also
- [[Use UI components|User-Guide: UI components]]
- [[Demo Smart Building|Demo-Smart-Building]]
- [Get Started](https://openremote.io/get-started-manager/)