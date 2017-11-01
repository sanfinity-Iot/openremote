## Dependencies
Ensure you have installed and configured the following tools:

* Java 8 JDK ([OpenJDK](http://openjdk.java.net/), [Oracle Java SE JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html))
* [GIT](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/engine/installation/)
* [NodeJS](https://nodejs.org/en/download/current/)
* [Bower](https://bower.io/)

Ensure the following commands execute successfully:

```
java -version
git --version
docker ps
docker-compose version
docker-machine ls
node -v
bower -v
```
A Docker engine is required to host the services used. Please refer to the Docker sections of the Developer Guide for help in installing and using Docker.

All Docker and Gradle commands **must be executed in the project root directory**.

## Importing the project in an IDE

#### IntelliJ IDEA

- Create a `New Project From Existing Sources` and import as a Gradle project
- Note that IntelliJ might time out if a background Gradle process (for example, running the GWT compiler server) blocks the Gradle import. Stop and start the background process to unblock.

The log messages of the running application can be colour-highlighted with the [GrepConsole plugin](https://plugins.jetbrains.com/plugin/7125-grep-console) and our [configuration](https://gist.github.com/christianbauer/9cd3ef6a871c2a3472bd70a216f3eb14).

#### Eclipse

- Run `./gradlew eclipse`
- In Eclipse go to `File` > `Import` and import the project as `Existing Projects into Workspace`

The working directory in your IDE must always be set to the **project root directory**. We recommend you set this as the default directory in your IDE for all *Run Configurations*.