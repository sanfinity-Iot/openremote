This guide assumes you have installed the Docker engine and connected to that engine as described in the other developer guides). Ensure that you have followed the [Preparing the Environment Guide](../Developer-Guide%3A-Preparing-the-environment) and also built the code base using the following commands before trying to deploy any Docker profiles (unless you are pulling services from Docker Hub):

```
(cd manager/client/src/main/webapp && bower prune && bower update)
(cd deployment/manager/resources_console/customerA/ && bower prune && bower update)
./gradlew clean prepareImage
```

## Docker services
The following services are used by the main OpenRemote code base:

* haproxy - Reverse SSL proxy for the web services
* letsencrypt - SSL certificate generation
* manager - Runs the OpenRemote Manager (by default depends on postgresql and keycloak)
* postgresql - PostgreSQL DB
* keycloak - Keycloak identity provider service

## Docker deployment profiles

Docker deployment profiles (Docker Compose files) are used to configure and start required services; the standard profiles are located in the `profile` folder of the main OpenRemote repository. Custom projects should use the same profile naming convention where possible and just link to the main repository profiles; but custom projects can define new profiles as required (they should also be stored in `profile` folder of the custom project repository).

The default configuration for profiles assumes that all services are on localhost if this is not the case then a configuration override will be required for the affected services.

The standard profiles are:

### Deploy (deploy.yml)
Starts all the services and uses SSL with HA Proxy; by default it runs on localhost using a self signed SSL certificate:

#### Exposed Services
* HTTP/HTTPS: 80/443 (Manager and Consoles e.g. `https://localhost/master`)

#### Volume Maps
* /data/haproxy/letsencrypt - Used for storage of letsencrypt generated certificates (this allows haproxy and letsencrypt services to interact but also means certificates lifespan is longer than the services)
* /data/haproxy/acme-webroot - Used by ACME plugin for SSL generation (this allows haproxy and letsencrypt services to interact)

### Deploy Raspberry Pi (deploy-rpi.yml)
This is for deployment to a Docker Engine running on a Raspberry Pi; it doesn't use the keycloak identity service but instead uses a simple identity service.

Apart from above this profile is the same as the Deploy profile.

### Full Development (dev.yml)
This is for doing development work on the full stack (i.e. Manager backend, Manager client, consoles and/or keycloak template). It starts only the keycloak (volume mapped) and postgres services and will require an IDE to run the manager backend and GWT super dev to run the manager client (refer to the guides in the rest of the wiki).

#### Exposed Services
* HTTP/HTTPS: 8081 (Keycloak: `http://localhost:8081/auth`)
* POSTGRES: 5432 (PostgreSQL DB: `jdbc:postgresql://localhost:5432/openremote`)

#### Volume Maps
* ../keycloak/theme - Maps custom keycloak theme into the Keycloak service; changes to the local theme are instantly available to the keycloak service

### Console Development (dev-console.yml)
This is for doing development work on the consoles and/or keycloak template)

#### Exposed Services
* HTTP/HTTPS: 8081 (Keycloak: `http://localhost:8081/auth`)
* POSTGRES: 5432 (PostgreSQL DB: `jdbc:postgresql://localhost:5432/openremote`)

#### Volume Maps
* ../keycloak/theme - Maps custom keycloak theme into the Keycloak service; changes to the local theme are instantly available to the keycloak service
* ../deployment/manager - Maps the console(s) and manager client config


## Docker Compose commands

Services will be built automatically when they do not exist in the Docker engine image cache or when the Dockerfile for the service changes. **Docker Compose does not track changes to the files used in a service so when code changes are made you will need to manually force a build of the service**.

Here are a few useful Docker Compose commands:

* `docker-compose -f <PROFILE> pull <SERVICE> <SERVICE>...` - Force pull requested services from Docker Hub, if no services specified then all services will be pulled (e.g. docker-compose -f profile/manager.yml pull to pull all services)
* 'docker-compose -f <PROFILE> build <SERVICE> <SERVICE>...` - Build/Rebuild requested services, if no services specified then all services will be built
* `docker-compose -f <PROFILE> up <SERVICE> <SERVICE>...` - Creates and starts requested services, if no services specified then all services will be created and started (also auto attaches to console output from each service use `-d` to not attach to the console output)
* `docker-compose -f <PROFILE> up --build <SERVICE> <SERVICE>...` - Creates and starts requested services but also forces building the services first (useful if the source code has changed as docker-compose will not be aware of this change and you would otherwise end up deploying 'stale' services)
* `docker-compose -f <PROFILE> down <SERVICE> <SERVICE>...` - Stops and removes requested services, if no services specified then all services will be stopped and removed

When deploying profiles you can provide a project name to prefix the container names (by default Docker Compose will use the configuration profile's folder name); the project name can be specified with the -p argument using the CLI:

```
docker-compose -p <your_project_name> -f <profile> -f <profile_override> up -d <service1> <service2> ...
```

## Useful notes

Working with Docker might leave exited containers, untagged images, and dangling volumes. The following bash function can be used to clean up:

```
function dcleanup(){
    docker rm -v $(docker ps --filter status=exited -q 2>/dev/null) 2>/dev/null
    docker rmi $(docker images --filter dangling=true -q 2>/dev/null) 2>/dev/null
    docker volume rm $(docker volume ls -qf dangling=true 2>/dev/null) 2>/dev/null
}
```