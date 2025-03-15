# postgres-lesson

VM
ssh nicewone@62.84.120.108
DB
psql -h 62.84.120.108 -U postgres -W

-------------
HomeWork
--- 1
lesson=> select count(*) from book.tickets;
  count  
---------
 5185505
(1 row)

lesson=> select count(*)
lesson-> from book.ride;
 count  
--------
 144000
(1 row)
---
