This guide assumes you have installed the docker engine and connected to that engine as described in the other developer guides). Ensure that you have also built the code base before trying to initialise any docker profiles:

```
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
Docker deployment profiles (docker compose files) are used to configure and start required services; the standard profiles are located in the `profile` folder of the main OpenRemote repository. Custom projects should use the same profiles where possible but can define new profiles as required (they should be stored in `profile` folder of the custom project repository).

The standard profiles are:

* manager (manager.yml) - Contains all the services that may be required to run the manager as a service (haproxy, letsencrypt, manager, postgresql, keycloak)

### Overriding profile configuration
The configuration required for different environments may well vary so a configuration override file should be used (see [here](https://docs.docker.com/compose/extends/) for more details). Override configurations should be located in the same folder as the profiles using the naming convention:

`<PROFILE>.<ENVIRONMENT>.yml e.g. manager.demo.yml`

Running profiles with an override is simply a matter of specifying the override file using the CLI:

`docker-compose -f manager.yml -f manager.demo.yml up -d`

### Specifying which services to start
When starting a profile (docker compose up) all services will be started by default but the services to start can be specified using the CLI:

`docker-compose -f manager.yml -f manager.demo.yml up -d postgresql keycloak`

**NOTE: Services linked to the requested services will also be started**