This tutorial explains how to connect to the [Open Weather Map](https://openweathermap.org/) API using the [Http Agent](./User-Guide%3A-HTTP-Agent) and is based on the basic environment described in the quick start. 

## Prerequisites

* A running Open Remote instance (this tutorial assumes `https://localhost`)
* A free API key for the [Open Weather Map](https://openweathermap.org/) API

## Create the Agent
1. Login to the manager UI (`https://localhost/` `admin/secret`)
2. Navigate to the Assets page and click the `+` at the top of the Asset list on the left to add an Agent or Asset.
3. In the dialog do the following:
   * Name: `HTTP API Agent`
   * Select the agent from the list: `HTTP Agent`
   * Confirm with `Add`
4. The agent is now created with pre-configured attributes. We will set some of those to establish the connection:
   * Base URI: `https://api.openweathermap.org/data/2.5/` (Don't forget to send the value by clicking the send button on the right or pressing Enter)
   * Request query parameters: `{"appid": ["YOUR_API_KEY"]}` (Input the API key from you openweathermap account)

You now have a basic HTTP API protocol ready to be linked to by asset attributes. 

In the next step we will need the Asset ID of this agent, so copy that from the URL now. e.g. `501p87wK1bhf6Dh2M5ZQZj` if URL is: `https://localhost/main/#!assets/false/501p87wK1bhf6Dh2M5ZQZj`

## Create the weather asset
Now we will create a weather asset where we show the temperature and humidity in Rotterdam that we collect using the OpenWeatherMap API.
1. Click the `+` to add an asset:
   * Name: `Weather Rotterdam`
   * Select the asset type from the list: `Weather Asset`
   * Confirm with `Add`
The weather asset will now appear in the list as a child of the `HTTP API Agent`. You can change its parent if you wish.

## Add the Agent Links
1. Go to the edit mode by clicking the toggle at the top of the asset page. In the edit mode you can modify the attributes of an asset and set configuration items.
2. First we will set up the humidity value:
   * Expand the `humidity` attribute
   * Click `Add configuration item` and select and add `Agent link`. 
   * Paste the following in the text area and replace the `id` with the agent `id` from above:
```
{
  "type": "HTTPAgent.HTTPAgentLink",
  "id": "501p87wK1bhf6Dh2M5ZQZj",
  "queryParameters": {
    "q": [
      "Rotterdam,nl"
    ],
    "units": [
      "metric"
    ]
  },
  "pollingMillis": 60000,
  "path": "weather",
  "valueFilters": [
      {
        "type": "jsonPath",
        "path": "$.main.humidity",
        "returnFirst": true,
        "returnLast": false
      }
    ]
}
```

What this means is that, we are looking for the data of the city of Rotterdam, in metric units. We want this information every minute. The information can be found at the base URI + `/weather`. Then the returned data (JSON payload) is filtered to find the humidity value.

1. We do the same for the temperature value:
   * Expand the `temperature` attribute
   * Click `Add configuration item` and select and add `Agent link`. 
   * Paste the following in the text area and replace the `id` with the agent `id` from above
```
{
  "type": "HTTPAgent.HTTPAgentLink",
  "id": "501p87wK1bhf6Dh2M5ZQZj",
  "queryParameters": {
    "q": [
      "Rotterdam,nl"
    ],
    "units": [
      "metric"
    ]
  },
  "pollingMillis": 60000,
  "path": "weather",
  "valueFilters": [
      {
        "type": "jsonPath",
        "path": "$.main.temp",
        "returnFirst": true,
        "returnLast": false
      }
    ]
}
```
2. Save the asset (top right)

Switch to view mode (by clicking the edit mode toggle at the top again) to view the live values. You now have the live weather data of Rotterdam linked.

## Setting multiple attributes with one agent link

As you may have noticed we do two calls in the above example, one to get the humidity and one to get the temperature. You can imagine that when collecting all the attributes this way, and for several assets, will greatly increase the number of calls you make. This can be optimized by doing one call and extracting all the values you need and pushing them to the attributes of your assets. We will recreate the example above using this method.

1. First lets remove the agent links from the `Weather` asset attributes `temperature` and `humidity`.
   * Go to the edit mode in the `Weather` asset
   * Expand `humidity`
   * Click the 'X' on the right of the `Agent link` configuration item
2. Do the same for the temperature asset.
3. Now we create a custom attribute that will hold the weather data we collect so that we can push the values from there. You could also do this from the `HTTP API Agent` asset or any other asset if that makes more sense to you.
   * Click `Add attribute` at the bottom of the list of attributes.
   * Select custom, select the attribute type `JSON object` and give it a name: `weatherData`. Click `Add`
   * Add the configuration item `Agent link`, `Attribute link` and `Read only` by selecting them in the add configuration item dialog and clicking `Add`.
4. In the `Agent link` configuration item add the following, again using the ID from your weather agent:
```
{
  "type": "HTTPAgent.HTTPAgentLink",
  "id": "501p87wK1bhf6Dh2M5ZQZj",
  "queryParameters": {
    "q": [
      "Rotterdam,nl"
    ],
    "units": [
      "metric"
    ]
  },
  "pollingMillis": 60000,
  "path": "weather"
}
```
5. In the `attribute link` configuration item add the following, using the ID copied from this assets URL (the asset that has the attributes you want to push the data to):
```
[
  {
    "ref": {
      "id": "THE_ASSET_ID_YOU_COPY_FROM_ITS_URL",
      "name": "temperature"
    },
    "filters": [
      {
        "type": "jsonPath",
        "path": "$.main.temp",
        "returnFirst": true,
        "returnLast": false
      }
    ]
  },
  {
    "ref": {
      "id": "THE_ASSET_ID_YOU_COPY_FROM_ITS_URL",
      "name": "humidity"
    },
    "filters": [
      {
        "type": "jsonPath",
        "path": "$.main.humidity",
        "returnFirst": true,
        "returnLast": false
      }
    ]
  }
]
```
6. Click read only to set it to true.
7. Save the asset (top right)

With the attribute link configuration item you filter the values collected with the agent link and push them to the attributes in your specified asset. When you go to view mode you will see the weather data, temperature, and humidity value in their respective attributes.

## Additional Exercises

Try and create additional attributes that link to the OpenWeatherMap API, some ideas:
   * Get temperature in Fahrenheit
   * Get windspeed (a place to get started with this: https://openweathermap.org/current)