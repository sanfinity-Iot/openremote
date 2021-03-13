Customising your deployment can be done through a combination of environment variables and docker volume mappings, the following docker compose file details all of the environment variables and common volume mappings that you may want to use:

https://github.com/openremote/openremote/blob/master/profile/deploy.yml

### Customising the Manager UI
It is possible to do some basic customisation of the manager UI using a `JSON` file:

- Create a directory called `deployment` in the same parent directory as the OpenRemote `docker-compose.yml`
- Create a file called `manager_config.json` and add the following content:

```json
{
  "realms": {
	"default": {
		"appTitle": "ACME IOT",
		"logo": "data:image/png;base64,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JPtQ6aBgNJZlSJ5LOPIECfHxKG0QFxERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERERGRGdjZ+X9DreN6TgXDawAAAABJRU5ErkJggg=="		
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
Now when you load the Manager UI you will see a customised logo and title (images can be provided as files with paths relative to the `manager_config.json` file.


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