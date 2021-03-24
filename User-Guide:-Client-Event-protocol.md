**NOTE: CLIENT EVENT AGENT IS NOT YET ACCESSIBLE VIA THE NEW MANAGER UI. IT IS HOWEVER ALREADY PART OF THE CODE BASE AND WILL BE ADDED IN NEXT RELEASES.**

## Purpose

This protocol is for pub-sub devices to connect to the manager client event broker (MQTT or websocket);
attributes then linked to this protocol can be written to/read by the devices. The protocol handles the
creation of a Keycloak client that allows client secret 'authentication' for machines therefore this protocol
requires the Keycloak identity provider.

## Setup/Provisioning

Create a `ClientEvent` `agent` in the realm that contains the `asset(s)` you wish to subscribe/publish to. This will automatically provision the keycloak client either with a randomly generated client ID and secret or with the values supplied in the `clientId` and `clientSecret` attributes of the agent; once the Keycloak client is generated the `clientId` and `clientSecret` attributes will be populated with the corresponding values.