The examples below describe interactively linking asset attributes to HTTP APIs using the [HttpClientProtocol](https://github.com/openremote/openremote/blob/master/agent/src/main/java/org/openremote/agent/protocol/http/HttpClientProtocol.java). The following examples are assuming that you are using the basic environment which can be deployed in docker using the demo.yml profile (refer to the main page of the [wiki](https://github.com/openremote/openremote/wiki)).

# Basic HTTP API

The following example uses a weather API provided by [OpenWeatherMap](https://openweathermap.org/) you will need to signup for a free API Key to access their service. The API Key is included in the request URL as a query parameter

1. Login to the manager UI (`https://localhost/master` `admin/secret`)
2. Select the Assets tab and click `Create asset` at the top of the Asset list on the left
3. Set the following:
   * Asset name: `HTTP API Agent`
   * Parent asset: `Smart Home`
   * Asset Type: `Agent`
4. Click `Create asset` then click `Edit asset` in the top right
5. Add a new attribute:
   * Name: `weatherApi`
   * Type: `Http Client`
6. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then set the following:
   * HTTP Base URI: `https://api.openweathermap.org/data/2.5/`

You now have a basic HTTP API protocol ready to be linked to by asset attributes.

1. Select the Customer A -> Smart Home asset in the asset list
2. Click `Edit asset` in the top right
3. Add a new attribute:
   * Name: `temp`
   * Type: `Temperature in Celcius`
4. Click `Add attribute` and then expand the new attribute (using button on the right of the attribute) then set the following:
   * Attribute link: `HTTP API Agent -> weatherApi`
   *
   weather?q=London,uk

