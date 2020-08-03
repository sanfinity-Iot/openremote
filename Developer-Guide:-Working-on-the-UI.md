Working on the UI means working on any of: 

* Web applications that provide functionality to endusers
* Web components and their demos, shared between applications
* Keycloak theme(s) used in authentication screens
* Working on the (legacy GWT) Manager client

## Quickstart
You will need the standard tool chain (see [[Preparing the environment|Developer Guide: Preparing the environment]]) to be able to build and run the components and apps. Working on the web applications and/or components will also generally require backend services to interact with, you can either:

* Run a Manager instance in an IDE (refer to [[Setting up an IDE|Developer Guide: Setting up an IDE]])
* Deploy the Manager using a Docker container (refer to [UI Development profile](./Developer-Guide%3A-Docker-compose-profiles#ui-development-devyml))

As an example if working on the `rest` component then `cd` into the `demo/demo-rest` app and run the following `npm` scripts at the same time:

- `npm run modelWatch` - Starts a gradle task to watch the model `java` code for changes and auto generates the `model` and `restclient` typescript files and compiles them
- `npm run watch` - Watches typescript files (including referenced typescript projects) for changes and auto transpiles to javascript
- `npm run serve` - Starts webpack dev server and serves the web app which can then be accessed at `http://localhost:9000/demo-rest/`

If you want to create a new `component` or `app` then simply copy an existing one as a template (when creating a `component` then you may need to create a corresponding `demo` which acts as a development harness that can be served by webpack dev server - one demo may act as harness for multiple components).

## UI Components & Apps (`/ui`)
All UI components and apps are located in the `ui` directory; here you can find the standard OpenRemote web UI components and apps using a monorepo architecture. The code is divided into categories by directory:
* [`component`](../tree/master/ui/component) - Base OpenRemote JS modules and web components (built using Polymer) these are written as ES6 modules
* [`app`](../tree/master/ui/app) - Built-in OpenRemote web applications (applications can be built with whatever frameworks/libraries are desired)
* [`demo`](../tree/master/ui/demo) - Demos of each web component (provides a development harness for developers working on the components)
* [`keycloak/themes`](../tree/master/ui/keycloak/themes) - Keycloak themes which provide custom styled login and account management screens (see Keycloak [documentation](https://www.keycloak.org))

Typescript is used to provide static typing with the OpenRemote model available in the `@openremote/model` component package; the components are published to `npm` under the `@openremote` scope; see the README in each component for information about each specific component.

Yarn workspaces are used to symlink the OpenRemote packages into a root `node_modules` directory (refer to the Yarn [documentation](https://yarnpkg.com/) for more details). Yarn acts as an alternative to `npm` so please use it.

NPM scripts are used for build and development purposes (the main build tool for the entire code base is `gradle` so there are `gradle` tasks that launch NPM scripts - these `gradle` tasks have the prefix `npm` followed by the NPM script name e.g. `npmBuild`).

The following standard NPM scripts are used throughout the components and apps for consistency:

* `clean` - Cleans up any build artefacts (typically uses the [`shx`](https://www.npmjs.com/package/shx) package)
* `build` - Build the component/app ready for publishing/deployment
* `test` - Run any automated tests
* `modelWatch` - Starts a `gradle` task to watch the `java` model code and to run the `typescript` generator when changes are detected, the generated `model` and `restclient` typescript code is then transpiled to `javascript`
* `modelBuild` - Similar to `modelWatch` but just does a onetime build rather than watching for changes
* `serve` - Starts `webpack dev server` typically on `http://localhost:9000/[app|demo]/`
* `watch` - Runs the typescript compiler in watch mode with project references (e.g. `npx tsc -b -w`) to compile `typescript` code on changes

The above script names should be used in `package.json` files and then appropriate `build.gradle` files should be added, see existing components, apps, demos for examples. Typically only the `apps` need to be built by `gradle` tasks as typescript should follow project references and compile any dependent components.

### Components
Components can be developed and tested in isolation (with dependencies on other components and/or public npm modules as required). Some components have no visuals and provide standard OpenRemote functionality e.g. `@openremote/core`, whilst others provide visuals that allow interaction with the Manager backend.

#### Maps
If you are working on a map component then please refer to the [working on maps](./Developer-Guide:-Working-on-maps).

### Apps
Apps bring together components and/or public NPM modules and can be written using any framework/library etc that is
compatible with web components (see [here](https://custom-elements-everywhere.com/)).

### Demos
These are apps for development purposes Generally a 1-1 mapping between components and demos; they provide a simple harness for the components that can be used during development and optionally can be deployed to offer component demos. 

### Keycloak Themes
Each theme is located in its own directory within the `keycloak/themes` directory. When running any of the `dev` docker compose profiles; each theme can be volume mapped into the keycloak service and changes are reflected in real time allowing for easy development of these themes (see [Docker compose profiles](./Developer-Guide:-Docker-compose-profiles) for more details).

## Working on the legacy Manager GWT web application (`/client`)
If you are working on the legacy GWT based Manager web application you will need to start the GWT compiler and keep it running in the background; this service listens for compilation requests and transforms Java into JavaScript code:
```
./gradlew gwtSuperDev
```

# See also

- [Get Started](https://openremote.io/get-started-manager/)
