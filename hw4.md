```sql
create table if not exists accounts
(
    id     integer,
    amount numeric
);
insert into accounts
values (1, 100),
       (2, 200),
       (3, 300);
 ```
term N1:
```psql
thai=# begin;
BEGIN
thai=*# update accounts set amount =250 where id =2;
UPDATE 1
thai=*# update accounts set amount =355 where id =3;
UPDATE 1
```

term N2:
```psql
thai=# begin;
BEGIN
thai=*# update accounts set amount =350 where id =3;
UPDATE 1
thai=*# update accounts set amount =255 where id =2;
ERROR:  deadlock detected
DETAIL:  Process 184228 waits for ShareLock on transaction 5729; blocked by process 184219.
Process 184219 waits for ShareLock on transaction 5730; blocked by process 184228.
HINT:  See server log for query details.
CONTEXT:  while updating tuple (0,2) in relation "accounts"
thai=!# 
```
sudo tail /var/log/postgresql/postgresql-16-main.log
```log
2025-03-23 15:01:53.737 UTC [184228] postgres@thai ERROR:  deadlock detected
2025-03-23 15:01:53.737 UTC [184228] postgres@thai DETAIL:  Process 184228 waits for ShareLock on transaction 5729; blocked by process 184219.
	Process 184219 waits for ShareLock on transaction 5730; blocked by process 184228.
	Process 184228: update accounts set amount =255 where id =2;
	Process 184219: update accounts set amount =355 where id =3;
2025-03-23 15:01:53.737 UTC [184228] postgres@thai HINT:  See server log for query details.
2025-03-23 15:01:53.737 UTC [184228] postgres@thai CONTEXT:  while updating tuple (0,2) in relation "accounts"
2025-03-23 15:01:53.737 UTC [184228] postgres@thai STATEMENT:  update accounts set amount =255 where id =2;
```
