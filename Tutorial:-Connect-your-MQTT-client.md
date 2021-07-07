In this tutorial we will use the [MQTT API](https://github.com/openremote/openremote/wiki/User-Guide%3A-Manager-APIs#mqtt-api-mqtt-broker) to subscribe to changes of asset attributes values and write to them from a MQTT Client. The OpenRemote Manager functions as the MQTT Broker. We will use the app [MQTT X](https://mqttx.app/) as our desktop client in this tutorial, but you can use your preferred tool or better yet the device you want to connect.

## Create an asset with attributes
We will create a Thing asset, but feel free to use an AssetType that matches your physical device
1. On the assets page, click the `+` in the asset tree on the left
1. Select the Thing asset type, give it a friendly name and click 'Add'
1. Add two new attributes in the Edit asset mode: 
   1. subscribeAttribute
      - type: `Custom Attribute`
      - name: `subscribeAttribute`
      - Valuetype: `Boolean`
   2. writeAttribute
      - type: `Custom Attribute`
      - name: `writeAttribute`
      - Valuetype: `Number`

## Create a service user
The service user will give programmatic access to the MQTT client.
1. Go to the users page and create a new service user (second panel on the page)
2. Name the service user `mqttuser` and give the user the read and write role for the sake of convenience. It is advised to configure a more restricted role for your service users.
3. Click 'Create', a secret will be generated automatically.
4. Open the 'mqttuser' user to see and copy the secret

## Establish a connection from client to broker
In your MQTT client set up a new connection:
- host: `mqtt://localhost` (or URL of your hosted environment e.g. demo.openremote.io)
- Port: `1883` for localhost (no SSL), `8883` for hosted environment (SSL)
- password: the secret generated for the mqtt service user (you can find it on the mqttuser users page)
- username: `master:mqttuser` ({realm}:{user})

## Subscribe to attributes using the MQTT API
In this tutorial we will be looking at specific attributes of a specific asset. There are [many more options](https://github.com/openremote/openremote/wiki/User-Guide%3A-Manager-APIs#mqtt-api-mqtt-broker) of subscribing to (all) updates of assets and attributes. The asset attributes that you will be subscribing to can be written by the user, by rules, or can be a live value gathered through an Agent link with another device in the field. You can imagine this boolean value could toggle a function of the device subscribed to the attribute
1. Get the ID of the Thing asset by navigating to its asset page and copying the ID in the URL (e.g. `http://localhost:9000/manager/#!assets/false/6xIa9MkpZuR7slaUGB6OTZ` => `6xIa9MkpZuR7slaUGB6OTZ`)
2. Create a subscription for the subscribeAttribute in your MQTT client with the topic: `attribute/6xIa9MkpZuR7slaUGB6OTZ/subscribeAttribute` 
3. In the view mode of the Thing asset in the OpenRemote Manager, write a new value to the 'Subscribe attribute' by clicking the checkbox.
4. Verify that you see the value (`true`/`false`) coming in on your MQTT client!

## Write attribute values from the MQTT client
You can write data from your MQTT client (device) to the OpenRemote manager so that you can monitor the device and create rules.
1. Define the correct topic. For directly writing an attribute value: attributevalue/{assetID}/{attributeName}. So in our case this will be `attributevalue/6xIa9MkpZuR7slaUGB6OTZ/writeAttribute`
2. Send the JSON over this topic. For a number, this is really simple: `{23}`
3. Send and check whether the value updated in the Manager!

**Note: currently there is an unresolved bug related writing to attributes with the Read Only config item. Make sure it is not on the attribute you want to write to, even if its unchecked. You need to delete the item from the attribute.**

## Additional exercises
