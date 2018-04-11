Any asset can have a location, how the location is updated is specific to the asset type and whether the location attribute is linked to a protocol. Some example scenarios: -

### GPS Tracker
A GPS tracker with internet connection (dedicated device, smart phone/watch, etc); the device pushes its location to the server, an asset is used to represent the tracker and its location is updated when an update is received from the physical device. Provides fine grained location data and following concerns must be considered:

* Data/battery usage
* Privacy issues
* Storage of data (current and historical)

### Smart device with geofencing capabilities (e.g. Android/iOS)
A smart device such as a mobile phone (e.g. Android or iOS) that has a geofence API for coarse location tracking; geofences are defined on the device and a callback is triggered when the geofence is:

* Entered
* Exited
* Entered and Exited

### A plain old asset
Any asset that doesn't actively monitor its location but instead the location is manually updated by a user.

## Location triggered rules
There are many scenarios when it is desirable to perform some action when an assets location meets some criteria, e.g: - 
* when an asset enters a specified region then send a notification to user/users
* when a user's mobile phone leaves a specified region then show a notification on the mobile phone reminding them to do something

It