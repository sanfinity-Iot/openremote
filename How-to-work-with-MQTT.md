## Connecting

### When working locally (e.g. with an IDE)
To connect with an MQTT client to the MQTT Broker in OpenRemote, you need the following info:
* Client ID: \<realm\>_\<generatedId\>
* Username: mqtt-\<keycloakMqttClientId\>
* Password: \<keycloakMqttClientSecret\>
* Host: localhost
* Port: 1883
* SSL: false

### When HAproxy container is up (when deployed)
* Client ID: \<realm\>_\<generatedId\>
* Username: mqtt-\<keycloakMqttClientId\>
* Password: \<keycloakMqttClientSecret\>
* Host: \<DOMAINNAME\>
* Port: 8883
* SSL: true

### Note: See `KeycloakDemoSetup.java` for a demo setup of Keycloak MQTT client

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