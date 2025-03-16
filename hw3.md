1. Создать таблицу с текстовым полем и заполнить случайными или сгенерированными данным в размере 1 млн строк
  
```sql
create table if not exists hw3
(
    text_filed text
);
INSERT INTO hw3 (text_filed)
SELECT md5(random()::text)
FROM generate_series(1, 1000000);
```

2. Посмотреть размер файла с таблицей
```sql 
select table_name,
     pg_size_pretty(pg_total_relation_size(quote_ident(table_name))),
     (pg_total_relation_size(quote_ident(table_name)))
from information_schema.tables
where table_name = 'hw3';
-- hw3,65 MB,68329472
```
3. 5 раз обновить все строчки и добавить к каждой строчке любой символ
4. Посмотреть количество мертвых строчек в таблице и когда последний раз приходил автовакуум
```sql
select n_live_tup, n_dead_tup, relname, last_autovacuum
  from pg_stat_all_tables
  where relname = 'hw3';
-- 1000000,1000000,hw3,2025-03-16 16:12:04.139643 +00:00
-- 1000000,0,hw3,2025-03-16 16:23:03.827240 +00:00
```
5. Подождать некоторое время, проверяя, пришел ли автовакуум
6. 5 раз обновить все строчки и добавить к каждой строчке любой символ
7. Посмотреть размер файла с таблицей
```sql
select table_name,
       pg_size_pretty(pg_total_relation_size(quote_ident(table_name))),
       (pg_total_relation_size(quote_ident(table_name)))
  from information_schema.tables
  where table_name = 'hw3';
-- hw3,716 MB,751157248
```
8. Отключить Автовакуум на конкретной таблице
9. 10 раз обновить все строчки и добавить к каждой строчке любой символ
10. Посмотреть размер файла с таблицей
``` 
hw3,732 MB,767344640
hw3,732 MB,767369216
```
11. Объясните полученный результат
```
Автовакум не удаляет данные физически, лишь помечает их на удаление, это пространство испольщуется для новых инсертов.
```
13. Не забудьте включить автовакуум)
    
Задание со *:
```sql
DO
$$
    BEGIN
        FOR i IN 0..9
            LOOP
                update hw3 set text_filed = substr(text_filed, 1, length(text_filed) - 1) || i::text;
                RAISE NOTICE 'i is %',i;
            END LOOP;
    END
$$;
```
