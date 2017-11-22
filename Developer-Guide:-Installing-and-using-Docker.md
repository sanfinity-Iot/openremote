Install [Docker Community Edition](https://www.docker.com/). Ensure the following commands work:

```
docker version
docker-machine version
```

You might want to install bash auto-completion for Docker commands. On OS X, install:

```
brew install bash-completion
```

Then add this to your `$HOME/.profile`:

```
if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi
```

And link the completion-scripts from your local Docker install:

```
find /Applications/Docker.app \
-type f -name "*.bash-completion" \
-exec ln -s "{}" "$(brew --prefix)/etc/bash_completion.d/" \;
```
Start a new shell or source your profile to enable auto-completion.

## Local Engine

For a local engine (developer workstation setup) simply installing Docker Community Edition is enough. Ensure that `docker version` on the command line works.

Once Docker Community edition is installed then you will be connected to your local Docker engine; if you have used docker-machine to connect to a remote engine then you can `disconnect` from that remote engine using the command:

```
docker-machine env -u
```

## Remote Engine

### Installing a remote engine

To install a remote engine (hosted deployment), you need SSH public key access to a Linux host (preferably CentOS), and then Docker Machine can do the rest:

```
docker-machine create --driver generic \
    --generic-ip-address=<HOST> \
    --generic-ssh-port=<SSH PORT> \
    <DOCKER MACHINE NAME>
```

Follow the instructions [here](https://docs.docker.com/machine/drivers/generic/).

After you let Docker Machine install the Docker daemon on the remote host, you must fix the generated Docker client credentials configuration files.

Move `~/.docker/machine/certs/*` into `~/.docker/machine/machines/<DOCKER MACHINE NAME>/` and fix the paths in `~/.docker/machine/machines/<DOCKER MACHINE NAME>/config.json`.

When a remote engine is first installed the client credentials should be zipped and made available in a private and secure location. The client credentials can be found at `~/.docker/machine/machines/<DOCKER MACHINE NAME>/`.

### Using a remote engine

You do not need SSH access on the remote host to simply use an already installed Docker engine.

Manually copy the client credentials of the remote engine from the secure location to your local machine by unzipping the credentials into `~/.docker/machine/machines/<DOCKER MACHINE NAME>/` and then you will need to fix the paths in `~/.docker/machine/machines/<DOCKER MACHINE NAME>/config.json`.

***For Windows you will have to use escaped backslashes e.g. `C:\\Users\Me\\.docker\\machine\\`.***

Once the remote engine is installed ensure that `docker-machine ls` shows the new engine and that the State is `Running`:

```
docker-machine ls
eval $(docker-machine env or-host123)
docker ps
```

This will show running containers on Docker Machine `or-host123`.

## Using Docker Compose

We use Docker Compose to configure a stack of containers as services.

Service images will be built automatically when they do not exist in the Docker engine image cache or when the `Dockerfile` for the service changes. **Docker Compose does not track changes to the files used in a service so when code changes are made you will need to manually force a build of the service**.

Here are a few useful Docker Compose commands:

* `docker-compose -f <PROFILE> pull <SERVICE> <SERVICE>...` - Force pull requested services from Docker Hub, if no services specified then all services will be pulled (e.g. docker-compose -f profile/manager.yml pull to pull all services)

* 'docker-compose -f <PROFILE> build <SERVICE> <SERVICE>...` - Build/Rebuild requested services, if no services specified then all services will be built

* `docker-compose -f <PROFILE> up <SERVICE> <SERVICE>...` - Creates and starts requested services, if no services specified then all services will be created and started (also auto attaches to console output from each service use `-d` to not attach to the console output)

* `docker-compose -f <PROFILE> up --build <SERVICE> <SERVICE>...` - Creates and starts requested services but also forces building the services first (useful if the source code has changed as docker-compose will not be aware of this change and you would otherwise end up deploying 'stale' services)

* `docker-compose -f <PROFILE> down <SERVICE> <SERVICE>...` - Stops and removes requested services, if no services specified then all services will be stopped and removed. Sometimes the shutdown doesn't work properly and you have to run `down` again to completely remove all containers and networks.

When deploying profiles you can provide a project name to prefix the container names (by default Docker Compose will use the configuration profile's folder name); the project name can be specified with the -p argument using the CLI:

```
docker-compose -p <your_project_name> -f <profile> -f <profile_override> up -d <service1> <service2> ...
```

## Useful notes

Working with Docker might leave exited containers, untagged images. The following bash function can be used to clean up:

```
function dcleanup(){
    docker rm -v $(docker ps --filter status=exited -q 2>/dev/null) 2>/dev/null
    docker rmi $(docker images --filter dangling=true -q 2>/dev/null) 2>/dev/null
}
```

To remove data volumes no longer referenced by a container (deleting ALL persistent data!), use:

```
docker volume prune
```

<!--
## VirtualBox Engine
you can install a virtual machine as follows:

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