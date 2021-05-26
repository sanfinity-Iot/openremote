The examples below describe interactively linking asset attributes to SNMP traps using the [SNMPClientProtocol](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/snmo/SNMPClientProtocol.java). The following examples are assuming that you are using the basic environment, which can be deployed in docker using the demo.yml profile (refer to the main page of the [wiki](https://github.com/openremote/openremote/wiki)). 

# Setup listening to traps

1. Login to the manager UI (`https://localhost/` `admin/secret`)
2. Navigate to the Assets page and click the + at the top of the Asset list on the left to add an Agent or Asset.
3. In the dialog do the following:
   * Name: `SNMP Agent`
   * Select the agent from the list: `SNMP Client Agent`
   * Confirm with `Add`
4. The agent is now created with preconfigured attributes. We will set some of those to establish the connection:
   * Bindhost: `0.0.0.0` (Don't forget to send the value by clicking the send button on the right or pressing Enter)
   * Bindport: `162`

You now have SNMP protocol ready to be linked to by asset attributes. 
In the next step we will need the Asset ID of this agent, so copy that from the URL now. e.g. `501p87wK1bhf6Dh2M5ZQZj` if URL is: `https://localhost/main/#!assets/false/501p87wK1bhf6Dh2M5ZQZj`

## Setting up an agent link

Now we will create an asset where we can store the values from the trap messages.
1. Click the '+' to add an asset:
   * Name: `SNMP Trap Values`
   * Select the asset type from the list: `Thing Asset`
   * Confirm with `Add`
The thing asset will now appear in the list as a child of the `SNMP Agent`. You can change its parent if you wish.
2. Go to the edit mode by clicking the toggle at the top of the asset page. In the edit mode you can modify the attributes of an asset and set configuration items.
3. Setup a value:
   * Click `Add Attribute` and fill in `SNMP Value` as name and select `Text` as type. (You can name it whatever you want and choose the type to suit your needs)
   * Click `Add`
   * Uncollapse the `SNMP Value` attribute
   * Click `Add configuration item` and select and add `agent link`. 
   * Paste the following in the text area:
```
{
  "type": "SNMPAgentLink",
  "id": "501p87wK1bhf6Dh2M5ZQZj",
  "oid": "1.3.6.1.4.1.8072.2.3.2.1"
}
```
We create an agentlink of the type SNMPAgentLink, the id of the agent is the one that you copied from the URL of the agent asset.

To test it send from a command line the following message:

`sudo snmptrap -v 2c -c public <you-machines-ip-address> '' 1.3.6.1.4.1.8072.2.3.0.1 1.3.6.1.4.1.8072.2.3.2.1 i 123456`

The SNMP value attribute should now have the value `123456 `

## Wildcard

To get the whole SNMP message into an attribute, create an attribute of the type `JSON object` and use an asterisk as wildcard notation:

```
{
  "type": "SNMPAgentLink",
  "id": "501p87wK1bhf6Dh2M5ZQZj",
  "oid": "*"
}
```
 
This will result in example:

```
{
  "1.3.6.1.2.1.1.3.0": "6 days, 2:58:40.00",
  "1.3.6.1.6.3.1.1.4.1.0": "1.3.6.1.4.1.31977.4.3.1.4.10",
  "1.3.6.1.4.1.31977.9.3": "Trap Test!",
  "1.3.6.1.6.3.1.1.4.3.0": "1.3.6.1.4.1.31977"
}
```

# See also

- [Agent overview](https://github.com/openremote/openremote/wiki/User-Guide%3A-Agent-Overview)
- [Quick Start](https://github.com/openremote/openremote/blob/master/README.md)
- [[Manager UI Guide|User-Guide:-Manager-UI]]
- [[Custom Deployment|User-Guide:-Custom-deployment]]
- [Setting up an IDE](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Setting-up-an-IDE)
- [Working on the UI](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-UI-apps-and-components)