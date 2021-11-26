The manager_config.json can be used in a [custom deployment](https://github.com/openremote/openremote/wiki/User-Guide%3A-Custom-deployment) to style your deployment and configure how the pages of the Manager displays the assets and attributes in your system.
On this page we will use an example config and give a short description of each section you could include. Note that some pages have a default config that you can overwrite using the manager_config.

**Map:**

You can set the attributes to exclude (or include) on the top right card of the map when an asset is selected. This can be done for all asset types (by using default), or per asset type (as shown for WeatherAsset).
```JSON
{
  "pages": {
    "map": {
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
    },
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
**Insights:** 

The insights page layout and its panel types can be modified.
```JSON
    "insights": {
      "panels": {
        "chart": {
          "type": "chart",
          "hideOnMobile": true,
          "panelStyles": {
            "gridColumn": "1 / -1"
          }
        },
        "kpi1": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "kpi2": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "kpi3": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "kpi4": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "kpi5": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "kpi6": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "kpi7": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "kpi8": {
          "type": "kpi",
          "hideOnMobile": false
        },
        "chart2": {
          "type": "chart",
          "hideOnMobile": true,
          "panelStyles": {
            "gridColumn": "1 / -1"
          }
        },
        "chart3": {
          "type": "chart",
          "hideOnMobile": true,
          "panelStyles": {
            "gridColumn": "1 / -1"
          }
        }
      }
    },
```
**Assets - tree:** The insights page layout and its panel types can be modified.
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
**Assets - viewer:** The insights page layout and its panel types can be modified.
```JSON
      "viewer": {
        "default": {
          "panels": {
            "attributes": {
              "type": "info",
              "attributes": {
                "exclude": ["location", "notes", "manufacturer", "model"]
              },
              "properties": {
                "include": []
              }
            }
          }
        },
        "assetTypes": {
          "WeatherAsset": {
            "panels": {
              "attributes": {
                "type": "info",
                "attributes": {
                  "exclude": ["location", "Radiation", "UVIndex", "currentWeather", "notes", "manufacturer", "model"]
                },
                "properties": {
                  "include": []
                }
              }
            }
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
```
**Realm configuration:** The insights page layout and its panel types can be modified.
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
    }
  }
}
```