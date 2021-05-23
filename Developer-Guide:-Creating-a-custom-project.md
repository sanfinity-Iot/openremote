* [Project Structure](#project-structure)
* [Setup code](#setup-code)

The setup described here is useful if you want to create a custom project that extends OpenRemote, and keep extensions and other deployment-specific details separate in a possibly private Git repository.

Your project code and configuration files can use the main OpenRemote project directly, but you can manage them independently. You can track and merge changes on the upstream main OpenRemote project, targeting specific tags for your integration. Or you can keep up with the main development branch and pull the latest code.

[[Preparing the environment|Developer Guide: Preparing the environment]] is required before starting a custom project.

## Project Structure

Custom projects should have a dependency on the main OpenRemote repository using a Git submodule as described below, and the following folder(s) should exist in your `myproject` folder:

* `openremote` - Submodule of main OpenRemote repository, Git instructions below.

* `deployment` - Project data, console apps JS/HTML resources, map data, and the extensions directory for your projects JAR files. Copy the deployment folder of the main OpenRemote repository and customise. The original OpenRemote folder is included in the OpenRemote images and can be overridden at deployment-time with your folder.

* `profile` - Project configuration, Docker Compose files for different environments (e.g. staging, production) that extend the OpenRemote main profiles

* `myextension1` - Project code extending OpenRemote, includes Java/Groovy source and should produce a tested JAR for the `deployment/extensions/` directory. Extensions will be loaded automatically on startup, you can create as many JARs as required. Your Protocol, Setup, and other implementation can be in separate Gradle sub-projects.

* `console` - Project native consoles (Android, iOS) extending the folders of the main OpenRemote repository (TODO: We should support extensions for console native shells and not need this in custom projects)

* `ui` - Project code for frontend

* `agent` - Project code for custom/project specific protocols


## Working with the Git repositories

### Creating a new project

If you are working on an existing custom project then you can skip this section.

First you must create a new Git repository for your project, then add the main [OpenRemote repository](https://github.com/openremote/openremote.git) as a [submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Your project is the root project, and the main OpenRemote repository and its projects become sub-projects of your project, on which you can depend in your build and code.

To add a submodule of the OpenRemote main repository use the following commands (this will track the `master` branch of the main OpenRemote repository, change as required):

```
mkdir myproject/
cd myproject/
git init
git submodule add -b master https://github.com/openremote/openremote.git openremote/
git submodule status
```

This will generate a special 'openremote' submodule link object (folder) and also add an entry in `.gitmodules`. The generated `.gitmodules` file contains a list of all submodules in your project. You commit the submodule link to your project repository and this tells Git which commit to checkout in that submodule.

### Creating a .gitignore

Also prevent committing build/runtime files that may be generated inside your project directory, add a `.gitignore`:

```
.DS_*
.gradle
.vagrant
.classpath
.project
.settings
.vscode

build/
bin/
out/

*.iml
*.ipr
*.iws
.idea/
.local/
*~
*.sh

node_modules/
yarn-error.log
deployment/proxy/
deployment/postgresql/
deployment/manager/extensions/
deployment/manager/openremote.log*
deployment/manager/app/
deployment/manager/shared/
deployment/keycloak/
# If you have a large map, don't commit it but store it only locally
# deployment/map/*.mbtiles
```

### Pulling the latest changes

Everyone who checks out your project repository must initialize and fetch the submodules when they clone your project for the first time. Clone the repository:

```
git clone git@github.com:myorg/myproject.git
```

Then fetch the submodule(s):

```
cd myproject/
git submodule init
```

From then on when you pull changes for the project you must also update any submodule(s) to the correct commit that the project is using:

```
git submodule update
```

You can update the submodule to the latest commit of the tracked branch by using the following:

```
git submodule update --remote --rebase
```

When you want to work on your project and files inside the submodule at the same time, you must make two commits: one on the submodule's repository and one in your project's repository, the commit on the project repository also includes the updated reference to the new submodule commit.

## Setting up the Gradle build

Adding the OpenRemote code repository as a submodule is the first step for integration. You should also include it in the Gradle build of your project, so you can depend directly on OpenRemote.

You'll need the following files in your `myproject` directory:

* `settings.gradle` - This defines which projects and sub-projects you have.
* `gradle.properties` - Key-value configuration items that can be used in build scripts, such as version numbers.
* `build.gradle` - The build settings common to all projects.
* `myextension1/build.gradle` - Your extension's build settings.

To install the Gradle wrapper in the project (these files should be committed to your repo):

```
cd myproject/
gradle wrapper
```

Then write a `settings.gradle` file:

```
rootProject.name = "myproject"

// Include sub-projects dynamically, every directory with a build.gradle (and no .buildignore)
fileTree(dir: rootDir, include: "**/build.gradle", excludes: ["**/node_modules/**", "**/generic_app/**"])
        .filter { it.parent != rootDir }
        .filter { !file("${it.parent}/.buildignore").exists() }
        .each {
    include it.parent.replace(rootDir.canonicalPath, "").replace("\\", ":").replace("/", ":")
}
```

These settings will load sub-projects (those with a `build.gradle` file) recursively, so everything under the `openremote/` submodule will become part of your project. Your project is the root project of the Gradle build.

Continue with a `build.gradle` file in your root project directory, applying build settings common to all projects:

```
allprojects {
    // Apply common project build script but exclude submodule, it has its own build.gradle
    if (!path.startsWith(":openremote")) {
        apply from: "${project(":openremote").projectDir}/project.gradle"
    }
}
```

Add OpenRemote dependencies and tasks to your `myextension1/build.gradle' with:

```
apply plugin: "java"
apply plugin: "groovy"

dependencies {
    // Agent and protocol SPI - Uncomment if adding custom `agent` code to your custom project
    // compile project(":agent")

    // Setup SPI
    compile project(":openremote:manager")

    // Your own dependencies
    // compile "org.slf4j:slf4j-api:$slf4jVersion"

    // Testing API
    testCompile project(":openremote:test")
}

test {
    // Tests should always execute in the openremote main repo root directory
    workingDir = project(":openremote").projectDir
}

# Automatically copy this extension into the deployment/extensions/ directory
task installDist(type: Copy) {
    from jar.outputs
    into "${project(':deployment').buildDir}/manager/extensions"
}
```

Finally, externalise version strings for your code and own dependencies, and add any other build settings to `gradle.properties`:

```
projectVersion = 1.0-SNAPSHOT

slf4jVersion = 1.7.25
groovyVersion = 2.4.12
```

## Setting up the deployment directory

Copy the `deployment` directory of the `openremote` submodule into your project's root (e.g. `./myproject/deployment`) and:

1. Customize logging of the Manager in `manager/logging.properties`
1. Change the map settings and  data in `map` (see [working on maps](./Developer-Guide%3A-Working-on-maps))
1. Put any extension JAR files that aren't built by a gradle project in this repository into `manager/extensions`
1. Put any shared static HTML/JS code into `manager/shared`
1. Edit the `build.gradle` to copy any apps, shared static HTML/JS and/or keycloak themes from the `openremote` submodule deployment into this deployment:

```
apply plugin: 'idea'
idea {
    module {
        excludeDirs += file("manager/app")
        excludeDirs += file("keycloak")
        excludeDirs += file("manager/extensions")
        excludeDirs += file("manager/shared")
    }
}

task copyKeycloakThemes(type: Copy) {
    dependsOn resolveTask(":ui:keycloak:installDist")
    from "${resolveProject(':deployment').projectDir}/keycloak"
    into "keycloak"
}

task copyManagerApp(type: Copy) {
    dependsOn resolveTask(":client:installDist")
    from "${resolveProject(':deployment').projectDir}/manager/app/manager"
    into "manager/app/manager"
}

task copyManagerShared(type: Copy) {
    from "${resolveProject(':deployment').projectDir}/manager/shared"
    into "manager/shared"
}

task clean() {
    doLast {
        delete "manager/app/manager"
        delete "manager/shared"
        delete "manager/extensions"
        delete "keycloak"
    }
}

task installDist(type: Copy) {
    dependsOn copyKeycloakThemes, copyManagerApp, copyManagerShared
}
```
6. Create deployment `Dockerfile` with the following content:
```
# Default deployment when manager-data volume is empty
FROM debian:stretch

RUN mkdir -p /deployment/extensions
ADD build /deployment
```
7. Create deployment `.dockerignore` file with the following content:
```
# Uncomment below if map is too large and should be deployed with direct file copying
#build/map/mapdata.mbtiles
build/manager/openremote.log*
```

## Creating custom  UI apps, components and Keycloak themes
All frontend related code should be within a `ui` directory, copy the same directory layout as found in the `openremote` submodule and copy the following files to the same location in the custom project:

```
ui/.gitignore
ui/build.gradle
```

Then copy the `yarn.lock` from the `openremote` repo into the project root.

Then create a `package.json` in the repo root with the following content:

```json
{
  "private": true,
  "workspaces": [
    "openremote/ui/util",
    "openremote/ui/app/*",
    "openremote/ui/component/*",
    "openremote/ui/demo/*",
    "openremote/client/src/main/webapp",
    "ui/app/*",
    "ui/component/*",
    "ui/demo/*"
  ]
}
```

Then create `tslint.json` in the `ui` dir with the following content:

```json
{
  "extends": "../openremote/ui/tslint.json"
}
```

If custom keycloak themes are required then you need to create `dev-testing.yml` in the `profile` dir with the following content:

```yml
#
# Profile for doing IDE development work and running build tests.
#
# Run this in the background for full ./gradlew clean build
#
version: '2.4'

volumes:
  postgresql-data:

services:

  keycloak:
    extends:
      file: ../openremote/profile/deploy.yml
      service: keycloak
    volumes:
      - ../openremote/ui/keycloak/themes:/deployment/keycloak/themes
      - ../ui/keycloak/themes:/deployment/keycloak/customthemes
    # Access directly if needed on localhost
    ports:
      - "8081:8080"
    depends_on:
      postgresql:
        condition: service_healthy

  postgresql:
    extends:
      file: ../openremote/profile/deploy.yml
      service: postgresql
    # Access directly if needed on localhost
    ports:
      - "5432:5432"
    volumes:
      - postgresql-data:/var/lib/postgresql/data
```

Custom apps, components and keycloak themes can then be created as required for the project (see [working on the UI](./Developer-Guide%3A-Working-on-the-UI))

## Adding agent module (optional)
If you need to write custom protocol(s) then create an `agent` directory in the root of your project (e.g. `./myproject/agent`) and create `build.gradle` in this directory with the following content:

```
apply plugin: "java"
apply plugin: "groovy"

dependencies {
    compile project(":openremote:agent")
}

task installDist(type: Copy) {
    from jar.outputs
    into "${project(':deployment').buildDir}/manager/extensions"
}
```

Then update your extension `build.gradle` (e.g. `myextension1/build.gradle`) and uncomment the `compile project(":agent")` line.



## Setup code
Custom setup code allows for programmatic configuration of a clean installation including the provisioning of `Agents`, `Assets`, `users`, `rules`, etc. This means that the system loads in a pre-configured state that can easily be reproduced in a new instance.

Setup code is executed via the `SetupService` and is only executed if the `SETUP_WIPE_CLEAN_INSTALL` environment variable is set to `true` or if the database that the instance uses is empty. `SetupTasks` implementations are discovered using the standard java `ServiceLoader` mechanism and must therefore be registered via `META-INF/services` mechanism, generally implementations should extend the `EmptySetupTasks` which will do basic configuration of `keycloak`:

```
public class TestSetupTasks extends EmptySetupTasks {

    @Override
    public List<Setup> createTasks(Container container) {
        super.createTasks(container);

        // Do custom setup here
    }
```