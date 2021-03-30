Customising your deployment can be done through a combination of environment variables and docker volume mappings, the following docker compose file details all of the environment variables and common volume mappings that you may want to use:

https://github.com/openremote/openremote/blob/master/profile/deploy.yml

### Customising the Manager UI
It is also possible to do some basic customisation of the manager UI using a `JSON` file:

- Create a directory called `deployment` in the same parent directory as the OpenRemote `docker-compose.yml`
- Create a file called `manager_config.json` and add the following content:

```json
{
  "realms": {
    "default": {
      "appTitle": "ACME IoT",
      "styles": ":host > * {--or-app-color2: #F9F9F9; --or-app-color3: #22211f; --or-app-color4: #1b5630; --or-app-color5: #CCCCCC;}",
      "logo": "/images/logo.png",
      "logoMobile": "/images/logo-mobile.png",
      "favicon": "/images/favicon.png",
      "language": "en"
    }
  }
}
```

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
Now when you load the Manager UI you will see a customised logo and title (images can be provided as files with paths relative to the `manager_config.json` file). 

With the `manager_config.json` you can also change color styling, configure pages, such as excluding Asset Types from the Add asset modal or from the Rules page, changing the layout of the Insights page, and hiding attributes on the Assets page. For some pointers view the reference below.

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

### Customising the map
You can set your own map and its styling by adding them to the deployment directory. Read more about [[Working on maps|Developer Guide: Working on maps]].

### Custom domain
If you want to deploy the OpenRemote stack on a custom domain then all that is needed is to ensure that the docker host where the stack is running is reachable using the custom domain name on the following ports:

- `80` HTTP (needed for SSL generation)
- `443` HTTPS
- `8883` MQTT

The `proxy` service uses `letsencrypt` to auto generate the SSL certificate for the domain and it will also auto renew the certificates; if you already have an SSL certificate for the domain then this can be volume mapped into the `proxy` service.


### AWS

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