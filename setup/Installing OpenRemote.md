# Setting up a demo system

1. Install [Docker Toolbox](https://www.docker.com/products/overview#/docker_toolbox)
1. Start Kitematic and open the *Docker CLI*
1. Get the IP of your Docker host VM with: `docker-machine ip default`
1. Download [docker-compose-quickstart.yml](https://raw.githubusercontent.com/openremote/openremote/master/docker-compose-quickstart.yml)
1. Edit this file and review the settings
1. Deploy the whole stack with: `docker-compose -f docker-compose-quickstart.yml [up|down]`
1. Access the Manager on `http://DOCKER_HOST_IP:MAPPED_PUBLIC_PORT` in your browser (default [http://192.168.99.100:8080/](http://192.168.99.100:8080/)) 
1. Access the Controller to sync with Designer or use WebConsole (default [http://192.168.99.100:8688/controller](http://192.168.99.100:8688/controller) and [http://192.168.99.100:8688/webconsole](http://192.168.99.100:8688/webconsole))
