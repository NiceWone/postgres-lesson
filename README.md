# postgres-lesson

## VM
ssh nicewone@62.84.120.108
## DB
psql -h 62.84.120.108 -U postgres -W


# HomeWork
## Task 1
```
postgres=# \c thai
You are now connected to database "thai" as user "postgres".
thai=# select count(*) from thai.book.tickets;
  count  
---------
 5185505
(1 row)

thai=# select count(*) from book.ride;
 count  
--------
 144000
(1 row)
```
## Task 2
