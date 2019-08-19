The examples below describe interactively linking asset attributes to HTTP APIs using the [HttpClientProtocol](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/http/HttpClientProtocol.java). The following examples are assuming that you are using the basic environment which can be deployed in docker using the demo.yml profile (refer to the main page of the [wiki](https://github.com/openremote/openremote/wiki)). 

# Basic HTTP API

The following example uses a weather API provided by [OpenWeatherMap](https://openweathermap.org/) you will need to signup for a free API Key to access their service. The API Key is included in the request URL as a query parameter

1. Login to the manager UI (`https://localhost/master` `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left
3. Set the following:
   * Asset name: `HTTP API Agent`
   * Parent asset: `Tenant A -> Smart Building` and select 'OK'
   * Asset Type: `Agent`
4. Click `Create asset` at bottom of screen
5. Click `Edit asset` and add a new attribute:
   * Name: `weatherApi`
   * Type: `Http Client`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then configure the Attribute configuration by setting/adding configuration items as follows:
   * HTTP base URI: `https://api.openweathermap.org/data/2.5/`
   * HTTP query parameters: `{"appid": "YOUR_API_KEY"}` (Click the JSON Object button to open the JSON editor)

**Make sure you click `Add item` when creating new attribute configuration items**.

7. Click `Save asset` at the bottom of the screen

You now have a basic HTTP API protocol ready to be linked to by asset attributes.

**NOTE: The protocol configuration status will show as `UNKNOWN` until an attribute is linked and a request is made; or the HTTP Ping attribute configuration items are configured**.

1. Select the Tenant A -> Smart Building asset in the asset list
2. Click `Edit asset` in the top right
3. Add a new attribute:
   * Name: `outsideTemp`
   * Type: `Temperature in Celcius`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then add the following configuration items:
   * Agent protocol link: `HTTP API Agent -> weatherApi`
   * HTTP path: `weather`
   * HTTP query parameters: `{"q": "Eindhoven,nl", "units": "metric"}`
   * HTTP polling interval (s): `60`

**NOTE: At this point you have enough to pull back the entire response of the weather API request but this is a JSON payload so we need to extract the temperature value from this using a filter**

5. Add the following additional configuration item:
   * Protocol response filters: `[{"type": "json", "path":["main","temp"]}]`

## Additional Exercises

Try and create additional attributes that link to the OpenWeatherMap API, some ideas:
* Get temperature in Farenheit
* Get humidity
* Get pressure

# See also

- [[Use UI components|User-Guide: UI components]]
- [[Demo Smart Building|Demo-Smart-Building]]
- [Get Started](https://openremote.io/get-started-manager/)