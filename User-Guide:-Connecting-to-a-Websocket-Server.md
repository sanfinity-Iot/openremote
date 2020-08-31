The examples below describe interactively linking asset attributes to Websocket Servers using the [WebsocketClientProtocol](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/websocket/WebsocketClientProtocol.java). The following examples assume that you are running the [Demo docker compose profile](https://github.com/openremote/openremote/wiki/Developer-Guide:-Docker-compose-profiles#demo-docker-composeyml).

# Basic Websocket Server connection

The following example connects to the OpenRemote 3.0 Manager running on `https://localhost` and subscribes to `Attribute Events` for the `Apartment 1` `Living Room` asset.

1. Login to the manager UI (`https://localhost/manager` `admin/secret`)
2. Select the Assets tab
3. Select the following asset from the Asset tree `Smart City -> Simulator Agent`
3. Click `Edit asset` in the top right corner
3. Enter `websocket` in the New attribute input and select type as `Websocket Client` then click `Add attribute`
3. Expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows:
   * `Endpoint URI`: `wss://localhost/websocket/events?Auth-Realm=master`
   * `OAuth Grant`: `{
    "grant_type": "password",
    "tokenEndpointUri": "https://localhost/auth/realms/master/protocol/openid-connect/token",
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

We'll now add a linked attribute to show the CO2 Level of the Living room:

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
5. Click `Save asset`

The `websocketCo2Level` should show the same value as the `CO2 level` attribute, now go to the `Service Agent (Simulator)` and expand the `apartmentSimulator` then in the `Living Room:co2Level` field enter an integer e.g. `500` then click `Write simulator state`, now go back to the `Living Room` asset and both the `CO2 level` and `websocketCo2Level` attributes should show the new value e.g. `500`.

## Writable attributes

## Executable attributes
It is possible to create executable attributes (i.e. add the `Executable` configuration item) for this protocol provided the `Write value` configuration item is specified for the linked attribute, this should contain the value to send to the server when the attribute is executed.

## Dynamic value
It is possible to use the dynamic value placeholder `{$value}` within the `Write value` configuration item and then any value written to the linked attribute will be inserted into the `Write value` replacing each occurrence of the placeholder.

Here's an example for creating an attribute to write to the `Living Room` `Ceiling lights (range)` attribute using the dynamic value functionality:

1. Select the following asset from the Asset tree `Tenant A -> Smart Building -> Apartment 1 -> Living Room`
2. Click `Edit asset` in the top right
3. Add a new attribute:
   * Name: `websocketWrite`
   * Type: `Number`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * `Agent protocol link`: `Service Agent (Simulator) -> websocket`
   * `Write value`: `"EVENT:{\"eventType\": \"attribute\", \"attributeState\": {\"attributeRef\": {\"entityId\": \"3rsAZ4SwFKUEBgtjVtlGb1\", \"attributeName\": \"lightsCeiling\"}, \"value\": {$value}}}"`
5. Click `Save asset`

Now you can write a value (`0-100`) to the new `websocketWrite` attribute and it will be written to the `Ceiling lights (range)` attribute.

Let's create an executable attribute to set the ceiling lights to 30%:

1. Click `Edit asset` in the top right
2. Add a new attribute:
   * Name: `websocketExec`
   * Type: `String`
3. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * `Agent protocol link`: `Service Agent (Simulator) -> websocket`
   * `Write value`: `"EVENT:{\"eventType\": \"attribute\", \"attributeState\": {\"attributeRef\": {\"entityId\": \"3rsAZ4SwFKUEBgtjVtlGb1\", \"attributeName\": \"lightsCeiling\"}, \"value\": 30}}"`
   * `Executable`: `true`
4. Click `Save asset`

You can now click `Start` on the `websocketExec` attribute and the `Ceiling lights (range)` attribute should change to `30`.

## Protocol configuration items
* `Endpoint URI` - Websocket endpoint (should be a full valid URI)
* `OAuth grant` - Will attempt to get an access token before establishing the connection; the access token is added to the connection request headers using the standard `Authorization` header.
* `Connect headers` - `Map<String, String>` headers to be added to the connection request
* `Username` - Username for basic authentication; the hashed username/password is added to the connection request headers using the standard `Authorization` header
* `Password` - Password for basic authentication; the hashed username/password is added to the connection request headers using the standard `Authorization` header
* `Subscriptions` - Array of `Websocket Subscriptions` that are executed once the websocket is connected (there is a built in delay between connection established and these being sent to ensure the server is ready to receive messages).

## Linked attribute configuration items
* `Message match filters` - Array of `Value filters` to apply to incoming messages the filtered string that results is used to apply the `Message match predicate`
* `Message match predicate` - `String Predicate` to apply to the filtered incoming message (if no filters are specified then it is applied to the entire incoming message)
* `Subscriptions` - Array of `Websocket Subscriptions` that are executed once the attribute is linked.
* `Write value` - Indicates the payload to be written to the server (can contain dynamic placeholders)
* `Value filters` - Array of `Value filters` to apply to matched incoming messages to extract the actual value that should be written to the attribute for more information see [here](https://github.com/openremote/openremote/wiki/User-Guide:-Generic-protocols#response-value-filtering).

# See also

- [[Use UI components|User-Guide:-UI components]]
- [[Demo Smart City|Demo-Smart-City]]
- [Get Started](https://openremote.io/get-started-manager/)