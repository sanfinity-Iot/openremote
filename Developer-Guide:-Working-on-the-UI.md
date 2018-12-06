Working on the UI means working on any of:

* Web applications
* Web components
* Keycloak theme(s)

## Prerequisites
You will need the standard tool chain (see [Setting up the environment](./Developer-Guide%3A-Preparing-the-environment)) to be able to build and run the components and apps. Working on the web applications and/or components will also generally require a manager to interact with, you can either:

* Run a Manager instance in an IDE (refer to [Working on the Manager](./Developer-Guide%3A-Working-on-the-Manager))
* Deploy the Manager using a Docker container (refer to [UI Development profile](./Developer-Guide%3A-Docker-compose-profiles#ui-development-devyml))

## UI Components & Apps (`/ui`)
All UI components and apps are located in the `ui` directory; here you can find the standard OpenRemote web UI components and apps using a monorepo architecture. The code is divided into categories by directory:

* `component` - Base OpenRemote JS modules and web components (built using Polymer) these are written as ES6 modules
* `app` - Built-in OpenRemote web applications (applications can be built with whatever frameworks/libraries are desired)
* `demo` - Demos of each web component (provides a development harness for developers working on the components)
* `keycloak/themes` - Keycloak themes which provide custom styled login and account management screens (see Keycloak [documentation](https://www.keycloak.org))

Typescript is used to provide static typing with the OpenRemote model available in the `@openremote/model` component package; the components are published to `npm` under the `@openremote` scope; see the README in each component for information about each specific component.

Yarn workspaces are used to symlink the OpenRemote packages into a root `node_modules` directory (refer to the Yarn [documentation](https://yarnpkg.com/) for more details). Yarn acts as an alternative to `npm` so please use it.

Gradle is used used as the standard build tool throughout the stack, there are gradle tasks available which delegate to node as follows:

* `npmInstall/yarnInstall` -> `yarn install`
* `npmClean` -> `npm run-script clean`
* `npmBuild` -> `npm run-script build`
* `npmTest` -> `npm run-script test`
* `npmServe` -> `npm run-script serve`
* `npmServeProduction` -> `npm run-script serveProduction`

The above script names should be used in `package.json` files and then appropriate `build.gradle` files should be added, see existing components, apps, demos for examples.

### Components
Components can be developed and tested in isolation (with dependencies on other components and/or public npm modules as required). Some components have no visuals and provide standard OpenRemote functionality e.g. `@openremote/core`, whilst others provide visuals that allow interaction with the Manager backend.

#### Maps
If you are working on a map component then please refer to the [working on maps](./Developer-Guide:).

### Apps
Apps bring together components and/or public modules and can be written using any framework/library etc that is
compatible with web components (see [here]([https://custom-elements-everywhere.com/)).

### Demos
Generally a 1-1 mapping between components and demos; they provide a simple harness for the components that can be used during development and optionally can be deployed to offer component demos. 

### Keycloak Themes
Each theme is located in its own directory within the `keycloak/themes` directory. When running any of the `dev` docker compose profiles; each theme can be volume mapped into the keycloak service and changes are reflected in real time allowing for easy development of these themes (see [Docker compose profiles](./Developer-Guide:-Docker-compose-profiles) for more details).

## Legacy Manager GWT web application (`/client`)
If you are working on the legacy GWT based Manager web application you will need to start the GWT compiler and keep it running in the background; this service listens for compilation requests and transforms Java into JavaScript code:
```
./gradlew -p gwtSuperDev
```