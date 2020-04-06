# Docker services
The following services are used by the main OpenRemote code base:

* proxy - Reverse SSL proxy (HAProxy) for the web services with auto SSL certificate generation (letsencrypt)
* manager - Runs the OpenRemote Manager (by default depends on postgresql and keycloak)
* postgresql - PostgreSQL DB
* keycloak - Keycloak identity provider service
* map - tileserver-gl service that is used for serving raster map tiles (only needed for UI components/apps that use mapbox-js)

# Docker compose profiles
Docker compose profiles (Docker Compose `.yml` files) are used to configure and start required services; the standard profiles are located in the profile folder of the main [OpenRemote repository](https://github.com/openremote/openremote/tree/master/profile) although the `demo` profile is in the root of the repo.

The standard profiles are:

## Deploy (deploy.yml)
This is the main profile which all other profiles extend; it can't be used directly.

## Demo (../docker-compose.yml)
This is a demo profile which starts all services and provides a quick start for getting a running local deployment.

### Prerequisites
Docker images must have been pulled from Docker Hub or the full stack must be built ready to build the docker images locally:
```
./gradlew clean installDist
```

## Demo Raspberry Pi (demo-rpi.yml)
This is for demo for running the full stack on a Raspberry Pi; it doesn't use the keycloak identity service but instead uses a simple identity service.

## UI Development (dev.yml)
This is for doing development work on the UI (i.e. Front end apps and/or components and/or Keycloak themes).

### Prerequisites
Docker images must have been pulled from Docker Hub:
```
docker-compose -f profile/dev.yml pull
```

Or the docker images must have been built locally (requires the full stack be built):

```
./gradlew clean installDist
docker-compose -f profile/dev.yml build
```

To start the containers:

```
docker-compose -f profile/dev.yml up -d
```

### Exposed Services
* Manager: http://localhost:8080
* Keycloak: http://localhost:8081
* PostgreSQL DB: jdbc:postgresql://localhost:5432/openremote

### Volume Maps
* `../ui/keycloak/themes/openremote` - Maps custom openremote keycloak theme into the Keycloak service; changes to the local theme are instantly available to the keycloak service; any new themes will need to be volume mapped individually
* `../deployment` - Maps deployment folder into the Manager service; the mapped local path can be overridden using the `DEPLOYMENT_DIRECTORY` environment variable (.e.g. = `DEPLOYMENT_DIRECTORY=../../deployment docker-compose -f openremote/profile/dev.yml`)
* `../client` - Maps the manager GWT client into the Manager service

## Full Stack Development / Running Tests (dev-testing.yml)
This is for running tests or doing development work on the Manager (in an IDE) as well as the UI (i.e. Front end apps and/or components and/or Keycloak themes); starts the Keycloak and PostgreSQL services, see the [[Setting up an IDE|Developer Guide: Setting up an IDE]] guide for running the Manager in an IDE.

### Exposed Services
* Keycloak: http://localhost:8081/auth
* PostgreSQL DB: jdbc:postgresql://localhost:5432/openremote

### Volume Maps
* `../ui/keycloak/themes/openremote` - Maps custom keycloak theme into the Keycloak service; changes to the local theme are instantly available to the keycloak service

## Full Stack Development with HTTPS Proxy (dev-proxy.yml)
This is the same as the Full Stack Development profile but also adds the proxy service to allow development/testing of the Manager running behind the reverse proxy with HTTPS (so development environment matches final deployment configuration).

To use this proxy correctly you will need to set the correct environment variables for the manager running behind SSL proxy as described in [[Setting up an IDE|Developer Guide: Setting up an IDE]].

### Exposed Services
* Manager: https://localhost
* Keycloak: http://localhost:8081/auth
* PostgreSQL DB: jdbc:postgresql://localhost:5432/openremote

### Volume Maps
* `../ui/keycloak/themes/openremote` - Maps custom keycloak theme into the Keycloak service; changes to the local theme are instantly available to the keycloak service

## Map (dev-map.yml)
This is used for starting the map service and is required when working on raster map components/apps.

### Exposed Services
* Tileserver-gl: http://localhost:8082

### Volume Maps
* `../deployment/manager/shared` - Maps shared resources (sprites and fonts) into the service **NOTE that tileserver-gl expects fonts to not be gzipped**
* `../deployment/map` - Maps the map settings and DB into the service