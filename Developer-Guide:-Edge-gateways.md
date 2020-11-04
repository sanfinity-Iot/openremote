The Edge gateway stack is currently composed of the standard stack with the exclusion of `keycloak`, the following assumes that `ARM64` hardware is used for running the stack with basic requirements of 4GB RAM and a reasonable powerful processor, the stack can be further optimised as required.

Our pipeline does support docker manifests so all images (AMD64, ARM and ARM64) are tagged with the same label and are build at the same time.

```
openremote/manager:latest
openremote/postgresql:latest
openremote/proxy:latest
```

You will also need a deployment image for map data and automatic provisioning, the OpenRemote demo deployment image can be used or a custom one can be built following the Custom project info in the wiki.

```
openremote/deployment:latest
```

We use the following [GitHub Actions workflow](https://github.com/openremote/openremote/actions/runs/343881895/workflow) to create Docker images. It uses the official Docker QEMU and buildx actions and targets following platforms:

- linux/amd64
- linux/arm64
- linux/arm/v7
- linux/arm/v6

## Edge gateway OS installation steps (Example is for an `ODROID C2`)
1. Download Armbian (https://dl.armbian.com/odroidc2/Buster_current_minimal) and flash to SD card using Etcher
1. Power on and SSH into ODROID then follow Armbian prompts to change root password and Ctrl-C to skip/abort creating a new user
1. Install curl `apt-get install curl`
1. Install docker using convenience script:
   1. `curl -fsSL https://get.docker.com -o get-docker.sh`
   2. `sh get-docker.sh`
1. Check install was successful with `docker --version`
1. Install docker-compose (I generally use docker-compose and docker-machine from my desktop and remotely deploy images to docker engines but the following will install docker-compose directly on the ODROID for simplicity):
   1. `curl -L "https://github.com/ubiquiti/docker-compose-aarch64/releases/download/1.22.0/docker-compose-Linux-aarch64" -o /usr/local/bin/docker-compose`
   1. `chmod +x /usr/local/bin/docker-compose`
1. Check install was successful with `docker-compose --version`
1. Download basic demo docker compose profiles and dependencies:
   1. `curl https://raw.githubusercontent.com/openremote/openremote/master/profile/demo-basic.yml -o demo-basic.yml -L`
   1. `curl https://raw.githubusercontent.com/openremote/openremote/master/profile/deploy.yml -o deploy.yml -L`
1. Modify `demo-basic.yml` and replace service image tag names with `latest-arm64` (only required whilst docker hub images are using manifests)
1. Pull OpenRemote images: `docker-compose -f demo-basic.yml pull` (if docker compose complains about missing build directories just create them using `mkdir -p`)
1. Stop existing stack (if previously deployed): `docker-compose -f sfella.yml -f demo-basic.yml down`
1. Cleanup any dangling volumes and images:
   1. `docker volume prune`
   1. `docker rmi $(docker images -f "dangling=true" -q)`
1. Start the stack (can optionally specify `GATEWAY_CLIENT_ID` and `GATEWAY_CLIENT_SECRET` environment variables to automatically set the `Manager interconnect` settings):
   `docker-compose -f demo-basic.yml up -d --no-build` or with environment variables `GATEWAY_CLIENT_ID=XXXX GATEWAY_CLIENT_SECRET=YYYY docker-compose -f demo-basic.yml up -d --no-build` 
   
1. You should now be able to access the OpenRemote manager:
   1. OpenRemote Manager = https://IPADDRESS/
