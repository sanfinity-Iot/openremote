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
7. Assuming all providers initialise correctly then the client posts a message to the console 

# Client/console API

## Client URL
Console loads client with the following query parameters in the URL to override desired functionality and to inform the client about itself (ones in bold are required):

* **console [string]** - Name of the console
* **consoleVersion [string]** - Version of the console
* **platform [string]** - Name of the platform
* **paltformVersion [string]** - Version of the platform
* provides [string] - The name of some functionality that this console provides either on top of or in place of standard client functionality; this parameter can be used multiple times (one for each piece of functionality) currently supported  functionality that the console understands are:
   * push - Push notification functionality (Web push, Android and iOS push notifications)
   * geofence - Geofence APIs (Android and iOS)
   * errorhandling - Ability for console to decide how to handle errors (displaying splash screen or other custom UI)

## Provider Initialisation
1. For each provider specified by the console (via query parameters) the client posts a message asking the console to initialise the provider:
```

```