This guide helps you set up an environment with an IDE when you are done [[Preparing the environment|Developer Guide: Preparing the environment]], so you can work comfortably on the Manager backend services and/or the Manager UI.

## Starting required service dependencies

If you work with an IDE or want to run `./gradlew clean build`, this should be running on your system:

`docker-compose -p openremote -f profile/dev-testing.yml up --build`

If you are [[Creating a custom project|Developer Guide: Creating a custom project]], use this:

`docker-compose -p myproject -f openremote/profile/dev-testing.yml up --build`

If you are working on Manager UI, start the GWT compiler and keep it running in the background:

```
./gradlew -p gwtSuperDev
```

This service listens for compilation requests and transforms Java into JavaScript code.

## Importing a project in an IDE

#### IntelliJ IDEA

- Create a `New Project From Existing Sources` and import as a Gradle project
- Note that IntelliJ might time out if a background Gradle process (for example, running the GWT compiler server) blocks the Gradle import. Stop and start the background process to unblock.

The log messages of the running application can be colour-highlighted with the [GrepConsole plugin](https://plugins.jetbrains.com/plugin/7125-grep-console) and our [configuration](https://gist.github.com/christianbauer/9cd3ef6a871c2a3472bd70a216f3eb14).

#### Eclipse

- Run `./gradlew eclipse`
- In Eclipse go to `File` > `Import` and import the project as `Existing Projects into Workspace`

## Setting the working directory

All Docker and Gradle commands **must be executed in the project root directory**. If you are working on the main OpenRemote repository, this means the root of the repository. If you are [[Creating a custom project|Developer Guide: Creating a custom project]], this means the root of your project's repository.

The working directory in your IDE however must always be set to the **OpenRemote project directory**. All configuration settings in source code default to this location. This means if you are [[Creating a custom project|Developer Guide: Creating a custom project]], your IDE will work in the `openremote` submodule directory.

We recommend you set this as the default directory in your IDE for all *Run Configurations*. To improve creation and execution of ad-hoc tests in the IDE you should set the default working directory for JUnit Run Configurations:

[[resources/Intellij - Run configuration default settings.png]]

TODO: If we could get rid of this wart, that would be nice...

## Running and debugging the Manager

Set up a *Run Configuration*:

- Module/Classpath: `manager:server`
- Working directory: *Must be set to OpenRemote main project directory!*
- Main class: `org.openremote.manager.server.Main`
- Any environment variables that customise deployment (usually custom projects have some)

You can now open [http://localhost:8080/](http://localhost:8080/) in your browser. The default login is username `admin` with password `secret`.

*NOTE: Please be aware that currently by default the web server binds to all interfaces (i.e. `0.0.0.0`). This can be a security problem if your development machine's network is not private and secure.*

## Executing Manager tests

Execute `./gradlew test` or run the individual test classes in your IDE directly, or do a full build with `./gradle clean build`. This should be done before committing.

Some of these tests are end-to-end tests that require running background container services. You might want to start with clean containers before running tests and you might have to restart containers after (failed) tests with `docker-compose [up|down]`. You probably want to manage the data volumes with `docker volume`, although the default setup is to wipe and clean install everything over an existing database.
