# devops-netology
Home work 06-db-04-postgresql

Домашнее задание к занятию "6.4. PostgreSQL"  
Задача 1  
Используя docker поднимите инстанс PostgreSQL (версию 13). Данные БД сохраните в volume.
````
 docker run -e POSTGRES_PASSWORD=password -e PGDATA=/var/lib/postgresql/data -p 5432:5432 -v postgres13data:/var/lib/postgresql/data -d postgres:13.6 

````
Подключитесь к БД PostgreSQL используя psql.

Воспользуйтесь командой \? для вывода подсказки по имеющимся в psql управляющим командам.

Найдите и приведите управляющие команды для:

вывода списка БД
подключения к БД
вывода списка таблиц
вывода описания содержимого таблиц
выхода из psql

````
\l[+]   [PATTERN]      list databases
\c[onnect] {[DBNAME|- USER|- HOST|- PORT|-] | conninfo}  connect to new database (currently "postgres")
\d[S+]                 list tables, views, and sequences
\d[S+]  NAME           describe table, view, sequence, or index
\q
````
Задача 2  
Используя psql создайте БД test_database.
````
postgres@06646e1bb6a0:/$ psql
psql (13.6 (Debian 13.6-1.pgdg110+1))
Type "help" for help.

postgres=# create database test_database;
CREATE DATABASE

````
Изучите бэкап БД.

Восстановите бэкап БД в test_database.
````
  psql -U postgres -d test_database -f test_dump.sql

````
Перейдите в управляющую консоль psql внутри контейнера.

Подключитесь к восстановленной БД и проведите операцию ANALYZE для сбора статистики по таблице.
````
test_database=# ANALYZE VERBOSE public.orders;
INFO:  analyzing "public.orders"
INFO:  "orders": scanned 1 of 1 pages, containing 8 live rows and 0 dead rows; 8 rows in sample, 8 estimated total rows
ANALYZE

````
Используя таблицу pg_stats, найдите столбец таблицы orders с наибольшим средним значением размера элементов в байтах.

Приведите в ответе команду, которую вы использовали для вычисления и полученный результат.
````
test_database=# select attname, avg_width from pg_stats where tablename = 'orders'
order by avg_width DESC limit 1;
 attname | avg_width
---------+-----------
 title   |        16
(1 row)


````
Задача 3  
Архитектор и администратор БД выяснили, что ваша таблица orders разрослась до невиданных размеров и поиск по ней занимает долгое время. Вам, как успешному выпускнику курсов DevOps в нетологии предложили провести разбиение таблицы на 2 (шардировать на orders_1 - price>499 и orders_2 - price<=499).

Предложите SQL-транзакцию для проведения данной операции.
````
create table ord1 (LIKE orders);
INSERT INTO ord1 SELECT * FROM orders where price > 499;

create table ord2 (LIKE orders);
INSERT INTO ord2 SELECT * FROM orders where price <= 499;
````
Можно ли было изначально исключить "ручное" разбиение при проектировании таблицы orders?
````
Да, с помощью применения декларативного секционирования/партиционирования.

````
Задача 4
Используя утилиту pg_dump создайте бекап БД test_database.
````
 pg_dump -U postgres test_database > /var/lib/postgresql/tesdb.pgdmp
````
Как бы вы доработали бэкап-файл, чтобы добавить уникальность значения столбца title для таблиц test_database?

````
В запросе CREATE TABLE добавить ограничение UNIQUE на столбец title
````