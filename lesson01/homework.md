# 作业

题目描述：

本地下载 TiDB，TiKV，PD 源代码，改写源码并编译部署以下环境：

- 1 TiDB

- 1 PD

- 3 TiKV

改写后：使得 TiDB 启动事务时，能打印出一个 “hello transaction” 的 日志

输出：一篇文章介绍以上过程

## 搭建

pd、tidb 克隆项目，执行`make`

tikv 执行`make`之前需要安装 cmake，macOS 执行`brew install cmake`

## 编译

pd-server、tikv-server、tidb-server

进入项目目录，然后执行`make`

## 启动

### pd-server

```bash
./pd-server -h
Usage of pd:
  -L string
    	log level: debug, info, warn, error, fatal (default 'info')
  -V	print version information and exit
  -advertise-client-urls string
    	advertise url for client traffic (default '${client-urls}')
  -advertise-peer-urls string
    	advertise url for peer traffic (default '${peer-urls}')
  -cacert string
    	path of file that contains list of trusted TLS CAs
  -cert string
    	path of file that contains X509 certificate in PEM format
  -client-urls string
    	url for client traffic (default "http://127.0.0.1:2379")
  -config string
    	config file
  -config-check
    	check config file validity and exit
  -data-dir string
    	path to the data directory (default 'default.${name}')
  -force-new-cluster
    	force to create a new one-member cluster
  -initial-cluster string
    	initial cluster configuration for bootstrapping, e,g. pd=http://127.0.0.1:2380
  -join string
    	join to an existing cluster (usage: cluster's '${advertise-client-urls}'
  -key string
    	path of file that contains X509 key in PEM format
  -log-file string
    	log file path
  -metrics-addr string
    	prometheus pushgateway address, leaves it empty will disable prometheus push
  -name string
    	human-readable name for this pd member
  -peer-urls string
    	url for peer traffic (default "http://127.0.0.1:2380")
  -version
    	print version information and exit
```

启动 pd-server

```bash
./pd-server --name=pd-0 --data-dir=./pd-0/data --peer-urls=http://127.0.0.1:2380 --advertise-peer-urls=http://127.0.0.1:2380 --client-urls=http://127.0.0.1:2379 --advertise-client-urls=http://127.0.0.1:2379 --log-file=./pd-0/pd.log --initial-cluster=pd-0=http://127.0.0.1:2380

```

如果启动多个 pd-server

```bash
 ./pd-server --name=pd-0 --data-dir=./pd-0/data --peer-urls=http://127.0.0.1:2380 --advertise-peer-urls=http://127.0.0.1:2380 --client-urls=http://127.0.0.1:2379 --advertise-client-urls=http://127.0.0.1:2379 --log-file=./pd-0/pd.log --initial-cluster=pd-0=http://127.0.0.1:2380,pd-1=http://127.0.0.1:2381,pd-2=http://127.0.0.1:2383

 ./pd-server --name=pd-1 --data-dir=./pd-1/data --peer-urls=http://127.0.0.1:2381 --advertise-peer-urls=http://127.0.0.1:2381 --client-urls=http://127.0.0.1:2382 --advertise-client-urls=http://127.0.0.1:2382 --log-file=./pd-1/pd.log --initial-cluster=pd-0=http://127.0.0.1:2380,pd-1=http://127.0.0.1:2381,pd-2=http://127.0.0.1:2383

./pd-server --name=pd-2 --data-dir=./pd-2/data --peer-urls=http://127.0.0.1:2383 --advertise-peer-urls=http://127.0.0.1:2383 --client-urls=http://127.0.0.1:2384 --advertise-client-urls=http://127.0.0.1:2384 --log-file=./pd-2/pd.log --initial-cluster=pd-0=http://127.0.0.1:2380,pd-1=http://127.0.0.1:2381,pd-2=http://127.0.0.1:2383
```

### tikv-server

```bash
./tikv-server -h
TiKV
Release Version:   4.1.0-alpha
Edition:           Community
Git Commit Hash:   ae7a6ecee6e3367da016df0293a9ffe9cc2b5705
Git Commit Branch: master
UTC Build Time:    2020-08-14 01:44:59
Rust Version:      rustc 1.46.0-nightly (16957bd4d 2020-06-30)
Enable Features:   jemalloc sse protobuf-codec
Profile:           release

A distributed transactional key-value database powered by Rust and Raft

USAGE:
    tikv-server [FLAGS] [OPTIONS]

FLAGS:
        --config-check           Check config file validity and exit
    -h, --help                   Prints help information
        --print-sample-config    Print a sample config to stdout
    -V, --version                Prints version information

OPTIONS:
    -A, --addr <IP:PORT>                     Set the listening address
        --advertise-addr <IP:PORT>           Set the advertise listening address for client communication
        --advertise-status-addr <IP:PORT>    Set the advertise listening address for the client communication of status
                                             report service
        --capacity <CAPACITY>                Set the store capacity
    -C, --config <FILE>                      Set the configuration file
    -s, --data-dir <PATH>                    Set the directory used to store data
        --labels <KEY=VALUE>...              Sets server labels
    -f, --log-file <FILE>                    Sets log file
    -L, --log-level <LEVEL>                  Set the log level [possible values: trace, debug, info, warn, warning,
                                             error, critical]
        --metrics-addr <IP:PORT>             Sets Prometheus Pushgateway address
        --pd-endpoints <PD_URL>...           Sets PD endpoints
        --status-addr <IP:PORT>              Set the HTTP listening address for the status report service
```

启动 tikv-server

```bash
./tikv-server --addr=127.0.0.1:20160 --advertise-addr=127.0.0.1:20160 --status-addr=127.0.0.1:20180 --pd=http://127.0.0.1:2379 --config=./tikv-0/tikv.toml --data-dir=./tikv-0/data --log-file=./tikv-0/tikv.log
```

pd-server 需要先启动，tikv 启动时连接 pd-server 即可

tikv.toml

```toml
[rocksdb]
max-open-files = 256

[raftdb]
max-open-files = 256

[storage]
reserve-space = 0

```

如果启动多个 tikv-server

```bash
./tikv-server --addr=127.0.0.1:50415 --advertise-addr=127.0.0.1:50415 --status-addr=127.0.0.1:50416 --pd=http://127.0.0.1:2379,http://127.0.0.1:2382,http://127.0.0.1:2384 --config=./tikv-0/tikv.toml --data-dir=./tikv-0/data --log-file=./tikv-0/tikv.log
./tikv-server --addr=127.0.0.1:50417 --advertise-addr=127.0.0.1:50417 --status-addr=127.0.0.1:50418 --pd=http://127.0.0.1:2379,http://127.0.0.1:2382,http://127.0.0.1:2384 --config=./tikv-1/tikv.toml --data-dir=./tikv-1/data --log-file=./tikv-1/tikv.log
./tikv-server --addr=127.0.0.1:50419 --advertise-addr=127.0.0.1:50419 --status-addr=127.0.0.1:50420 --pd=http://127.0.0.1:2379,http://127.0.0.1:2382,http://127.0.0.1:2384 --config=./tikv-2/tikv.toml --data-dir=./tikv-2/data --log-file=./tikv-2/tikv.log
```

关键就是指定 pd-server 集群地址

### tidb-server

```bash
./tidb-server -h
Usage of ./tidb-server:
  -L string
    	log level: info, debug, warn, error, fatal (default "info")
  -P string
    	tidb server port (default "4000")
  -V	print version information and exit (default false)
  -advertise-address string
    	tidb server advertise IP
  -affinity-cpus string
    	affinity cpu (cpu-no. separated by comma, e.g. 1,2,3)
  -config string
    	config file path
  -config-check
    	check config file validity and exit (default false)
  -config-strict
    	enforce config file validity (default false)
  -cors string
    	tidb server allow cors origin
  -enable-binlog
    	enable generate binlog (default false)
  -host string
    	tidb server host (default "0.0.0.0")
  -lease string
    	schema lease duration, very dangerous to change only if you know what you do (default "45s")
  -log-file string
    	log file path
  -log-slow-query string
    	slow query file path
  -metrics-addr string
    	prometheus pushgateway address, leaves it empty will disable prometheus push.
  -metrics-interval uint
    	prometheus client push interval in second, set "0" to disable prometheus push. (default 15)
  -path string
    	tidb storage path (default "/tmp/tidb")
  -plugin-dir string
    	the folder that hold plugin (default "/data/deploy/plugin")
  -plugin-load string
    	wait load plugin name(separated by comma)
  -proxy-protocol-header-timeout uint
    	proxy protocol header read timeout, unit is second. (default 5)
  -proxy-protocol-networks string
    	proxy protocol networks allowed IP or *, empty mean disable proxy protocol support
  -repair-list string
    	admin repair table list
  -repair-mode
    	enable admin repair mode (default false)
  -report-status
    	If enable status report HTTP service. (default true)
  -require-secure-transport
    	require client use secure transport
  -run-ddl
    	run ddl worker on this tidb-server (default true)
  -socket string
    	The socket file to use for connection.
  -status string
    	tidb server status port (default "10080")
  -status-host string
    	tidb server status host (default "0.0.0.0")
  -store string
    	registered store name, [tikv, mocktikv] (default "mocktikv")
  -token-limit int
    	the limit of concurrent executed sessions (default 1000)
```

```bash
./tidb-server -P 4000 --store=tikv --host=127.0.0.1 --status=10080 --path=127.0.0.1:2379 --log-file=./tidb-0/tidb.log
```

--host 为 pd-server 的地址

如果启动多个 tidb-server

```bash
./tidb-server -P 50421 --store=tikv --host=127.0.0.1 --status=50422 --path=127.0.0.1:2379,127.0.0.1:2382,127.0.0.1:2384 --log-file=./tidb-0/tidb.log
./tidb-server -P 50423 --store=tikv --host=127.0.0.1 --status=50424 --path=127.0.0.1:2379,127.0.0.1:2382,127.0.0.1:2384 --log-file=./tidb-1/tidb.log
```

关键也是指定 pd-server 地址

## 源码修改

在 session/session.go 中 修改

```go
func (s *session) NewTxn(ctx context.Context) error {}
```

添加

```go
logutil.BgLogger().Info("hello transaction")
```

重新编译运行

```bash
mysql> begin;  # 开始事务
Query OK, 0 rows affected (0.00 sec)

mysql> insert into user value(5,'5')
Query OK, 1 rows affected (0.01 sec)

mysql> commit; # 提交事务
Query OK, 0 rows affected (0.01 sec)
```
