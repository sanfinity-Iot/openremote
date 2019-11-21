This page describes a typical Smart Building Application, which can be a starting point for apartment buildings or large offices. The example integrates several functions, 'Lighting', 'Heating', 'Presence', and 'Climate' for three apartments.

There are two applications which are part of the manager, once you have set it up yourself (See [Get Started](https://openremote.io/get-started-manager/)): the Application for Technical manager (for configuration of the project) and the Smart Building demo, which giving full control over the application. We'll describe both.

For convenience you can have a look at the online version of the [Demo Smart Building](https://demo.openremote.io/main/?realm=building) using the credentials (building / building).

# Application for Technical Manager

The application for the Technical Manager requires you to set up the demo environment locally first. Follow the [Get Started](https://openremote.io/get-started-manager/). Next, you can access both the Application for Technical Manager and the Smart Building Demo via your browser (if you are using Docker Toolbox replace localhost with 192.168.99.100):

Application for Technical Manager: https://localhost (Username: admin, Password: secret)

Application for Demo Smart Building: https://localhost/main/?realm=building/ (Username: building, Password: building)

If you open the Application for the Technical Manager, you will see three Realms on the left: Master, Smart Building, and Smart City. The Demo Smart Building refers to the Realm: Smart Building. It includes three apartments with rooms and different attributes. If you use the 'Assets' view in the Technical Manager, you will notice that you see exactly the same attributes as visible in the Demo Smart Building. The Service Agent (Simulator) in Apartment 1 can be used to change the simulated sensor values for Apartment 1. 

## Apartments, rooms and attributes

As you navigate through the 'Assets' view you will notice that each apartment and room contains several attributes. There are lights, climate sensors, motion and presence sensors, and a thermostat control. For those attributes you can control (eg. a light), you can type in values and save it by pressing the 'Write' button. For adding sensor values you can use the simulator.

## Service Agent (simulator)

The service agent in apartment 1, can be used to simulate some values to the sensors (as you haven't connected real sensors yet). To do that select the attribute 'apartmentSimulator' and open it up via the arrow on the right. Type in values and don't forget to press the 'Write simulator state' button.

## Scene Agent

The Scene Agent allows you to define scenes: a series of pre-sets for attributes, once a user selects a scene. We will not use the scene agent in this demo. However, if you select the edit view (upper right), and open up one of the attributes, you can see how they work.

# Demo Smart Building

To view an example application we have added the Demo Smart Building which gives control over all apartments. It's build using the OpenRemote UI components (for Developers see [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Working-on-the-UI).

_We are working on the demo environment adding this Application_

We'll describe 4 examples of what will be possible

## Health status monitoring

## Receiving feedback from users

## Communicating to users

## Building optimisation

## Creating workflow rules

# See Also
- [[Use UI components|User-Guide: UI components]]
- [Demo Smart City](Demo-Smart-City)
- [Get Started](https://openremote.io/get-started-manager/)