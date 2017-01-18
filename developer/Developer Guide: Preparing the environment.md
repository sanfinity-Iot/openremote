A Docker host is required to work on the main [OpenRemote repository](https://github.com/openremote/openremote/). We are using [Docker Compose](https://docs.docker.com/compose/) for most deployment tasks.

An installation of Java 8 ([OpenJDK](http://openjdk.java.net/), [Oracle Java SE JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html)) is also required.

 All Docker and Gradle commands **must be executed in the project root directory**.

## Configuring a Docker host

You should be able to execute `docker`, `docker-compose`, and ideally `docker-machine` client commands in a shell and communicate with your Docker host.

If you don't have a Docker host, we recommend installing [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox). Our default configuration is prepared for that environment.

If you already have a Docker host, make sure the environment variables `DOCKER_HOST`, `DOCKER_CERT_PATH` are set. All build operations will use these settings to create images, start & stop containers, and so on.

For Docker volume mapping to work correctly on Windows and OS X ensure that your working directory is located somewhere under your home directory.

You might notice that the Docker host virtual machine's time will drift when you are using VirtualBox. You will see authentication on OpenRemote failing and "token is not active" verification errors if the time skew is too large. Until this is [fixed](https://github.com/boot2docker/boot2docker/issues/69), periodically run `docker-machine ssh default 'sudo ntpclient -s -h pool.ntp.org'` to update the virtual machine's clock.

Working with Docker might leave exited containers, untagged images, and dangling volumes. The following bash function can be used to clean up:

```
function dcleanup(){
    docker rm -v $(docker ps --filter status=exited -q 2>/dev/null) 2>/dev/null
    docker rmi $(docker images --filter dangling=true -q 2>/dev/null) 2>/dev/null
    docker volume rm $(docker volume ls -qf dangling=true 2>/dev/null) 2>/dev/null
}
```

<!--
## Setting up a custom Docker host

If you do not have Docker host, you can install a virtual machine as follows:

- Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- Install [Vagrant](http://www.vagrantup.com/downloads.html)
- Install [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox)
- Check out the [OpenRemote project](https://github.com/openremote/openremote) and change to `$PROJECT_DIRECTORY/platform/`
- Execute `vagrant up` to start a virtual machine

Configure the virtual machine as a Docker host machine with:
```
docker-machine create \
  -d generic \
  --generic-ssh-user=$(vagrant ssh-config | grep ' User ' | cut -d ' ' -f 4) \
  --generic-ssh-key=$(vagrant ssh-config | grep IdentityFile | cut -d ' ' -f 4) \
  --generic-ip-address=$(vagrant ssh-config | grep HostName | cut -d ' ' -f 4) \
  --generic-ssh-port=$(vagrant ssh-config | grep Port | cut -d ' ' -f 4) \
  openremote
```

Check your Docker host machines with `docker-machine ls`.

To prepare your shell environment (variables), run `eval $(docker-machine env openremote)`. Now  execute `docker [version|images|ps|...]` to interact with your Docker host. You can login directly on your Docker host machine with `vagrant ssh`.
-->

## Importing the project in an IDE

#### IntelliJ IDEA

- Create a `New Project From Existing Sources` and import as a Gradle project
- Note that IntelliJ might time out if a background Gradle process (for example, running the GWT compiler server) blocks the Gradle import. Stop and start the background process to unblock.

#### Eclipse

- Run `./gradlew eclipse`
- In Eclipse go to `File` > `Import` and import the project as `Existing Projects into Workspace`

The working directory in your IDE must always be set to the **project root directory**. We recommend you set this as the default directory in your IDE for all *Run Configurations*.

## Working with deployment profiles

Before you can work on OpenRemote code, you must start dependencies by selecting one or more Docker Compose configuration profiles.

For example, the DMBS and the identity service are required to work on most of the projects. These containers are started and run in the background, the processes and tests you then execute in your IDE utilize the containers.

The syntax for starting deployment profiles is:

```
docker-compose -p <your_project_name> -f profile/<profile1>.yml -f profile/<profileN>.yml up
```

Here `your_project_name` is a convenient handle that groups all Docker images and containers in a deployment by prefixing their names with your project name. You can pick any name you like.

The default configuration of all services is for Docker host IP `192.168.99.100`. If this is not your Docker host IP, you must set various environment variables and configure your stack, see the individual profiles for more information.
