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

Force garbage collection with: 

```
docker exec -it <Manager Container ID/Name> /usr/bin/jcmd 1 GC.run
```

Get current memory configuration:

```
docker exec -it <Manager Container ID/Name> /usr/bin/jstat -gccapacity 1
```

Output contains:

```
NGCMN: Minimum new generation capacity (kB).
NGCMX: Maximum new generation capacity (kB).
NGC: Current new generation capacity (kB).
S0C: Current survivor space 0 capacity (kB).
S1C: Current survivor space 1 capacity (kB).
EC: Current eden space capacity (kB).
OGCMN: Minimum old generation capacity (kB).
OGCMX: Maximum old generation capacity (kB).
OGC: Current old generation capacity (kB).
OC: Current old space capacity (kB).
MCMN: Minimum metaspace capacity (kB).
MCMX: Maximum metaspace capacity (kB).
MC: Metaspace capacity (kB).
CCSMN: Compressed class space minimum capacity (kB).
CCSMX: Compressed class space maximum capacity (kB).
CCSC: Compressed class space capacity (kB).
YGC: Number of young generation GC events.
FGC: Number of full GC events.
```

## Restarting a Manager with new settings

Increase the log detail by adjusting the levels; first copy [logging.properties](https://raw.githubusercontent.com/openremote/openremote/master/deployment/manager/logging.properties) onto your Docker host (e.g. to `/data/manager/`) and then edit the file. 

Edit your `docker-compose.yml`:

```
  manager:
    environment:

      # Optional: Override built-in logging.properties with a file of your choice.
      # LOGGING_CONFIG_FILE: /my/manager/logging.properties

    # Override entrypoint for custom JVM options
    # entrypoint:
    #  - "java"
    #  - "-Xmx1024m"
    #  - "-XX:+PrintGCDetails"
    #  - "-cp"
    #  - "/opt/app/lib/*"
    #  - "org.openremote.manager.server.Main"
```

See [demo.yml](https://github.com/openremote/openremote/blob/master/profile/demo.yml) for all options.

After changing any configuration, restart the service with:

```
SERVICE=manager && PROJECT=<Your project name> && PROFILE=<Your docker-compose.yml> && \
  docker-compose -p $PROJECT -f $PROFILE stop $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE rm -f $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE create $SERVICE && \
  docker-compose -p $PROJECT -f $PROFILE start $SERVICE 
```

Make sure the `SETUP_*` variables are configured as needed, you may wipe your database if this is wrong!

