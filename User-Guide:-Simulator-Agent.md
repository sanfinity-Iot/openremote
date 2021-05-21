The simulator agent enables writing scheduled values to attributes. This can be used simulate the behaviour of an asset attribute before you have it connected to 'real' data. You can link multiple attributes of multiple assets to the same Simulator Agent, so you will only need one in your project/realm.

## Example: Simulating flexible tariff of an energy supplier
Use case: you are creating an energy management system that makes decisions based on the import and export tariffs of an energy supplier. However, you are not connected to a live API of that supplier yet. You will be simulating the daily energy import tariff of the supplier using the simulator agent.

1. Add a Simulator Agent asset
2. Go to edit mode and in your Energy Supplier asset uncollapse the `import tariff` attribute
3. Add the `Agent link` configuration item to the attribute
4. Create the link to the simulator agent by using its `Asset ID` in the following JSON: (you can find the Asset ID at the end of the URL when you have the simulator agent selected) 
```json
{
  "type": "SimulatorAgentLink",
  "id": "2Y3M6ScH9jnrKJixyB00ZT",
  "simulatorReplayData": [
    {
      "timestamp": 0,
      "value": 0.02
    },
    {
      "timestamp": 3600,
      "value": 0.01
    },
    {
      "timestamp": 7200,
      "value": -0.01
    },
    {
      "timestamp": 10800,
      "value": -0.03
    },
    {
      "timestamp": 14400,
      "value": -0.03
    },
    {
      "timestamp": 18000,
      "value": -0.01
    },
    {
      "timestamp": 21600,
      "value": 0
    },
    {
      "timestamp": 25200,
      "value": 0
    },
    {
      "timestamp": 28800,
      "value": 0.03
    },
    {
      "timestamp": 32400,
      "value": 0.05
    },
    {
      "timestamp": 36000,
      "value": 0.01
    },
    {
      "timestamp": 39600,
      "value": 0.01
    },
    {
      "timestamp": 43200,
      "value": 0.01
    },
    {
      "timestamp": 46800,
      "value": 0.01
    },
    {
      "timestamp": 50400,
      "value": 0.01
    },
    {
      "timestamp": 54000,
      "value": 0.04
    },
    {
      "timestamp": 57600,
      "value": 0.08
    },
    {
      "timestamp": 61200,
      "value": 0.1
    },
    {
      "timestamp": 64800,
      "value": 0.1
    },
    {
      "timestamp": 68400,
      "value": 0.04
    },
    {
      "timestamp": 72000,
      "value": 0.03
    },
    {
      "timestamp": 75600,
      "value": 0.02
    },
    {
      "timestamp": 79200,
      "value": 0.02
    },
    {
      "timestamp": 82800,
      "value": 0.02
    }
  ]
}
``` 
We make the link to the simulator agent, and for each timestamp set the value we would like to write. The timestamp is based on seconds of the day. 0 is 00:00:00 in the system time of the machine that hosts the manager.

5. Save the asset and verify that values are written correctly. In the example above we write a value every hour, for test purposes you could change a timestamp to your current time so that you not have to wait for a full hour.

# See also

- [Agent overview](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview)
- [Quick Start](https://github.com/openremote/openremote/blob/master/README.md)
- [[Manager UI Guide|User-Guide:-Manager-UI]]
- [[Custom Deployment|User-Guide:-Custom-deployment]]
- [Setting up an IDE](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Setting-up-an-IDE)
- [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-UI-apps-and-components)