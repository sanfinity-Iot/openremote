## Manager APIs
The OpenRemote Manager API is composed of the following APIs, each API requires authentication (with the exception of `read`/`write` of public `asset` `attributes` see [Asset security](./User-Guide%3A-Asset-Security)).

To be able to authenticate you'll need to create a service user using the Manager UI (must be logged in as super user to access this functionality), please refer to the Manager UI user guide; access tokens can be obtained from the token endpoint using standard OAuth 2.0 techniques.

* OAuth 2.0 Token Endpoint: `/auth/realms/{realm}/protocol/openid-connect/token`
* Regular user supported grant type(s): `authorizationCode`
* Service user supported grant type(s): `client_credentials`



## HTTP API
This is the traditional request response API with live documentation available via Swagger UI (see `/swagger/` URL of your manager) or you can look at the [demo environment](https://demo.openremote.io/swagger/). Authentication is done using standard `Authorization` header bearer token where the token  is a valid access token obtained from the OAuth 2.0 token endpoint.

* Base URL: `/api/{realm}/`
* Authorization Header:  `Authorization: Bearer {accessToken}`

## WS (Websocket) API
This is a publish subscribe API that is event based, where events are of type [SharedEvent](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/event/shared/SharedEvent.java). Authentication is done using `Authorization` query parameter with a valid access token obtained from the token endpoint. The realm of the authenticating user must also be included as an `Auth-Realm` query parameter.

* URL: `/websocket/events?Auth-Realm={realm}&Authorization={accessToken}`

### Subscriptions
A subscription is created by sending an [EventSubscription](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/event/shared/EventSubscription.java) as `JSON` prefixed with `SUBSCRIBE:`. When a subscribe message is sent then the server will determine if the requesting user is authorised to make such a subscription and if so then the manager will reply with the same subscription `JSON` object but prefixed with `SUBSCRIBED:`, if the user is not authorised then the subscription `JSON` object will be returned but prefixed with `UNAUTHORIZED:`.

When an event occurs in the manager that matches an existing subscription then a [TriggeredEventSubscription](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/event/TriggeredEventSubscription.java) as `JSON` prefixed with `TRIGGERED:` will be sent to the client.

### Publish
[AttributeEvents](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/attribute/AttributeEvent.java) can be published and you can wait for the change to take place and an [AttributeEvent](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/attribute/AttributeEvent.java) to be returned when the attribute does get updated.

It is also possible to emulate the request response nature of the HTTP API in order to read data using the Websocket API by sending an [EventRequestResponseWrapper](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/event/shared/EventRequestResponseWrapper.java) as `JSON` prefixed with `REQUESTRESPONSE:`, the `messageId` is used to allow the client to associate the request message with the response message. This request response emulation is useful for example to read an asset.

For more details on the structure of messages please refer to the `javadoc` of each object type for detailed information.

## MQTT API (MQTT Broker)
Another publish subscribe API, authentication requires a service user username and secret and is done using standard `MQTT` username and password mechanism, to connect to the broker:

* Host: Host of the manager (e.g. `demo.openremote.io`)
* Port: 8883 (if running with SSL i.e. the standard stack with reverse SSL proxy) 1883 (if running without SSL proxy)
* Encryption/TLS: true (port 8883) false (port 1883)
* Username: `{realm}:{username}`
* Password: `{secret}`
* ClientId: any thing you like

#### Note
It's important that the `ClientId` in the topic matches the one in the MQTT credentials.

### Subscriptions
#### [AssetEvents](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/asset/AssetEvent.java)
It is possible to subscribe to these events using the topic format:

```{realm}/{clientId}/asset/{assetId}```

Examples:

* `{realm}/{clientId}/asset/#` - All asset events in the realm
* `{realm}/{clientId}/asset/+`- All asset events for direct children of the realm
* `{realm}/{clientId}/asset/{assetId}`- All asset events for the specified asset
* `{realm}/{clientId}/asset/{assetId}/#`- All asset events for descendants of the specified asset
* `{realm}/{clientId}/asset/{assetId}/+`- All asset events for direct children of the specified asset

#### [AttributeEvents](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/attribute/AttributeEvent.java) 
It is possible to subscribe to these events using the topic format:

```{realm}/{clientId}/attribute/{attributeName}/{assetId}```

Examples:

* `{realm}/{clientId}/attribute/+/#` - All attribute events in the realm
* `{realm}/{clientId}/attribute/+/+` - All attribute events for direct children of the realm
* `{realm}/{clientId}/attribute/{attributeName}/#` - All attribute events for specified attribute name
* `{realm}/{clientId}/attribute/{attributeName}/+` - All attribute events for direct child assets of the realm with specified attribute name
* `{realm}/{clientId}/attribute/+/{assetId}`- All attribute events for specified asset
* `{realm}/{clientId}/attribute/{attributeName}/{assetId}`- All attribute events for specified asset with specified attribute name
* `{realm}/{clientId}/attribute/{attributeName}/{assetId}/#`- All attribute events for descendants of the specified asset with specified attribute name
* `{realm}/{clientId}/attribute/{attributeName}/{assetId}/+`- All attribute events for direct children of the specified asset with specified attribute name


**NOTE: `attributevalue` topic prefix can be used in place of `attribute` to only return the value of the [AttributeEvent](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/attribute/AttributeEvent.java) rather then the entire event.**


### Publish
It is possible to publish attribute events to specific assets using the following topics and payloads:

* `{realm}/{clientId}/writeattribute`- Payload: [AttributeEvent](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/attribute/AttributeEvent.java)
* `{realm}/{clientId}/writeattributevalue/{attributeName}/{assetId}` - Payload: `JSON` of attribute value

### MQTT custom handlers
It is possible to inject custom handlers for MQTT messages by implementing the [MQTTCustomHandler](https://github.com/openremote/openremote/blob/master/manager/src/main/java/org/openremote/manager/mqtt/MQTTCustomHandler.java) interface and adding it to the [MQTTBrokerService](https://github.com/openremote/openremote/blob/master/manager/src/main/java/org/openremote/manager/mqtt/MqttBrokerService.java) during startup. The custom handler can choose to intercept messages based on topic, user and/or whether it is a pub or sub request, see javadoc of `MQTTCustomHandler` for more details.