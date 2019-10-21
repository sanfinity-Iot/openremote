To build OpenRemote projects, you have to first prepare the environment on your developer workstation or build machine.

Ensure you have installed and configured the following tools:

# Runtime tooling
To run docker images from Docker Hub you need to install the following tooling:
* Docker see [here](https://github.com/openremote/openremote/wiki/Developer-Guide:-Installing-and-using-Docker#local-engine)

Ensure the following commands execute successfully:

```
docker ps
docker-compose version
docker-machine ls
```

# Development tooling
For development you need the following in addtion to the runtime tooling:

* Java 8 JDK ([OpenJDK](http://openjdk.java.net/), [Oracle Java SE JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html), on MacOS use [AdoptOpenJDK](https://github.com/AdoptOpenJDK/homebrew-openjdk))
* [GIT](https://git-scm.com/downloads)
* [NodeJS](https://nodejs.org/en/download/current/) (>= 10.13.0, on MacOS you can use [Homebrew](https://brew.sh/) and `brew install node@10`)
* [yarn](https://yarnpkg.com/lang/en/) `npm install --global yarn` (>=1.12.1)

Ensure the following commands execute successfully:

```
java -version
git --version
node -v
yarn -v
```

Ensure that you have the `JAVA_HOME` environment variable set to the path of JDK.

# See also

- [[Installing and using Docker|Developer-Guide:-Installing and using Docker]]
- [Next 'Get Started' step: Build the code and run the manager](https://github.com/openremote/openremote/blob/master/README.md)
- [Get Started](https://openremote.io/get-started-manager/)
