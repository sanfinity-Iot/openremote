By default our stack includes the [Keycloak](https://www.keycloak.org/) `OpenID Connect Provider` for authentication and authorization via `OAuth 2.0`.

Generally within in an instance of the `OpenRemote` stack the `Keycloak` server is accessible at: `https://<DOMAIN>/auth`.

For each realm created within the Manager (via UI, provisioning code or REST API) a client called `openremote` is automatically created within `Keycloak` which allows interactive authentication using the [Authorization code](https://oauth.net/2/grant-types/authorization-code/) grant type; this is primarily intended for accessing the `Manager UI` and is not suitable for programmatic access to the `Manager APIs` (i.e. `MQTT`, `Websockets` and/or `HTTP`).

If you want to be able to programmatically access the system then you will need a client with 'Client Credentials` grant type enabled (this is called `Service Account` in `Keycloak` terminology, this can be done in one of the following ways:

## Create a ClientEvent Agent
Simply add a new `ClientEvent` Agent to the realm you want to access see [here](https://github.com/openremote/openremote/wiki/User-Guide%3A-Setup-Client-Event-protocol).

## Programmatically provision a client in your custom setup code
A client can be programmatically added in setup code (for more details see [here](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Writing-setup-code)) which executes when the system starts with an empty database; the `ClientEventService` (simpler) or `KeycloakIdentityProvider` (more advanced) via the `IdentityService` can be used to manage clients.
```
        ProtocolClientEventService.ClientCredentials clientCredentials =
            new ProtocolClientEventService.ClientCredentials(
                agent.getRealm(),
                new ClientRole[] {ClientRole.READ_ASSETS, ClientRole.WRITE_ASSETS}),
                clientId,
                clientSecret
            );

        clientEventService.addClientCredentials(clientCredentials);
```

## Manually create a new client in the `Keycloak` admin UI
Log into the `Keycloak` UI for your instance as super user and then create a new client in the appropriate realm with the `Service Account Enabled` option checked, refer to the [Keycloak documentation](https://www.keycloak.org/docs/latest/server_admin/#_clients) for more details. A client will need to be created for each realm that requires access.