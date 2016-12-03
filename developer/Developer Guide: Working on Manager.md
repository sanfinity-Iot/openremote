## Starting required services

Run required containers with:

```
docker-compose -p my_project -f profile/manager_dev.yml up
```

You also have to start the GWT compiler and keep it running the background. This service listens for compilation requests and transforms Java into JavaScript code.

```
./gradlew -p manager/client gwtSuperDev
```

## Running and debugging the Manager service

Configure your IDE as described in [[Developer Guide: Preparing the  environment]] and set up a *Run Configuration*:

- Module/Classpath: `manager/server`
- Working directory: *Must be set to project root directory!*
- Main class: `org.openremote.manager.server.Main`

You can now open http://localhost:8080/ in your browser.

*NOTE: Please be aware that currently by default the web server binds to all interfaces (i.e. `0.0.0.0`)*

*NOTE: During development it can be useful to disable all security checks on the HTTP remote API, for example, to test without authentication and authorization in a simple HTTP client such as `curl`. Start the Manager with the `DISABLE_API_SECURITY=true` environment variable setting to disable security.*

## Executing Manager tests

You can run the tests of the `manager` project with `./gradlew -p manager test` or run the individual test classes in your IDE directly. The working directory must always be set to the project root directory!

Some of these tests are end-to-end tests that require running background container services. You might want to start with clean containers before running tests and you might have to restart containers after (failed) tests with `docker-compose [up|down]`.

## Updating Manager map data

We currently do not have our own pipeline for extracting/converting OSM data into vector tilesets but depend on the extracts offered on https://github.com/osm2vectortiles/osm2vectortiles.

You can extract smaller tilesets with the following procedure:

1. Install tilelive converter: 
    `npm install -g mapnik mbtiles tilelive tilelive-bridge`
1. Select and copy boundary box coordinates of desired region: 
    http://tools.geofabrik.de/calc/#tab=1 
1. Extract the region with: 
    `tilelive-copy --minzoom=0 --maxzoom=14 --bounds="BOUNDARY BOX COORDINATES" theworld.mbtiles myextract.mbtiles`
