This page describes a typical Residential Application, which can be a starting point for multi-tenant apartment buildings. It integrates several functions, 'Lighting', 'Start (Delayed Start)', 'Safety', and 'Climate'. It includes a 'Smart' function which predicts your presence schedules and automatically switches between different scenes for each function. The basic set-up is made with [Velbus](Velbus), but can of course be exchanged by other brands or protocols, eg. [Z-wave](Z-wave) or [KNX](KNX). It includes:
- PIR sensor;
- Window Sensor;
- Temperature Sensor;
- Outdoor weather service for the Outdoor Temperature;
- Light switch with dimmer;
- Thermostat;
- Radiator Valve;
- Ventilation switch

![Figure 1. Panels for Lighting, Smart Start, and Security as available within Home Example.](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Example%20Home%20-%20Panels%201.jpg)

_Figure 1. Panels for Lighting, Start, and Security as available within Home Example._

We have described the application here, and will give you the means to tailor it your own application. We are addressing 3 types of users: first of all the [Home User](#application-for-home-user), secondly the [Installer](#application-for-installer), and third the [Service Provider](#application-for-service-provider). For the installer we added a few functions. The first is a PID control function. This function allows you to create a feedback loop and can be used for eg. heating or ventilation function. The second function allows you to change sensor values (eg. Presence, or Window). This function is helpful to validate whether the rules-logic behind 'Smart' has been implemented for your application. By changing sensor values you can verify whether the application behaves as you expect it to behave without having to wait. For the Service Provider, we offer management tools which allow for remote management of large numbers of Home Installations.

This project can be downloaded as [Designer 2.5 ZIP](https://github.com/openremote/Documentation/blob/master/referenceprojects/openremote.zip) file and the corresponding [Adobe Illustrator file](https://github.com/openremote/Documentation/blob/master/referenceprojects/Example-Home.ai), so you can use it as a starting point for your own application. Before using this project you can already view a demo in your [iOS](iOS-Console-Configuration) or [Android](Android-Console-Configuration) panels by selecting the controller: _demo.openremote.com:8688/controller_.

# Application for Home User

If you are a Home user you first of all like to have a user friendly application, solving your control needs for several applications. Let's describe the applications for you first.

## Smart, predicting your schedule

The Application panels are shown in Figure 1 and Figure 2. The settings panel (Figure 2) indicates the predicted schedules for four different times of the day. With these predictions the four functions can be automated. The schedules are estimated based on the presence being noticed by a PIR, averaged over the last couple of weeks (see [Rules for Predicted Presence](#rules-for-predicted-presence) for details). 

In this Home example application we have only implemented the times for 'Morning' (first column) and 'Night' last column. We welcome users, interested to extend this application to four time zones.

## Lighting and Smart

The Lighting Panel (Figure 1) is used to select four different scenes. With the Designer you can define these four scenes, either using [Macros](Macros) or [Rules](Rules). When you switch on the 'Smart function' the scenes will automatically switch based on your predicted schedule. At 'Morning' time it will switch on the 'Morning' scene. This will remain active until 'Night' time, when it will switch to 'Night'.

The 'Smart' function can be extended to four times of the day, once the presence detection is extended to four time periods per day.

## Start and Smart (Electricity)

The Start Panel is intended for switching your electricity. In the example we display two power outlets which can be switched on and off. Once you turn on 'Smart', you will notice the 'Morning' time. When you now switch on an outlet it will delay the actual switching time to the indicated time. So for example, this allows you to take care you always have fresh coffee when waking up.

This function can be extended for users who have solar panels or flexible electricity tariffs. The time as indicated will be replaced by the time you want your device to switch on at the latest. Based on additional rules (eg. monitoring Solar production or tariffs) your system will decide when to swith on your device.

## Security and Smart

The Security Panel is intended to use your presence sensors for alerting you about unwanted guests. When switching on the Alarm you will see the 'Eye' indicating presence in orange, when either PIR notices movement or the Window is open. When you turn on 'Smart', the Alarm is automatically turned on during the 'Night'.

To extend the application, in stead of the 'Eye' turning orange, you can switch on a light or siren, or arrange a notification service. Once the schedule is extended to four scenes, of course the Alarm can also be automatically switched on for 'Away'. 

## Climate and Smart

The Climate Panel is used for both heating as well as ventilation (Figure 2). You can read the actual temperature and increase the Setpoint temperature. In addition you can toggle between two settings 'Day' and 'Night'. The setpoints for these two can be changed in the Designer. When 'Smart' is switched on, the system will automatically switch between these two settings, based on your Presence schedule.

For Auto-ventilation you can switch the ventilation to 'off' or 'on'. For now, 'on' will use the 'Morning' to switch ventilation to 'low'. At 'Night' it will switch to 'off'. 

To extend the application, you can add a Humidity sensor or CO2 sensor, and even integrate the PID control loop. When 'Auto-ventilation' is turned on you could regulate the ventilation level to 'high' when Humidity is high or CO2 is getting too high. Also, once there are four time periods defined as part of the presence detection, the ventilation can be switched off when 'Away'.

![Figure 2. Panels for Climate, Main menu, and Settings.](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Example%20Home%20-%20Panels%202.jpg)

_Figure 2. Panels for Climate, Main menu, and Settings._

## Settings

In settings you can see the predicted presence schedule for 4 time periods. In the Home Example only 'Morning' and 'Night' are based on actual predictions, using [Rules](##Rules).

The 'Vacation Settings' allows you to manually override the automated prediction. It will switch on the alarm, switch climate to 'Night', and will switch 'Lighting' automatically according to your time schedule.

To extend your application you could consider adding a service using a phone number to send a Text Message in case both Vacation as well as the Alarm is switched on.

# Application for Installer 

As an installer you want to have tools so you can test the system you have build for your customer (the Home user). We offer you three tools to help you. First, an approach to test the system by 'faking' sensor changes. Secondly a PID control solution. And finally a set of rules, which you can change and adopt to your needs. We explain all three.

## Test your system

As there are quite some rules in your system and you might be changing or adding some, it's critical to test whether you system behaves as it should be. Therefore, we added a Configuration Panel in which you can change the value of the Sensors: presence, window, indoor-, and outdoor temperature. By changing these sensor values you can test whether the system behaves as it should without having to wait. These panels are available for iPhone5 (Config-iPhone5) and Samsung Galaxy S6 (Config-SamsungS6), selecting the Tab 'Sensors'.

![Figure 3. Configuration Panels for faking sensor values (for testing), and setting the parameters for the PID control function.](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Example%20Home%20-%20Configuration.jpg)

_Figure 3. Configuration Panels for faking sensor values (for testing), and setting the parameters for the PID control function._

## PID Controller

There is a PID controller added to the controller. If you select the same Configuration Panel, same as for the sensors, and select the Tab 'PID Config', you will see the configuration parameters for a PID feedback loop:
- Proportional Gain Kp
- Integral Gain Ki
- Derivative Gain Kd
- Setpoint

For output you see:
- Steady State Error SE
- Output

The rule set is included in the demo and the downloadable Home example. For installers, who are looking for ways to prevent temperature overshoots, or want to manage CO2 levels with a more responsive system, we have added this function. Note, you still have to hook it up to the correct sensors and commands yourself.

## Rules for Predicted Presence

The rules for predicted presence are currently creating two time periods per day of the week. The rules are written in Drools (see more about [Rules](Rules)). The rules offer a method to predict these times on actual presence in the previous week.

# Application for Service Provider

In case you have to manage a large series of Homes or Users, your next challenge is safeguarding that all these systems keep on working. We are working on our Manager 3.0, which will allow you to have remote monitoring, account management and messaging services. Follow the status of the development at [OpenRemote.IO](https://openremote.io/) and as part of the documentation. If you are a large Service Provider, we are happy to show you a demo in person. Just [Contact Us](https://openremote.io/contact/).

# How to get started

The example as shown here is build with Velbus Hardware. However it can be changed to any other hardware, brands, or protocols. To get started with this example from scratch, we recommend the following steps: 
 
1. Set up OpenRemote and familiarize yourself with the basic functions: [Get Started](http://www.openremote.com/get-started/). Add real devices, using the tutorial book [How To Smart Home](http://howtosmarthome.com/)
2. Try the Home Example by going to the demo environment with your [iOS or Android Console](http://www.openremote.com/download-the-apps/), using demo.openremote.com:8688/controller
3. Download the ZIP file and [Import](Export-&-Import) it in your OpenRemote Designer
4. In the Building Modeler, adjust the devices, commands and sensors, eg using [Velbus](Velbus), [Z-wave](Z-wave), [KNX](http://www.openremote.org/display/docs/OpenRemote+2.0+How+To+-+KNX) or any of the other supported products & protocols 
5. In the UI Designer, adjust your UI Designs. Use the [Adobe Illustrator file](https://github.com/openremote/Documentation/blob/master/referenceprojects/Example-Home.ai), to adjust images in whatever way you want.

# See Also

- 