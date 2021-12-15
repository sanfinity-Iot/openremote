Customising your deployment means setting of environment variables and docker volume mappings for the different services, styling the manager UI, configuring the Manager and customising the map. 
In this guide we assume you have followed the [Quickstart](https://github.com/openremote/openremote/blob/master/README.md#quickstart). Optionally you can run the [custom deployment video](https://www.youtube.com/watch?v=_u2IgdioQR8) in parallel while reading this guide.

## Setting environment variables and docker volume mappings for services

The following docker compose file details all of the environment variables (e.g. for e-mail or push notifications) and common volume mappings that you may want to use:

https://github.com/openremote/openremote/blob/master/profile/deploy.yml

## Styling of the Manager UI
It is also possible to do basic customisation of the manager UI using a `JSON` file:

- Create a directory called `deployment` in the same parent directory as the OpenRemote `docker-compose.yml`
- Add logo, logo-mobile and favicon images as files in a directory called `images`
- Create a file called `manager_config.json` and add the following content:

```json
{
  "realms": {
    "default": {
      "appTitle": "ACME IoT",
      "styles": ":host > * {--or-app-color2: #F9F9F9; --or-app-color3: #22211f; --or-app-color4: #1b5630; --or-app-color5: #CCCCCC;}",
      "logo": "/images/logo.png",
      "logoMobile": "/images/logo-mobile.png",
      "favicon": "/images/favicon.png"
    }
  }
}
```

<details><summary>Click to view how the --or-app-colors are used in the demo deployment.</summary>

![Default colors in OpenRemote](https://openremote.io/wp-content/uploads/2021/04/coloruse-04.jpg)

</details>
- Modify the `docker-compose.yml` to add a volume mapping to the `manager` service:

```yaml
version: '2.4'

services:

  proxy:
    image: openremote/proxy:${DATE_TAG:-latest}
    depends_on:
      manager:
        condition: service_healthy
    ports:
      - "80:80"
      - "443:443"
      - "8883:8883"

  manager:
    image: openremote/manager:${DATE_TAG:-latest}
    depends_on:
      keycloak:
        condition: service_healthy
    volumes:
      - ./deployment:/deployment/manager/app

  keycloak:
    image: openremote/keycloak:${DATE_TAG:-latest}
    depends_on:
      postgresql:
        condition: service_healthy

  postgresql:
    image: openremote/postgresql:${DATE_TAG:-latest}
```

- Start/Restart the stack:
```
docker-compose up
```
Now when you load the Manager UI you will see customised colors, logo and title. 

## Configuration of the Manager UI
With the `manager_config.json` you can also configure pages, such as excluding Asset Types from the Add asset modal or from the Rules page, changing the layout of the Insights page, and hiding attributes on the Assets page. For more details view ['Configuring the Manager UI'](https://github.com/openremote/openremote/wiki/User-Guide:-Configuring-the-Manager-UI).

<details><summary>Click to view a reference manager_config with some more customisation functionality</summary>
<p>

```json
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

## Customising the map
You can set your own map and its styling by adding them to the deployment directory. Read more about setting up your map: [[Working on maps|Developer Guide: Working on maps]]. Once you have the map file, you can download and customize [mapsettings.json](https://github.com/openremote/openremote/blob/master/manager/src/map/mapsettings.json) to adjust the centerpoint, boundaries, zoomlevel and styling. If you want to fully customize styling, you can use [Mapbox Studio](https://www.mapbox.com/mapbox-studio) to create your style and copy it into mapsettings.
- Download the map and mapsettings
- Adjust the filestructure of `deployment` as follows:
```
deployment
|-- manager
|    |-- app
|    |    +-- images
|    |    |-- manager_config.json
|-- map
|    |-- mapdata.mbtiles
|    |-- mapsettings.json
```
- Change the volume mapping of the manager container to:
```yaml
    volumes:
      - ./deployment:/deployment
```

## Custom domain
If you want to deploy the OpenRemote stack on a custom domain then all that is needed is to ensure that the docker host where the stack is running is reachable using the custom domain name on the following ports:

- `80` HTTP (needed for SSL generation)
- `443` HTTPS
- `8883` MQTT

The `proxy` service uses `letsencrypt` to auto generate the SSL certificate for the domain and it will also auto renew the certificates; if you already have an SSL certificate for the domain then this can be volume mapped into the `proxy` service.


## AWS

Deploy the stack using docker on Mac/Linux:
```bash
docker run --rm -ti -v ~/.aws:/root/.aws openremote/openremote-cli deploy --provider aws -v --dnsname test-osx.mvp.openremote.io
```
Deploy the stack using docker on Windows:
```bash
docker run --rm -ti -v %userprofile%\.aws:/root/.aws openremote/openremote-cli deploy --provider aws -v --dnsname test-win.mvp.openremote.io
```

This procedure was tested on AWS t4g EC2 instance using [CloudFormation template](https://github.com/openremote/openremote/gitlab-ci/aws-cloudformation.template.arm64.yml).

**Tip:** [AWS CloudFormation template](https://github.com/openremote/openremote/gitlab-ci/aws-cloudformation.template.arm64.yml) Resources.EC2Instance.Properties.UserData section contains installation script for AWS Linux ARM device.

# See Also

- [Installing Docker (incl. SBCs as RaspeberryPi or Odroid)](https://github.com/openremote/openremote/wiki/Developer-Guide%3A-Installing-and-using-Docker)