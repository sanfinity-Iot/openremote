The examples below describe interactively linking asset attributes to Websocket Servers using the [WebsocketClientProtocol](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/websocket/WebsocketClientProtocol.java). The following examples assume that you are running the [Demo docker compose profile](https://github.com/openremote/openremote/wiki/Developer-Guide:-Docker-compose-profiles#demo-docker-composeyml).

# Basic Websocket Server connection

The following example connects to the OpenRemote 3.0 Manager running on `https://localhost`.

1. Login to the manager UI (`https://localhost/master` `admin/secret`)
2. Select the Assets tab
3. Select the following asset from the Asset tree `Tenant A -> Smart Building -> Apartment 1 -> Service Agent (Simulator)`
3. Click `Edit asset` in the top right corner
3. Enter `websocket` in the New attribute input and select type as `Websocket Client` then click `Add attribute`
3. Expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows:
   * `Endpoint URI`: `http://localhost:8080/websocket/events?Auth-Realm=master`
   * `OAuth Grant`: `{
    "grant_type": "password",
    "tokenEndpointUri": "http://localhost:8080/auth/realms/master/protocol/openid-connect/token",
    "client_id": "openremote",
    "username": "admin",
    "password": "secret"
}`
   *  `Subscriptions`: `[
  {
     "type": "websocket",
     "body": "SUBSCRIBE:{\"eventType\": \"attribute\", \"filter\": {\"filterType\": \"asset-id\", \"assetIds\": [\"3rsAZ4SwFKUEBgtjVtlGb1\"]}}"
  }
]`

**Make sure you click `Add item` when creating new attribute configuration items**.

7. Click `Save asset` at the bottom of the screen

You now have a basic Websocket client protocol ready to be linked to by asset attributes; the protocol configuration status should show as `CONNECTED`.


1. Select the following asset from the Asset tree `Tenant A -> Smart Building -> Apartment 1 -> Living Room`
2. Click `Edit asset` in the top right
3. Add a new attribute:
   * Name: `websocketCo2Level`
   * Type: `Number`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * `Agent protocol link`: `Service Agent (Simulator) -> websocket`
   * `Read only`: `true`
   * `Message match filters`: `[
    {
        "type": "substring",
        "beginIndex": 6
    },
    {
        "type": "jsonPath",
        "path": "$..attributeState.attributeRef"
    }
]`
   * `Message match predicate`: `{
    "predicateType": "string",
    "match": "CONTAINS",
    "value": "co2Level"
}`
   * `Value filters`: `[
{
        "type": "substring",
        "beginIndex": 6
    },
    {
        "type": "jsonPath",
        "path": "$..events[?(@.attributeState.attributeRef.attributeName == \"co2Level\")].attributeState.value"
    },
    {
        "type": "jsonPath",
        "path": "$.min()"
    }
]`
   * `Subscriptions`: `[
  {
     "type": "websocket",
     "body": "EVENT:{\"eventType\": \"read-asset-attributes\", \"assetId\": \"3rsAZ4SwFKUEBgtjVtlGb1\", \"attributeNames\": [\"co2Level\"]}"
  }
]`

**NOTE: At this point you have enough to be able to send data to the UDP server (just write to the attribute the data you wish to send)**

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