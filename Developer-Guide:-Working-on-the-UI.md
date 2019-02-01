Working on the UI means working on any of:

* Web applications
* Web components
* Keycloak theme(s)

## Quickstart
You will need the standard tool chain (see [Setting up the environment](./Developer-Guide%3A-Preparing-the-environment)) to be able to build and run the components and apps. Working on the web applications and/or components will also generally require a manager to interact with, you can either:

* Run a Manager instance in an IDE (refer to [Working on the Manager](./Developer-Guide%3A-Working-on-the-Manager))
* Deploy the Manager using a Docker container (refer to [UI Development profile](./Developer-Guide%3A-Docker-compose-profiles#ui-development-devyml))

As an example if working on the `rest` component then run the following `gradle` tasks at the same time:

- `./gradlew -t -p ui modelWatch` - Watches the model for changes and auto generates the `model` typescript definition and `restclient`.
- `./gradlew -p ui/demo/demo-rest tscWatch` - Watches typescript files (including referenced typescript projects) for changes and auto transpiles to javascript
- `./gradlew -p ui/demo/demo-rest npmServe` - Starts webpack dev server and serves the web app which can then be accessed at `http://localhost:9000/demo-rest/`

If you want to create a new `component` or `app` then simply copy an existing one as a template (when creating a `component` then you need to create a corresponding `demo` which acts as a development harness that can be served by webpack dev server).

## UI Components & Apps (`/ui`)
All UI components and apps are located in the `ui` directory; here you can find the standard OpenRemote web UI components and apps using a monorepo architecture. The code is divided into categories by directory:

* `component` - Base OpenRemote JS modules and web components (built using Polymer) these are written as ES6 modules
* `app` - Built-in OpenRemote web applications (applications can be built with whatever frameworks/libraries are desired)
* `demo` - Demos of each web component (provides a development harness for developers working on the components)
* `keycloak/themes` - Keycloak themes which provide custom styled login and account management screens (see Keycloak [documentation](https://www.keycloak.org))

Typescript is used to provide static typing with the OpenRemote model available in the `@openremote/model` component package; the components are published to `npm` under the `@openremote` scope; see the README in each component for information about each specific component.

Yarn workspaces are used to symlink the OpenRemote packages into a root `node_modules` directory (refer to the Yarn [documentation](https://yarnpkg.com/) for more details). Yarn acts as an alternative to `npm` so please use it.

Gradle is used as the standard build tool throughout the stack, there are gradle tasks available which delegate to node as follows:

* `npmInstall/yarnInstall` -> `yarn install`
* `npmClean` -> `npm run-script clean`
* `npmBuild` -> `npm run-script build`
* `npmTest` -> `npm run-script test`
* `npmServe` -> `npm run-script serve`
* `npmServeProduction` -> `npm run-script serveProduction`
* `tscWatch` -> `npx tsc -b --watch`

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

## Working on apps and components
Before you can work on any apps or components you will need to issue the following commands in order to perform a `yarn install` and to build the `model` and `rest` components:

```
./gradlew -p ui yarnInstall
./gradlew -p ui/component/model build
./gradlew -p ui/component/rest build
```

or alternatively build the whole `ui` project (will take a little longer but is just a single command):

```
./gradlew -p ui build
```

**NOTE: The `yarnInstall` task must be run whenever new components are added in order to make them available for import in other components/demos/apps and the model and rest `build` tasks must be run whenever the openremote model changes. Whenever you do a pull from the remote repository it is worth running the above commands to ensure things are up to date**

If you are making frequent changes to the openremote model (which includes the REST API) then you can use the continuous mode of gradle in combination with the `watch` task to rebuild the `model` and `rest` components automatically whenever the model files change:

```
./gradlew -t -p ui modelWatch
```

### Compiling typescript
Typescript is used throughout the `ui` code base and this requires transpiling to javascript; there are several ways to trigger compilation but project references in combination with `tsc -b --watch` is preferred as it handles incremental builds across multiple typescript projects (each component, app, demo is a separate typescript project). The gradle `tscWatch` task can be used to start a typescript incremental build:

```
./gradlew -p <COMPONENT|DEMO|APP> tscWatch          e.g. ./gradlew -p ui/demo/demo-or-map tscWatch
```

### Running demos and apps
Demos and apps use `webpack` for module bundling and each can be served using `webpack-dev-server` which provides source maps and hot module replacement functionality, the following gradle command can be used (which just delegates to `npm` as described above):

```
./gradlew -p <COMPONENT|DEMO|APP> npmServe          e.g. ./gradlew -p ui/demo/demo-or-map npmServe
```

## Working on the legacy Manager GWT web application (`/client`)
If you are working on the legacy GWT based Manager web application you will need to start the GWT compiler and keep it running in the background; this service listens for compilation requests and transforms Java into JavaScript code:
```
./gradlew -p gwtSuperDev
```