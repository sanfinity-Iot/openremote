Welcome to the OpenRemote wiki.

## OpenRemote Projects

* [Console](https://github.com/openremote/openremote/tree/master/console) - Builders and administrators of IoT networks require custom monitoring and control panels, small applications that help installing and maintaining their systems. End-users want their own automation control panels with custom designs, tailored to the task at hand, in stationary or mobile environments and according to personal taste. With OpenRemote's web components you can create a custom application in JavaScript/HTML5 easily. The Console is a shell that will take care of deploying and running your applications on Android, iOS, and in web browsers. It also integrates mobile features like notifications and geo-fencing.

* [Manager](https://github.com/openremote/openremote/tree/master/manager) - Provides IoT backend services and a web-based operations frontend and management application. An asset storage and processing service allows you to capture a dynamic schema of all things in your IoT network. This includes agents, their devices and services, and any other assets (Buildings, Rooms, Flights, Boats, Customers) you want to organise. Changes to assets are processed by a rules system, so you can design custom data flow, business rules and notification conditions. The Manager will also host your Console applications.

* [Agent](https://github.com/openremote/openremote/tree/master/agent) - The agent connects sensors and actuators to your IoT network and creates the interface to 3rd party APIs and service protocols. OpenRemote has many built-in protocols and it's easy to create new adapters. Co-locate your agents with backend services or install agents on gateways, close to devices.

* *Designer* - In OpenRemote 2.x you can create control panels and end-user applications with a visual designer tool. In OpenRemote 3.x, we provide web components for any HTML design tool, you don't need much more than a simple text editor to build custom automation panels, see [Example Home](https://github.com/openremote/Documentation/wiki/Example-Home).

## Quickstart

Checkout the [main OpenRemote project](https://github.com/openremote/openremote) and run a demo setup with [Docker Community Edition](https://www.docker.com/):

```
docker-compose -p openremote -f profile/demo.yml up
```

Access the manager UI and API on https://localhost/ with username admin and password secret (accept the 'insecure' self-signed SSL certificate). Configuration options of the images are documented in the compose profile `demo.yml`.

## Architecture overview

[[resources/Architecture.png]]
