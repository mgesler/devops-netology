# devops-netology
Home work 06-db-02-sql  

Домашнее задание к занятию "6.2. SQL"  

Задача 1  
Используя docker поднимите инстанс PostgreSQL (версию 12) c 2 volume, в который будут складываться данные БД и бэкапы.  
Приведите получившуюся команду или docker-compose манифест.
````
docker run -e POSTGRES_PASSWORD=password -e PGDATA=/var/lib/postgresql/data -p 5432:5432 -v /tmp/postgres/data:/var/lib/postgresql/data -v /tmp/postgres/backups:/var/lib/postgresql/backups -d postgres:12.10
````
Задача 2  
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
![](https://github.com/mgesler/devops-netology/blob/main/pic/sql-l.jpg)
описание таблиц (describe)
![](https://github.com/mgesler/devops-netology/blob/main/pic/sql-d.jpg)
SQL-запрос для выдачи списка пользователей с правами над таблицами test_db
````
SELECT DISTINCT grantee, privilege_type FROM information_schema.table_privileges where table_catalog = 'test_db';
````
список пользователей с правами над таблицами test_db
![](https://github.com/mgesler/devops-netology/blob/main/pic/sql-grant.jpg)

Задача 3  
Используя SQL синтаксис - наполните таблицы следующими тестовыми данными:

Таблица orders

Наименование	цена
Шоколад	10
Принтер	3000
Книга	500
Монитор	7000
Гитара	4000
````
INSERT INTO orders (name, price) VALUES ('Шоколад', 10), ('Принтер', 3000),('Книга', 500), ('Монитор', 7000),('Гитара', 4000); 
````
Таблица clients

ФИО	Страна проживания
Иванов Иван Иванович	USA
Петров Петр Петрович	Canada
Иоганн Себастьян Бах	Japan
Ронни Джеймс Дио	Russia
Ritchie Blackmore	Russia
````
INSERT INTO clients ("last_name", "country") VALUES ('Иванов Иван Иванович','USA'),('Петров Петр Петрович','Canada'),('Иоганн Себастьян Бах','Japan'),('Ронни Джеймс Дио','Russia'),('Ritchie Blackmore','Russia'); 
````

Используя SQL синтаксис:

вычислите количество записей для каждой таблицы
приведите в ответе:
запросы
результаты их выполнения.
````
test_db=# SELECT COUNT(*) FROM orders; 
 count 
-------
     5

test_db=# SELECT COUNT(*) FROM clients; 
 count 
-------
     5
     
````

Задача 4  
Часть пользователей из таблицы clients решили оформить заказы из таблицы orders.

Используя foreign keys свяжите записи из таблиц, согласно таблице:

ФИО	Заказ
Иванов Иван Иванович	Книга
Петров Петр Петрович	Монитор
Иоганн Себастьян Бах	Гитара
Приведите SQL-запросы для выполнения данных операций.

Приведите SQL-запрос для выдачи всех пользователей, которые совершили заказ, а также вывод данного запроса.

Подсказк - используйте директиву UPDATE.

````
UPDATE clients SET order_id = 3 WHERE id = 1;
UPDATE clients SET order_id = 4 WHERE id = 2;
UPDATE clients SET order_id = 3 WHERE id = 3;

test_db=# SELECT * FROM clients INNER JOIN orders o on o.id = clients.order_id;
 id |      last_name       | country | order_id | id |  name   | price 
----+----------------------+---------+----------+----+---------+-------
  1 | Иванов Иван Иванович | USA     |        3 |  3 | Книга   |   500
  2 | Петров Петр Петрович | Canada  |        4 |  4 | Монитор |  7000
  3 | Иоганн Себастьян Бах | Japan   |        3 |  3 | Книга   |   500
````

Задача 5  
Получите полную информацию по выполнению запроса выдачи всех пользователей из задачи 4 (используя директиву EXPLAIN).

Приведите получившийся результат и объясните что значат полученные значения.
````
test_db=# explain
select * from clients where order_id is not null;
                        QUERY PLAN                         
-----------------------------------------------------------
 Seq Scan on clients  (cost=0.00..18.10 rows=806 width=72)
 
 В результете запроса была последовательно просканирована таблица clients и были отобраны записи, 
 подходящие под условие "заказ не пуст", т.е. как раз клиенты, имеющие заказы.
````


Задача 6  
Создайте бэкап БД test_db и поместите его в volume, предназначенный для бэкапов (см. Задачу 1).

Остановите контейнер с PostgreSQL (но не удаляйте volumes).

Поднимите новый пустой контейнер с PostgreSQL.

Восстановите БД test_db в новом контейнере.

Приведите список операций, который вы применяли для бэкапа данных и восстановления.

````
pg_dump -U postgres test_db > /var/lib/postgresql/backups/test_db.sql

psql -U postgres test_db < /var/lib/postgresql/backups/test_db.sql
````