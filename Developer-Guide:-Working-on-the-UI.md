Working on the UI means working on any of:

* Web applications
* Web components
* Keycloak theme(s)

# Prerequisites
You will need the standard tool chain (see [Setting up the environment](./Developer-Guide%3A-Preparing-the-environment)) to be able to build and run the components and apps. Working on the web applications and/or components will also generally require a manager to interact with, you can either:

* Run a Manager instance in an IDE (refer to [Working on the Manager](./Developer-Guide%3A-Working-on-the-Manager))
* Deploy the Manager using a Docker container (refer to [UI Development profile](./Developer-Guide%3A-Docker-compose-profiles#ui-development-devyml))

# UI Components & Apps (`/ui`)
All UI components and apps are located in the `ui` directory; here you can find the standard OpenRemote web UI components and apps using a monorepo architecture. The code is divided into 3 categories by directory:

* `component` - Base OpenRemote JS modules and web components (built using Polymer) these are written as ES6 modules
* `app` - Built-in OpenRemote web applications (applications can be built with whatever frameworks/libraries are desired)
* `demo` - Demos of each web component (provides a development harness for developers working on the components)

## Components
Components can be developed and tested in isolation (with dependencies on other components and/or public modules as required).

## Apps
Apps bring together components and/or public modules and can be written using any framework/library etc that is
compatible with web components (see [here]([https://custom-elements-everywhere.com/)).

## Demos
Generally a 1-1 mapping between components and demos; they provide a simple harness for the components that can be used during
development and optionally can be deployed to offer working demos. 

# Yarn workspaces
Yarn workspaces are used to symlink the OpenRemote packages into a root `node_modules` directory (refer to the Yarn [documentation](https://yarnpkg.com/) for more details). Yarn acts as an alternative to `npm` so please use it.

# Legacy Manager GWT web application (`/client`)
If you are working on the legacy GWT based Manager web application you will need to start the GWT compiler and keep it running in the background; this service listens for compilation requests and transforms Java into JavaScript code:
```
./gradlew -p gwtSuperDev
```