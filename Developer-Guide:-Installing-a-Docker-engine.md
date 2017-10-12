Install [Docker Community Edition](https://www.docker.com/). Ensure the following commands work:

```
docker run hello-world
docker-machine version
```

## Local Engine
For a local engine (developer workstation setup) simply installing Docker Community Edition is enough.

## Remote Engine
For a remote engine (hosted deployment), you need SSH public key access to a Linux host (preferably CentOS), and then Docker machine can do the rest. Follow the instructions [here](https://docs.docker.com/machine/drivers/generic/).

Fix the generated Docker client credentials configuration files by moving `~/.docker/machine/certs/*` into `~/.docker/machine/machines/<DOCKER MACHINE NAME>/` and fixing the paths in `~/.docker/machine/machines/<DOCKER MACHINE NAME>/config.json`. For Windows you will have to use escaped backslashes e.g. `C:\\Users\Me\\.docker\\machine\\`.

Once the remote engine is installed ensure that `docker-machine ls` shows the new engine and that the State is `Running`.