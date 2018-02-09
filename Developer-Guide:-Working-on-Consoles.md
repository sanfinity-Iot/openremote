Working on the consoles means working on the frontend console applications or the keycloak theme.

## Starting required services

Requires a manager service to be running on localhost that is volume mapped to the console HTML code refer to the [Console Development profile](./Developer-Guide%3A-Deploying-to-a-Docker-engine#console-development-dev-consoleyml).

Run required services with:

```
./gradlew clean installDist
docker-compose -p openremote -f profile/dev.yml down
docker-compose -p openremote -f profile/dev.yml up --build
```

Alternatively services can be pulled from Docker Hub (doesn't require compiling the code base locally):

```
docker-compose -p openremote -f profile/dev.yml down
docker-compose -p openremote -f profile/dev.yml pull
docker-compose -p openremote -f profile/dev.yml up
```

You can then work on the consoles using your preferred HTML/JS IDE and changes will be instantly available in the browser.

If you want to expose the service on your LAN (and not only on localhost), set env variables:

```
IDENTITY_NETWORK_HOST=<YOUR IP>
WEBSERVER_LISTEN_HOST=0.0.0.0
```
