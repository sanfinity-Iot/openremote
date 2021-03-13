
```openremote-cli``` (short ```or```) is a command line tool which can be used for installing an instance of OpenRemote stack on local machine. In this method you won't have to pull the openremote repository. You should already have ```python```, ```wget```, ```docker``` and ```docker-compose``` installed. Note that this method is in beta.

```bash
pip3 install -U openremote-cli
openremote-cli -V
```
There is also docker image provided:
```bash
docker run --rm -ti openremote/openremote-cli <command>
```
Note that the image ENTRYPOINT is set to the openremote-cli command (the same way as amazon/aws-cli docker image) therefore ```docker run --rm -ti openremote/openremote-cli -V``` is equivalent to ```openremote-cli -V```.

#### Deploy on localhost

```bash
or deploy --action create
```
using docker
```bash
docker run --rm -ti -v /var/run/docker.sock:/var/run/docker.sock openremote/openremote-cli deploy
```
#### Deploy on AWS

Prerequisites:
  - [aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
  - `openremote-cli` AWS profile. If you have Id and AWS secret key, you can use following command:
  ```bash
  or configure_aws --id <id> --secret <secret> -v
  ```
  At the moment the default region must be set to eu-west-1 (Ireland), this is done by the OpenRemote-cli. This can be could be changed using *aws-cli* (not recommended):
  ```bash
  aws configure --profile=openremote-cli
  ```
  
Deploy the stack:
```bash
or deploy --provider aws --dnsname test.mvp.openremote.io -v
```
Remove the stack and clean resources:
```bash
or deploy -a remove --provider aws --dnsname test.mvp.openremote.io -v