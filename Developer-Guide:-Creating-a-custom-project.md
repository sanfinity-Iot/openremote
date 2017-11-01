The setup described here is useful if you want to create a custom project that extends OpenRemote, and keep that extensions separate in a possibly private Git repository.

Your service provider code and project-specific configuration can be managed independently. You can track and merge changes on the upstream main OpenRemote project, targeting specific tags for your integration or always keeping up with the main development branch.

## Project Structure

Custom projects should have a dependency on the main OpenRemote repository (using a submodule as described below) and the following folder(s) should be used:

* console - Should replace the console folder of the main OpenRemote repository
* deployment - Should replace the deployment folder of the main OpenRemote repository
* openremote - Submodule of main OpenRemote repository
* profile - Should replace the profile folder of the main OpenRemote repository and the services should link to the main repository services; refer to the [Developer Guide](./Developer-Guide%3A-Deploying-to-a-Docker-engine#docker-deployment-profiles) for information about Docker profiles and conventions.
* setup - Should contain custom setup tasks (loaded at runtime using ServiceLoader) and tests for the custom project


## Working with the git repositories

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

In IntelliJ IDEA open `Settings` > `Version Control` and add the new submodule directory as a Git repository. IntelliJ will now pull and push automatically on the corresponding remote repository when any changes are made on submodules.


## Writing a Gradle build script

Adding the OpenRemote code repository as a submodule is the first step for integration. You should also include it in the Gradle build of your project, so you can depend directly on OpenRemote.

Initialize Gradle wrapper:

```
cd myproject/
gradle wrapper --gradle-version 3.3
```

Then write a `settings.gradle` file:

```
rootProject.name = "myproject"

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

Continue with a `build.gradle` file in your project directory:

```
allprojects {
    // Apply common project build script but exclude submodule, it has its own build.gradle
    if (!path.startsWith(":openremote")) {
        apply from: "${project(":openremote").projectDir}/project.gradle"
    }
}
```

In this example, when you'll work with Gradle in your project directory, you'll depend on the published and versioned OpenRemote agent SPI and testing API artifacts. However, when you have a checked out submodule of the main OpenRemote project, your custom project will use an internal project dependency on the SPI and API code.

You can depend on any API or SPI of OpenRemote and at the same time browse and refactor the OpenRemote code.

Finally, externalize version strings and other build settings and add them to `gradle.properties`:

```
projectVersion = 1.0-SNAPSHOT

slf4jVersion = 1.7.10
groovyVersion = 2.4.1
spockVersion = 1.1-groovy-2.4
junitVersion = 4.12
```

Your tests can run inside the same environment as OpenRemote tests. Use [Spock](spockframework.org/spock/docs/) and write [Groovy](http://www.groovy-lang.org/) code in `myproject/src/test/groovy/org/myorg/test/MyProjectTest.groovy`:

```
package org.myorg.test

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