The settings option 'Appearance' can be used in a [custom deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment). You can use it to style your deployment and configure how the pages of the Manager display the assets and attributes in your system. Some of these settings can be done directly in the user interface, others can be set in the JSON editor. The configuration get's stored in the manager_config file.

In this user guide we will use an example JSON manager_config and give a short description of each section you could include. Note that some pages have a default config that you can overwrite using the manager_config.

**Note:** if you are logged in as the default 'admin' user (a super user) most of these styling changes will _not_ be shown. This is to make sure the 'admin' user has all functionality available to them.  
**Make sure to log in as a different user to see all effects of the manager_config.** 

***

<details><summary>View the example manager_config used in the sections on this page.</summary>
<p>

```json
{
  "pages": {
    "map": {
      "card": {
        "default": {
          "exclude": [
            "notes"
          ]
        },
        "assetTypes": {
          "WeatherAsset": {
            "exclude": [
              "location",
              "notes",
              "model",
              "manufacturer"
            ]
          }
        }
      }
    },
    "rules": {
      "rules": {
        "controls": {
          "allowedLanguages": ["JSON", "FLOW", "GROOVY"],
          "allowedActionTargetTypes": {
            "actions": {
              "email": [
                "CUSTOM", "USER"
              ],
              "push": [
                "ASSET", "USER"
              ]
            }
          }
        },
        "descriptors": {
          "all": {
            "excludeAssets": [
              "TradfriLightAsset",
              "TradfriPlugAsset",
              "ArtnetLightAsset"
            ],
            "assets": {
              "*": {
                "excludeAttributes": [
                  "location"
                ]
              }
            }
          }
        }
      }
    },
    "assets": {
      "tree": {
        "add": {
          "typesParent": {
            "default": {
              "exclude": [
                "TradfriLightAsset",
                "TradfriPlugAsset",
                "ArtnetLightAsset",
                "ArtnetAgent",
                "MacroAgent",
                "ControllerAgent",
                "KNXAgent",
                "ZWAgent",
                "TradfriAgent",
                "TimerAgent",
                "VelbusTcpAgent",
                "VelbusSerialAgent"
              ]
            }
          }
        }
      },
      "viewer": {
        "assetTypes": {
          "WeatherAsset": {
            "viewerStyles": {},
            "panels": [
              {
                "type": "group",
                "title": "underlyingAssets"
              },
              {
                "type": "info",
                "title": "location",
                "properties": {
                  "include": []
                },
                "attributes": {
                  "include": [
                    "location"
                  ],
                  "itemConfig": {
                    "location": {
                      "label": ""
                    }
                  }
                }
              },
              {
                "type": "info",
                "hideOnMobile": true,
                "properties": {
                  "include": []
                },
                "attributes": {
                  "include": [
                    "notes",
                    "manufacturer",
                    "model"
                  ]
                }
              },
              {
                "type": "info",
                "title": "Weather data",
                "attributes": {
                  "exclude": [
                    "location",
                    "radiation",
                    "rainfall",
                    "uVIndex",
                    "currentWeather",
                    "notes",
                    "manufacturer",
                    "model"
                  ]
                },
                "properties": {
                  "include": []
                }
              },
              {
                "type": "info",
                "title": "Extra details",
                "column": 1,
                "properties": {
                  "include": []
                },
                "attributes": {
                  "include": [
                    "rainfall",
                    "uVIndex"
                  ]
                }
              },
              {
                "type": "history",
                "column": 1
              },
              {
                "type": "linkedUsers",
                "column": 1
              }
            ]
          }
        },
        "historyConfig": {
          "table": {
            "attributeNames": {
              "optimiseTarget": {
                "columns": [
                  {
                    "header": "Optimise target",
                    "type": "prop",
                    "path": "$."
                  },
                  {
                    "header": "Timestamp",
                    "type": "timestamp"
                  }
                ]
              }
            }
          }
        }
      }
    }
  },
  "realms": {
    "default": {
      "appTitle": "ACME IoT",
      "headers": [
        "map", "assets", "rules", "insights", "language", "users", "roles", "account", "logs", "logout"
      ],
      "styles": ":host > * {--or-app-color2: #F9F9F9; --or-app-color3: #22211f; --or-app-color4: #1b5630; --or-app-color5: #CCCCCC;}",
      "logo": "/images/logo.png",
      "logoMobile": "/images/logo-mobile.png",
      "favicon": "/images/favicon.png",
      "language": "en"
    }
  }
}
```

</p>
</details>

**Map - Card:**
You can set the attributes to exclude (or include) on the top right card of the map when an asset is selected. This can be done for all asset types (by using default), or per asset type (as shown for WeatherAsset).
```JSON
{
  "pages": {
    "map": {
      "card": {
        "default": {
          "exclude": [
            "notes"
          ]
        },
        "assetTypes": {
          "WeatherAsset": {
            "exclude": [
              "location",
              "notes",
              "model",
              "manufacturer"
            ]
          }
        }
      }
    },
```
**Map - Marker config:**
This configures how the markers behave. They can either change their colour based on an attribute value (number, boolean, or string), show a label with or without units, and/or show the direction an asset is facing. Note that this part of the config is not in the manager_config used in the manager demo yet.
```JSON
"markers": {
  "ElectricityProducerSolarAsset": {
    "attributeName": "energyExportTotal",
    "showLabel": true,
    "showUnits": true,
    "colours": {
      "type": "range",
      "ranges": [
        {
          "max": 50,
          "colour": "39B54A"
        },
        {
          "max": 80,
          "colour": "F7931E"
        },
        {
          "max": 100,
          "colour": "C1272D"
        }
      ]
    }
  }
}

```
**Rules - Controls:** 
Set which types of rules are available (for users with the correct permissions), and which actions a rule can perform. 
```JSON
    "rules": {
      "rules": {
        "controls": {
          "allowedLanguages": ["JSON", "FLOW", "GROOVY"],
          "allowedActionTargetTypes": {
            "actions": {
              "email": [
                "CUSTOM", "USER"
              ],
              "push": [
                "ASSET", "USER"
              ]
            }
          }
        },
```
**Rules - When-Then:** 
Set which assettypes are excluded from the list of asset types that can be selected in the When-Then rule. Additionally you can set per asset (or all '*') which attributes should be excluded from the select list.
```JSON
        "descriptors": {
          "all": {
            "excludeAssets": [
              "TradfriLightAsset",
              "TradfriPlugAsset",
              "ArtnetLightAsset"
            ],
            "assets": {
              "*": {
                "excludeAttributes": [
                  "location"
                ]
              }
            }
          }
        }
      }
    },
```
**Assets - tree:** Exclude asset types from the 'Add asset' dialog.
```JSON
    "assets": {
      "tree": {
        "add": {
          "typesParent": {
            "default": {
              "exclude": [
                "TradfriLightAsset",
                "TradfriPlugAsset",
                "ArtnetLightAsset",
                "ArtnetAgent",
                "MacroAgent",
                "ControllerAgent",
                "KNXAgent",
                "ZWAgent",
                "TradfriAgent",
                "TimerAgent",
                "VelbusTcpAgent",
                "VelbusSerialAgent"
              ]
            }
          }
        }
      },
```
**Assets - viewer:** Configure which panels are shown on the assets page. You can include or exclude attributes to shown per panel. These panels can be set for all asset types, or specified per type. This is an overwrite of the default config of the [asset-viewer](https://github.com/openremote/openremote/blob/master/ui/component/or-asset-viewer/src/index.ts). In `historyConfig` an example is given on how to specify the columns shown in a table for an attribute that is not a number or boolean; if no config is given, it will automatically create columns.
```JSON
      "viewer": {
        "assetTypes": {
          "WeatherAsset": {
            "viewerStyles": {},
            "panels": [
              {
                "type": "group",
                "title": "underlyingAssets"
              },
              {
                "type": "info",
                "title": "location",
                "properties": {
                  "include": []
                },
                "attributes": {
                  "include": [
                    "location"
                  ],
                  "itemConfig": {
                    "location": {
                      "label": ""
                    }
                  }
                }
              },
              {
                "type": "info",
                "hideOnMobile": true,
                "properties": {
                  "include": []
                },
                "attributes": {
                  "include": [
                    "notes",
                    "manufacturer",
                    "model"
                  ]
                }
              },
              {
                "type": "info",
                "title": "Weather data",
                "attributes": {
                  "exclude": [
                    "location",
                    "radiation",
                    "rainfall",
                    "uVIndex",
                    "currentWeather",
                    "notes",
                    "manufacturer",
                    "model"
                  ]
                },
                "properties": {
                  "include": []
                }
              },
              {
                "type": "info",
                "title": "Extra details",
                "column": 1,
                "properties": {
                  "include": []
                },
                "attributes": {
                  "include": [
                    "rainfall",
                    "uVIndex"
                  ]
                }
              },
              {
                "type": "history",
                "column": 1
              },
              {
                "type": "linkedUsers",
                "column": 1
              }
            ]
          }
        },
        "historyConfig": {
          "table": {
            "attributeNames": {
              "optimiseTarget": {
                "columns": [
                  {
                    "header": "Optimise target",
                    "type": "prop",
                    "path": "$."
                  },
                  {
                    "header": "Timestamp",
                    "type": "timestamp"
                  }
                ]
              }
            }
          }
        }
      }
```
**Realm configuration:** You can set the branding per realm. In the example below you can see how the page title, headers, colors, and logo's are set as default (for any new realm created through the UI), as well as for the 'master' and 'clienta' realms.
```JSON
  "realms": {
    "default": {
      "appTitle": "ACME IoT",
      "headers": [
        "map", "assets", "rules", "insights", "language", "users", "roles", "account", "logs", "logout"
      ],
      "styles": ":host > * {--or-app-color2: #F9F9F9; --or-app-color3: #22211f; --or-app-color4: #1b5630; --or-app-color5: #CCCCCC;}",
      "logo": "/images/logo.png",
      "logoMobile": "/images/logo-mobile.png",
      "favicon": "/images/favicon.png",
      "language": "en"
    },
    "master": {
      "appTitle": "ACME Master",
      "styles": ":host > * {--or-app-color2: #F9F9F9; --or-app-color3: #22211f; --or-app-color4: #275582; --or-app-color5: #CCCCCC;}",
      "logo": "/images/logo.png",
      "logoMobile": "/images/logo-mobile.png",
      "favicon": "/images/logo-favicon.png",
      "language": "en"
    },
    "clienta": {
      "appTitle": "Client A",
      "styles": ":host > * {--or-app-color2: #F9F9F9; --or-app-color3: #22211f; --or-app-color4: #275582; --or-app-color5: #CCCCCC;}",
      "logo": "/images/clienta-logo.png",
      "logoMobile": "/images/clienta-logoMobile.png",
      "favicon": "/images/clienta-favicon.png",
      "language": "de"
    }
  }
}
```
This is what the --or-app-colors look like in the demo deployment:
![Default colors in OpenRemote](https://openremote.io/wp-content/uploads/2021/04/coloruse-04.jpg)
