# unix_sockets

# Redis with Unix Sockets benchmark

## Configuration

Edit configuration file : 

![https://github.com/helenebrulin/unix_sockets/Config.png]

```sh 
root@helene-test:~# redis-server /etc/redis/redis.conf

#Test connection with redis-cli
root@helene-test:~# redis-cli -s /tmp/redis.sock
redis /tmp/redis.sock>
```


## Machine : e2-standard-4

### Reads and Writes - Unix Socket
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       178978.89          ---          ---         1.76954         1.75900         2.75100         3.40700     34587.72
Gets       715915.57     18875.13    697040.44         1.76954         1.75900         2.75100         3.40700     30707.89
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     894894.46     18875.13    697040.44         1.76954         1.75900         2.75100         3.40700     65295.61


### Reads and Writes - TCP port : 
```sh
./memtier_benchmark/memtier_benchmark --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       136262.82          ---          ---         2.33128         2.33500         3.53500         4.47900     26332.79
Gets       545051.30     28620.14    516431.16         2.33128         2.33500         3.53500         4.47900     25508.20
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     681314.12     28620.14    516431.16         2.33128         2.33500         3.53500         4.47900     51840.99


### Variation
- Sets/sec : +23.87%
- Gets/sec : +23.87%
- Latency : -32.36%


## Machine : e2-standard-8

### Reads and Writes - Socket : 
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       216486.48          ---          ---         1.46476         1.45500         2.22300         2.84700     41836.03
Gets       865945.92     46211.38    819734.54         1.46476         1.45500         2.22300         2.84700     40636.54
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals    1082432.40     46211.38    819734.54         1.46476         1.45500         2.22300         2.84700     82472.58


### Reads and Writes - TCP 
```sh
./memtier_benchmark/memtier_benchmark --ratio=1:4 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       156194.90          ---          ---         2.03508         2.11100         3.07100         3.85500     30184.71
Gets       624779.61     32779.59    592000.03         2.03508         2.11100         3.07100         3.85500     29235.29
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     780974.52     32779.59    592000.03         2.03508         2.11100         3.07100         3.85500     59420.00

### Reads and Writes variation

- Sets/sec : +27.85%
- Gets/sec : +27.85%
- Latency : -39.04%


### Read-only - socket
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=0:1 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets            0.00          ---          ---             ---             ---             ---             ---         0.00
Gets       819595.63    802050.72     17544.91         1.94356         1.97500         2.70300         3.21500    151763.95
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     819595.63    802050.72     17544.91         1.94356         1.97500         2.70300         3.21500    151763.95

### Read-only - TCP
```sh
./memtier_benchmark/memtier_benchmark --ratio=0:1 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets            0.00          ---          ---             ---             ---             ---             ---         0.00
Gets       625430.76    625430.76         0.00         2.54938         2.73500         3.67900         4.15900    117811.08
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     625430.76    625430.76         0.00         2.54938         2.73500         3.67900         4.15900    117811.08

### Read-Only Variation
- Gets/sec : +23.69%
- Latency : -30.93%

### Write-only - socket
```sh
./memtier_benchmark/memtier_benchmark --unix-socket=/tmp/redis.sock --ratio=1:0 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       791207.88          ---          ---         2.00976         1.99100         2.87900         5.98300    152901.40
Gets            0.00         0.00         0.00             ---             ---             ---             ---         0.00
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     791207.88         0.00         0.00         2.00976         1.99100         2.87900         5.98300    152901.40


### Write only - TCP
```sh
./memtier_benchmark/memtier_benchmark --ratio=1:0 --test-time=120 -d 150 -t 8 -c 5 --pipeline=40 
```

ALL STATS
============================================================================================================================
Type         Ops/sec     Hits/sec   Misses/sec    Avg. Latency     p50 Latency     p99 Latency   p99.9 Latency       KB/sec
----------------------------------------------------------------------------------------------------------------------------
Sets       551133.27          ---          ---         2.88970         2.92700         3.80700         4.44700    106506.88
Gets            0.00         0.00         0.00             ---             ---             ---             ---         0.00
Waits           0.00          ---          ---             ---             ---             ---             ---          ---
Totals     551133.27         0.00         0.00         2.88970         2.92700         3.80700         4.44700    106506.88

### Write-Only Variation
- Sets/sec : +30.34%
- Latency : -44%

# Links
- https://gulchuk.com/blog/how-to-connect-to-redis-by-unix-socket-only 
- https://github.com/redis/node-redis/issues/204
- https://stackoverflow.com/questions/52028547/how-to-use-unix-socket-with-node-redis 
- https://docs.v2ep.com/BackEnd/Python/Python-3-redis-py-Connection-Pool-with-unix-sockets/ 