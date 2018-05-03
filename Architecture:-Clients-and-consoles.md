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
3. Console 
4. The client in a hidden iframe, the URL should include query parameters to instruct the client what behaviour they wish to override (see the Console/client API below) once the client is loaded then the iframe is shown in place of the splash screen
3. 

# Client/console API
Console loads client with the following query parameters to override desired functionality and to inform the client about itself (ones in bold are required):

* **console [string]** - Name of the console
