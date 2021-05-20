Authentication and Authorization in the OpenRemote stack is powered by the [Keycloak](https://www.keycloak.org/) `OpenID Connect Provider` and utilises `OAuth 2.0`. Generally within in an instance of the `OpenRemote` stack the `Keycloak` server is accessible at: `https://<DOMAIN>/auth` but should only be used by advanced users that know what they're doing as **you can completely break your instance**.

## Realms
Realms (also known as Tenants) in OpenRemote provide a layer of isolation with each realm having their own users, assets, rules and even UI styling. This allows for multi-tenancy use cases and realms can only be managed by super users. A realm user can only see and access their own realm and resources within this realm, super users are able to access all realms.

## Users
There are two basic types of user within OpenRemote, both can be managed within the Manager UI or programmatically via custom setup code:

### Regular users
These are users that login interactively by filling in their username and password on the login page, in OAuth 2.0 terminology this is the `authorizationCode` grant type.

### Service users
These are users that login programmatically using a client ID and secret and is designed for confidential clients to connect to the `Manager APIs` (i.e. `MQTT`, `Websockets` and/or `HTTP`) without user interaction, in OAuth 2.0 terminology this is the `client_credentials` grant type.

## Roles
Roles (technically composite roles or role groups) can be defined by selecting the various 'read' and 'write' access rights for the various functions of the system. Each realm has it's own set of roles and a user can be assigned zero or more of these roles within their realm and they are composite as they combine together to form the overall authorization/permissions for a user.