### NOTE: For now it isn't possible to access this API without using the Keycloak javascript 

We have a generic Asset and Attribute model which you can access via our API's. 

# Get Asset/Attribute/MetaItem information
To know which Asset, Attribute and MetaItem types are available in a certain realm, we can do a GET on the following endpoints.

## Asset Descriptors

To know which type of Assets are available in a certain realm, we do a GET on the following endpoint:

`https://<your-domain>/api/<realm>/model/asset/descriptors`

Which returns for e.g.:

```
[
  {
    "icon": "cube",
    "accessPublicRead": false,
    "name": "CUSTOM"
  },
  {
    "type": "urn:openremote:asset:building",
    "icon": "building",
    "accessPublicRead": false,
    "attributeDescriptors": [
      {
        "attributeName": "surfaceArea",
        "valueDescriptor": {
          "icon": "numeric",
          "valueType": "NUMBER",
          "name": "NUMBER"
        },
        "metaItemDescriptors": [
          {
            "urn": "urn:openremote:asset:meta:label",
            "valueType": "STRING",
         ...
]
```

## Attribute type descriptors

If you woud like to know which attribute types are available you can do a GET on:
`https://<your-domain>/api/<realm>/model/attribute/typeDescriptors`

Which would return for e.g.:
```
[
  {
    "attributeName": "consoleName",
    "valueDescriptor": {
      "icon": "format-text",
      "valueType": "STRING",
      "name": "STRING"
    }
  },
  {
    "attributeName": "city",
    "valueDescriptor": {
      "icon": "format-text",
      "valueType": "STRING",
      "name": "STRING"
    },
    "metaItemDescriptors": [
      {
        "urn": "urn:openremote:asset:meta:label",
        "valueType": "STRING",
        "access": {
          "restrictedRead": false,
          "restrictedWrite": false,
          "publicRead": false
        },
        "required": false,
        "maxPerAttribute": 1,
        "initialValue": "City",
        "valueFixed": false
      },
     ...
]
```

## Value descriptors

It's also necessary to know which value types are available by doing a GET on:

`https://<your-domain>/api/<realm>/model/attribute/valueDescriptors`

Which gives us something like:
```
[
  {
    "icon": "format-text",
    "valueType": "STRING",
    "name": "STRING"
  },
  {
    "icon": "numeric",
    "valueType": "NUMBER",
    "name": "NUMBER"
  },
  {
    "icon": "toggle-switch",
    "valueType": "BOOLEAN",
    "name": "BOOLEAN"
  },
  ...
]
```

## MetaItem descriptors

And of course it's good to know which MetaItem types are part of the realm. We can get the information by doing a GET on:

`https://<your-domain>/api/<realm>/model/metaItem/descriptors`

Which will give us back:

```
[
  {
    "urn": "urn:openremote:asset:meta:protocolConfiguration",
    "access": {
      "restrictedRead": false,
      "restrictedWrite": false,
      "publicRead": false
    },
    "valueType": "BOOLEAN",
    "initialValue": true,
    "valueFixed": true,
    "maxPerAttribute": 1,
    "required": false
  },
  {
    "urn": "urn:openremote:asset:meta:agentLink",
    "access": {
      "restrictedRead": false,
      "restrictedWrite": false,
      "publicRead": false
    },
    "valueType": "ARRAY",
    "valueFixed": false,
    "maxPerAttribute": 1,
    "required": false,
    "validator": {}
  },
  ...
]
```

# Retrieve Assets

Before you start make sure you've got an access token from Keycloak by singing in. This access token should be passed which each request as an `Authorization` header prefixed with `Bearer `.

## Query Assets

To get assets of a certain type or a specific attribute value, you can do a POST request on the following endpoint:
`https://<your-domain>/api/<realm>/asset/query`

To for instance get all assets of asset type flight, we would POST the following JSON:

```
{
    "select":{"include":"ALL_EXCEPT_PATH"},
    "type":{"predicateType":"string","value":"urn:openremote:asset:flight"}
}
```
This would return all assets matching that type.

To see more query options, please refer to [BaseAssetQuery.java](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/query/BaseAssetQuery.java)

## Query Public Assets

It could be you've created assets which are public, by specifying `MetaItemType.ACCESS_PUBLIC_READ` with value `true`. These assets can be retrieved by doing a POST on the following endpoint:
`https://<your-domain>/api/<realm>/asset/public/query`

Just like Query Assets you POST a body based on [BaseAssetQuery.java](https://github.com/openremote/openremote/blob/master/model/src/main/java/org/openremote/model/query/BaseAssetQuery.java).

**No authentication is needed for this request.**

## Create an Asset

To create an asset, the user should have the `write:assets` role. Create one by doing a POST on:

`https://<your-domain>/api/<realm>/asset`

With for e.g. the following JSON body:

*TBF*

# See also

- [[The Asset model and API|Architecture:-The Asset model and API]]