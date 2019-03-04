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

### Importing asset data points
To import data points stored in multiple exports then first create a temp table without any key constraints:
```
create table ASSET_DATATEMP (
  TIMESTAMP      int8         not null,
  ENTITY_ID      varchar(36)  not null,
  ATTRIBUTE_NAME varchar(255) not null,
  VALUE          jsonb        not null
)
```

Import the CSV files into this temp table:
`COPY DATATEMP FROM '/deployment/datapoints.csv' DELIMITER ',' CSV;`

Select distinct rows and insert into `ASSET_DATAPOINT` table:
```
INSERT INTO ASSET_DATAPOINT
SELECT DISTINCT ON (timestamp, entity_id, attribute_name) *
FROM DATATEMP
ORDER BY timestamp DESC
```

Drop temp table:
`DROP TABLE DATATEMP CASCADE;`

## Consoles

### Remove consoles that haven't registered for more than N days
**NOTE: Following will require manager to be restarted**

`DELETE FROM asset a WHERE a.asset_type = 'urn:openremote:asset:console' AND to_timestamp((a.attributes#>>'{consoleName, valueTimestamp}')::bigint /1000) < (current_timestamp - interval '30 days');`

## Notifications

### Count notifications with specific title sent to consoles (Android/iOS) in past N days

`select count(*) FROM notification n JOIN asset a ON a.id = n.target_id WHERE n.message ->> 'title' = 'NOTIFICATION TITLE' and n.sent_on > (current_timestamp - interval '7 days') and a.attributes #>> '{consolePlatform, value}' LIKE 'Android%' and n.acknowledged_on is not null;`

### Count notifications with specific title sent to consoles (Android/iOS) and acknowledged by:

#### Closing/Dismissing
`select count(*) FROM notification n JOIN asset a ON a.id = n.target_id WHERE n.message ->> 'title' = 'Kijk mee naar de proefbestrating op de Demer' and n.acknowledged_on is not null and n.acknowledgement = 'CLOSED';`

#### Clicking the notification
`select count(*) FROM notification n JOIN asset a ON a.id = n.target_id WHERE n.message ->> 'title' = 'NOTIFICATION TITLE' and n.acknowledged_on is not null and n.acknowledgement is null;`

#### Clicking a specific action button
`select count(*) FROM notification n JOIN asset a ON a.id = n.target_id WHERE n.message ->> 'title' = 'Kijk mee naar de proefbestrating op de Demer' and n.acknowledged_on is not null and n.acknowledgement LIKE '%BUTTON TITLE%';`