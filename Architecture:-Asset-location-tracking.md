## Terminology
### Asset and asset location
Asset in the scope of this document refers specifically to assets that have a geographical location and asset location refers to a well known attribute called location which is defined on the asset; how the location is updated is specific to the asset type and whether the location attribute is linked to a protocol. Some example scenarios: -

* **GPS Tracker** - A GPS tracker with internet connection (dedicated device, smart phone/watch, etc); the device pushes its location to the server, an asset is used to represent the tracker and its location is updated when an update is received from the physical device. Provides fine grained location data and following concerns must be considered:

  1. Data/battery usage
  2. Privacy issues
  3. Storage of data (current and historical)

* **Smart device with geofencing capabilities (e.g. Android/iOS)** - A smart device such as a mobile phone (e.g. Android or iOS) that has a geofence API for coarse location tracking; geofences are defined on the device and a callback is triggered when the geofence is:

  1. Entered
  2. Exited
  3. Entered and Exited

* **A plain old asset** - Any asset that doesn't actively monitor its location but instead the location is manually updated by a user.

### Client
Refers to a backend (manager) consumer (same meaning as OAuth2 client). The scope of a client is determined on a per project basis but generally there are the following types:

  * Manager client web app
  * Realm web app(s)
  * Realm console/mobile app(s)

Clients have a 1-1 mapping with clients in keycloak.

### Client instance
Refers to an instance of a client; not necessarily an app installation - think of a user loading a web app in their browser, this is a client instance, just like an installation of a console app on an Android/iOS device.

**A smart phone can be either a geographical asset or a client or both.**

## Location tracking scenarios
Reasons/use cases for location tracking:

1. Showing an asset on a map
2. Performing some action (e.g. send a notification) when an asset enters/exits a specific region
3. Recording historical location data (privacy issues need consideration and any users should be fully aware)

Scenario 3 is not in the scope of this document and will be ignored; scenario 1 can be achieved by simply plotting the asset on a map and subscribing to location attribute event changes for that asset and updating the map accordingly. Scenario 2 is essentially location triggered rules and is discussed below.

## Location triggered rules
The common use case is to specify a geographical region and to then perform some action when this region is entered, exited or both, this is known as geofencing and depending on the asset type this can either be implemented on the physical asset or within the manager; implementing within the manager requires constant location updates from the physical asset which has drawbacks as described above. Where supported; geofence APIs on the physical asset should be preferred.

It is possible to define location triggers in rule LHS and how this is implemented on the affected assets is abstracted away by the OpenRemote manager (i.e. the manager determines whether or not geofence APIs can be used or not). The below information describes how this is achieved:

1. Asset is provisioned in the manager (either manually or automatically via a protocol)
2. Asset type and attributes indicate geofence API support (which API and version)
3. Any pre-existing rules that apply to this asset that have location predicates in LHS are then 'pushed' to the asset based on whether:
   1. The asset supports geofence APIs
   2. The asset can be notified that geofence definitions need fetching/refreshing
   3. The location predicate is supported by the geofence API on the asset (Android and iOS only support radial geofences)
4. If there are rules with location predicates but they cannot be implemented using geofence APIs on the asset (as described in 3) then the manager does nothing and the location attribute is expected to be updated using some other mechanism (asset pushes location, protocol polling, manual, etc.)


### Client instance registration
When a client instance loads it contacts the `client/register` endpoint providing details about itself (ID, owning user, platform, FCM/Web push registration token, geofence API support, etc.) if the ID is specified then the backend will retrieve the existing client instance asset for that id; if not found then a new asset is created, this asset is then updated/populated with the client instance details and the following data is returned to the client instance:

* Asset ID
* Existing geofence definitions (when the client instance supports geofence APIs - discussed later)

This process should occur every time the client instance is loaded.

### Creating a location triggered rule
When a ruleset is created that has a rule with a location based condition; as soon as that ruleset version is deployed for the first time all client instance assets that support geofence APIs and are in the scope of the ruleset are notified (using push notification) to refresh their geofence definitions, the client instance then:

* Calls `client/geofences` endpoint providing its asset ID, the backend then returns all geofence definitions applicable to this asset
* The asset then updates its geofence definitions accordingly

When a geofence is triggered on an asset then the asset should update its own location by posting to the public endpoint `client/location` (this allows anonymous client instances to update their location), the location value sent should be as follows:

* Geofence Enter - Send geofence centre point as location (centre point should have been provided in the geofence definition retrieved from the backend)
* Geofence Exit - Send null (this will clear the devices location and indicate that the asset has left the geofence)


## Discussion/TODOs
* How many geofence definitions can fit in a 4KB FCM message)
* Should push notification try to include geofence definitions (push notification size is limited)?
* Need to figure out how to identify rules with location based conditions during rule deployment
* Define data that client instance provides to the backend
* Define a geofence definition (geofence APIs only seem to support radial fences), should include centre point to be sent when triggered
* Only console instances that are anonymous should allow public updates of their location (is the asset ID enough of a security mechanism to prevent spoofing)
* Assets that support geofence APIs must also support a mechanism for remotely updating/refreshing the geofences (Andrdoid/iOS=push notification subscription to 'geofence' topic?, GPS trackers=SMS)