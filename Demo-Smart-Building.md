This page describes a typical Smart Building Application, which can be a starting point for apartment buildings or large offices. The example integrates several functions, 'Lighting', 'Start (Delayed Start)', 'Safety', 'Energy', and 'Climate' and includes an apartment user application and a Service provider application. The apartment user application is designed once, and gives you access to the correct apartment based on your user credentials. 

The application includes a couple of services, not just convenient for the individual apartment user, but also for the service provider: scenes, scheduling, problem reporting, notification services, and health status.

The basic set-up is part of the [online demo](https://demo.openremote.io) as well as the main branch (See [Get Started](https://openremote.io/get-started-manager/)). You can take it as a starting point for developing your own application. You can view the apartment user application in our [online demo](https://demo.openremote.io) using the credentials for apartment 1 (building / building).

We have described the application here. We are addressing three types of users: the [Apartment User](#application-for-apartment-user), the [Service Manager](#application-for-service-provider), and the [Technical Manager](#Technical-Manager)

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

# Smart Building in the Manager

This example is available as part of the main branch. Follow the [Get Started](https://openremote.io/get-started-manager/) or go to the [Developers page](https://openremote.io/developers/). If you have set up the manager 

# See Also
- [[Use UI components|User-Guide: UI components]]
- [Demo Smart City](Demo-Smart-City)
- [Get Started](https://openremote.io/get-started-manager/)