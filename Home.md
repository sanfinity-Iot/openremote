Welcome to the OpenRemote wiki!

[[resources/Architecture.png]]

The core part of the OpenRemote system is the Manager which is a headless java application that forms an IoT Context Broker with a rules engine. IoT devices are connected to the Manager via Agents and the sensors and actuators of these devices are linked to assets and attributes within the context broker.

The Manager provides communication mechanisms for monitoring and administrating the system:

* JAX-RS based REST API
* Event API (Websockets with fallback to long polling mechanism)

The Manager also offers a web server that can host web applications which means anyone can create a bespoke web application to fit the specific needs of the user(s) allowing a perfectly tailored solution to the task at hand:

* Multi tenancy monitoring dashboard
* Home automation control panel
* Smart city monitoring dashboard

Security is paramount when it comes to IoT and out of the box the Manager integrates with [Keycloak](https://www.keycloak.org/) to provide industry standard multi tenant authentication; out of the box we also provide 256-bit encryption via HTTPS when using our HAProxy based reverse proxy.

## OpenRemote Projects

### Backend
* [Manager](https://github.com/openremote/openremote/tree/master/manager) - Provides IoT backend services and a web-based operations frontend and management application. Multi-tenancy is supported and multiple IoT networks can be managed together or isolated. An asset storage and processing service allows you to capture a dynamic schema of all things in your IoT network. This includes agents, their devices and services, and any other domain-specific assets (Buildings, Rooms, Flights, Boats, Customers) you want to organise. Any changes to assets are processed by a rules system, so you can design custom data flow, business rules and notification conditions. The Manager can also host web applications.

### Frontend
* [Web Components](https://github.com/openremote/openremote/tree/master/ui/component) - Contains web components and JS modules that are the building blocks for web applications that can connect to an OpenRemote manager and be loaded within a console. [Demos](https://github.com/openremote/openremote/tree/master/ui/demo) provide a development harnesses for the various components and also demonstrate the functionality and usage of each component.

* [Web Applications](https://github.com/openremote/openremote/tree/master/ui/app) - Contains OpenRemote web applications (e.g. the Manager admin UI), these can be used as templates for building custom web applications.

* [Console](https://github.com/openremote/openremote/tree/master/console) - Native mobile applications (iOS and Android) that act as a shell for web applications built with the OpenRemote web components; a web browser is also considered to be a console and the OpenRemote console component automatically integrates native console features like push notifications and geo-fencing.

* [Agent](https://github.com/openremote/openremote/tree/master/agent) - The Agent connects sensors and actuators to your IoT network and creates the interface to 3rd party APIs and service protocols. OpenRemote has many built-in protocols and it's easy to create new adaptors. Co-locate your agents with the Manager or install agents on gateways, close to devices. You can link a 2.x controller to a Manager, see [Connecting to Controller 2.5](https://github.com/openremote/openremote/wiki/User-Guide%3A-Connecting-to-Controller-2.5)

* *Designer* - In OpenRemote 2.x you can create control panels and end-user applications with a visual designer tool. In OpenRemote 3.x, we provide web components for any HTML design tool, you don't need much more than a simple text editor to build custom automation panels. See [Example Home 2.x](https://github.com/openremote/Documentation/wiki/Example-Home) and the port to web components in [Smart Home example in v3](https://github.com/openremote/openremote/tree/master/deployment/manager/consoles/customerA).

## Quickstart

Checkout the [main OpenRemote project](https://github.com/openremote/openremote) and run a demo setup with [Docker Community Edition](https://www.docker.com/):

```
docker-compose -p openremote -f profile/demo.yml up
```

Access the manager UI and API on https://localhost/ with username admin and password secret (accept the 'insecure' self-signed SSL certificate). Configuration options of the images are documented in the compose profile `demo.yml`.