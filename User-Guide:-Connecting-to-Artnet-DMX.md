The example below describes interactively linking asset attributes to Artnet Servers using the [ArtnetClientProtocol](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/dmx/artnet/ArtnetClientProtocol.java). The following examples assume that you are running the [Demo docker compose profile](https://github.com/openremote/openremote/wiki/Developer-Guide:-Docker-compose-profiles#demo-docker-composeyml).

# Setup the basic Artnet connection
***
The following examples assume that the DMX controller is bound to the loopback address `127.0.0.1` on port `6454`:
1. Login to the manager UI (`https://localhost/master` `admin/secret`).
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left.
3. Set the following:
    * `Asset name`: `Artnet Agent`
    * `Parent asset`: `Tenant A -> Smart Building` and select 'OK'
    * `Asset Type`: Agent
4. Click `Create asset` at the bottom of the screen.
5. Click `Edit asset` and add a new attribute:
    * `Name`: `artnetClient`
    * `Type`: `Artnet Client`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows:
    * `Artnet server hostname/IP`: `127.0.0.1`
    * `Artnet server port`: `6454`

**Make sure you click `Add Item` when creating new attribute configuration items.**

7. Click `Save asset` at the bottom of the screen.

You now have a basic ArtnetClientProtocol ready to be linked to by asset attributes; the defaults assume that the DMX controller receives a ByteBuf (Byte array). 

**NOTE: The protocol configuration status will show as `CONNECTED` even if the server is not actually reachable. This is due to the fact that `UDP` has no notion of a connection.**

# Adding Light assets to the Artnet Agent
***
Light assets are conventially added through use of OpenRemote's Import feature.
This feature is available in the ArtNet Agent asset.
While importing, a specific file is expected.
In the case for light assets, this is a specifically formatted JSON file.

The following is an example of the format expected in the Import file;
this example defines two unique lights:
```json
{
	"lights": [
		{
			"lightId": 0,
			"groupId": 0,
			"universe": 0,
			"amountOfLeds": 3,
			"requiredValues": "r,g,b"
		},
		{
			"lightId": 1,
			"groupId": 0,
			"universe": 1,
			"amountOfLeds": 6,
			"requiredValues": "g,r,g,b,w"
		}
	]
}
```

Light definitions can be added to the JSON file by adding new ones in the "lights" array.
Importing the above file, will result in the Manager adding two unique ArtNet lights to the asset tree.

**NOTE: All stated properties are expected while importing. The "lightId" attribute must be unique to each specified light.**

## Importing the light JSON
***
**NOTE: This step-by-step guide requires the use of the OpenRemote Manager.**

To import light assets, a ArtNet Agent must have been added to the asset tree.  
From there, edit the ArtNet Agent's asset.  
By collapsing the "ArtnetProtocolAgent" protocol configuration, you will be able to import Light assets in the "Protocol link import/discovery" attribute.  
Before importing the JSON, an 'parent' asset must be targeted to where the lights will be appended to. We recommend to choose the same asset to were the ArtNet Agent resides.  
Finally the JSON can be imported by making use of the "Upload & import links from file" button.
 

The imported file must suffix with a ".json" file extension.  
The filename doesn't matter to the import and will accept as long as the contents are correctly formatted.

## Import behaviour
***
T.B.D
