Working on the UI means working on any of:

* Web applications
* Web components
* Keycloak theme(s)

Working on the web applications and/or components will generally require a manager to interact with you can either:

* Run a Manager instance in an IDE (refer to [Working on the Manager](./Developer-Guide%3A-Working-on-the-Manager))
* Deploy the Manager using a Docker container (refer to [UI Development profile](/Developer-Guide%3A-Docker-compose-profiles#ui-development-devyml))

## Manager web application
If you are working on GWT based Manager web application you will need to start the GWT compiler and keep it running in the background; this service listens for compilation requests and transforms Java into JavaScript code:
```
./gradlew -p gwtSuperDev
```



## Starting required services

Requires a manager service to be running on localhost that is volume mapped to the console HTML code refer to the 

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
