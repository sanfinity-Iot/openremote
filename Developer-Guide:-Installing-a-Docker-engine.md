Install [Docker Community Edition](https://www.docker.com/). Ensure the following commands work:

```
docker run hello-world
docker-machine version
```

## Local Engine
For a local engine (developer workstation setup) simply installing Docker Community Edition is enough.

## Remote Engine
For a remote engine (hosted deployment), you need SSH public key access to a Linux host (preferably CentOS), and then Docker Machine can do the rest. Follow the instructions [here](https://docs.docker.com/machine/drivers/generic/).

When a remote engine is first installed the client credentials should be zipped and made available in a private and secure location. The client credentials can be found at `~/.docker/machine/machines/<DOCKER MACHINE NAME>/`.

If the remote host already has a running docker engine then you can manually copy the client credentials to your local machine by unzipping the credentials into `~/.docker/machine/machines/<DOCKER MACHINE NAME>/` and then you will need to fix the paths in `~/.docker/machine/machines/<DOCKER MACHINE NAME>/config.json`.

***For Windows you will have to use escaped backslashes e.g. `C:\\Users\Me\\.docker\\machine\\`.***

Once the remote engine is installed ensure that `docker-machine ls` shows the new engine and that the State is `Running`.


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
