## Welcome to the OpenRemote wiki!

**OpenRemote is a platform that simplifies connecting networked assets to mobile- and web applications.**

![Architecture 3.0](https://github.com/openremote/Documentation/blob/master/manuscript/figures/architecture-3.jpg)

The core of the OpenRemote system is the [manager](https://github.com/openremote/openremote/tree/master/manager), a headless Java application that forms an IoT context broker which captures the current asset state of the system. You can create a dynamic schema of your assets and their attributes in the manager, modelling the problem domain. For example, you would create Building, Apartment, Room, and Sensor assets to model an IoT system for a smart home or office.

Rules can be written in Groovy, JavaScript, a Rules JSON, or Flow model, and dynamically deployed. Rules execute actions when matching asset state or sequence of events are detected. For example, when a mobile asset enters a geographic fence, or when humidity in a room keeps increasing, you can notify a group of users via email and on their mobile devices.

Networked things and devices are connected to the manager via [agents](https://github.com/openremote/openremote/tree/master/agent), they are the interface to 3rd party APIs and service protocols. OpenRemote has many built-in protocols and it's easy to create new adapters. Co-locate your agents with the manager or install agents on gateways, close to devices. You can link a 2.x controller to a manager, see [Connecting to Controller 2.5](https://github.com/openremote/openremote/wiki/User-Guide%3A-Connecting-to-Controller-2.5)

The manager provides communication mechanisms for monitoring and administrating the system:

* JAX-RS based REST API
* Event API (Websockets with fallback to long polling mechanism)

The OpenRemote console simplifies the creation and deployment of user interfaces such as:

* Multi tenancy monitoring dashboard
* Home automation control panel
* Smart city monitoring dashboard

We support the latest HTML standards and provide [web components](https://github.com/openremote/openremote/tree/master/ui/component) to build applications quickly, utilising the OpenRemote asset model and APIs: you can easily show all your assets on a map, for example. [The demos](https://github.com/openremote/openremote/tree/master/ui/demo) provide a development harnesses for the various components and also demonstrate the functionality and usage of each component. [Full web applications](https://github.com/openremote/openremote/tree/master/ui/app) are also bundled with OpenRemote, these can be used as templates for building custom web applications.


The OpenRemote [consoles](https://github.com/openremote/openremote/tree/master/console) are native mobile applications (iOS and Android) that act as a shell for web applications built with the OpenRemote web components; a web browser is also considered to be a console and we automatically integrate native console features like push notifications and geo-fencing on each platform. If you have an existing website, add OpenRemote web components and wrap it in the OpenRemote console to connect your mobile users to your IoT network.

Security is paramount when it comes to IoT and out of the box the manager integrates with [Keycloak](https://www.keycloak.org/) to provide industry standard multi-tenant authentication; out of the box we also provide TLS/SSL when using our HAProxy-based reverse proxy.

We rely on PostgreSQL and its GIS and JSON extensions to provide a stable asset database management system.


