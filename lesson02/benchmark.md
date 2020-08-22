# 测试数据

## 环境配置

```text
CPU:8核
Memory:16G
OS:Linux
Kernal:4.14.81.bm.15-amd64
Machine:x86_64

```

topology.yaml 配置

```yaml
global:
  user: "wangxiaodong.gd"
  ssh_port: 22
  deploy_dir: "/tidb-deploy"
  data_dir: "/tidb-data"

pd_servers:
  - host: 10.227.14.244

tidb_servers:
  - host: 10.227.14.244

tikv_servers:
  - host: 10.227.14.244

monitoring_servers:
  - host: 10.227.14.244

grafana_servers:
  - host: 10.227.14.244

alertmanager_servers:
  - host: 10.227.14.244
```

## sysbench

```text
sysbench --config-file=config oltp_update_index --threads=32 --tables=32 --table-size=10000 run
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 32
Report intermediate results every 3 second(s)
Initializing random number generator from current time


Initializing worker threads...

Threads started!

[ 3s ] thds: 32 tps: 448.27 qps: 448.27 (r/w/o: 0.00/448.27/0.00) lat (ms,95%): 110.66 err/s: 0.00 reconn/s: 0.00
[ 6s ] thds: 32 tps: 409.73 qps: 409.73 (r/w/o: 0.00/409.73/0.00) lat (ms,95%): 102.97 err/s: 0.00 reconn/s: 0.00
[ 9s ] thds: 32 tps: 530.46 qps: 530.46 (r/w/o: 0.00/530.46/0.00) lat (ms,95%): 101.13 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            0
        write:                           4841
        other:                           0
        total:                           4841
    transactions:                        4841   (481.75 per sec.)
    queries:                             4841   (481.75 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0471s
    total number of events:              4841

Latency (ms):
         min:                                   17.27
         avg:                                   66.23
         max:                                  414.23
         95th percentile:                      102.97
         sum:                               320637.21

Threads fairness:
    events (avg/stddev):           151.2812/3.62
    execution time (avg/stddev):   10.0199/0.01
```

```text
sysbench --config-file=config oltp_update_index --threads=32 --tables=32 --table-size=10000 run
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 32
Report intermediate results every 3 second(s)
Initializing random number generator from current time


Initializing worker threads...

Threads started!

[ 3s ] thds: 32 tps: 511.37 qps: 511.37 (r/w/o: 0.00/511.37/0.00) lat (ms,95%): 101.13 err/s: 0.00 reconn/s: 0.00
[ 6s ] thds: 32 tps: 447.51 qps: 447.51 (r/w/o: 0.00/447.51/0.00) lat (ms,95%): 101.13 err/s: 0.00 reconn/s: 0.00
[ 9s ] thds: 32 tps: 428.84 qps: 428.84 (r/w/o: 0.00/428.84/0.00) lat (ms,95%): 101.13 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            0
        write:                           4608
        other:                           0
        total:                           4608
    transactions:                        4608   (457.95 per sec.)
    queries:                             4608   (457.95 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          10.0610s
    total number of events:              4608

Latency (ms):
         min:                                   17.79
         avg:                                   69.68
         max:                                  328.26
         95th percentile:                      101.13
         sum:                               321066.08

Threads fairness:
    events (avg/stddev):           144.0000/3.49
    execution time (avg/stddev):   10.0333/0.02

```

```text
sysbench --config-file=config oltp_read_only --threads=32 --tables=32 --table-size=10000 run
sysbench 1.0.20 (using bundled LuaJIT 2.1.0-beta2)

Running the test with following options:
Number of threads: 32
Report intermediate results every 3 second(s)
Initializing random number generator from current time


Initializing worker threads...

Threads started!

[ 3s ] thds: 32 tps: 24.97 qps: 501.02 (r/w/o: 440.76/0.00/60.26) lat (ms,95%): 2159.29 err/s: 0.00 reconn/s: 0.00
[ 6s ] thds: 32 tps: 11.66 qps: 201.17 (r/w/o: 177.52/0.00/23.65) lat (ms,95%): 2159.29 err/s: 0.00 reconn/s: 0.00
[ 9s ] thds: 32 tps: 26.00 qps: 398.97 (r/w/o: 347.31/0.00/51.66) lat (ms,95%): 2238.47 err/s: 0.00 reconn/s: 0.00
[ 12s ] thds: 31 tps: 6.67 qps: 60.07 (r/w/o: 53.06/0.00/7.01) lat (ms,95%): 2009.23 err/s: 0.00 reconn/s: 0.00
[ 15s ] thds: 20 tps: 3.66 qps: 10.66 (r/w/o: 7.00/0.00/3.66) lat (ms,95%): 4599.99 err/s: 0.00 reconn/s: 0.00
SQL statistics:
    queries performed:
        read:                            3080
        write:                           0
        other:                           440
        total:                           3520
    transactions:                        220    (14.13 per sec.)
    queries:                             3520   (226.13 per sec.)
    ignored errors:                      0      (0.00 per sec.)
    reconnects:                          0      (0.00 per sec.)

General statistics:
    total time:                          15.5646s
    total number of events:              220

Latency (ms):
         min:                                  295.59
         avg:                                 1677.52
         max:                                15181.76
         95th percentile:                     3639.94
         sum:                               369054.49

Threads fairness:
    events (avg/stddev):           6.8750/1.34
    execution time (avg/stddev):   11.5330/1.27
```

## go-ycsb

```text

./bin/go-ycsb run mysql -P workloads/workloada -p recordcount=10000 -p mysql.host=10.227.14.244 -p mysql.port=4000 --threads 64
***************** properties *****************
"readallfields"="true"
"scanproportion"="0"
"workload"="core"
"insertproportion"="0"
"readproportion"="0.5"
"mysql.port"="4000"
"mysql.host"="10.227.14.244"
"dotransactions"="true"
"updateproportion"="0.5"
"requestdistribution"="uniform"
"threadcount"="64"
"recordcount"="10000"
"operationcount"="1000"
**********************************************
Run finished, takes 2.531798778s
READ   - Takes(s): 2.5, Count: 491, OPS: 199.9, Avg(us): 51077, Min(us): 10527, Max(us): 1736438, 99th(us): 1736000, 99.9th(us): 1737000, 99.99th(us): 1737000
UPDATE - Takes(s): 2.5, Count: 469, OPS: 189.6, Avg(us): 248146, Min(us): 12515, Max(us): 1677164, 99th(us): 1650000, 99.9th(us): 1678000, 99.99th(us): 1678000
```

## go-tpc

tpc-c

```text
./bin/go-tpc tpcc -H 10.227.14.244 -P 4000 -D tpcc --warehouses 10 run --time 1m --threads 8
[Current] DELIVERY - Takes(s): 6.7, Count: 7, TPM: 63.0, Sum(ms): 7958, Avg(ms): 1136, 90th(ms): 2000, 99th(ms): 2000, 99.9th(ms): 2000
[Current] NEW_ORDER - Takes(s): 8.6, Count: 72, TPM: 499.7, Sum(ms): 35015, Avg(ms): 486, 90th(ms): 1000, 99th(ms): 1000, 99.9th(ms): 1000
[Current] ORDER_STATUS - Takes(s): 8.8, Count: 8, TPM: 54.6, Sum(ms): 758, Avg(ms): 94, 90th(ms): 160, 99th(ms): 160, 99.9th(ms): 160
[Current] PAYMENT - Takes(s): 8.6, Count: 72, TPM: 501.6, Sum(ms): 24144, Avg(ms): 335, 90th(ms): 512, 99th(ms): 1000, 99.9th(ms): 1000
[Current] STOCK_LEVEL - Takes(s): 8.5, Count: 14, TPM: 98.7, Sum(ms): 1154, Avg(ms): 82, 90th(ms): 112, 99th(ms): 112, 99.9th(ms): 112
[Current] DELIVERY - Takes(s): 9.9, Count: 8, TPM: 48.7, Sum(ms): 9390, Avg(ms): 1173, 90th(ms): 2000, 99th(ms): 2000, 99.9th(ms): 2000
[Current] NEW_ORDER - Takes(s): 10.0, Count: 90, TPM: 541.9, Sum(ms): 42822, Avg(ms): 475, 90th(ms): 1000, 99th(ms): 1000, 99.9th(ms): 1000
[Current] ORDER_STATUS - Takes(s): 9.7, Count: 8, TPM: 49.7, Sum(ms): 735, Avg(ms): 91, 90th(ms): 112, 99th(ms): 112, 99.9th(ms): 112
[Current] PAYMENT - Takes(s): 9.8, Count: 82, TPM: 504.6, Sum(ms): 26697, Avg(ms): 325, 90th(ms): 512, 99th(ms): 1000, 99.9th(ms): 1000
[Current] STOCK_LEVEL - Takes(s): 9.9, Count: 4, TPM: 24.2, Sum(ms): 289, Avg(ms): 72, 90th(ms): 96, 99th(ms): 96, 99.9th(ms): 96
[Current] DELIVERY - Takes(s): 9.5, Count: 11, TPM: 69.4, Sum(ms): 10900, Avg(ms): 990, 90th(ms): 1500, 99th(ms): 1500, 99.9th(ms): 1500
[Current] NEW_ORDER - Takes(s): 10.0, Count: 89, TPM: 535.9, Sum(ms): 41558, Avg(ms): 466, 90th(ms): 1000, 99th(ms): 1000, 99.9th(ms): 1000
[Current] ORDER_STATUS - Takes(s): 9.4, Count: 9, TPM: 57.4, Sum(ms): 795, Avg(ms): 88, 90th(ms): 112, 99th(ms): 112, 99.9th(ms): 112
[Current] PAYMENT - Takes(s): 10.0, Count: 85, TPM: 512.1, Sum(ms): 26245, Avg(ms): 308, 90th(ms): 512, 99th(ms): 1500, 99.9th(ms): 1500
[Current] STOCK_LEVEL - Takes(s): 7.8, Count: 11, TPM: 84.6, Sum(ms): 723, Avg(ms): 65, 90th(ms): 96, 99th(ms): 96, 99.9th(ms): 96
[Current] DELIVERY - Takes(s): 9.2, Count: 10, TPM: 65.1, Sum(ms): 10649, Avg(ms): 1064, 90th(ms): 1500, 99th(ms): 1500, 99.9th(ms): 1500
[Current] NEW_ORDER - Takes(s): 9.9, Count: 91, TPM: 551.8, Sum(ms): 43005, Avg(ms): 472, 90th(ms): 1000, 99th(ms): 1000, 99.9th(ms): 1000
[Current] ORDER_STATUS - Takes(s): 8.9, Count: 10, TPM: 67.7, Sum(ms): 857, Avg(ms): 85, 90th(ms): 96, 99th(ms): 112, 99.9th(ms): 112
[Current] PAYMENT - Takes(s): 9.8, Count: 82, TPM: 500.4, Sum(ms): 24640, Avg(ms): 300, 90th(ms): 512, 99th(ms): 1000, 99.9th(ms): 1000
[Current] STOCK_LEVEL - Takes(s): 9.1, Count: 13, TPM: 85.7, Sum(ms): 844, Avg(ms): 64, 90th(ms): 96, 99th(ms): 96, 99.9th(ms): 96
[Current] DELIVERY - Takes(s): 6.3, Count: 8, TPM: 76.7, Sum(ms): 9314, Avg(ms): 1164, 90th(ms): 2000, 99th(ms): 2000, 99.9th(ms): 2000
[Current] NEW_ORDER - Takes(s): 10.0, Count: 76, TPM: 456.1, Sum(ms): 40719, Avg(ms): 535, 90th(ms): 1000, 99th(ms): 1500, 99.9th(ms): 1500
[Current] ORDER_STATUS - Takes(s): 9.0, Count: 8, TPM: 53.6, Sum(ms): 1091, Avg(ms): 136, 90th(ms): 512, 99th(ms): 512, 99.9th(ms): 512
[Current] PAYMENT - Takes(s): 10.0, Count: 73, TPM: 438.1, Sum(ms): 27949, Avg(ms): 382, 90th(ms): 1000, 99th(ms): 1000, 99.9th(ms): 1000
[Current] STOCK_LEVEL - Takes(s): 1.7, Count: 5, TPM: 181.4, Sum(ms): 331, Avg(ms): 66, 90th(ms): 96, 99th(ms): 96, 99.9th(ms): 96
Finished
[Summary] DELIVERY - Takes(s): 56.7, Count: 50, TPM: 52.9, Sum(ms): 54589, Avg(ms): 1091, 90th(ms): 1500, 99th(ms): 2000, 99.9th(ms): 2000
[Summary] DELIVERY_ERR - Takes(s): 56.7, Count: 1, TPM: 1.1, Sum(ms): 182, Avg(ms): 182, 90th(ms): 192, 99th(ms): 192, 99.9th(ms): 192
[Summary] NEW_ORDER - Takes(s): 58.6, Count: 521, TPM: 533.0, Sum(ms): 250924, Avg(ms): 481, 90th(ms): 1000, 99th(ms): 1000, 99.9th(ms): 1500
[Summary] NEW_ORDER_ERR - Takes(s): 58.6, Count: 3, TPM: 3.1, Sum(ms): 565, Avg(ms): 188, 90th(ms): 256, 99th(ms): 256, 99.9th(ms): 256
[Summary] ORDER_STATUS - Takes(s): 58.8, Count: 49, TPM: 50.0, Sum(ms): 4792, Avg(ms): 97, 90th(ms): 112, 99th(ms): 512, 99.9th(ms): 512
[Summary] PAYMENT - Takes(s): 58.6, Count: 474, TPM: 485.2, Sum(ms): 155478, Avg(ms): 328, 90th(ms): 512, 99th(ms): 1000, 99.9th(ms): 1500
[Summary] PAYMENT_ERR - Takes(s): 58.6, Count: 4, TPM: 4.1, Sum(ms): 346, Avg(ms): 86, 90th(ms): 256, 99th(ms): 256, 99.9th(ms): 256
[Summary] STOCK_LEVEL - Takes(s): 58.5, Count: 54, TPM: 55.4, Sum(ms): 3803, Avg(ms): 70, 90th(ms): 96, 99th(ms): 112, 99.9th(ms): 112
tpmC: 533.0
```

tpc-h

```text
Finished
[Summary] Q1: 2.23s
[Summary] Q10: 1.14s
[Summary] Q11: 1.00s
[Summary] Q12: 1.10s
[Summary] Q13: 1.27s
[Summary] Q14: 1.04s
[Summary] Q15: 1.95s
[Summary] Q16: 0.59s
[Summary] Q17: 2.93s
[Summary] Q18: 3.80s
[Summary] Q19: 1.45s
[Summary] Q2: 0.92s
[Summary] Q20: 0.95s
[Summary] Q21: 2.22s
[Summary] Q22: 0.77s
[Summary] Q3: 1.50s
[Summary] Q4: 0.92s
[Summary] Q5: 6.45s
[Summary] Q6: 0.85s
[Summary] Q7: 1.41s
[Summary] Q8: 1.59s
[Summary] Q9: 3.58s
```
