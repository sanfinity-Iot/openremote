# Terminology
## Client
A consumer of the OR API; same meaning as OAuth client (The OR Manager web application is a client; for custom projects there could be zero or more clients for each realm that provide very specific functionality as required by the project and/or realm). These are generally responsive web applications.

## Console
This is the application used to load the client and can be thought of as a wrapper around a client e.g. Web Browser, Android/iOS App. Generally this is an application capable of loading a web view that renders the client. A console could be hardcoded to a specific realm and client or it could be more configurable depending on requirements. A web browser requires no installation where as Android/iOS consoles are pre-compiled and distributed.

# Console/client interaction (HTML5 PostMessage)
Clients and consoles should be able to exchange information via HTML5 postMessage mechanism using the well defined API (see below); for clients this functionality is provided by the or-app web component (TODO) and for Android/iOS consoles this functionality is provided by the base OpenRemote console (TODO). Clients loaded via a web browser don't technically have a console wrapping them so in this instance the client behaves like the console by providing default console behaviour and the Android/iOS consoles should then override this behaviour as required.

# Build and deployment
Clients should be built and deployed within the OR manager web server using the standard tooling e.g. `gradle clean installDist` (refer to documentation).
Console build and deployment will depend on the specific tools and platform used but where possible the standard tooling and deployment process should be used (TODO)

# Runtime behaviour
1. Console is `launched` (e.g. typing URL in browser or opening the app)
2. Console then displays a splash screen indicating that the client is loading
3. Client is loaded in a hidden iframe, the URL should include query parameters to instruct the client what functionality they wish to override/provide to the client (see the Console/client API below) 
4. Console registers to receive postMessages from the client iframe
5. When client loads it registers to receive postMessages from the console
6. Client posts message to console asking it to initialise each `provider` one at a time (see provider initialisation below)
7. Assuming all providers initialise correctly then the client posts a `ready` message to the console indicating that it is ready and the console should now show the client iframe instead of the splash screen
8. The client should now call the `console/register` endpoint on the OR manager with the following data in a JSON payload:
* name: string [Name of the console (default: platform.name from [Platform.js](https://github.com/bestiejs/platform.js/) e.g. Chrome, Safari, etc.)]
* version: string [Version of the console (default: platform.version from [Platform.js](https://github.com/bestiejs/platform.js/))] 
* platform: string [Name of platform/OS default: platform.os from [Platform.js](https://github.com/bestiejs/platform.js/))]
* providers: array of providers and any associated data (see list of standard providers below)
Example:
```
{
   name: "ExampleConsole",
   version: "1.0.0",
   platform: "Android 7.1.2",
   providers: [
      {
         provider: "push",
         data: {
            type: "fcm",
            token: "323daf3434098fabcbc",
            topics: ["update", "maintenance"],
            silent: true,
         }
      },
      {
         provider: "geofence",
         data: {
            type: "or"
         }
      }
   ]
}
```

# Client/console API
## Client URL
Console loads client with the following query parameters in the URL to override desired functionality and to inform the client about itself (ones in bold are required):

* **name[string]** - Name of the console
* **version [string]** - Version of the console
* **platform [string]** - Name of the platform
* provides [string] - The name of some functionality that this console provides either on top of or in place of standard client functionality; this parameter can be used multiple times; one for each provider (see below for currently supported/standard providers)

## Provider Initialisation
The list of providers is constructed from the standard providers the client understands and the providers that the console has specified via the query parameters (a console provider overrides a client provider of the same name).

1. For each provider the client posts a message asking the console to initialise the provider:
```
{
   action: "PROVIDER_INIT",
   provider: "PROVIDER_VALUE",
}
```
2. The console then does any required initialisation and posts a message back to the client:
```
{
   action: "PROVIDER_INIT",
   provider: "PROVIDER_VALUE",
   success: true|false|null [true=init success; false=init failure; null=ignoring and client should fallback to default behaviour/provider]
   version: int [integer indicating the version of this provider - so the client knows how to interact with it]
}
```
3. If a provider initialisation call returns false then an error is generated and passed to the registered error handler:
```
{
   error: "PROVIDER_INIT",
   detail: "PROVIDER_VALUE"
}
```
4. Once all providers are initialised then the client is free to decide when to `start` each provider (starting should involve asking the user for any required permission(s) for the functionality to work and any other specific requirements), the client is best placed to decide when and how to ask for permissions and should use good UX principles to avoid users denying such permission requests (see [https://developers.google.com/web/fundamentals/push-notifications/permission-ux](https://developers.google.com/web/fundamentals/push-notifications/permission-ux)) **[NOTE: Any providers requested by the console that the client doesn't understand will be started immediately]**. The start message structure is:
```
{
   action: "PROVIDER_START",
   provider: "PROVIDER_VALUE",
   data: JSON (optional JSON structure to pass any relevant data the provider may need for starting)
}
```
5. The console then does any required start logic and posts a message back to the client:
```
{
   action: "PROVIDER_START",
   provider: "PROVIDER_VALUE",
   success: true|false [true=start success; false=start failure]
   data: JSON (optional JSON structure to pass any relevant data back to the client)
}
```


## Standard Providers
* push - Push notification that shows a notification to the user (web push API, Android or iOS)
* notification - Show a notification immediately (without using Push API)
* modal - Show a modal dialog to the user immediately
* geofence - Geofence APIs (Android and iOS)
* errorhandling - Ability for console to decide how to handle errors (displaying splash screen or other custom UI)
* location - Get current location (if this is not overridden by consoles then an undesirable permission message will likely be shown when falling back to the client's navigator API)

**NOTE: Additional providers should be used on custom projects as required**

## Other Messages
### From console

### From client
* ready - Indicates that client is ready to be shown `{action: "READY"}`
* exit - Indicates that the client wants to exit so the console should be closed `{action: "EXIT"}`