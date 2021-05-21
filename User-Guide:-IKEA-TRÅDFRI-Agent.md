**NOTE: THIS PROTOCOL IS NOT YET ACCESSIBLE VIA THE NEW MANAGER UI. IT IS HOWEVER ALREADY PART OF THE CODE BASE AND WILL BE ADDED IN NEXT RELEASES.**

You can connect an IKEA TRÅDFRI Gateway to the manager. The manager will import all assets automatically. Here we describe the steps you need to take.

# Docker configuration

If you are using Docker (and docker-compose) to run OpenRemote, you need to change the configuration of docker-compose in order to use the IKEA TRÅDFRI protocol.
In the `profile/deploy.yml` file, uncomment the lines below the following line: "# Uncomment to support the IKEA TRÅDFRI protocol. USE AT YOUR OWN RISK.".
This line occurs 6 times in the `profile/deploy.yml` file. Make sure to find all of them.


**Examples:**
```yml
# Uncomment to support the IKEA TRÅDFRI protocol. USE AT YOUR OWN RISK.
network_mode: host
```

```yml
# Uncomment to support the IKEA TRÅDFRI protocol. USE AT YOUR OWN RISK.
# Also remove (or comment) the already existing variables with the same name.
KEYCLOAK_HOST: 127.0.0.1
KEYCLOAK_PORT: 8081
DATABASE_CONNECTION_URL: jdbc:postgresql://localhost/openremote
```
  
# Connect an IKEA TRÅDFRI Gateway

In the following example, you link your existing IKEA TRÅDFRI Gateway by using its IP address, eg. `192.163.1.2`.

1. Login to the manager UI (`https://localhost/manager` as `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left
3. Set the following:
   * Asset name: `Tradfri Agent`
   * Asset Type: `Agent`
4. Click `Create asset` at the bottom of the screen and then click `Edit asset` in the top right
5. Add a new attribute:
   * Name: `Gateway`
   * Type: `IKEA TRÅDFRI`
6. Click `Add attribute` and then expand the new attribute (using the button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows: 
   * Gateway host: `192.163.1.2`
   * Gateway security code: Fill in the security code of the IKEA TRÅDFRI Gateway (This can be found on the bottom of the gateway)
7. Click `Save asset` at the bottom of the screen

You now have an IKEA TRÅDFRI Agent to communicate with your own IKEA TRÅDFRI Gateway.

The protocol connection status changes to `CONNECTED` as soon as an IKEA TRÅDFRI Gateway is available on the provided host and the correct security code is provided.

# See also

- [Agent overview](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview)
- [Quick Start](https://github.com/openremote/openremote/blob/master/README.md)
- [[Manager UI Guide|User-Guide:-Manager-UI]]
- [[Custom Deployment|User-Guide:-Custom-deployment]]
- [Setting up an IDE](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Setting-up-an-IDE)
- [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-UI-apps-and-components)