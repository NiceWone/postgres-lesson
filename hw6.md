Развернуть асинхронную реплику (можно использовать 1 ВМ, просто рядом кластер
развернуть и подключиться через localhost):

Развернул на 2-х отдельных вм в ЯО
```
51.250.110.75
10.129.0.7
51.250.23.131
10.129.0.11
ssh nicewone@51.250.110.75
ssh nicewone@51.250.23.131
```
```
sudo su postgres 
cd ~
cat >> /etc/postgresql/17/main/postgresql.conf << EOL
listen_addresses = '10.129.0.7'
EOL
cat >> /etc/postgresql/17/main/pg_hba.conf << EOL
host replication replicator 10.129.0.7/16 scram-sha-256
EOL
cat >> ~/.pgpass << EOL
10.129.0.11:5432:*:replicator:Erdfcvq1w2e3
EOL
```
```
pg_basebackup -h 10.129.0.11 -p 5432 -U replicator -R -S test -D /var/lib/postgresql/17/main
```
```
/usr/lib/postgresql/17/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload2.sql -n -U postgres -p 5432 thai
-- tps = 1176.214659 
/usr/lib/postgresql/17/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -U postgres thai
-- tps = 2610.178021 (without initial connection time)
```
pg_ctlcluster 17 main start
```
/usr/lib/postgresql/17/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload2.sql -n -U postgres -p 5432 thai
-- tps = 1316.387626 (without initial connection time)
/usr/lib/postgresql/17/bin/pgbench -c 8 -j 4 -T 10 -f ~/workload.sql -n -U postgres thai
-- tps = 23038.378288 (without initial connection time)
```
```
ALTER SYSTEM SET synchronous_commit='off';
pg_ctlcluster 17 main reload
```
-- tps = 7232.666684 (without initial connection time)
```
