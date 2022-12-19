Sometimes it is desriable to bulk edit existing assets (add/remove attributes and/or configuration items) rather than wiping the DB and starting again.

DB migration scripts can be used to perform these migrations but also raw SQL can be used to make such alterations.

There are several DB functions included in the system to help with this task:

## DB Functions
The DB functions and their arguments can be found in the code at:

https://github.com/openremote/openremote/blob/master/manager/src/main/resources/org/openremote/manager/setup/database/V20221219_HelperFunctions.sql

## Examples

### Add/Update meta items to specific attribute of asset(s)
SELECT a.id, ADD_UPDATE_META(a, 'newAttribute1', jsonb_build_object('meta1', false, 'meta2', 456, 'meta3', 'Some text')) from asset a where...;

### Remove meta items from specific attribute of asset(s)
SELECT a.id, REMOVE_META(a, 'newAttribute1', 'meta2', 'meta3') from asset a where...;

### Remove attributes from specific asset(s)
SELECT a.id, REMOVE_ATTRIBUTES(a, 'oldAttribute1', 'oldAttribute2') from asset a where...;

### Add attribute to specific asset(s) (with meta items)
SELECT a.id, ADD_ATTRIBUTE(a, 'newAttribute2', 'GEO_JSONPoint', '1'::jsonb, now(), jsonb_build_object('meta1', true, 'meta2', 123)) from asset a where...;

### Add attribute to specific asset(s) (without meta items)
SELECT a.id, ADD_ATTRIBUTE(a, 'newAttribute1', 'GEO_JSONPoint', '1'::jsonb, now(), null) from asset a where...;