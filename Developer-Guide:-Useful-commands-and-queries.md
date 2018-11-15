# Commands
Replace `<PROJECT_NAME>` with value used when creating the container with `docker-compose up` (`docker ps` will show the actual names).
## Restart manager docker container

`docker restart <PROJECT_NAME>_manager_1`

## Start interactive `psql` shell in postgresql container
`docker exec -it <PROJECT_NAME>_postgresql_1 psql -U openremote`

## Copy exported data point file to local machine (exported using query below)
`docker cp <PROJECT_NAME>_postgresql_1:/deployment/datapoints.csv ./`

# Queries

## Data points
### Export all asset data points
`copy asset_datapoint to '/deployment/datapoints.csv' delimiter ',' CSV;`

### Delete all asset datapoints older than N days
`delete from asset_datapoint where timestamp < extract('epoch' from (current_timestamp - interval '5 days'))::bigint*1000;`

## Consoles

### Remove consoles that haven't registered for more than N days
**NOTE: Following will require manager to be restarted**

`DELETE FROM asset a WHERE a.asset_type = 'urn:openremote:asset:console' AND to_timestamp((a.attributes#>>'{consoleName, valueTimestamp}')::bigint /1000) < (current_timestamp - interval '30 days');`

## Notifications

### Count notifications with specific title sent to consoles (Android/iOS) in past N days

`select count(*) FROM notification n JOIN asset a ON a.id = n.target_id WHERE n.message ->> 'title' = 'Notification Title' and n.sent_on > (current_timestamp - interval '7 days') and a.attributes #>> '{consolePlatform, value}' LIKE 'Android%' and n.acknowledged_on is not null;`