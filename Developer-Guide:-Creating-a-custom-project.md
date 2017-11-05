The setup described here is useful if you want to create a custom project that extends OpenRemote, and keep that extensions separate in a possibly private Git repository.

Your service provider code and project-specific configuration can be managed independently. You can track and merge changes on the upstream main OpenRemote project, targeting specific tags for your integration or always keeping up with the main development branch.

## Project Structure

Custom projects should have a dependency on the main OpenRemote repository (using a submodule as described below) and the following folder(s) should be used in a `myproject` folder:

* `openremote` - Submodule of main OpenRemote repository
* `deployment` - Project configuration, console apps JS/HTML resources, map data, and the extensions directory for your projects JAR files. Copy the deployment folder of the main OpenRemote repository and customise. The original OpenRemote folder is included in the OpenRemote images and can be overridden at deployment-time with your folder.
* `myextension1` - Project code extending OpenRemote, includes Java/Groovy source and should produce a tested JAR for the `deployment/extensions/` directory. Extensions will be loaded automatically on startup, you can create as many JARs as required (e.g. your Protocol, Setup code in separate modules).
* `console` - Project consoles (Android, iOS) extending the folders of the main OpenRemote repository (TODO: We should support extensions for console shells and not need this in custom projects)

## Working with the Git repositories

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

Adding a submodule will generate a special g=Git submodule object (folder) and also add an entry in `.gitmodules`. The generated `.gitmodules` file contains a list of all submodules in your project. You commit the submodule link to your project repository and this tells Git which commit to checkout in that submodule.

Also prevent committing build/runtime files that may be generated inside your project directory, add a `.gitignore`:

```
bower_components/
deployment/proxy/
deployment/postgresql/
deployment/manager/extensions/
# If you have a large map, don't commit it but store it only locally
# deployment/manager/*/*.mbtiles
```


### Updating the submodule

Whenever you want to update your project with a new version of OpenRemote, you must update the submodule of the main OpenRemote repository you have checked out.

If your submodule is tracking a branch then you can update the submodule to use the latest commit on that branch. Execute in your project directory:

```
git submodule update --remote
```

If your submodule is tracking a commit then you can update the submodule to use the latest commit by:

```
cd openremote
git checkout master
git pull
```

**Whenever a submodule is updated to use a different commit (no matter whether it tracks a branch or a specific commit) you have to then commit this change in your project repository:**

```
git add openremote
git commit -m "Updated OpenRemote main repository submodule link"
```

### Cloning the project repository

Everyone who checks out your project repository must initialize and fetch the submodules when they clone your project for the first time:

```
git submodule init
git submodule update
```

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
rootProject.name = "myextension1"

// Include sub-projects dynamically, every directory with a build.gradle (and no .buildignore)
def rootDir = new File(".").canonicalPath
fileTree(dir: rootDir, include: "**/build.gradle")
        .filter { it.parent != rootDir }
        .filter { !file("${it.parent}/.buildignore").exists() }
        .each {
    include it.parent.replace(rootDir, "").replace("\\", ":").replace("/", ":")
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

## Customizing deployment

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

If you work only on the console frontend apps and want to deploy the full stack in development mode, run in `myproject` folder:

`DEPLOYMENT_DIRECTORY=$PWD/deployment docker-compose -p myproject -f openremote/profile/dev.yml up --build`

TODO




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
