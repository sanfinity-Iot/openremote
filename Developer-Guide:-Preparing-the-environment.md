To build OpenRemote projects, you have to first prepare the environment on your developer workstation or build machine.

Ensure you have installed and configured the following tools:

* Java 8 JDK ([OpenJDK](http://openjdk.java.net/), [Oracle Java SE JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html))
* [GIT](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/engine/installation/)
* [NodeJS](https://nodejs.org/en/download/current/) (>= 10.13.0)
* [yarn](https://yarnpkg.com/lang/en/) `npm install --global yarn` (>=1.12.1)

Ensure the following commands execute successfully:

```
java -version
git --version
docker ps
docker-compose version
docker-machine ls
node -v
yarn -v
```
