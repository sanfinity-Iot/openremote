Working on the consoles means working on the frontend console applications or the keycloak theme.

## Starting required services

Requires a manager service to be running on localhost that is volume mapped to the console HTML code refer to the [Console Development profile](./Developer-Guide%3A-Deploying-to-a-Docker-engine#console-development-dev-consoleyml).

Run required services with:

```
docker-compose -f profile/dev-console.yml down
docker-compose -f profile/dev-console.yml up --build
```

Alternatively services can be pulled from Docker Hub (doesn't require compiling the code base locally):

```
docker-compose -f profile/dev-console.yml down
docker-compose -f profile/dev-console.yml pull
docker-compose -f profile/dev-console.yml up
```

**TODO: OpenRemote Manager JS Library**

You can then work on the consoles using your preferred HTML/JS IDE and changes will be instantly available in the browser.