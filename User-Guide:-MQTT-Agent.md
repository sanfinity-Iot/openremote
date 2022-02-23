There is an MQTT Agent (Client) in OpenRemote that you can use to connect to an external MQTT Broker. First use the MQTT Agent to establish the connection, then create an asset with attributes that have the configuration item 'Agent Link'. In this agent link select your MQTT Agent and add the parameter Publish Topic or Subscription Topic. 
We have no extensive documentation yet, and recommend to [check our forum](https://forum.openremote.io/t/mqtt-agents-publish-subscription/985). 

OpenRemote also has an [MQTT Broker](https://github.com/openremote/openremote/wiki/User-Guide%3A-Manager-APIs#mqtt-api-mqtt-broker) (or MQTT API). 

# See also

- [Agent overview](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview)
- [MQTT Broker](https://github.com/openremote/openremote/wiki/User-Guide%3A-Manager-APIs#mqtt-api-mqtt-broker)
- [Quick Start](https://github.com/openremote/openremote/blob/master/README.md)
- [[Manager UI Guide|User-Guide:-Manager-UI]]
- [[Custom Deployment|User-Guide:-Custom-deployment]]
- [Setting up an IDE](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Setting-up-an-IDE)
- [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-UI-apps-and-components)