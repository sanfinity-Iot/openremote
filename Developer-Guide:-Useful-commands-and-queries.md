# Commands
## Restart manager docker container
Replace `<PROJECT_NAME>` with value used when creating the container with `docker-compose up` (`docker ps` will show the actual names):

`docker restart <PROJECT_NAME>_manager_1`

# Queries

## Consoles

### Remove consoles that haven't registered for more than N days
**NOTE: Following will require manager to be restarted**

`DELETE FROM asset a WHERE a.asset_type = 'urn:openremote:asset:console' AND to_timestamp((a.attributes#>>'{consoleName, valueTimestamp}')::bigint /1000) < (current_timestamp - interval '30 days');`

## Notifications

### Count notifications with specific title sent to consoles (Android/iOS) in past N days

`select count(*) FROM notification n JOIN asset a ON a.id = n.target_id WHERE n.message ->> 'title' = 'Notification Title' and n.sent_on > (current_timestamp - interval '7 days') and a.attributes #>> '{consolePlatform, value}' LIKE 'Android%' and n.acknowledged_on is not null;`