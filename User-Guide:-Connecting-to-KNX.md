You can connect a KNX Gateway to the manager. When you import the corresponding ETS file, the manager will import all assets in the manager automatically. Here we describe the steps you need to take.

# Connect a KNX Gateway

In the following example, you link your existing KNX Gateway by using the its IP address, eg. `http://192.163.1.2`.

1. Login to the manager UI (`https://localhost/master` as `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left
3. Set the following:
   * Asset name: `KNX Agent`
   * Parent asset: `CustomerA -> Smart Home` and select 'OK'
   * Asset Type: `Agent`
4. Click `Create asset` at bottom of screen and then click `Edit asset` in the top right
5. Click `Edit asset` and add a new attribute:
   * Name: `Gateway`
   * Type: `KNX`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows: 
   * KNX gateway host: `http://192.163.1.2`
   * You can configure the gateway by adding a new configuration item, by selecting it from the list: KNX gateway host, KNX gateway port, KNX local bus address, KNX local host, KNX use NAT **(do not forget to click on 'Add item' for custom items)** (also see 'Configuring your KNX Gateway').
7. Click `Save asset` at bottom of the screen

You now have a KNX protocol Agent to communicate with your own KNX Gateway as long as you have an internet connection and an attribute will be linked.

The attribute status is DISCONNECTED until a KNX Gateway is really available on the provided base URL, with the correct configuration.

## Configuring your KNX Gateway ##

By adding a configuration item you can configure your KNX Gateway. The following commands are available:
- KNX gateway host, to...
- KNX gateway port, to...
- KNX local bus address, to...
- KNX local host, to...
- KNX use NAT, to...

# Import Devices via ETS file

You can now add linked attributes in the Manager to the new Agent by importing an ETS file, such that we can get sensors status and execute commands.

1. Go back to the 'Gateway' attribute, click 'Edit Asset', and expand the 'Gateway' attribute.
2. Select the Parent of imported assets first, eg. 'Smart Home' and press 'OK'
3. Click 'Upload & import links from file', and select the ETS file from a folder.
4. After you click 'Open' you will see that all available commands and sensors are added as assets and attributes

You can now read sensor data as well as send commands to you your KNX devices.

# See also

- [[Use Web components|User-Guide:-Use Web components]]