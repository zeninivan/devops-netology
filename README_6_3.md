# Домашнее задание к занятию "6.3. MySQL"

## Задача 1
Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.
Изучите бэкап БД и восстановитесь из него.
Перейдите в управляющую консоль mysql внутри контейнера.
Используя команду \h получите список управляющих команд.
Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД.
Подключитесь к восстановленной БД и получите список таблиц из этой БД.
Приведите в ответе количество записей с price > 300.
В следующих заданиях мы будем продолжать работу с данным контейнером.

### Решение:

Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.

````
ivan@HP-Pavilion-dv6:~$ docker pull mysql:8.0
8.0: Pulling from library/mysql
32c1bf40aba1: Pull complete 
3ac22f3a638d: Pull complete 
b1e7273ed05e: Pull complete 
20be45a0c6ab: Pull complete 
410a229693ff: Pull complete 
1ce71e3a9b88: Pull complete 
c93c823af05b: Pull complete 
c6752c4d09c7: Pull complete 
d7f2cfe3efcb: Pull complete 
916f32cb0394: Pull complete 
0d62a5f9a14f: Pull complete 
Digest: sha256:ce2ae3bd3e9f001435c4671cf073d1d5ae55d138b16927268474fc54ba09ed79
Status: Downloaded newer image for mysql:8.0
docker.io/library/mysql:8.0

ivan@HP-Pavilion-dv6:~$ docker volume create MySQL_vol

ivan@HP-Pavilion-dv6:~$ docker volume ls
DRIVER    VOLUME NAME
local     035a7a069b1d08087633b36cfc61fcf44ccb3a64ef3baa5fbf3d20cabd6e9fad
local     MySQL_vol
local     PostgreSQL_volume_1
local     PostgreSQL_volume_2
local     my-vol
````

Запускаем контейнер:
````
docker run --rm --name MySQL-docker -e MYSQL_ROOT_PASSWORD=my123sql -it -p 33060:3306 -v MySQL_vol:/var/opt/mysql -d mysql:8.0
````

Сначала используйте команду docker exec для создания папки резервного копирования. Следующая команда создает каталог /var/opt/mysql/backup внутри контейнера SQL Server.
````
sudo docker exec -it MySQL-docker mkdir /var/opt/mysql/backup
````

Используйте docker cp, чтобы скопировать файл резервной копии в контейнер в каталог /var/opt/mysql/backup.
````
sudo docker cp test_dump.sql MySQL-docker:/var/opt/mysql/backup
````

Изучите бэкап БД и восстановитесь из него.
Перейдите в управляющую консоль mysql внутри контейнера.

Подключаемся к контейнеру:
````
docker exec -it d57310779f48 mysql -p
````

Создайте новую БД, чтобы переместить в неё данные из дампа:
````
CREATE DATABASE test_db;
exit
````

Перенаправьте дамп-файл в файл БД:
````
docker exec -i MySQL-docker mysql -uroot -pmy123sql test_db < test_dump.sql
````

Снова подключаемся в контейнер:
````
docker exec -it d57310779f48 mysql -p
SHOW DATABASES;
USE test_db;

ysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
+--------------------+
5 rows in set (0.00 sec)

mysql> USE test_db;
Database changed
````

Используя команду \h получите список управляющих команд.
Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД.
````
mysql> status
--------------
mysql  Ver 8.0.30 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:		13
Current database:	test_db
Current user:		root@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		8.0.30 MySQL Community Server - GPL
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	latin1
Conn.  characterset:	latin1
UNIX socket:		/var/run/mysqld/mysqld.sock
Binary data as:		Hexadecimal
Uptime:			1 hour 23 min 52 sec

Threads: 2  Questions: 73  Slow queries: 0  Opens: 185  Flush tables: 3  Open tables: 103  Queries per second avg: 0.014
--------------
mysql>
````

Подключитесь к восстановленной БД и получите список таблиц из этой БД.
````
SHOW TABLES;
mysql> SHOW TABLES;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)
````

Приведите в ответе количество записей с price > 300.
````
mysql> select count(*) from orders where price>300;
+----------+
| count(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)
````
В следующих заданиях мы будем продолжать работу с данным контейнером.

---

## Задача 2
Создайте пользователя test в БД c паролем test-pass, используя:
    плагин авторизации mysql_native_password
    срок истечения пароля - 180 дней
    количество попыток авторизации - 3
    максимальное количество запросов в час - 100
    аттрибуты пользователя:
        Фамилия "Pretty"
        Имя "James"

Предоставьте привелегии пользователю test на операции SELECT базы test_db.

Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю test и приведите в ответе к задаче.

### Решение:

Создание пользователя test в БД c паролем test-pass
````
mysql> create user 'test'@'localhost' 
    ->     identified with mysql_native_password by 'test-pass' 
    ->     with max_queries_per_hour 100
    ->     password expire interval 180 day 
    ->     failed_login_attempts 3 
    ->     attribute '{"fname": "James","lname": "Pretty"}';
Query OK, 0 rows affected (0.02 sec)
````

Предоставьте привелегии пользователю test на операции SELECT базы test_db.
````
mysql> GRANT Select ON test_db.orders TO 'test'@'localhost';
Query OK, 0 rows affected, 1 warning (0.18 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)
````

Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю test и приведите в ответе к задаче.
````
mysql> SELECT * FROM INFORMATION_SCHEMA.USER_ATTRIBUTES WHERE USER='test';
+------+-----------+---------------------------------------+
| USER | HOST      | ATTRIBUTE                             |
+------+-----------+---------------------------------------+
| test | localhost | {"fname": "James", "lname": "Pretty"} |
+------+-----------+---------------------------------------+
1 row in set (0.00 sec)

mysql>
````
---

## Задача 3
Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.
Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.
Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:

    на MyISAM
    на InnoDB


### Решение:

Установите профилирование SET profiling = 1. 
````
mysql> set profiling=1;
Query OK, 0 rows affected, 1 warning (0.00 sec)
````

Изучите вывод профилирования команд SHOW PROFILES;.
````
mysql> SHOW PROFILES;
+----------+------------+-----------------------------------------+
| Query_ID | Duration   | Query                                   |
+----------+------------+-----------------------------------------+
|        1 | 0.00410650 | show table status where name = 'orders' |
+----------+------------+-----------------------------------------+
1 row in set, 1 warning (0.01 sec)
````

Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.
````
mysql> show table status where name = 'orders';
+--------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+--------------------+----------+----------------+---------+
| Name   | Engine | Version | Row_format | Rows | Avg_row_length | Data_length | Max_data_length | Index_length | Data_free | Auto_increment | Create_time         | Update_time         | Check_time | Collation          | Checksum | Create_options | Comment |
+--------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+--------------------+----------+----------------+---------+
| orders | InnoDB |      10 | Dynamic    |    5 |           3276 |       16384 |               0 |            0 |         0 |              6 | 2022-08-21 18:43:32 | 2022-08-21 18:43:32 | NULL       | utf8mb4_0900_ai_ci |     NULL |                |         |
+--------+--------+---------+------------+------+----------------+-------------+-----------------+--------------+-----------+----------------+---------------------+---------------------+------------+--------------------+----------+----------------+---------+
1 row in set (0.00 sec)
````
Используется engine InnoDB.

Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:
    на MyISAM
    на InnoDB
````
mysql> ALTER TABLE orders ENGINE = MyISAM;
Query OK, 5 rows affected (0.06 sec)
Records: 5  Duplicates: 0  Warnings: 0
````
Время переключения на  MyISAM: (0.06 sec)

````
mysql> ALTER TABLE orders ENGINE = InnoDB;
Query OK, 5 rows affected (0.08 sec)
Records: 5  Duplicates: 0  Warnings: 0
````
Время переключения на  InnoDB: (0.08 sec)
````
mysql> show profiles;
+----------+------------+-----------------------------------------+
| Query_ID | Duration   | Query                                   |
+----------+------------+-----------------------------------------+
|        1 | 0.00410650 | show table status where name = 'orders' |
|        2 | 0.06025025 | ALTER TABLE orders ENGINE = MyISAM      |
|        3 | 0.07241450 | ALTER TABLE orders ENGINE = InnoDB      |
+----------+------------+-----------------------------------------+
3 rows in set, 1 warning (0.00 sec)
````

---

## Задача 4
Изучите файл my.cnf в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):

    Скорость IO важнее сохранности данных
    Нужна компрессия таблиц для экономии места на диске
    Размер буффера с незакомиченными транзакциями 1 Мб
    Буффер кеширования 30% от ОЗУ
    Размер файла логов операций 100 Мб

Приведите в ответе измененный файл my.cnf.

### Решение:

К сожалению, решение подготовить не успел.

---
