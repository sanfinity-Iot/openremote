You can connect an IKEA TRÅDFRI Gateway to the manager. The manager will import all assets automatically. Here we describe the steps you need to take.

# Connect an IKEA TRÅDFRI Gateway

In the following example, you link your existing IKEA TRÅDFRI Gateway by using the its IP address, eg. `192.163.1.2`.

1. Login to the manager UI (`https://localhost/master` as `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left
3. Set the following:
   * Asset name: `Tradfri Agent`
   * Asset Type: `Agent`
4. Click `Create asset` at bottom of screen and then click `Edit asset` in the top right
5. Click `Edit asset` and add a new attribute:
   * Name: `Gateway`
   * Type: `IKEA TRÅDFRI`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows: 
   * Gateway host: `192.163.1.2`
   * Gateway security code: Fill in the security code of the IKEA TRÅDFRI Gateway (This can be found on the bottom of the gateway)
7. Click `Save asset` at bottom of the screen

You now have a IKEA TRÅDFRI Agent to communicate with your own IKEA TRÅDFRI Gateway.

The protocol connection status is `DISCONNECTED` until a IKEA TRÅDFRI Gateway is really available on the provided base host, with the correct security code.

# See also

- [[Demo Smart City|Demo-Smart-City]]
- [Get Started](https://openremote.io/get-started-manager/)