The ```openremote-cli``` (or ```or``` for short) is a command line tool that can be used for deploying the OpenRemote stack, note that this tool is still in `beta`.

Prerequisites: 

- `python`
- `wget` (available as part of `git BASH` etc. on windows)
- `docker`
- `docker-compose`

### Install
```bash
pip3 install -U openremote-cli
openremote-cli -V
```

There is also docker image provided:

```bash
docker run --rm -ti openremote/openremote-cli <command>
```

Note that the image ENTRYPOINT is set to the openremote-cli command (the same way as amazon/aws-cli docker image) therefore ```docker run --rm -ti openremote/openremote-cli -V``` is equivalent to ```openremote-cli -V```.

### Deploy on localhost

```bash
or deploy --action create
```
using docker
```bash
docker run --rm -ti -v /var/run/docker.sock:/var/run/docker.sock openremote/openremote-cli deploy
```
### Deploy on AWS

Prerequisites:

  - [aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
  - `openremote-cli` AWS profile. If you have Id and AWS secret key, you can use following command:
  ```bash
  or configure_aws --id <id> --secret <secret> -v
  ```
  At the moment the default region must be set to eu-west-1 (Ireland), this is done by the `openremote-cli`. This can be changed using *aws-cli* (not recommended):
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
```