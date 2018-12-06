### Vector maps (Mapbox GL)

The manager has built in support for `Mapbox GL` and can serve vector tile data for this to work please ensure that you have the following files available in the deployment folder:

* `map/mapsettings.json` - Contains settings related to the map tiles source data and also UI rendering settings (e.g. center point of map when first loaded)
* `map/mapdata.mbtiles` - Contains the vector map data (see Creating a custom project](./Developer-Guide:-Creating-a-custom-project) for more details)

If you have the map data and/or settings in a different location then please ensure that the manager environment variables are set also:
```
MAP_TILES_PATH=../deployment/map/mapdata.mbtiles
MAP_SETTINGS_PATH=../deployment/map/mapsettings.json
```

### Raster Maps (Mapbox JS)
If you are working on raster maps (Mapbox JS) then you will need to have the `map` docker container running, this container serves the raster map tiles from the vector map data. 

The container can be started by using the `dev-map.yml` profile (see [here](/Developer-Guide:-Docker-compose-profiles)) or you can add a `map` service to an existing custom project profile (copy the `dev-map.yml` as a template).
