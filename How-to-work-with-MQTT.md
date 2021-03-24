## Setup/Provisioning

There are two ways to configure MQTT access:

* The guide found here: [[Setup Websocket/MQTT connections|User Guide: Setup Client Event protocol]]

* Manually add a client in `Keycloak` to the realm that contains the `asset(s)` you wish to subscribe/publish to, the client have `Client Credentials` grant type enabled (it's called `Service Accounts` in the Keycloak admin UI). It is also possible to provision these Keycloak clients in setup code (see `KeycloakDemoSetup.java` for a demo setup of Keycloak MQTT client)

## Connecting

Connecting to the MQTT broker requires a valid client ID, username and password as supported by the MQTT protocol using the following convention:

* Host: base URL of the manager (e.g. `demo.openremote.io`)
* Port: 1883 (if running in IDE without SSL), 8883 (if running with SSL i.e. in docker)
* Encryption/TLS: false (if running in IDE without SSL), true (if running with SSL i.e. in docker)
* Client ID: \<realm\>_\<KEYCLOAK_CLIENT_ID\> (e.g. `master_ClientEvent-3G9cvEFvGH65dywnvF6urx`)
* Username: KEYCLOAK_CLIENT_ID (e.g. `ClientEvent-3G9cvEFvGH65dywnvF6urx`)
* Password: KEYCLOAK_CLIENT_SECRET (e.g. `ce755edc-63a1-4007-81ca-6313e9cd0029`)


## Subscribe

We offer two different ways to subscribe on assets through MQTT.

### Subscribe on Asset

When you want to receive all asset attribute updates you can subscribe on the whole asset:

```
Topic:
assets/<assetId>
```

or subscribe on one attribute:
```
Topic:
assets/<assetId>/<attributeName>
```
This will return `AttributeEvents` in JSON format:
```
{
  "eventType" : "attribute",
  "timestamp" : 1591265208179,
  "attributeState" : {
    "attributeRef" : {
      "entityId" : "3SJDo13zQjtqvJGC1sOsEt",
      "attributeName" : "humidity"
    },
    "value" : 0.75,
    "deleted" : false
  },
  "realm" : "building",
  "parentId" : "2YS9PgtLmfI2ZMk85sryMD"
}
```

### Subscribe on Asset Attribute Value

When you just want to receive raw values of an certain asset attribute, you subscribe like this:
```
Topic:
assets/<assetId>/<attributeName>/value
```
Which will return values raw values (when subscribed to `humidity`):
```
0.75
```

## Publish

Publishing works similar to subscribe. You can publish on the asset in JSON:
```
Topic:
assets/<assetId>
{
    "humidity": 0.65
}
```
### Note: It's only possible to publish **one** attribute when publishing on the asset.

Or on an asset attribute:
```
Topic:
assets/<assetId>/<attributeName>
0.65
```

or:

```
Topic:
assets/<assetId>/<attributeName>/value
0.65
```

### Custom authorisation and message processing

If you would like to customise the way authorisation is done, then you need to create a class which implements `IAuthorizatorPolicy`.
The `MqttBrokerService` has the function `addAuthorizerPolicy` which accepts an `IAuthorizatorPolicy`.

In order to customise the processing of messages, you can implement `InterceptHandler` or extend `AbstractInterceptHandler`.
The `MqttBrokerService` has the function `addInterceptHandler` which accepts an `InterceptHandler`.

# See also

- [[Setup Websocket/MQTT connections|User Guide: Setup Client Event protocol]]