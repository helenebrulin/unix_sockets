# unix_sockets benchmark

## Configuration

Edit configuration file : 

![](https://github.com/helenebrulin/unix_sockets/blob/main/Config.png)

```sh 
# Restart server
root@helene-test:~# redis-server /etc/redis/redis.conf

# Test connection with redis-cli
root@helene-test:~# redis-cli -s /tmp/redis.sock
redis /tmp/redis.sock>
```


## Machine : e2-standard-4

### Reads and Writes - Unix Socket
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       178978.89          ---          ---         1.76954         1.75900         2.75100         3.40700     34587.72
Gets       715915.57     18875.13    697040.44         1.76954         1.75900         2.75100         3.40700     30707.89
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     894894.46     18875.13    697040.44         1.76954         1.75900         2.75100         3.40700     65295.61
```


### Reads and Writes - TCP port : 
```sh
./memtier_benchmark/memtier_benchmark --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       136262.82          ---          ---         2.33128         2.33500         3.53500         4.47900     26332.79
Gets       545051.30     28620.14    516431.16         2.33128         2.33500         3.53500         4.47900     25508.20
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     681314.12     28620.14    516431.16         2.33128         2.33500         3.53500         4.47900     51840.99
```

### Variation
- <b>Sets/sec : +23.87%</b>
- <b>Gets/sec : +23.87%</b>
- <b>Latency : -32.36%</b>


## Machine : e2-standard-8

### Reads and Writes - Socket : 
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       216486.48          ---          ---         1.46476         1.45500         2.22300         2.84700     41836.03
Gets       865945.92     46211.38    819734.54         1.46476         1.45500         2.22300         2.84700     40636.54
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals    1082432.40     46211.38    819734.54         1.46476         1.45500         2.22300         2.84700     82472.58
```

### Reads and Writes - TCP 
```sh
./memtier_benchmark/memtier_benchmark --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       156194.90          ---          ---         2.03508         2.11100         3.07100         3.85500     30184.71
Gets       624779.61     32779.59    592000.03         2.03508         2.11100         3.07100         3.85500     29235.29
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     780974.52     32779.59    592000.03         2.03508         2.11100         3.07100         3.85500     59420.00
```

### Reads and Writes variation

- <b>Sets/sec : +27.85%</b>
- <b>Gets/sec : +27.85%</b>
- <b>Latency : -39.04%</b>


### Read-only - socket
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=0:1 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets            0.00          ---          ---             ---             ---             ---             ---         0.00
Gets       819595.63    802050.72     17544.91         1.94356         1.97500         2.70300         3.21500    151763.95
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     819595.63    802050.72     17544.91         1.94356         1.97500         2.70300         3.21500    151763.95
```

### Read-only - TCP
```sh
./memtier_benchmark/memtier_benchmark --ratio=0:1 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets            0.00          ---          ---             ---             ---             ---             ---         0.00
Gets       625430.76    625430.76         0.00         2.54938         2.73500         3.67900         4.15900    117811.08
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     625430.76    625430.76         0.00         2.54938         2.73500         3.67900         4.15900    117811.08
```

### Read-Only Variation
- <b>Gets/sec : +23.69%</b>
- <b>Latency : -30.93%</b>

### Write-only - socket
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=1:0 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       791207.88          ---          ---         2.00976         1.99100         2.87900         5.98300    152901.40
Gets            0.00         0.00         0.00             ---             ---             ---             ---         0.00
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     791207.88         0.00         0.00         2.00976         1.99100         2.87900         5.98300    152901.40
```

### Write only - TCP
```sh
./memtier_benchmark/memtier_benchmark --ratio=1:0 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       551133.27          ---          ---         2.88970         2.92700         3.80700         4.44700    106506.88
Gets            0.00         0.00         0.00             ---             ---             ---             ---         0.00
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     551133.27         0.00         0.00         2.88970         2.92700         3.80700         4.44700    106506.88
```

### Write-Only Variation
- <b>Sets/sec : +30.34%</b>
- <b>Latency : -44%</b>


## Colruyt use case
Goal : 
- Write 7.5GB or 5M keys of 1.5kb per second, with MSETs of 3000 keys.
- 4 machines of 8 Cpus and 32 RAM. 50 clients. 


### Colruyt use case - MSETs of 3000 keys - socket
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock -d 1500 --command="MSET __key__ __data__  __key__ __data__ .... --command-ratio=1 --command-key-pattern=P --clients=50 --threads=1 --test-time=60
```

```sh
ALL STATS
==================================================================================================
Type         Ops/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
--------------------------------------------------------------------------------------------------
Msets         110.97       450.53612       440.31900       819.19900       913.40700    497692.44
Totals        110.97       450.53612       440.31900       819.19900       913.40700    497692.44
```

Result : 
- 111 * 3000 = 333000 keys written per sec per machine
- 1 332 000 keys written per second with 4 machines.
- Almost 2GB written per second. 
- Goal not achieved.

Notes : using 4/8 threads and 12/6 clients doesn't change much, msets/sec are a bit lower.


### Colruyt use case - MSETs of 3000 keys - TCP
```sh
./memtier_benchmark/memtier_benchmark -d 1500 --command="MSET __key__ __data__  __key__ __data__ .... --command-ratio=1 --command-key-pattern=P --clients=50 --threads=1 --test-time=60
```

```sh
ALL STATS
==================================================================================================
Type         Ops/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
--------------------------------------------------------------------------------------------------
Msets          88.12       554.83211       511.99900       905.21500      1032.19100    395232.88
Totals         88.12       554.83211       511.99900       905.21500      1032.19100    395232.88
```


### Colruyt use case - MSETs of 3000 keys - Variations
- <b>Msets/sec : +20.59% </b>
- <b>Latency : -23.15% </b>


### Colruyt use case - pipelines - sockets
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=1:0 --test-time=60 -d 1500 -c 50 --pipeline=3000 -t 1
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       374726.37          ---          ---       399.45648       401.40700       425.98300       432.12700    566806.12
Gets            0.00         0.00         0.00             ---             ---             ---             ---         0.00
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     374726.37         0.00         0.00       399.45648       401.40700       425.98300       432.12700    566806.12
```


### Colruyt use case - pipelines - TCP
```sh
./memtier_benchmark/memtier_benchmark --ratio=1:0 --test-time=60 -d 1500 -c 50 --pipeline=3000 -t 1
```

```sh
ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       329733.28          ---          ---       453.84694       456.70300       491.51900       511.99900    498750.15
Gets            0.00         0.00         0.00             ---             ---             ---             ---         0.00
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     329733.28         0.00         0.00       453.84694       456.70300       491.51900       511.99900    498750.15
```

### Colruyt use case - Variations
- <b>Sets/sec : +12% </b>
- <b>Latency : -13.62%</b>


# Links
- https://gulchuk.com/blog/how-to-connect-to-redis-by-unix-socket-only 
- https://github.com/redis/node-redis/issues/204
- https://stackoverflow.com/questions/52028547/how-to-use-unix-socket-with-node-redis 
- https://docs.v2ep.com/BackEnd/Python/Python-3-redis-py-Connection-Pool-with-unix-sockets/ 