The following URL endpoint patterns are used by default:

* `/` - Redirects to default app (typically `/manager`) - Default app can be specified using `APP_DEFAULT` environment variable.
* `/<APP_NAME>` - Loads the specified app (as found in deployment/app directory or custom directory if `APP_DOCROOT` is set)
* `/api/<REALM>/<REST_API_ENDPOINT>` - REST API, must include the realm being used 
* `/websocket` - Websocket endpoint

## App Realm
Generally applications are linked to a specific realm which cannot be changed but apps can support multiple realms, in this instance the app can either present a list of realms for the user to select and/or the app could support setting the realm via a query parameter; which is what the manager app currently does:

e.g.  `http://localhost:8080/manager?realm=customerA`