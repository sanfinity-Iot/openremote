This page describes a typical Smart Building Application, which can be a starting point for apartment buildings or lage offices. The example integrates several functions, 'Lighting', 'Start (Delayed Start)', 'Safety', 'Energy', and 'Climate' and includes an apartment user application and a Service provider application. The apartment user application is designed once, and gives you access to the correct apartment based based on your user credentials. 

The application includes a couple of services, not just convenient for the individual apartment user, but also for the service provider: scenes, scheduling, problem reporting, notification services, and health status.

The basic set-up is part of the demo environment as well as the main branch. You can take it as a starting point for developing your own application. You can view the apartment user application in our [online demo](https://demo.openremote.io) using the credentials for apartment 1 (building / building).

temporarily see [Demo UI](https://xd.adobe.com/view/e48ac2cb-4060-45f2-5c33-6fa30abe6818-92bb/screen/8f384d1b-8d9b-4733-8789-3438a1ed8f29/Scenes)

_Figure 1. The user application for a single apartment, as part of the demo environment and main branch._

We have described the application here. We are addressing both types of users: the [Apartment User](#application-for-apartment-user), and the [Service Provider](#application-for-service-provider). 

# Application for Apartment User

If you are an apartment user you first of all like to have a user friendly application, solving your control needs for several applications. Let's describe the applications for you first.

## Scenes

You have 4 scenes 'Morning', 'Day', 'Evening', and 'Night' available for which you can configure the default heating, ventilation and alarm values on the 'settings' page. When you select 'Automatic scenes scheduling', scenes will automatically changes based on the time schedule you have programmed.

## Lighting and Devices

The Lighting Panel (Figure 1) allows you to switch (or dim) your different lights or switches.

The washing machine and dishwasher are examples of devices you can also control by using three options: "Now on", "On at" (delayed start). and "Ready By". The latter option is intended for optimisation purposes in case of changing tariffs or changing availability of local (green) energy. It requires a system rules which know the running time of your device, and monitors the best time to start your device, taking care it is finished in time.

## Climate

The Climate Panel is used for both heating, ventilation and the air quality in the rooms of your apartment (Figure 2). 

The ventilation can be switched to 3 different levels as well as 'auto'. The 'auto' setting uses a rule which increases the ventilation in case of high humidity or CO2 levels.

temporarily see [Demo UI](https://xd.adobe.com/view/e48ac2cb-4060-45f2-5c33-6fa30abe6818-92bb/screen/32ab82f9-8bf0-49ba-b9d0-4e17eae23469/Klimaat)

_Figure 2. Panels for Climate, Main menu, and Settings._

## Climate

The Climate Panel is used for both heating, ventilation and the air quality in the rooms of your apartment (Figure 2). 

The ventilation can be switched to 3 different levels as well as 'auto'. The 'auto' setting uses a rule which increases the ventilation in case of high humidity or CO2 levels.

temporarily see [Demo UI](https://xd.adobe.com/view/e48ac2cb-4060-45f2-5c33-6fa30abe6818-92bb/screen/32ab82f9-8bf0-49ba-b9d0-4e17eae23469/Klimaat)

_Figure 2. Panels for Climate, Main menu, and Settings._

## Energy

The Energy Panel gives you an overview of the energy used in your building. You can change the time scale and go back in history.

## Security

The Security Panel gives you an overview of any presence detected in your apartment, and in which room. The moment you turn on your alarm, you will receive a push notification in case of any presence detected.  

## Settings

In 'settings' (option in the menu) you can define the time schedule for 4 scenes, define a holiday period, and set your preferences per scene for climate, ventilation, and alarm. 

## Received notifications

This page (option in the menu) gives you an overview of all received notifications, either triggered by you security alarm, or shared with you by the service provider.

## Notify the service provider

This page (option in the menu) allows you to send notifications to your service provider. In the demo you will be sending an e-mail to OpenRemote. 

# Application for Service Provider

In case you have to manage a large series of apartments or offices, your challenge is safeguarding that all these systems keep on working. In addition the fact that all apartments, offices, and users are connected offers you an opportunity to organise several building related services effectively and efficiently.

We'll describe 4 examples of what's possible

## Health status monitoring

## Receiving feedback from users

## Communicating to users

## Building optimisation

# How to get started

The example as shown here is available as part of the main branch. Follow the [Get Started](https://openremote.io/get-started-manager/) or go to the [Developers page](https://openremote.io/developers/).

# See Also
- [[Use UI components|User-Guide: UI components]]
- [Demo Smart City](Demo-Smart-City)
- [Get Started](https://openremote.io/get-started-manager/)