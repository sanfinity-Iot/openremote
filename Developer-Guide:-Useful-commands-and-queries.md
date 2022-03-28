# Docker
Replace `<PROJECT_NAME>` with value used when creating the container with `docker-compose up` (`docker ps` will show the actual names).
## Restart manager docker container

`docker restart <PROJECT_NAME>_manager_1`

## Start interactive `psql` shell in postgresql container
`docker exec -it <PROJECT_NAME>_postgresql_1 psql -U postgres`

## Copy exported data point file to local machine (exported using query below)
`docker cp <PROJECT_NAME>_postgresql_1:/deployment/datapoints.csv ./`

## Restart exited containers (without using `docker-compose up`)
If the containers are exited then they can be restarted using the `docker` command, the startup order is important:

* docker start `<PROJECT_NAME>_postgresql_1`
* docker start `<PROJECT_NAME>_keycloak_1`
* docker start `<PROJECT_NAME>_map_1` (only if there is a map container)
* docker start `<PROJECT_NAME>_manager_1`
* docker start `<PROJECT_NAME>_proxy_1`

**NOTE:On Docker v18.02 there is a bug which means you might see the message `Error response from daemon: container "<CONTAINER_ID>": already exists` to resolve this simply enter the following command (you can ignore any error messages) and try starting again**

`docker-containerd-ctr --address /run/docker/containerd/docker-containerd.sock --namespace moby c rm $(docker ps -aq --no-trunc)`

## Running demo deployment

```
eval $(docker-machine env or-host1)

OR_ADMIN_PASSWORD=******** OR_SETUP_RUN_ON_RESTART=true OR_HOST=demo.openremote.io \
OR_EMAIL_ADMIN=support@openremote.io docker-compose -p demo up --build -d
```

To find out the password:

`docker exec demo_manager_1 env | awk -F= '/ADMIN_PASSWORD/ {print $2}'`


## Enabling bash auto-completion

You might want to install bash auto-completion for Docker commands. On OS X, install:

```
brew install bash-completion
```

Then add this to your `$HOME/.profile`:

```
if [ -f $(brew --prefix)/etc/bash_completion ]; then
. $(brew --prefix)/etc/bash_completion
fi
```

And link the completion-scripts from your local Docker install:

```
find /Applications/Docker.app \
-type f -name "*.bash-completion" \
-exec ln -s "{}" "$(brew --prefix)/etc/bash_completion.d/" \;
```
Start a new shell or source your profile to enable auto-completion.

## Cleaning up images, containers, and volumes

Working with Docker might leave exited containers and untagged images. If you build a new image with the same tag as an existing image, the old image will not be deleted but simply untagged. If you stop a container, it will not be automatically removed. The following bash function can be used to clean up untagged images and stopped containers, add it to your `$HOME/.profile`:

```
function dcleanup(){
    docker rm -v $(docker ps --filter status=exited -q 2>/dev/null) 2>/dev/null
    docker rmi $(docker images --filter dangling=true -q 2>/dev/null) 2>/dev/null
}
```

To remove data volumes no longer referenced by a container (deleting ALL persistent data!), use:

```
docker volume prune
```

## Disconnecting from a remote engine

Once Docker Community edition is installed then you will be connected to your local Docker engine; if you have used docker-machine to connect to a remote engine then you can `disconnect` from that remote engine using the command:

```
docker-machine env -u
```


# Queries

## Data points
### Export all asset data points
```
copy asset_datapoint to '/var/lib/postgresql/datapoints.csv' delimiter ',' CSV;
```

or with asset names instead of IDs and a header row in the export:
```
copy (select ad.timestamp, a.name, ad.attribute_name, ad.value from asset_datapoint ad, asset a where ad.entity_id = a.id) to '/var/lib/postgresql/datapoints.csv' delimiter ',' CSV HEADER;
```

### Export a subset of asset data points (asset and direct child assets)
```
copy (select ad.timestamp, a.name, ad.attribute_name, value from asset_datapoint ad, asset a where ad.entity_id = a.id and (ad.entity_id = 'ID1' or a.parent_id = 'ID1')) to '/var/lib/postgresql/datapoints.csv' delimiter ',' CSV HEADER;
```

### Delete all asset datapoints older than N days
`delete from asset_datapoint where timestamp < extract('epoch' from (current_timestamp - interval '5 days'))::bigint*1000;`

### Importing asset data points
To import data points stored in multiple exports (with potential duplicates) then first create a temp table without any key constraints:
```
create table DATATEMP (
  TIMESTAMP      timestamp    not null,
  ENTITY_ID      varchar(36)  not null,
  ATTRIBUTE_NAME varchar(255) not null,
  VALUE          jsonb        not null
)
```

Import the CSV files into this temp table:
`COPY DATATEMP FROM '/var/lib/postgresql/datapoints.csv' DELIMITER ',' CSV;`

Select distinct rows and insert into `ASSET_DATAPOINT` table:
```
INSERT INTO ASSET_DATAPOINT
SELECT DISTINCT ON (timestamp, entity_id, attribute_name) *
FROM DATATEMP
ORDER BY timestamp DESC
```

Drop temp table:
`DROP TABLE DATATEMP CASCADE;`

## Selfsigned SSL
To add the OpenRemote selfsigned certificate to the default java keystore `cacerts`:

1. Convert to `p12`: `openssl pkcs12 -export -in proxy/selfsigned/localhost.pem -out or_selfsigned.p12` (use default password `changeit`
2. Import into default java keystore: `openssl pkcs12 -export -in openremote/proxy/selfsigned/localhost.pem -out or_selfsigned.p12` (Windows will need to execute this in Command Prompt with Admin permissions)

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
