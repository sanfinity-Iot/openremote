Generic protocols whilst being very flexible require more configuration than targeted protocols due to their nature; below is some general information that relates to generic protocols and support/functionality will depend on individual protocol implementations.

## Dynamic values
When writing to linked attributes it can be desirable to use insert the written value into a bigger payload before sending to the protocol; the dynamic value placeholder `{$value}` makes this possible and every occurrence within the bigger payload is replaced by the value written to the linked attribute.


## Response value filtering
When a generic protocol returns a payload it can be desirable to filter that payload to extract a specific piece of information that should actually be written to the linked attribute; that is the purpose of the `Value filters` configuration item for protocol linked attributes, it takes an array of [Value filters]() as serialised `JSON Objects` e.g.:

```
[
    {
        "type": "json",
        "path": [
             "response",
             "body"
        ]
    },
    {
        "type": "regex",
        "pattern": ".*(\\d)$",
        "matchGroup": 1
    }
]
```

The payload returned from the server is passed through each filter in the order specified in the `JSON Array`.