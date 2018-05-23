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
1. Console is 'launched' (e.g. typing URL in browser or opening the app)
2. Console then displays a splash screen indicating that the client is loading
3. Client is loaded in a hidden iframe, the URL should include query parameters to instruct the client what functionality they wish to override/provide to the client (see the Console/client API below) 
4. Console registers to receive postMessages from the client iframe
5. When client loads it registers to receive postMessages from the console
6. Client posts message to console asking it to initialise each `provider` one at a time (see provider initialisation below)
7. Assuming all providers initialise correctly then the client posts a `ready` message to the console indicating that it is ready and the console should now show the client iframe instead of the splash screen
8. The client can choose to enable any providers at this stage (see Provider interaction below)
9. The client should now post a console registration to the `console/register` endpoint on the OR manager:
* id: string [Console asset ID (if previously registered otherwise null)]
* name: string [Name of the console (default: platform.name from [Platform.js](https://github.com/bestiejs/platform.js/) e.g. Chrome, Safari, etc.)]
* version: string [Version of the console (default: platform.version from [Platform.js](https://github.com/bestiejs/platform.js/))] 
* platform: string [Name of platform/OS default: platform.os from [Platform.js](https://github.com/bestiejs/platform.js/))]
* providers: array of providers and any associated data already collected (see list of standard providers below)
Example:
```
{
   name: "ExampleConsole",
   version: "1.0.0",
   platform: "Android 7.1.2",
   providers: {
      push: {
         version: "ORConsole",
         requiresPermission: true,
         hasPermission: true,
         disabled: false,
         data: {
            token: "323daf3434098fabcbc",
            topics: ["update", "maintenance"]
         }
      },
      geofence: {
         version: "ORConsole",
         requiresPermission: true,
         hasPermission: true,
         disabled: false
      }
   }
}
```
If registration is successful then the server will return the saved console registration (including the id); which should be stored for future registration requests.

**N.B. the console registration is converted into an asset which is stored on the server inside the appropriate realm under an asset called 'Consoles' and if the client is authenticated then the console asset is linked with the authenticated user**

10. The client can enable any remaining providers as it requires, for example the client might want to wait until a user requests functionality provided by a provider before enabling it. When a providers status changes the client should update the server by calling the `console/register` endpoint again.

**To save unnecessary requests to the server the client should be efficient in enabling providers and aggregating provider statuses before contacting the server**

# Client/console API
## Client URL
Console loads client with the following query parameters in the URL to override desired functionality and to inform the client about itself (ones in bold are required):

* **consoleName[string]** - Name of the console
* **consoleVersion [string]** - Version of the console
* **consolePlatform [string]** - Name of the platform
* consoleProviders [string] - Space delimited list of providers that this console supports (see below for currently supported/standard providers)

## Provider interaction
The providers to initialise is determined by the client and is dependent on the requirements of each client (e.g. client requires push notifications, current location, etc.).

### Initialise
For each provider the client posts a message asking the console to initialise the provider:
```
{
   action: "PROVIDER_INIT",
   provider: "PROVIDER_NAME"
}
```
The console then does any required initialisation and posts a message back to the client:
```
{
   action: "PROVIDER_INIT",
   provider: "PROVIDER_NAME",
   version: "PROVIDER_VERSION",
   requiresPermission: true|false [tells the client whether user permission is required for this provider]
   hasPermission: true|false|null [tells the client whether permission has already been granted true=permission granted; false=permission denied; null=permission not yet requested]
   success: true|false [true=init success; false=init failure]
}
```
If a provider initialisation call returns success=false then it is up to the client how to proceed, the provider can be marked as disabled or the app could halt (depending on how critical the provider is to the client etc.)

### Enable
Once all providers are initialised then the client is free to decide when to enable each provider; where a provider returned `requiresPermission=true && hasPermission=null` (the client is best placed to decide when and how to ask for permissions and should use good UX principles to avoid users denying such permission requests (see [permission-ux](https://developers.google.com/web/fundamentals/push-notifications/permission-ux)). Providers that don't require permissions or already have permissions could be enabled immediately. The enable message structure is:
```
{
   action: "PROVIDER_ENABLE",
   provider: "PROVIDER_NAME"
   data: JSON [any data that the client wishes to pass to this provider that may be required for enabling it]
}
```
The console then asks the user for the necessary permission(s) (if not done already) and enables the functionality of this provider then posts a message back to the client:
```
{
   action: "PROVIDER_ENABLE",
   provider: "PROVIDER_NAME",
   hasPermission: true|false [true=user granted permission; false=user denied permission]
   success: true|false [true=enabled success; false=enabled failure]
   data: JSON [any data that the provider wishes to return to the client for use by the client and/or for sending to the server]
}
```
### Disable
The client can disable a provider by sending the following message to the console:
```
{
   action: "PROVIDER_DISABLE",
   provider: "PROVIDER_NAME"
}
```
The console then does any required disabling of the provider and posts a message back to the client:
```
{
   action: "PROVIDER_DISABLE",
   provider: "PROVIDER_NAME"
}
```
### Provider independence
Some providers 'run' independently of the client in the background (e.g. push, geofence), providers can also communicate with each other where supported (e.g. push provider telling the geofence provider to refresh the geofences) how they do this is of no concern to the client.

As well as the standard messages above; the client can interact with individual providers using the provider's specific messages as described below.


# Standard Providers
## Push Provider (provider: "push")
Allows data/notifications to be remotely pushed to the console. There are two types of standard push provider depending on the platform and the data sent and returned from the enable message depends on the type used:

## FCM Push (Android & iOS)
* Supports silent (data only) push notifications
* Supports topics

### Enabled message request data (Client -> Console)
Array of topics to subscribe to:
```
{
   topics: ["update", "custom"]
}
```

### Enabled message response data (Console -> Client)
```
{
   token: "23123213ad2313b0897efd",
}
```

## Web Push (Web Browsers)
* No topic support (yet)
* No silent (data only) push notifications

### Enabled message request data (Client -> Console)
No data

### Enabled message response data (Console -> Client)
The data structure returned from the enabled message request is:
```
{
  "endpoint": "https://some.pushservice.com/something-unique",
  "keys": {
    "p256dh": "BIPUL12DLfytvTajnryr2PRdAgXS3HGKiLqndGcJGabyhHheJYlNGCeXl1dn18gSJ1WAkAPIxr4gK0_dQds4yiI=",
    "auth":"FPssNDTKnInHVndSTdbKFw=="
  }
}
```

### Received message (Console -> Client)
Called for both types when a push notification is received:
```
{
   provider: "PROVIDER_NAME",
   action: "PUSH_RECEIVED",
   title: "Hello",
   text: "This is a push notification",
   data: JSON (data payload supplied with the push message),
   foreground: true|false (was notification received whilst console/client was in the foreground)
   coldStart: true|false (was the console/client started by clicking the notification)
}
```

## Geofence Provider (provider: "geofence")
Use platform geofence APIs (Android and iOS); the provider expects a public endpoint on the OR manager at `rules/geofences/{consoleId}` which it can call to get the geofences for this console. The geofence definitions returned by this endpoint and the behaviour of this provider should match the definitions in the asset location tracking wiki.

### Enabled message request data (Client -> Console)
```
{
   consoleId: "23123213ad2313b0897efd"
}
```

### Enabled message response data (Console -> Client)
No data

### Refresh message (Client -> Console)
Tell the provider to fetch the latest geofence definitions and to update its local geofences.
```
{
   action: "GEOFENCE_REFRESH"
}
```

## Location Provider (provider: "location")
Get current location using platform API (if this is not overridden by consoles then an undesirable permission message will likely be shown when falling back to the client's navigator API)

### Enabled message request data (Client -> Console)
No data

### Enabled message response data (Console -> Client)
No data

### Get message (Client -> Console)
```
{
   action: "LOCATION_GET",
   enableHighAccuracy: true|false (Use more accurate methods, such as satellite positioning)
   maximumAge: 3000 (Accept a cached position whose age is no greater than the specified time in milliseconds)
   timeout: 10000 (milliseconds to wait before giving up trying to get the location)
}
```

### Located message (Console -> Client)
```
{
   action: "LOCATION_LOCATED",
   success: true|false,
   lat: latitude,
   lng: longitude
}
```

## Notification Provider (provider: "notification")
Show a notification immediately using the platforms standard mechanism (without using Push API)

### Enabled message request data (Client -> Console)
No data

### Enabled message response data (Console -> Client)
No data

### Show message (Client -> Console)
The client can show a notification by sending the following message to the console:
```
{
   action: "NOTIFICATION_SHOW",
   data: {
      id: 1,
      title: "Hello",
      text: "This is a notification message!",
      data: JSON (any data to pass to notification click handler)
   }
```

### Clicked message (Console -> Client)
The client can listen for notification click events by listening for the following messages:
```
{
   action: "NOTIFICATION_CLICKED",
   title: "Hello",
   text: "This is a push notification",
   data: JSON (data payload supplied with the notification),
}
```

## Modal Provider (provider: "modal")
Show a modal dialog to the user immediately using the native platform mechanism.
TODO

## ErrorHandler Provider (provider: "errorhandling")
Ability for console to decide how to handle errors (displaying splash screen or other custom UI)
TODO

## Other Messages
### From console

### From client
* ready - Indicates that client is ready to be shown `{action: "READY"}`
* exit - Indicates that the client wants to exit so the console should be closed `{action: "EXIT"}`