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
DETAIL:  Process 184085 waits for ShareLock on transaction 5727; blocked by process 184117.
Process 184117 waits for ShareLock on transaction 5728; blocked by process 184085.
HINT:  See server log for query details.
CONTEXT:  while updating tuple (0,2) in relation "accounts"
```
