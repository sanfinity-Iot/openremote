Install [Docker Community Edition](https://www.docker.com/). Ensure the following commands work:

```
docker run hello-world
docker-machine version
```

## Local Engine

For a local engine (developer workstation setup) simply installing Docker Community Edition is enough. Ensure that `docker version` on the command line works.

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

## Remote Engine

For a remote engine (hosted deployment), you need SSH public key access to a Linux host (preferably CentOS), and then Docker Machine can do the rest:

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

If the remote host already has a running Docker engine then you can manually copy the client credentials from the secure location to your local machine by unzipping the credentials into `~/.docker/machine/machines/<DOCKER MACHINE NAME>/` and then you will need to fix the paths in `~/.docker/machine/machines/<DOCKER MACHINE NAME>/config.json`.

***For Windows you will have to use escaped backslashes e.g. `C:\\Users\Me\\.docker\\machine\\`.***

## Using a machine

Once the remote engine is installed ensure that `docker-machine ls` shows the new engine and that the State is `Running`:

```
docker-machine ls
eval $(docker-machine env or-host123)
docker ps
```

This will show running containers on Docker Machine `or-host123`.

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
