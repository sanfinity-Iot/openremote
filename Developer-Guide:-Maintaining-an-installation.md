## Monitoring

Tail log output of all containers with `docker-compose -p openremote -f profile/demo.yml logs -f`.

Use `docker stats` to show CPU, memory, network read/writes, and total disk read/writes for running containers.

## Accessing the database

Run:

```
docker exec -it openremote_postgresql_1 psql -U openremote
```

## Diagnosing JVM memory problems

Use [jstat](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/jstat.html) to monitor a running system/JVM. 

Get current memory configuration:

```
docker exec -it openremote_manager_1 /usr/bin/jstat -gccapacity 1
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

The following command will connect to a Manager and print garbage collection statistics (polling 1000 times, waiting for 2.5 seconds):

```
docker exec -it openremote_manager_1 /usr/bin/jstat -gcutil 1 2500 1000
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
docker exec -it openremote_manager_1 /usr/bin/jcmd 1 GC.run
```

## Working on the JVM with a JMX command-line

Run [jmxterm](http://wiki.cyclopsgroup.org/jmxterm/manual.html):

```
docker exec -it openremote_manager_1 java -cp /opt/app/lib/* org.cyclopsgroup.jmxterm.boot.CliMain

Welcome to JMX terminal. Type "help" for available commands.
$>open 1
#Connection to 1 is opened
$>beans
...
```
