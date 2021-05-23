
## Endpoints

The OpenRemote Manager exposes the following endpoints:

* `/` - Redirects to redirect path (set by `ROOT_REDIRECT_PATH` default: `/manager`)
* `/manager` - Manager UI app (loaded from `$APP_DOCROOT` see paths below)
* `/console_loader` - Loader UI app used by iOS and Android consoles (loaded from `$APP_DOCROOT` see paths below)
* `/swagger` - Swagger UI app (loaded from `$APP_DOCROOT` see paths below)
* `/shared` - Shared UI resources (fonts, locale files, map sprites, icons, etc.) (loaded from `$APP_DOCROOT` see paths below)
* `/api` - HTTP API (see swagger endpoint for live documentation)
* `/auth` - Keycloak reverse proxy endpoint
* `/websocket` - Websocket endpoint
* `/*` - Any other path is resolved against `$CUSTOM_APP_DOCROOT` (see paths below)

### App Realm
Generally applications are linked to a specific realm which cannot be changed but apps can support multiple realms (like the Manager UI), in this instance the app can either present a list of realms for the user to select and/or the app could support setting the realm via a query parameter; which is what the Manager UI currently does:

e.g.  `/manager/?realm=smartcity`

## File paths

The following list shows the file paths used by our docker containers; these can be customised by changing environment variable values (where a file path is controlled by an environment variable) and/or by using volume mapping, this is how we provide customised instances of our system for specific projects. The default values shown below are the defaults as used by our docker containers.

### Manager

* `$APP_DOCROOT` - Location of well known UI apps and shared resources; Default: `/opt/web`
* `$CUSTOM_APP_DOCROOT` - Location used to load paths that aren't well known which allows custom web content to be served; Default: `/deployment/manager/app`
* `$FIREBASE_CONFIG_FILE` - Location of Firebase Cloud Messaging configuration file which allows push notification functionality `/deployment/manager/fcm.json`
* `$KEYCLOAK_GRANT_FILE` - Location of [OAuth Grant](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/auth/OAuthGrant.java) in `json` representation which is used internally by the manager to communicate with Keycloak (in order to provision users, tenants, etc.); Default: `/deployment/manager/keycloak.json`
* `$MAP_TILES_PATH` - Path of `mbtiles` map data file; Default: `/deployment/map/mapdata.mbtiles`
* `$MAP_SETTINGS_PATH` - Map settings path to `json` configuration for styling the map
* `/deployment/manager/extensions` - Location of custom java code that is added to the classpath of the manager during startup
* `$PROVISIONING_DOCROOT` - Location of provisioning directory which can contain the following sub-directories of `json` representations to be automatically provisioned into the manager during a clean install setup:
  * `assets` - Sorted alphabetically, each `json` file should contain exactly 1 asset in `json` representation
  * `consoleappconfig`- Each `json` file should contain exactly 1 [ConsoleAppConfig](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/apps/ConsoleAppConfig.java) in `json` representation