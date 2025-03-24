Протестироватþ падение производителþности при исполþзовании pgbouncer в разнýх
режимах: statement, transaction, session

postgres no bouncer socket:
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n thai
pgbench (16.8 (Ubuntu 16.8-0ubuntu0.24.04.1))
transaction type: /home/nicewone/workload.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 17577
number of failed transactions: 0 (0.000%)
latency average = 4.551 ms
initial connection time = 12.799 ms
tps = 1757.845374 (without initial connection time)
```

postgres no bouncer 100 coonections socket: 
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 100 -j 4 -T 10 -f ~/workload.sql -n thai
pgbench (16.8 (Ubuntu 16.8-0ubuntu0.24.04.1))
pgbench: error: connection to server on socket "/var/run/postgresql/.s.PGSQL.5432" failed: FATAL:  sorry, too many clients already
pgbench: error: could not create connection for client 99
```

postgres no bouncer web:
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -p 5432 -h localhost thai
pgbench (16.8 (Ubuntu 16.8-0ubuntu0.24.04.1))
transaction type: /home/nicewone/workload.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 23798
number of failed transactions: 0 (0.000%)
latency average = 3.340 ms
initial connection time = 86.547 ms
tps = 2395.390398 (without initial connection time)
```

postgres bouncer web (pool_mode=default(session)):
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
Password: 
pgbench (16.8 (Ubuntu 16.8-0ubuntu0.24.04.1))
transaction type: /home/nicewone/workload.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 78530
number of failed transactions: 0 (0.000%)
latency average = 1.008 ms
initial connection time = 103.844 ms
tps = 7934.037465 (without initial connection time)
```

postgres bouncer web (pool_mode=transaction):
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
Password: 
pgbench (16.8 (Ubuntu 16.8-0ubuntu0.24.04.1))
transaction type: /home/nicewone/workload.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 79684
number of failed transactions: 0 (0.000%)
latency average = 0.994 ms
initial connection time = 104.580 ms
tps = 8051.397021 (without initial connection time)
```

postgres bouncer web (pool_mode=statement):
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
Password: 
pgbench (16.8 (Ubuntu 16.8-0ubuntu0.24.04.1))
transaction type: /home/nicewone/workload.sql
scaling factor: 1
query mode: simple
number of clients: 8
number of threads: 4
maximum number of tries: 1
duration: 10 s
number of transactions actually processed: 79016
number of failed transactions: 0 (0.000%)
latency average = 1.002 ms
initial connection time = 105.079 ms
tps = 7984.106963 (without initial connection time)
```
