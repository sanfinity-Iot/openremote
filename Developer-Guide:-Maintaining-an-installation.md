## Monitoring

Use `docker stats` to show CPU, memory, network read/writes, and total disk read/writes for running containers.

## Finding problems on running Manager

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

