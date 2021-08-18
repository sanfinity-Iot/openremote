Connect to a LoRa Mesh network

## Proof of concept for LoRa mesh network for GPS trackers

A couple of students have worked on a generic way to use LoRa to connect several devices in a mesh network and link these through a Protocol Agent with OpenRemote. In the specific application they have developed a Range of LoRa GPS trackers which allow for integrating all these trackers, including their live location within OpenRemote.

Both the firmware running on the individual devices, as well as the Protocol Agent for OpenRemote is available as a separate project ['or-loratrackers'](https://github.com/openremote/or-loratrackers).
It includes a generic asset and attribute model such that the code can be applied for any kind of application and asset model in which you like to build your own connected network of devices leveraging a mesh. 

As this is a proof of concept we value feedback and looking for anybody interested in following up on this. Just get in touch.