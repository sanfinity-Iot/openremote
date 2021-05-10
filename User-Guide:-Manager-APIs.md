## Manager APIs
The OpenRemote Manager API is composed of following APIs, each API requires authentication (with the exception of `read`/`write` of public `asset` `attributes` see [Asset security](./User-Guide%3A-Asset-Security)). Refer to the [Security Guide](./User-Guide:-Using-OAuth) for details on setting up OAuth 2.0.

## HTTP API
This is the traditional request response API with live documentation is available via Swagger UI (see `/swagger/` URL of your manager) or you can look at the [demo environment](https://demo.openremote.io/swagger/). The base URL for the API is:
```
https://{domain}/api/{realm}/
```

## WS (Websocket) API
This is a publish subscribe API that is event based, the URL for this API is:

```
wss://{domain}/websocket/events?Auth-Realm={realm}&Authorization={bearerToken}
```


## MQTT API
Another publish subscribe 

