This page describes a typical Smart Building Application, which can be a starting point for apartment buildings or large offices. The example integrates several functions, 'Lighting', 'Heating', 'Presence', and 'Climate' for three apartments.

There are two applications which are part of the manager, once you have set it up yourself (See [Get Started](https://openremote.io/get-started-manager/)): the manager UI (for technical configuration of the project) and the Smart Building demo, which giving full control over the application. We'll describe both.

For convenience you can have a look at the online version of the Smart Building demo at [Smart Building Demo](https://demo.openremote.io/building) using the credentials (building / building).

# Application for Apartment User

If you are an apartment user you first of all like to have a user friendly application, solving your control needs for several applications. Let's describe the applications for you first.

## Scenes

You have 4 scenes 'Morning', 'Day', 'Evening', and 'Night' available for which you can configure the default heating, ventilation and alarm values on the 'settings' page. When you select 'Automatically switch scenes', scenes will automatically changes based on the time schedule you have programmed.

## Lighting and Devices

The Lighting Panel allows you to switch (or dim) your different lights or switches.

Switch A, B, and C represent devices you can also control by using the timer options: "Now on", "On at" (delayed start). and "Ready At". The latter option is intended for optimisation purposes in case of changing tariffs or changing availability of local (green) energy. It requires a system rules which know the running time of your device, and monitors the best time to start your device, taking care it is finished in time.

## Climate

The Climate Panel is used for both heating, ventilation and the air quality in the rooms of your apartment.

The ventilation can be switched to 3 different levels as well as 'auto'. The 'auto' setting uses a rule which changes the ventilation based on humidity and CO2 levels.

## Climate

The Climate Panel is used for both heating, ventilation and the air quality in the rooms of your apartment (Figure 2). 

The ventilation can be switched to 3 different levels as well as 'auto'. The 'auto' setting uses a rule which increases the ventilation in case of high humidity or CO2 levels.

## Energy

The Energy Panel will give you an overview of the energy used in your building. You can change the time scale and go back in history.

## Security

The Security Panel gives you an overview of any presence detected in your apartment, and in which room. The moment you turn on your alarm, you will receive a push notification in case of any presence detected. Presence is based on a combination of movement detection and CO2 changes.    

## Settings

In 'settings' (option in the menu) you can define the time schedule for 4 scenes, define a holiday period, and set your preferences per scene for climate, ventilation, and alarm. 

## Received notifications

_To be added_

## Notify the service provider

_To be added_

## Change your language

_To be added_

# Application for Service Provider

In case you have to manage a large series of apartments or offices, your challenge is safeguarding that all these systems keep on working. In addition the fact that all apartments, offices, and users are connected offers you an opportunity to organise several building related services effectively and efficiently.

_We are working on the demo environment adding this Application_

We'll describe 4 examples of what will be possible

## Health status monitoring

## Receiving feedback from users

## Communicating to users

## Building optimisation

## Creating workflow rules

# Application for Technical Manager

The application for the Technical Manager requires you to set up the demo environment locally first. Follow the [Get Started](https://openremote.io/get-started-manager/). Next, you can access both the Application for Technical Manager and the Apartment User via your browser (if you are using Docker Toolbox replace localhost with 192.168.99.100):

Application for Technical Manager: https://localhost (Username: admin, Password: secret)

Application for Apartment User: https://localhost/smart-building-v1/ (Username: building, Password: building)

The [Application for the Apartment User](#application-for-apartment-user) is the same as described earlier. If you open the Application for the Technical Manager, you will see Tenant A - Smart Building - Apartment 1. It includes a Service Agent (Simulator), five rooms, and a Scene Agent. If you use the 'Assets' view and select one of the rooms, you will notice that you see exactly the same attributes as visible in the App. The Service Agent (Simulator) can be used to change the simulated sensor values for Apartment 1. 

# See Also
- [[Use UI components|User-Guide: UI components]]
- [Demo Smart City](Demo-Smart-City)
- [Get Started](https://openremote.io/get-started-manager/)