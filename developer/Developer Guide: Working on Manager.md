## Starting required services

Run required containers with:

```
docker-compose -p openremote \
 -f profile/dependencies/keycloak_dev.yml \
 -f profile/dependencies/postgresql_dev.yml \
 up
```

You also have to start the GWT compiler and keep it running in the background. This service listens for compilation requests and transforms Java into JavaScript code:

```
./gradlew -p manager/client gwtSuperDev
```

## Running and debugging the Manager

Configure your IDE as described in [[Developer Guide: Preparing the environment]] and set up a *Run Configuration*:

- Module/Classpath: `manager/server`
- Working directory: *Must be set to project root directory!*
- Main class: `org.openremote.manager.server.Main`

You can now open [http://localhost:8080/](http://localhost:8080/) in your browser. The default login is username `admin` with password `CHANGE_ME_ADMIN_PASSWORD`.

*NOTE: Please be aware that currently by default the web server binds to all interfaces (i.e. `0.0.0.0`). This can be a security problem if your development machine's network is not private and secure.*

*NOTE: During development it can be useful to disable all security checks on the HTTP remote API, for example, to test without authentication and authorization in a simple HTTP client such as `curl`. Start the Manager with the `DISABLE_API_SECURITY=true` environment variable setting to disable security.*

## Executing Manager tests

Execute `./gradlew -p manager test` or run the individual test classes in your IDE directly.

Some of these tests are end-to-end tests that require running background container services. You might want to start with clean containers before running tests and you might have to restart containers after (failed) tests with `docker-compose [up|down]`.

<!-- TODO
## Building and publishing images

Build Docker images with `./gradlew clean buildImage`. Remove old images before if you don't want to use the Docker build cache.

Build and push images to our [Docker Hub Account](https://hub.docker.com/u/openremote/): `./gradlew clean pushImage -PdockerHubUsername=username -PdockerHubPassword=secret`
--->

## Updating Manager map data

We currently do not have our own pipeline for extracting/converting OSM data into vector tilesets but depend on the extracts offered on https://github.com/osm2vectortiles/osm2vectortiles.

You can extract smaller tilesets with the following procedure:

1. Install tilelive converter: 
    `npm install -g mapnik mbtiles tilelive tilelive-bridge`
1. Select and copy boundary box coordinates of desired region: 
    http://tools.geofabrik.de/calc/#tab=1 
1. Extract the region with: 
    `tilelive-copy --minzoom=0 --maxzoom=14 --bounds="BOUNDARY BOX COORDINATES" theworld.mbtiles myextract.mbtiles`

<!--
## Configuring security

In an OpenRemote system, servers use certificates to authenticate themselves to clients. Simple default TLS certificates (and the private key only known to the server(s)) have been created with:

```
#!/bin/bash

rm /tmp/or-*.jks /tmp/or-*.cer

keytool -genkeypair -alias "openremote" \
    -noprompt -keyalg RSA -validity 9999 -keysize 2048 \
    -dname "CN=openremote" \
    -keypass CHANGE_ME_SSL_KEY_STORE_PASSWORD \
    -keystore /tmp/or-keystore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD

keytool -exportcert \
    -alias "openremote" \
    -keystore /tmp/or-keystore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD \
    -file /tmp/or-certificate.cer

keytool -importcert -noprompt \
    -alias "openremote" \
    -file /tmp/or-certificate.cer \
    -keystore /tmp/or-truststore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD

keytool -list -v \
    -keystore /tmp/or-keystore.jks \
    -storepass CHANGE_ME_SSL_KEY_STORE_PASSWORD

cp /tmp/or-keystore.jks controller/conf/keystore.jks
cp /tmp/or-truststore.jks controller/conf/truststore.jks

cp /tmp/or-keystore.jks beehive/ccs/conf/keystore.jks

```

At a minimum, you should re-create the stores and private key with your own passwords when deploying OpenRemote.

TODO: Unify default CAs in trust stores in all server systems, which (root) CAs should stay and what is the setup we recommend for OpenRemote users, etc.
-->