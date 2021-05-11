## Manager APIs
The OpenRemote Manager API is composed of the following APIs, each API requires authentication (with the exception of `read`/`write` of public `asset` `attributes` see [Asset security](./User-Guide%3A-Asset-Security)). Refer to the [Security Guide](./User-Guide:-Using-OAuth) for details on setting up OAuth 2.0, the sort of functions 

## HTTP API
This is the traditional request response API with live documentation available via Swagger UI (see `/swagger/` URL of your manager) or you can look at the [demo environment](https://demo.openremote.io/swagger/). The base URL for the API is:
```
https://{domain}/api/{realm}/
```

## WS (Websocket) API
This is a publish subscribe API that is event based, the URL for connecting to this API is:

```
wss://{domain}/websocket/events?Auth-Realm={realm}&Authorization={authToken}
```

### Event subscriptions

A subscription is created by sending an [EventSubscription](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/event/shared/EventSubscription.java) as `JSON` prefixed with `SUBSCRIBE:`. When a subscribe message is sent then the server will determine if the requesting user is authorised to make such a subscription and if so then the manager will reply with the same subscription `JSON` object but prefixed with `SUBSCRIBED:`, if the user is not authorised then the subscription `JSON` object will be returned but prefixed with `UNAUTHORIZED:`.

When an event occurs in the manager that matches an existing subscription then a [TriggeredEventSubscription](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/event/TriggeredEventSubscription.java) as `JSON` prefixed with `TRIGGERED:` will be sent to the client.

### Request response emulation
To emulate the request response nature of the HTTP API it is possible to send an [EventRequestResponseWrapper](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/event/shared/EventRequestResponseWrapper.java) as `JSON` prefixed with `REQUESTRESPONSE:`, the `messageId` is used to allow the client to associate the request message with the response message. This request response emulation is useful for example to read an asset.

For more details on the structure of messages please refer to the `javadoc` of each object type for detailed information.

## MQTT API
Another publish subscribe 

