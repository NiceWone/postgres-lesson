Протестироватþ падение производителþности при исполþзовании pgbouncer в разнýх
режимах: statement, transaction, session

postgres no bouncer socket:
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n thai
number of transactions actually processed: 17577
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
number of transactions actually processed: 23798
latency average = 3.340 ms
initial connection time = 86.547 ms
tps = 2395.390398 (without initial connection time)
```

postgres bouncer web (pool_mode=default(session)):
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
number of transactions actually processed: 78530
latency average = 1.008 ms
initial connection time = 103.844 ms
tps = 7934.037465 (without initial connection time)
```
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 50 -j 5 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
number of transactions actually processed: 77258
latency average = 6.054 ms
initial connection time = 658.212 ms
tps = 8258.426384 (without initial connection time)
```

postgres bouncer web (pool_mode=transaction):
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
number of transactions actually processed: 79684
latency average = 0.994 ms
initial connection time = 104.580 ms
tps = 8051.397021 (without initial connection time)
```
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 50 -j 5 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
number of transactions actually processed: 68436
latency average = 6.836 ms
initial connection time = 650.223 ms
tps = 7314.659696 (without initial connection time)
```

postgres bouncer web (pool_mode=statement):
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
number of transactions actually processed: 79016
latency average = 1.002 ms
initial connection time = 105.079 ms
tps = 7984.106963 (without initial connection time)
```
```
sudo -u postgres /usr/lib/postgresql/16/bin/pgbench -c 50 -j 5 -T 10 -f ~/workload.sql -n -p 6432 -h localhost thai
number of transactions actually processed: 66902
latency average = 6.992 ms
initial connection time = 650.703 ms
tps = 7151.362016 (without initial connection time)
```


