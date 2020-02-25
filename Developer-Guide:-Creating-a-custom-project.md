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
fileTree(dir: rootDir, include: "**/build.gradle", excludes: ["**/node_modules/**"])
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
    // Agent and protocol SPI
    compile project(":agent")

    // Setup SPI
    compile project(":openremote:manager:server")

    // Your own dependencies
    compile "org.slf4j:slf4j-api:$slf4jVersion"

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
    into "${project(':deployment').projectDir}/manager/extensions"
}
clean {
    delete = "${project(':deployment').projectDir}/manager/extensions/${jar.archiveName}"
}
```

Finally, externalise version strings for your code and own dependencies, and add any other build settings to `gradle.properties`:

```
projectVersion = 1.0-SNAPSHOT

slf4jVersion = 1.7.25
groovyVersion = 2.4.12
```

## Setting up the deployment directory

Copy the `deployment` directory of the `openremote` submodule into your project's root (e.g. `/myproject/deployment`) and:

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
#map/mapdata.mbtiles
manager/openremote.log*
```

## Creating custom  UI apps, components and Keycloak themes
All frontend related code should be within a `ui` directory, copy the same directory layout as found in the `openremote` submodule and copy the following files:

```
ui/.gitignore
ui/build.gradle
```

Then create a `package.json` in the repo root with the following content:

```json
{
  "private": true,
  "workspaces": [
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

Custom apps, components and keycloak themes can then be created as required for the project (see [working on the UI](./Developer-Guide%3A-Working-on-the-UI))

## Building and running your project

If you work only on the console frontend apps and files in the `deployment` folder, and want to deploy the full stack in development mode, first execute the Gradle build without tests:

```
./gradlew clean installDist
```

This will build your extensions and the main OpenRemote services and prepare all files for Docker image build in the right places.

Then build Docker images and start the stack:

```
DEPLOYMENT_DIRECTORY=$PWD/deployment docker-compose -p myproject -f openremote/profile/dev.yml up --build
```

This will use the `deployment` folder of your project. You should specify a custom project name, it's the prefix of running containers and data volumes.

Each project should have its own space at runtime, but all containers should use the regular OpenRemote images. Customisation is best done in `deployment` extensions, the Docker images are not project-specific.

When you are working on your extension Java/Groovy code, you don't want to wait for a full stack build and deploy. So if you have not changed any files in the `openremote` directory, you can simply restart your project's stack to pick up the new extension JARs in the `deployment` directory after building only your extension(s):

```
./gradlew myextension1:installDist
... docker-compose down|up ...
```

### Working on Manager backend services or UI

Read the [[Setting up an IDE|Developer Guide: Setting up an IDE]], this helps you create Run/Debug Configurations in an IDE.

## Testing

The tests for `myextension1` should be created in the source directory `myextension1/src/test/groovy`.

Your tests can run inside the same environment as OpenRemote tests. Use [Spock](spockframework.org/spock/docs/) and write [Groovy](http://www.groovy-lang.org/) code in `myextension1/src/test/groovy/myextension1/test/MyProjectTest.groovy`:

```
package myextension1;

import ...

class MyProjectTest extends Specification implements ManagerContainerTrait {

    def "Check something"() {

        given: "expected conditions and environment"
        def conditions = new PollingConditions(timeout: 30, initialDelay: 3)

        when: "the manager server has been deployed"
        def serverPort = findEphemeralPort()
        def container = startContainer(defaultConfig(serverPort), defaultServices())

        then: "we poll and wait for a result"
        conditions.eventually {
            assert true // TODO write tests
        }

        cleanup: "the server should be stopped"
        stopContainer(container);
    }
}
```

### Running tests

Some of our (and hopefully your) tests are end-to-end tests that require running background container services.

You might want to start with clean containers before running tests and you might have to restart containers after (failed) tests with `docker-compose [up|down]`.

You probably want to manage the data volumes with `docker volume`, although the default setup is to wipe and clean install everything over an existing database.

First start required background services for integration tests, execute the following from your project's directory:

`docker-compose -p myproject -f openremote/profile/dev-testing.yml up --build -d`

Then execute the tests:

```
./gradlew clean build installDist
```

When you no longer need the background services, stop them:

`docker-compose -p myproject -f openremote/profile/dev-testing.yml down`

When you are [[Setting up an IDE|Developer Guide: Setting up an IDE]], you should also run the `dev-testing.yml` in the background as you execute JUnit tests ad-hoc from within your IDE. The services are also required to start the Manager in your IDE.

## Creating a production deployment

The `dev.yml` and `dev-testing.yml` profiles you have used are bundled with the main OpenRemote project. You should read the whole [deploy.yml](https://github.com/openremote/openremote/blob/master/profile/deploy.yml), it is the basis of all other profiles. A good starting point for your own production setup is [demo.yml](https://github.com/openremote/openremote/blob/master/profile/demo.yml).

First create a `myproject/profile` directory and copy `openremote/profile/demo.yml`. Rename it to match the desired configuration, typical use cases are separate settings for staging and production environments.

Follow the steps in the `demo.yml` example. At a minimum, you want to disable `SETUP_WIPE_CLEAN_INSTALL`!

Have a look at our [[Maintaining an installation|Developer Guide: Maintaining an installation]] guide for finding typical runtime problems and monitoring options.

### Restarting a Manager with new settings

After changing any configuration in the `deployment/manager` directory, for example when you change logging settings, you should restart the service with:

```
SERVICE=manager && PROJECT=openremote && PROFILE=profile/demo.yml && \
  docker-compose -p $PROJECT -f $PROFILE stop $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE rm -f $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE create $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE start $SERVICE
```

Make sure the `SETUP_WIPE_CLEAN_INSTALL` environment variable is not set!

### Configuring security

A regular OpenRemote deployment exposes services only through HTTPS and WSS, the `proxy` service of OpenRemote manages keys via [Let's Encrypt](https://letsencrypt.org/).

Do not forget to set `SETUP_ADMIN_PASSWORD` when going into production and exposing OpenRemote services beyond your development network.

TODO Build/setup should generate a password and print it on console, bit of a problem with wipe clean install option

<!--
## Configuring security

In an OpenRemote system, servers use certificates to authenticate themselves to clients. Simple default TLS certificates (and the private key only known to the server(s)) have been created with:

```
#!/bin/bash

rm /tmp/or-*.jks /tmp/or-*.cer

keytool -genkeypair -alias "openremote" \
    -noprompt -keyalg RSA -validity 9999 -keysize 2048 \
    -dname "CN=openremote" \
    -keypass CHANGE_ME_SSL_KEY_STORE_PASSWORD \
    -keystore /tmp/or-keystore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD

keytool -exportcert \
    -alias "openremote" \
    -keystore /tmp/or-keystore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD \
    -file /tmp/or-certificate.cer

keytool -importcert -noprompt \
    -alias "openremote" \
    -file /tmp/or-certificate.cer \
    -keystore /tmp/or-truststore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD

keytool -list -v \
    -keystore /tmp/or-keystore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD

cp /tmp/or-keystore.jks controller/conf/keystore.jks
cp /tmp/or-truststore.jks controller/conf/truststore.jks

cp /tmp/or-keystore.jks beehive/ccs/conf/keystore.jks

```

At a minimum, you should re-create the stores and private key with your own passwords when deploying OpenRemote.

TODO: Unify default CAs in trust stores in all server systems, which (root) CAs should stay and what is the setup we recommend for OpenRemote users, etc.
-->