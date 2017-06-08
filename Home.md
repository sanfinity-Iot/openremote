Welcome to the OpenRemote wiki.

## Quickstart

Checkout the [main OpenRemote project](https://github.com/openremote/openremote) and run a demo setup with [Docker Community Edition](https://www.docker.com/):

```
docker-compose -p openremote -f profile/demo.yml up
```

Access the manager UI and API on https://localhost/ with username admin and password secret (accept the 'insecure' self-signed SSL certificate). Configuration options of the images are documented in the compose profile `demo.yml`.

## Developer Guide

* [[Preparing the environment|Developer Guide: Preparing the environment]]
* [[Working on Manager|Developer Guide: Working on Manager]]
* [[Building a custom project|Developer Guide: Building a custom project]]
* [[Writing Console applications|Developer Guide: Writing Console applications]]
* [[Licensing guidelines for contributors|Developer Guide: Licensing guidelines for contributors]]

