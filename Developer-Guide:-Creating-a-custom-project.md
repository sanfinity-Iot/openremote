The setup described here is useful if you want to create a custom project that extends OpenRemote, and keep extensions and other deployment-specific details separate in a possibly private Git repository.

Your project code and configuration files can use the main OpenRemote project directly, but you can mange them independently. You can track and merge changes on the upstream main OpenRemote project, targeting specific tags for your integration. Or you can keep up with the main development branch and pull the latest code.

[[Preparing the environment|Developer Guide: Preparing the environment]] is required before starting a custom project.

## Project Structure

Custom projects should have a dependency on the main OpenRemote repository using a Git submodule as described below, and the following folder(s) should exist in your `myproject` folder:

* `openremote` - Submodule of main OpenRemote repository, Git instructions below.

* `deployment` - Project data, console apps JS/HTML resources, map data, and the extensions directory for your projects JAR files. Copy the deployment folder of the main OpenRemote repository and customise. The original OpenRemote folder is included in the OpenRemote images and can be overridden at deployment-time with your folder.

* `profile` - Project configuration, Docker Compose files for different environments (e.g. staging, production) that extend the OpenRemote main profiles

* `myextension1` - Project code extending OpenRemote, includes Java/Groovy source and should produce a tested JAR for the `deployment/extensions/` directory. Extensions will be loaded automatically on startup, you can create as many JARs as required. Your Protocol, Setup, and other implementation can be in separate Gradle sub-projects.

* `console` - Project native consoles (Android, iOS) extending the folders of the main OpenRemote repository (TODO: We should support extensions for console native shells and not need this in custom projects)


## Working with the Git repositories

### Cloning an existing project repository

Everyone who checks out your project repository must initialize and fetch the submodules when they clone your project for the first time. Clone the repository:

```
git clone git@github.com:myorg/myproject.git
```

Then fetch the submodule(s):

```
cd myproject/
git submodule init
git submodule update
```

### Creating a new project

First you must create a new Git repository for your project, then add the main [OpenRemote repository](https://github.com/openremote/openremote.git) as a [submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Your project is the root project, and the main OpenRemote repository and its projects become sub-projects of your project, on which you can depend in your build and code.

There are two ways of adding a Git submodule: one which tracks a branch of the main OpenRemote repository, and one which tracks a particular commit.

For development it is advisable to track a (master) branch and for releasing your project, it is best to track a commit. See [this article for more information](http://www.vogella.com/tutorials/GitSubmodules/article.html#submodules_trackbranch) about the two methods.

### Tracking a branch of the OpenRemote main repository

By tracking a branch you can issue a single command in your project repository to fetch new commits from the OpenRemote main repository, and push any changes you make in the submodule to the main OpenRemote repository. This setup will track the `master` branch of the main OpenRemote repository:

```
mkdir myproject/
cd myproject/
git init
git submodule add -b master https://github.com/openremote/openremote.git openremote/
git submodule status
```

### Tracking a commit of the OpenRemote main repository

This setup tracks the last commit of the OpenRemote main repository, effectively attaching your project to that version of OpenRemote:

```
mkdir myproject/
cd myproject/
git init
git submodule add https://github.com/openremote/openremote.git openremote/
git submodule status
```

Adding a submodule will generate a special 'openremote' submodule link object (folder) and also add an entry in `.gitmodules`. The generated `.gitmodules` file contains a list of all submodules in your project. You commit the submodule link to your project repository and this tells Git which commit to checkout in that submodule.

### Creating a .gitignore

Also prevent committing build/runtime files that may be generated inside your project directory, add a `.gitignore`:

```
bower_components/
deployment/proxy/
deployment/postgresql/
deployment/manager/extensions/
# If you have a large map, don't commit it but store it only locally
# deployment/manager/*/*.mbtiles
```

### Committing changes and staying up-to-date

Your projects' repository is the root and the OpenRemote submodule is another repository you have to handle. They are connected with the `openremote` special directory in your project, and this link has to be updated manually when it should reference a different version of the main OpenRemote repository.

Whenever you want to update your project with a new version of OpenRemote, you must update the submodule of the main OpenRemote repository you have checked out.

When you want to work on your project and files inside the submodule at the same time, you must make two commits: one on the submodule's repository and one in your project's repository.

If your submodule is tracking a branch then you can update the submodule to use the latest commit on that branch. Execute in your project directory:

```
git submodule update --remote --rebase
```

You now can make changes to the files in the `openremote` submodule directory and commit them on any branch in the main OpenRemote repository.

If your submodule is tracking a commit then you can update the submodule to use the latest commit by:

```
cd openremote
git checkout master
git pull
```

Committing changes to the `openremote` submodule and main repository means that the submodule reference has to be changed as well, so it will point to the desired commit/branch on the main repository.

**Whenever a submodule is updated to use a different commit (no matter whether it tracks a branch or a specific commit) you have to record this change in your project repository:**

```
git add openremote
git commit -m "Updated OpenRemote main repository submodule link"
```

This is the second commit we've mentioned earlier in this section.

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
gradle wrapper --gradle-version 4.2.1
```

Then write a `settings.gradle` file:

```
rootProject.name = "myproject"

// Include sub-projects dynamically, every directory with a build.gradle (and no .buildignore)
fileTree(dir: rootDir, include: "**/build.gradle")
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

If you have any directory with a `bower.json` file, include it in the build by placing a `build.gradle` next to it with this content:

```
task installDist {
    dependsOn bowerPrune, bowerUpdate
}
```

## Creating custom apps and extensions

Copy the `deployment` directory of the `openremote` submodule into your project's root (e.g. `/myproject/deployment`) and make changes:

1. Create JavaScript/HTML applications in `deployment/manager/consoles`
1. Change the Manager UI map data, default coordinates, zoom levels, etc. in `deployment/manager/map`
1. Extend the Manager UI and customize the theme in `deployment/manager/ui`
1. Put any extension JAR files into `deployment/manager/extensions`
1. Customize logging of the Manager in `logging.properties`

### Updating Manager map data

We currently do not have our own pipeline for extracting/converting OSM data into vector tilesets but depend on the extracts offered on https://openmaptiles.org/.

You can extract smaller tilesets with the following procedure:

1. Install tilelive converter: 
    `npm install -g mapnik mbtiles tilelive tilelive-bridge`
1. Select and copy boundary box coordinates of desired region: 
    http://tools.geofabrik.de/calc/#tab=1 
1. Extract the region with: 
    `tilelive-copy --minzoom=0 --maxzoom=14 --bounds="BOUNDARY BOX COORDINATES" theworld.mbtiles myextract.mbtiles`

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

See our guide for [[Working on Manager|Developer Guide: Working on Manager]], this helps you create Run/Debug Configurations in an IDE.

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

When you are [[Working on Manager|Developer Guide: Working on Manager]], you should also run the `dev-testing.yml` in the background as you execute JUnit tests ad-hoc from within your IDE. The services are also required to start the Manager in your IDE.

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

Do not forget to set `KEYCLOAK_PASSWORD` when going into production and exposing OpenRemote services beyond your development network.

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