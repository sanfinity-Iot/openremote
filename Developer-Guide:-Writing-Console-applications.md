An OpenRemote Console application runs in a webbrowser, or the webview of a native mobile Application. You can create your own applications writing HTML, Javascript, and CSS code, which is then deployed on the OpenRemote service.

## Deploying application resources

The directory `CONSOLE_DOCROOT` is served unsecured at `https://host/console/` on your OpenRemote server. This environment variable is set depending on deployment, for example, `CONSOLE_DOCROOT=deployment/manager/resources_console` would point to a subdirectory of the current working directory when starting the server.

Each tenant in OpenRemote can have a different console application, so you must first create a subdirectory where your tenant application files are stored, e.g. `CONSOLE_DOCROOT/mytenantapp`.

## Making secure API calls

Console resources are served without security, you should obfuscate the application code.  Directory listing is not allowed, so the request must specify a directory containing `index.html` (e.g. `/console/mytenantapp`) or an actual file that exists.

You can implement authentication through [keycloak.js](http://www.keycloak.org/) on the frontend `index.html` page, similar to Manager frontend `index.html` page. Login form and login procedure are handled completely in the Webview of Android and iOS applications, same as in a regular browser.

After successful authentication, the Javascript application obtains its access token and will periodically refresh it through Keycloak API and its `updateToken()` operation. Requests to the server API must be made with this access token.

If the frontend runs in the Android or iOS native shell: After successful authentication, the Webview will also obtain and pass an extra offline token into the native shell, where it will be stored for future requests by the native apps. When the native app wants to talk to the server, it must obtain an access token with this offline token. When the offline token expires, the user must login again through the Webview to get a new offline token.
