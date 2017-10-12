# Local engine
Once Docker Community edition is installed then you will be connected to your local Docker engine; if you have used docker-machine to connect to a remote engine then you can `disconnect` from that remote engine using the command:

```
docker-machine env -u
```

# Remote engine
To connect to a remote engine via SSH (generic Docker machine driver) you need to ensure the Docker engine's SSH keys are available locally in:

`~/.docker/machine/machines/<DOCKER MACHINE NAME>/`

If you installed the remote docker engine on your local machine then the certificates should already be in place; otherwise you will need to obtain the certificates which is outside the scope of this document.

Assuming the Docker engine SSH keys are available locally as described above then ensure that `docker-machine ls` shows the remote engine you want to connect to and its status is `Ready`.

To connect to the engine:

```
docker-machine ls
eval $(docker-machine env <DOCKER MACHINE NAME>)
docker ps
```