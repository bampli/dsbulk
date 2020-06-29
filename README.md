# dsbulk

DataStax Bulk Loader merged with DataStax graph-book in a single image. It expects a DSE server running with an external network named 'graph'. Check [DSE setup](https://github.com/bampli/bampli/tree/master/datastax).

## DataStax Bulk Loader

```console
docker run -it --network graph josemottalopes/dsbulk:1.5.0

cd /home/graph-book/data/ch6

dsbulk load -h 172.18.0.2 -k trees_dev -t Sensor -url Sensor.csv
dsbulk load -h 172.18.0.2 -k trees_dev -t Tower -url Tower.csv
dsbulk load -h 172.18.0.2 -k trees_dev -t Sensor__send__Sensor -url Sensor__send__Sensor.csv
dsbulk load -h 172.18.0.2 -k trees_dev -t Sensor__send__Tower -url Sensor__send__Tower.csv
```

### Example with trees_dev

```console
$ docker run -it --network graph josemottalopes/dsbulk:1.5.0
dsbulk@ad8fad005755:~$ cd /home/graph-book/data/ch6
dsbulk@ad8fad005755:/home/graph-book/data/ch6$ dsbulk load -h 172.18.0.2 -k trees_dev -t Sensor -url Sensor.csv
Operation directory: /home/graph-book/data/ch6/logs/LOAD_20200623-225035-213056
Provided keyspace is a graph; instead of schema.keyspace and schema.table, please use graph-specific options such as schema.graph, schema.vertex, schema.edge, schema.from and schema.to.
total | failed | vertices/s | p50ms |  p99ms | p999ms | batches
  279 |      0 |        188 | 42.02 | 187.70 | 228.59 |    1.00
Operation LOAD_20200623-225035-213056 completed successfully in 1 seconds.
Last processed positions can be found in positions.txt
```

## Graph-specific Bulk Loader

```console
docker run -it --network graph josemottalopes/dsbulk:1.5.0
cd /home/graph-book/data/ch7

dsbulk load -h 172.18.0.2 -g trees_prod -v Sensor -url Sensor.csv -header true
dsbulk load -h 172.18.0.2 -g trees_prod -v Tower -url Tower.csv -header true
dsbulk load -h 172.18.0.2 -g trees_prod -e send -from Sensor -to Sensor -url Sensor__send__Sensor.csv -header true
dsbulk load -h 172.18.0.2 -g trees_prod -e send -from Sensor -to Tower -url Sensor__send__Tower.csv -header true
```

### Example with trees_prod

```console
$ docker inspect seed | FINDSTR "IPAddress"'
            "SecondaryIPAddresses": null,
            "IPAddress": "",
                    "IPAddress": "172.18.0.2",
$ docker run -it --network graph josemottalopes/dsbulk:1.5.0
dsbulk@9f089e1d1717:~$ cd /home/graph-book/data/ch7
dsbulk@9f089e1d1717:/home/graph-book/data/ch7$ dsbulk load -h 172.18.0.2 -g trees_prod -v Sensor -url Sensor.csv -header true
Operation directory: /home/graph-book/data/ch7/logs/LOAD_20200624-161413-408496
total | failed | vertices/s | p50ms |  p99ms | p999ms | batches
  279 |      0 |        182 | 18.19 | 134.22 | 147.85 |    1.00
Operation LOAD_20200624-161413-408496 completed successfully in 1 seconds.
Last processed positions can be found in positions.txt
dsbulk@9f089e1d1717:/home/graph-book/data/ch7$ dsbulk load -h 172.18.0.2 -g trees_prod -v Tower -url Tower.csv -header true
Operation directory: /home/graph-book/data/ch7/logs/LOAD_20200624-161436-758491
total | failed | vertices/s | p50ms | p99ms | p999ms | batches
   80 |      0 |         70 | 22.09 | 95.94 |  95.94 |    1.00
Operation LOAD_20200624-161436-758491 completed successfully in 0 seconds.
Last processed positions can be found in positions.txt
dsbulk@9f089e1d1717:/home/graph-book/data/ch7$ dsbulk load -h 172.18.0.2 -g trees_prod -e send -from Sensor -to Sensor -url Sensor__send__Sensor.csv -header true
Operation directory: /home/graph-book/data/ch7/logs/LOAD_20200624-161458-550030
total | failed | edges/s |  p50ms |  p99ms | p999ms | batches
  595 |      0 |     329 | 352.44 | 704.64 | 788.53 |    1.23
Operation LOAD_20200624-161458-550030 completed successfully in 1 seconds.
Last processed positions can be found in positions.txt
dsbulk@9f089e1d1717:/home/graph-book/data/ch7$ dsbulk load -h 172.18.0.2 -g trees_prod -e send -from Sensor -to Tower -url Sensor__send__Tower.csv -header true
Operation directory: /home/graph-book/data/ch7/logs/LOAD_20200624-161518-942147
total | failed | edges/s | p50ms |  p99ms | p999ms | batches
  114 |      0 |     105 | 31.30 | 191.89 | 191.89 |    1.24
Operation LOAD_20200624-161518-942147 completed successfully in 0 seconds.
Last processed positions can be found in positions.txt

```
