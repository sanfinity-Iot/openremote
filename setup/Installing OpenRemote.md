# Setting up an OpenRemote 3.0 demo system

1. Install [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox)
1. Start Kitematic and open the *Docker CLI*
1. Get the IP of your Docker host VM with: `docker-machine ip default`
1. Download [docker-compose-demo.yml](https://raw.githubusercontent.com/openremote/openremote/master/docker-compose-demo.yml)
1. Edit this file and review the settings
1. Deploy the whole stack with: `docker-compose -f docker-compose-demo.yml -p myopenremotedemo [up|down]`
1. Access the Manager on `http://DOCKER_HOST_IP:MAPPED_PUBLIC_PORT` in your browser (default [http://192.168.99.100:8080/](http://192.168.99.100:8080/)) and sign-in with user `admin/admin` or `test/test`
1. Access the Controller to sync with your OpenRemote Designer account or use WebConsole (default [http://192.168.99.100:8688/controller](http://192.168.99.100:8688/controller) and [http://192.168.99.100:8688/webconsole](http://192.168.99.100:8688/webconsole))

<!--
## Setting up a Docker host

If you already have a Docker host, make sure the environment variables `DOCKER_HOST`, `DOCKER_CERT_PATH` are set. All build operations will use these settings to create images, start & stop containers, and so on.

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

