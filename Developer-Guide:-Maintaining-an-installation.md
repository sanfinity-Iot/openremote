## Monitoring

Tail log output of all containers with `docker-compose -p <Project Name> -f <Your docker-compose.yml> logs -f`.

Use `docker stats` to show CPU, memory, network read/writes, and total disk read/writes for running containers.

Use [jstat](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jstat.html) to monitor a running system/JVM. The following command will connect to a Manager and print garbage collection statistics (polling 1000 times, waiting for 2.5 seconds):

```
docker exec -it <Manager Container ID/Name> /usr/bin/jstat -gcutil 1 2500 1000
```

The output contains:

```
S0: Survivor space 0 utilization as a percentage of the space's current capacity.
S1: Survivor space 1 utilization as a percentage of the space's current capacity.
E: Eden space utilization as a percentage of the space's current capacity.
O: Old space utilization as a percentage of the space's current capacity.
M: Metaspace utilization as a percentage of the space's current capacity.
CCS: Compressed class space utilization as a percentage.
YGC: Number of young generation GC events.
YGCT: Young generation garbage collection time.
FGC: Number of full GC events.
FGCT: Full garbage collection time.
GCT: Total garbage collection time.
```

## Restarting a Manager with new settings

To get more details or set JVM options, edit your `docker-compose.yml` and change or add an `entrypoint` section:

```
  manager:
    entrypoint:
      - "java"
      - "-Xmx1024m"
      - "-XX:+PrintGCDetails"
      - "-cp"
      - "/opt/app/lib/*"
      - "org.openremote.manager.server.Main"
```

Then restart the service with:

```
SERVICE=manager && PROJECT=<Your project name> && PROFILE=<Your docker-compose.yml> && \
  docker-compose -p $PROJECT -f $PROFILE stop $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE rm -f $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE create $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE start $SERVICE 
```
