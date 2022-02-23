The OpenRemote Manager is responsive and can be used on mobile devices. To make use of the services of mobile devices (e.g. location service and push notifications), there are Android and iOS consoles. Next to being able to build your own app, using the project source code, we have an OpenRemote app in the Apple and Google stores which can be used as well.

# OpenRemote iOS and Android App

OpenRemote includes consoles for iOS and Android. The current apps we are hosting on the [Appstore](https://apps.apple.com/nl/app/openremote-app/id1526315885?mt=8) and [Google Play Store](https://play.google.com/store/apps/details?id=io.openremote.app&pcampaignid=pcampaignidMKT-Other-global-all-co-prtnr-py-PartBadge-Mar2515-1), can connect to your OpenRemote Manager instance. 

When you open the app the first time the app asks for the 'Project Domain' and 'Realm' names. If you are e.g. hosting an OpenRemote instance at https://yourhost.com/ and want to access the 'smartcity' realm use the following: 'Project Domain' is 'yourhost.com' and 'Realm' is 'smartcity' Note that you also need to add a configuration file for each realm in [consoleappconfig](https://github.com/openremote/openremote/blob/master/manager/src/consoleappconfig/).

![](https://github.com/openremote/Documentation/blob/master/manuscript/figures/Mobile%20app%20for%20OpenRemote%20Demo.png)
_Use the OpenRemote app on the Appstore and Google Play Store to access the demo and use location and push notification_

The first time it will ask for permission to use your location and send notifications. If enabled, you can use Rules in the Manager to define a Left-Hand-Side condition, based on a console location, or a Right-Hand-Side action to send a notification to this Console Asset (anonymous) or the Mobile User of this mobile device.

Once you come to the login page, you are accessing the Manager UI and seeing the mobile version.

Switching between projects and realms can be done by long-pressing the app icon on your home screen. Note that the default app in the Appstore and Google Play Store, direct to OpenRemote Managers we are hosting on the OpenRemote domain, for example demo.openremote.io.

# Building your own mobile apps

You can redirect the consoles to your own URL and build the apps yourself. Check out the links below and the code itself. 

# See Also
- [Working on the mobile consoles](Developer-Guide%3A-Working-on-the-mobile-consoles)
- [Architecture: Apps and consoles](Architecture%3A-Apps-and-consoles)