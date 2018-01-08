This guide helps you set up an environment with an IDE when you are done [[Preparing the environment|Developer Guide: Preparing the environment]], so you can work comfortably on the Manager backend services and/or the Manager UI.

## Run required services

Make sure testing services are running as described in [[Creating a custom project|Developer Guide: Creating a custom project]]:

```
docker-compose -p openremote -f profile/dev-testing.yml up --build -d
```

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

##### Recommended Plugins
- [Grep Console](https://plugins.jetbrains.com/plugin/7125-grep-console)
- [Markdown Navigator](https://plugins.jetbrains.com/plugin/7896-markdown-navigator)

##### Grep Console Styling
- Locate XML style config for Grep Console in openremote/tools/intellij
- Choice the default or dark styling config
- Copy the xml to your IntelliJ IDEA Config folder 

###### Linux
```
cp ~/<PATH_TO_PROJECT>/openremote/tools/intellij/Theme-<Default|Darkcula>-GrepConsole.xml ~/.IntelliJIdea<VERSION>/config/options/GrepConsole.xml
```


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

Make sure required testing services are running as described in [[Creating a custom project|Developer Guide: Creating a custom project]].

Set up a *Run Configuration*:

- Module/Classpath: `manager:server` or `myproject:myextension1` for custom projects
- Working directory: *Must be set to OpenRemote main project directory!*
- Main class: `org.openremote.manager.Main`
- Any environment variables that customise deployment (usually custom projects have some)

You can now open [http://localhost:8080/](http://localhost:8080/) in your browser. The default login is username `admin` with password `secret`.

*NOTE: Please be aware that currently by default the web server binds to all interfaces (i.e. `0.0.0.0`). This can be a security problem if your development machine's network is not private and secure.*

## VisualVM
To inspect the threads, analyzing CPU and memory allocation you should running a VisualVM.

- Download and install VisualVM from the [visualvm](https://visualvm.github.io/) website.
- Install the [VisualVM Launcher](https://plugins.jetbrains.com/plugin/7115-visualvm-launcher) for IntelliJ IDEA

## Executing Manager tests

Any JUnit test can be directly executed. Make sure required testing services are running as described in [[Creating a custom project|Developer Guide: Creating a custom project]].
