# Домашнее задание к занятию "6.4. PostgreSQL"

## Задача 1
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.

Подключитесь к БД PostgreSQL используя psql.

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

Найдите и приведите управляющие команды для:

    вывода списка БД
    подключения к БД
    вывода списка таблиц
    вывода описания содержимого таблиц
    выхода из psql

### Решение:

````
ivan@HP-Pavilion-dv6:~$ docker pull postgres:13
13: Pulling from library/postgres
Digest: sha256:cd6f40e3090735e297afa9c6597d5b5d57c9bf840bba1029014bb8c1853e0356
Status: Downloaded newer image for postgres:13
docker.io/library/postgres:13

ivan@HP-Pavilion-dv6:~$ docker volume create postgres_vol
postgres_vol
````

Поднимаем контейнер с postgres:13:
````
ivan@HP-Pavilion-dv6:~$ docker run --rm --name postgres13 -e POSTGRES_PASSWORD=pgs13 -it -p 5432:5432 -v postgres_vol:/var/lib/postgresql/data -d postgres:13
efb63c92ba8d7bcf29ee96640c38fd3b9572e63fd856aa3e0bd286a01229c903
ivan@HP-Pavilion-dv6:~$ docker ps
CONTAINER ID   IMAGE         COMMAND                  CREATED         STATUS         PORTS                                       NAMES
efb63c92ba8d   postgres:13   "docker-entrypoint.s…"   8 seconds ago   Up 7 seconds   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   postgres13
````

Подключитесь к БД PostgreSQL используя psql.

````
ivan@HP-Pavilion-dv6:~$ docker exec -it postgres13 bash
root@efb63c92ba8d:/# psql -h localhost -p 5432 -U postgres
psql (13.8 (Debian 13.8-1.pgdg110+1))
Type "help" for help.
postgres=#
````

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.
Найдите и приведите управляющие команды для:
    
	вывода списка БД

````
postgres=# \l
                                 List of databases
   Name    |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
-----------+----------+----------+------------+------------+-----------------------
 postgres  | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
 template1 | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
           |          |          |            |            | postgres=CTc/postgres
(3 rows)
````
	
    подключения к БД

````
  \c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo}
                         connect to new database (currently "postgres")	

postgres=# \c postgres
You are now connected to database "postgres" as user "postgres".
postgres=# \c template0
connection to server at "localhost" (127.0.0.1), port 5432 failed: FATAL:  database "template0" is not currently accepting connections
Previous connection kept
postgres=#
````

    вывода списка таблиц
````
postgres=# \dt
Did not find any relations.
postgres=# dtS
postgres-# \dtS
                    List of relations
   Schema   |          Name           | Type  |  Owner   
------------+-------------------------+-------+----------
 pg_catalog | pg_aggregate            | table | postgres
 pg_catalog | pg_am                   | table | postgres
 pg_catalog | pg_amop                 | table | postgres
 pg_catalog | pg_amproc               | table | postgres
......
......
 pg_catalog | pg_ts_template          | table | postgres
 pg_catalog | pg_type                 | table | postgres
 pg_catalog | pg_user_mapping         | table | postgres
(62 rows)
````

    вывода описания содержимого таблиц

````
postgres-# \dS+ pg_aggregate
                                   Table "pg_catalog.pg_aggregate"
      Column      |   Type   | Collation | Nullable | Default | Storage  | Stats target | Description 
------------------+----------+-----------+----------+---------+----------+--------------+-------------
 aggfnoid         | regproc  |           | not null |         | plain    |              | 
 aggkind          | "char"   |           | not null |         | plain    |              | 
 aggnumdirectargs | smallint |           | not null |         | plain    |              | 
 aggtransfn       | regproc  |           | not null |         | plain    |              | 
 aggfinalfn       | regproc  |           | not null |         | plain    |              | 
 aggcombinefn     | regproc  |           | not null |         | plain    |              | 
 aggserialfn      | regproc  |           | not null |         | plain    |              | 
 aggdeserialfn    | regproc  |           | not null |         | plain    |              | 
 aggmtransfn      | regproc  |           | not null |         | plain    |              | 
 aggminvtransfn   | regproc  |           | not null |         | plain    |              | 
 aggmfinalfn      | regproc  |           | not null |         | plain    |              | 
 aggfinalextra    | boolean  |           | not null |         | plain    |              | 
 aggmfinalextra   | boolean  |           | not null |         | plain    |              | 
 aggfinalmodify   | "char"   |           | not null |         | plain    |              | 
 aggmfinalmodify  | "char"   |           | not null |         | plain    |              | 
 aggsortop        | oid      |           | not null |         | plain    |              | 
 aggtranstype     | oid      |           | not null |         | plain    |              | 
 aggtransspace    | integer  |           | not null |         | plain    |              | 
 aggmtranstype    | oid      |           | not null |         | plain    |              | 
 aggmtransspace   | integer  |           | not null |         | plain    |              | 
 agginitval       | text     | C         |          |         | extended |              | 
 aggminitval      | text     | C         |          |         | extended |              | 
Indexes:
    "pg_aggregate_fnoid_index" UNIQUE, btree (aggfnoid)
Access method: heap
````

    выхода из psql
````
postgres-# \q
root@efb63c92ba8d:/# 
````

---

## Задача 2

Используя psql создайте БД test_database.

Изучите бэкап БД.

Восстановите бэкап БД в test_database.

Перейдите в управляющую консоль psql внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.

### Решение:

CREATE DATABASE test_database;

````
postgres=# CREATE DATABASE test_database;
CREATE DATABASE
postgres=# \l
                                   List of databases
     Name      |  Owner   | Encoding |  Collate   |   Ctype    |   Access privileges   
---------------+----------+----------+------------+------------+-----------------------
 postgres      | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
               |          |          |            |            | postgres=CTc/postgres
 template1     | postgres | UTF8     | en_US.utf8 | en_US.utf8 | =c/postgres          +
               |          |          |            |            | postgres=CTc/postgres
 test_database | postgres | UTF8     | en_US.utf8 | en_US.utf8 | 
(4 rows)
````

Изучите бэкап БД.
Восстановите бэкап БД в test_database.

Копируем дамп-файл в докер контейнер:

sudo docker cp test_dump.sql postgres13:/var/lib/postgresql/data

Восстанавливаем БД из дампа:

psql -U postgres -f ./test_dump.sql test_database

````
root@43083649c2d5:/var/lib/postgresql/data# psql -U postgres -f ./test_dump.sql test_database
SET
SET
SET
SET
SET
 set_config 
------------
 
(1 row)

SET
SET
SET
SET
SET
SET
CREATE TABLE
ALTER TABLE
CREATE SEQUENCE
ALTER TABLE
ALTER SEQUENCE
ALTER TABLE
COPY 8
 setval 
--------
      8
(1 row)

ALTER TABLE
root@43083649c2d5:/var/lib/postgresql/data#
````

Перейдите в управляющую консоль psql внутри контейнера.

psql -h localhost -p 5432 -U postgres -W

````
root@43083649c2d5:/var/lib/postgresql/data# psql -h localhost -p 5432 -U postgres -W
Password: 
psql (13.8 (Debian 13.8-1.pgdg110+1))
Type "help" for help.
postgres=#
````

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.

````
postgres=# \c test_database
Password: 
You are now connected to database "test_database" as user "postgres".
test_database=# ANALYZE VERBOSE public.orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE
test_database=#
````

Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.
Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.

````
test_database=# select avg_width from pg_stats where tablename='orders';
 avg_width 
-----------
         4
        16
         4
(3 rows)
test_database=# 
````

---

## Задача 3
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

### Решение:

Воспользуемся партиционированием для преобразования данных существующей таблицы "orders" в две новые таблицы.

Существующую таблицу переименуем в "orders_old":
````
test_database=# alter table orders rename to orders_old;
ALTER TABLE
````

Создадим новую таблицу "orders" с партиционированием по диапазону "price":
````
test_database=# create table orders (id integer, title varchar(80), price integer) partition by range(price);
CREATE TABLE
````

Создадим две новые новые таблицы с разделением по range(price), как отдельные части основной таблицы "orders":
````
test_database=# create table orders_less499 partition of orders for values from (0) to (499);
CREATE TABLE
test_database=# create table orders_more499 partition of orders for values from (499) to (999999999);
CREATE TABLE
````

Вставим все существующие строки из старой таблицы "orders_old" в новую "orders" :
````
test_database=# insert into orders (id, title, price) select * from orders_old;
INSERT 0 8
test_database=#
````

Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?

- Да, можно. На этапе проектирования новой таблицы можно было определить ее тип, как секционированная или партицинированная.

---

## Задача 4
Используя утилиту pg_dump создайте бекап БД test_database.

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

### Решение:
````
oot@5da4c9996347:/var/lib/postgresql/data# pg_dump -U postgres -d test_database > test_database_dump.sql
````

Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

Для обеспечения уникальности значения столбца title можно использовать ограничения уникальности или первичный ключ.
Например:
````
CREATE TABLE orders (
id integer, 
title varchar(80) UNIQUE, 
price integer
) 
partition by range(price);
````

---
