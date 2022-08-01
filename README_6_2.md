# Домашнее задание к занятию "6.2. SQL"

## Задача 1 
Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.
Приведите получившуюся команду или docker-compose манифест.

### Решение:

Команды:

docker run --name pgSQL12 -e POSTGRES_USER=ivan-pg -e POSTGRES_PASSWORD=HP-dv6-sql -p 5432:5432 -it -v PostgreSQL_volume_1:/var/lib/postgresql/data -v PostgreSQL_volume_2:/var/lib/postgresql postgres:12

docker exec -it 43dc5352174b bash

````
root@43dc5352174b:/# psql --username=ivan-pg --dbname=postgres
psql (12.11 (Debian 12.11-1.pgdg110+1))
Type "help" for help.

postgres=# select version ();
                                                            version                                                            
-------------------------------------------------------------------------------------------------------------------------------
 PostgreSQL 12.11 (Debian 12.11-1.pgdg110+1) on x86_64-pc-linux-gnu, compiled by gcc (Debian 10.2.1-6) 10.2.1 20210110, 64-bit
(1 row)

postgres=# \l
                                 List of databases
   Name    |  Owner  | Encoding |  Collate   |   Ctype    |    Access privileges    
-----------+---------+----------+------------+------------+-------------------------
 ivan-pg   | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | 
 postgres  | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | =c/"ivan-pg"           +
           |         |          |            |            | "ivan-pg"=CTc/"ivan-pg"
 template1 | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | =c/"ivan-pg"           +
           |         |          |            |            | "ivan-pg"=CTc/"ivan-pg"
(4 rows)
````

---

## Задача 2

В БД из задачи 1:

    создайте пользователя test-admin-user и БД test_db
    в БД test_db создайте таблицу orders и clients (спeцификация таблиц ниже)
    предоставьте привилегии на все операции пользователю test-admin-user на таблицы БД test_db
    создайте пользователя test-simple-user
    предоставьте пользователю test-simple-user права на SELECT/INSERT/UPDATE/DELETE данных таблиц БД test_db

Таблица orders:

    id (serial primary key)
    наименование (string)
    цена (integer)

Таблица clients:

    id (serial primary key)
    фамилия (string)
    страна проживания (string, index)
    заказ (foreign key orders)

Приведите:

    итоговый список БД после выполнения пунктов выше,
    описание таблиц (describe)
    SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
    список пользователей с правами над таблицами test_db

### Решение:
````
psql --username=ivan-pg --dbname=postgres
CREATE DATABASE test_db;
CREATE ROLE "test-admin-user" SUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;
GRANT ALL ON ALL TABLES IN SCHEMA "public" TO "test-admin-user";
CREATE ROLE "test-simple-user" NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA "public" TO "test-simple-user";
psql --username=ivan-pg --dbname=test_db

CREATE TABLE orders 
(
	id	serial PRIMARY KEY,
	order_name varchar(30) NOT NULL CHECK (order_name <> ''),
	price integer NOT NULL CHECK (price > 0)
);
CREATE TABLE clients 
(
        id serial PRIMARY KEY,
        last_name varchar(30) NOT NULL CHECK (last_name <> ''),
	country varchar(30) NOT NULL CHECK (country <> ''),
	order_number	integer REFERENCES orders
);
````

итоговый список БД и ролей после выполнения пунктов выше:

````
test_db=# \l
                                 List of databases
   Name    |  Owner  | Encoding |  Collate   |   Ctype    |    Access privileges    
-----------+---------+----------+------------+------------+-------------------------
 ivan-pg   | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | 
 postgres  | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | 
 template0 | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | =c/"ivan-pg"           +
           |         |          |            |            | "ivan-pg"=CTc/"ivan-pg"
 template1 | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | =c/"ivan-pg"           +
           |         |          |            |            | "ivan-pg"=CTc/"ivan-pg"
 test_db   | ivan-pg | UTF8     | en_US.utf8 | en_US.utf8 | 
(5 rows)

test_db=# \du
                                       List of roles
    Role name     |                         Attributes                         | Member of 
------------------+------------------------------------------------------------+-----------
 ivan-pg          | Superuser, Create role, Create DB, Replication, Bypass RLS | {}
 test-admin-user  | Superuser, No inheritance                                  | {}
 test-simple-user | No inheritance                                             | {}

test_db=# \dt
         List of relations
 Schema |  Name   | Type  |  Owner  
--------+---------+-------+---------
 public | clients | table | ivan-pg
 public | orders  | table | ivan-pg
(2 rows)
````

описание таблиц (describe):

````
est_db=# \d+ orders
                                                         Table "public.orders"
   Column   |         Type          | Collation | Nullable |              Default               | Storage  | Stats target | Description 
------------+-----------------------+-----------+----------+------------------------------------+----------+--------------+-------------
 id         | integer               |           | not null | nextval('orders_id_seq'::regclass) | plain    |              | 
 order_name | character varying(30) |           | not null |                                    | extended |              | 
 price      | integer               |           | not null |                                    | plain    |              | 
Indexes:
    "orders_pkey" PRIMARY KEY, btree (id)
Check constraints:
    "orders_order_name_check" CHECK (order_name::text <> ''::text)
    "orders_price_check" CHECK (price > 0)
Referenced by:
    TABLE "clients" CONSTRAINT "clients_order_number_fkey" FOREIGN KEY (order_number) REFERENCES orders(id)
Access method: heap

test_db=# \d+ clients
                                                          Table "public.clients"
    Column    |         Type          | Collation | Nullable |               Default               | Storage  | Stats target | Description 
--------------+-----------------------+-----------+----------+-------------------------------------+----------+--------------+-------------
 id           | integer               |           | not null | nextval('clients_id_seq'::regclass) | plain    |              | 
 last_name    | character varying(30) |           | not null |                                     | extended |              | 
 country      | character varying(30) |           | not null |                                     | extended |              | 
 order_number | integer               |           |          |                                     | plain    |              | 
Indexes:
    "clients_pkey" PRIMARY KEY, btree (id)
Check constraints:
    "clients_country_check" CHECK (country::text <> ''::text)
    "clients_last_name_check" CHECK (last_name::text <> ''::text)
Foreign-key constraints:
    "clients_order_number_fkey" FOREIGN KEY (order_number) REFERENCES orders(id)
Access method: heap
````

SQL-запрос для выдачи списка пользователей с правами над таблицами test_db:
select * from information_schema.table_privileges where grantee in ('test-admin-user','test-simple-user');

````
test_db=# select * from information_schema.table_privileges where grantee in ('test-admin-user','test-simple-user');
 grantor |     grantee      | table_catalog | table_schema | table_name | privilege_type | is_grantable | with_hierarchy 
---------+------------------+---------------+--------------+------------+----------------+--------------+----------------
 ivan-pg | test-admin-user  | test_db       | public       | orders     | INSERT         | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | orders     | SELECT         | YES          | YES
 ivan-pg | test-admin-user  | test_db       | public       | orders     | UPDATE         | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | orders     | DELETE         | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | orders     | TRUNCATE       | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | orders     | REFERENCES     | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | orders     | TRIGGER        | YES          | NO
 ivan-pg | test-simple-user | test_db       | public       | orders     | INSERT         | NO           | NO
 ivan-pg | test-simple-user | test_db       | public       | orders     | SELECT         | NO           | YES
 ivan-pg | test-simple-user | test_db       | public       | orders     | UPDATE         | NO           | NO
 ivan-pg | test-simple-user | test_db       | public       | orders     | DELETE         | NO           | NO
 ivan-pg | test-admin-user  | test_db       | public       | clients    | INSERT         | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | clients    | SELECT         | YES          | YES
 ivan-pg | test-admin-user  | test_db       | public       | clients    | UPDATE         | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | clients    | DELETE         | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | clients    | TRUNCATE       | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | clients    | REFERENCES     | YES          | NO
 ivan-pg | test-admin-user  | test_db       | public       | clients    | TRIGGER        | YES          | NO
 ivan-pg | test-simple-user | test_db       | public       | clients    | INSERT         | NO           | NO
 ivan-pg | test-simple-user | test_db       | public       | clients    | SELECT         | NO           | YES
 ivan-pg | test-simple-user | test_db       | public       | clients    | UPDATE         | NO           | NO
 ivan-pg | test-simple-user | test_db       | public       | clients    | DELETE         | NO           | NO
(22 rows)
````

список пользователей с правами над таблицами test_db:

SELECT table_name, grantee, privilege_type FROM information_schema.role_table_grants WHERE table_name='orders';
SELECT table_name, grantee, privilege_type FROM information_schema.role_table_grants WHERE table_name='clients';

````
test_db=# SELECT table_name, grantee, privilege_type FROM information_schema.role_table_grants WHERE table_name='orders';
 table_name |     grantee      | privilege_type 
------------+------------------+----------------
 orders     | ivan-pg          | INSERT
 orders     | ivan-pg          | SELECT
 orders     | ivan-pg          | UPDATE
 orders     | ivan-pg          | DELETE
 orders     | ivan-pg          | TRUNCATE
 orders     | ivan-pg          | REFERENCES
 orders     | ivan-pg          | TRIGGER
 orders     | test-admin-user  | INSERT
 orders     | test-admin-user  | SELECT
 orders     | test-admin-user  | UPDATE
 orders     | test-admin-user  | DELETE
 orders     | test-admin-user  | TRUNCATE
 orders     | test-admin-user  | REFERENCES
 orders     | test-admin-user  | TRIGGER
 orders     | test-simple-user | INSERT
 orders     | test-simple-user | SELECT
 orders     | test-simple-user | UPDATE
 orders     | test-simple-user | DELETE
(18 rows)

test_db=# SELECT table_name, grantee, privilege_type FROM information_schema.role_table_grants WHERE table_name='clients';
 table_name |     grantee      | privilege_type 
------------+------------------+----------------
 clients    | ivan-pg          | INSERT
 clients    | ivan-pg          | SELECT
 clients    | ivan-pg          | UPDATE
 clients    | ivan-pg          | DELETE
 clients    | ivan-pg          | TRUNCATE
 clients    | ivan-pg          | REFERENCES
 clients    | ivan-pg          | TRIGGER
 clients    | test-admin-user  | INSERT
 clients    | test-admin-user  | SELECT
 clients    | test-admin-user  | UPDATE
 clients    | test-admin-user  | DELETE
 clients    | test-admin-user  | TRUNCATE
 clients    | test-admin-user  | REFERENCES
 clients    | test-admin-user  | TRIGGER
 clients    | test-simple-user | INSERT
 clients    | test-simple-user | SELECT
 clients    | test-simple-user | UPDATE
 clients    | test-simple-user | DELETE
(18 rows)
````

---

## Задача 3

Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

````
Таблица orders
Наименование 	цена
Шоколад 	10
Принтер 	3000
Книга 	        500
Монитор 	7000
Гитара 	        4000

Таблица clients
ФИО 	                Страна проживания
Иванов Иван Иванович 	USA
Петров Петр Петрович 	Canada
Иоганн Себастьян Бах 	Japan
Ронни Джеймс Дио 	Russia
Ritchie Blackmore 	Russia
````

Используя SQL синтаксис:

    вычислите количество записей для каждой таблицы
    приведите в ответе:
        запросы
        результаты их выполнения.


### Решение:
````
INSERT INTO orders VALUES (1, 'Шоколад', 10), (2, 'Принтер', 3000), (3, 'Книга', 500), (4, 'Монитор', 7000), (5, 'Гитара', 4000);

INSERT INTO clients VALUES (1, 'Иванов Иван Иванович', 'USA'), (2, 'Петров Петр Петрович', 'Canada'), (3, 'Иоганн Себастьян Бах', 'Japan'), (4, 'Ронни Джеймс Дио', 'Russia'), (5, 'Ritchie Blackmore', 'Russia');

SELECT COUNT(*) FROM orders;

SELECT COUNT(*) FROM clients;
````

````
test_db=# SELECT COUNT(*) FROM orders;
 count 
-------
     5
(1 row)

test_db=# SELECT COUNT(*) FROM clients;
 count 
-------
     5
(1 row)
````

---

## Задача 4
Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:
````
ФИО 	                Заказ
Иванов Иван Иванович 	Книга
Петров Петр Петрович 	Монитор
Иоганн Себастьян Бах 	Гитара
````

Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.

Подсказк - используйте директиву UPDATE.

### Решение:
````
test_db=# SELECT * FROM orders;
 id | order_name | price 
----+------------+-------
  1 | Шоколад    |    10
  2 | Принтер    |  3000
  3 | Книга      |   500
  4 | Монитор    |  7000
  5 | Гитара     |  4000
(5 rows)

test_db=# SELECT * FROM clients;
 id |      last_name       | country | order_number 
----+----------------------+---------+--------------
  1 | Иванов Иван Иванович | USA     |             
  2 | Петров Петр Петрович | Canada  |             
  3 | Иоганн Себастьян Бах | Japan   |             
  4 | Ронни Джеймс Дио     | Russia  |             
  5 | Ritchie Blackmore    | Russia  |             
(5 rows)
````

UPDATE clients SET order_number=3 WHERE id=1;

UPDATE clients SET order_number=4 WHERE id=2;

UPDATE clients SET order_number=5 WHERE id=3;

SQL-запросы для выполнения данных операций:

SELECT * FROM clients;

SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса:

SELECT * FROM clients WHERE order_number IS NOT NULL;

````
test_db=# SELECT * FROM clients;
 id |      last_name       | country | order_number 
----+----------------------+---------+--------------
  4 | Ронни Джеймс Дио     | Russia  |             
  5 | Ritchie Blackmore    | Russia  |             
  1 | Иванов Иван Иванович | USA     |            3
  2 | Петров Петр Петрович | Canada  |            4
  3 | Иоганн Себастьян Бах | Japan   |            5
(5 rows)

test_db=# SELECT * FROM clients WHERE order_number IS NOT NULL;
 id |      last_name       | country | order_number 
----+----------------------+---------+--------------
  1 | Иванов Иван Иванович | USA     |            3
  2 | Петров Петр Петрович | Canada  |            4
  3 | Иоганн Себастьян Бах | Japan   |            5
(3 rows)
````
---

## Задача 5
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).
Приведите получившийся результат и объясните что значат полученные значения.

### Решение:
EXPLAIN SELECT * FROM clients WHERE order_number IS NOT NULL;

````
test_db=# EXPLAIN SELECT * FROM clients WHERE order_number IS NOT NULL;
                         QUERY PLAN                         
------------------------------------------------------------
 Seq Scan on clients  (cost=0.00..14.20 rows=418 width=164)
   Filter: (order_number IS NOT NULL)
(2 rows)
````
В выводе команды EXPLAIN для каждого узла в дереве плана отводится одна строка, где показывается базовый тип узла плюс оценка стоимости выполнения данного узла, которую сделал для него планировщик. 
Если для узла выводятся дополнительные свойства, в вывод могут добавляться дополнительные строки, с отступом от основной информации узла. В самой первой строке (основной строке самого верхнего узла) 
выводится общая стоимость выполнения для всего плана. Именно это значение планировщик старается минимизировать.

Числа, перечисленные в скобках (слева направо), имеют следующий смысл: 
- приблизительная стоимость запуска. Это время, которое проходит, прежде чем начнётся этап вывода данных, например для сортирующего узла это время сортировки;
- приблизительная общая стоимость. Она вычисляется в предположении, что узел плана выполняется до конца, то есть возвращает все доступные строки. На практике родительский узел может досрочно прекратить чтение строк дочернего;
- ожидаемое число строк, которое должен вывести этот узел плана. При этом так же предполагается, что узел выполняется до конца;
- ожидаемый средний размер строк, выводимых этим узлом плана (в байтах).

Во второй строке вывода EXPLAIN показано, что условие WHERE применено как «фильтр» к узлу плана Seq Scan (Последовательное сканирование). 
Это означает, что узел плана проверяет это условие для каждого просканированного им узла и выводит только те строки, которые удовлетворяют ему.
---

## Задача 6
Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).
Остановите контейнер с PostgreSQL (но не удаляйте volumes).
Поднимите новый пустой контейнер с PostgreSQL.
Восстановите БД test_db в новом контейнере.
Приведите список операций, который вы применяли для бэкапа данных и восстановления.

### Решение:
Создаем бэкап БД test_db:

pg_dump -U ivan-pg test_db -f /var/lib/postgresql/data/dump_test_db.sql

или												   
docker exec -t pgSQL12 pg_dump -U ivan-pg test_db -f /var/lib/postgresql/data/dump_test_db.sql

Останавливаем контейнер с PostgreSQL:
````
ivan@HP-Pavilion-dv6:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED       STATUS                            PORTS                                       NAMES
43dc5352174b   postgres:12   "docker-entrypoint.s…"   9 hours ago   Exited (255) About a minute ago   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   pgSQL12
ivan@HP-Pavilion-dv6:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
ivan@HP-Pavilion-dv6:~$ 
````

Поднимаем новый пустой контейнер с PostgreSQL:

docker run --name pgSQL12_new -e POSTGRES_USER=ivan-pg -e POSTGRES_PASSWORD=12345 -p 5432:5432 -it -v PostgreSQL_volume_1:/var/lib/postgresql/data -v PostgreSQL_volume_2:/var/lib/postgresql postgres:12

````
ivan@HP-Pavilion-dv6:~$ docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                           PORTS                                       NAMES
48ef8ea8a7c6   postgres:12   "docker-entrypoint.s…"   12 seconds ago   Up 11 seconds                    0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   pgSQL12_new
43dc5352174b   postgres:12   "docker-entrypoint.s…"   10 hours ago     Exited (255) About an hour ago   0.0.0.0:5432->5432/tcp, :::5432->5432/tcp   pgSQL12
ivan@HP-Pavilion-dv6:~$
````

Восстанавливаем БД test_db в новом контейнере:

docker exec -i pgSQL12_new psql -U ivan-pg -d test_db -f /var/lib/postgresql/data/dump_test_db.sql

````
ivan@HP-Pavilion-dv6:~$ docker exec -i pgSQL12_new psql -U ivan-pg -d test_db -f /var/lib/postgresql/data/dump_test_db.sql
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
psql:/var/lib/postgresql/data/dump_test_db.sql:34: ERROR:  relation "clients" already exists
ALTER TABLE
psql:/var/lib/postgresql/data/dump_test_db.sql:49: ERROR:  relation "clients_id_seq" already exists
ALTER TABLE
ALTER SEQUENCE
psql:/var/lib/postgresql/data/dump_test_db.sql:71: ERROR:  relation "orders" already exists
ALTER TABLE
psql:/var/lib/postgresql/data/dump_test_db.sql:86: ERROR:  relation "orders_id_seq" already exists
ALTER TABLE
ALTER SEQUENCE
ALTER TABLE
ALTER TABLE
psql:/var/lib/postgresql/data/dump_test_db.sql:122: ERROR:  duplicate key value violates unique constraint "clients_pkey"
DETAIL:  Key (id)=(4) already exists.
CONTEXT:  COPY clients, line 1
psql:/var/lib/postgresql/data/dump_test_db.sql:135: ERROR:  duplicate key value violates unique constraint "orders_pkey"
DETAIL:  Key (id)=(1) already exists.
CONTEXT:  COPY orders, line 1
 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

psql:/var/lib/postgresql/data/dump_test_db.sql:157: ERROR:  multiple primary keys for table "clients" are not allowed
psql:/var/lib/postgresql/data/dump_test_db.sql:165: ERROR:  multiple primary keys for table "orders" are not allowed
psql:/var/lib/postgresql/data/dump_test_db.sql:173: ERROR:  constraint "clients_order_number_fkey" for relation "clients" already exists
GRANT
GRANT
GRANT
GRANT
ivan@HP-Pavilion-dv6:~$
````
---

