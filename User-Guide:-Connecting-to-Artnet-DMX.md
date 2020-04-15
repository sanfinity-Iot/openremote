The example below describes interactively linking asset attributes to Artnet Servers using the [ArtnetClientProtocol](http://google.com). The following examples assume that you are running the [Demo docker compose profile](https://github.com/openremote/openremote/wiki/Developer-Guide:-Docker-compose-profiles#demo-docker-composeyml).

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

# Adding Light assets to the Artnet connection
***

