# devops-netology
Home work 06-db-03-mysql

Домашнее задание к занятию "6.3. MySQL"

Задача 1  
Используя docker поднимите инстанс MySQL (версию 8). Данные БД сохраните в volume.
````
docker volume create vol_mysql
docker run --rm --name mysql8 -e MYSQL_ROOT_PASSWORD=123456 -ti -p 3306:3306 -v vol_mysql:/etc/mysql/ -d mysql:8.0.28-debian
````
Изучите бэкап БД и восстановитесь из него.
````
mysql -p
create database test_db;
exit
 mysql -u root -p test_db < test_dump.sql
````
Перейдите в управляющую консоль mysql внутри контейнера.

Используя команду \h получите список управляющих команд.
````
mysql> \h

For information about MySQL products and services, visit:
   http://www.mysql.com/
For developer information, including the MySQL Reference Manual, visit:
   http://dev.mysql.com/
To buy MySQL Enterprise support, training, or other products, visit:
   https://shop.mysql.com/

List of all MySQL commands:
Note that all text commands must be first on line and end with ';'
?         (\?) Synonym for `help'.
clear     (\c) Clear the current input statement.
connect   (\r) Reconnect to the server. Optional arguments are db and host.
delimiter (\d) Set statement delimiter.
edit      (\e) Edit command with $EDITOR.
ego       (\G) Send command to mysql server, display result vertically.
exit      (\q) Exit mysql. Same as quit.
go        (\g) Send command to mysql server.
help      (\h) Display this help.
nopager   (\n) Disable pager, print to stdout.
notee     (\t) Don't write into outfile.
pager     (\P) Set PAGER [to_pager]. Print the query results via PAGER.
print     (\p) Print current command.
prompt    (\R) Change your mysql prompt.
quit      (\q) Quit mysql.
rehash    (\#) Rebuild completion hash.
source    (\.) Execute an SQL script file. Takes a file name as an argument.
status    (\s) Get status information from the server.
system    (\!) Execute a system shell command.
tee       (\T) Set outfile [to_outfile]. Append everything into given outfile.
use       (\u) Use another database. Takes database name as argument.
charset   (\C) Switch to another charset. Might be needed for processing binlog with multi-byte charsets.
warnings  (\W) Show warnings after every statement.
nowarning (\w) Don't show warnings after every statement.
resetconnection(\x) Clean session context.
query_attributes Sets string parameters (name1 value1 name2 value2 ...) for the next query to pick up.

For server side help, type 'help contents'

````
Найдите команду для выдачи статуса БД и приведите в ответе из ее вывода версию сервера БД.
````
mysql> \s
--------------
mysql  Ver 8.0.28 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:          13
Current database:
Current user:           root@localhost
SSL:                    Not in use
Current pager:          stdout
Using outfile:          ''
Using delimiter:        ;
Server version:         8.0.28 MySQL Community Server - GPL
Protocol version:       10
Connection:             Localhost via UNIX socket
Server characterset:    utf8mb4
Db     characterset:    utf8mb4
Client characterset:    latin1
Conn.  characterset:    latin1
UNIX socket:            /var/run/mysqld/mysqld.sock
Binary data as:         Hexadecimal
Uptime:                 21 min 22 sec

Threads: 2  Questions: 10  Slow queries: 0  Opens: 120  Flush tables: 3  Open tables: 39  Queries per second avg: 0.007
--------------



````
Подключитесь к восстановленной БД и получите список таблиц из этой БД.
````
mysql> use test_db;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> show tables from test_db;
+-------------------+
| Tables_in_test_db |
+-------------------+
| orders            |
+-------------------+
1 row in set (0.00 sec)

````
Приведите в ответе количество записей с price > 300.
````
mysql>  select COUNT(*) from orders where price > 300;
+----------+
| COUNT(*) |
+----------+
|        1 |
+----------+
1 row in set (0.00 sec)


````
В следующих заданиях мы будем продолжать работу с данным контейнером.  

Задача 2  
Создайте пользователя test в БД c паролем test-pass, используя:

плагин авторизации mysql_native_password
срок истечения пароля - 180 дней
количество попыток авторизации - 3
максимальное количество запросов в час - 100
аттрибуты пользователя:
Фамилия "Pretty"
Имя "James"
Предоставьте привелегии пользователю test на операции SELECT базы test_db.
````
CREATE USER 'test'@'localhost' IDENTIFIED BY '1234' WITH MAX_QUERIES_PER_HOUR 100 PASSWORD EXPIRE INTERVAL 180 DAY FAILED_LOGIN_ATTEMPTS 3 
PASSWORD_LOCK_TIME 2 ATTRIBUTE '{"fname": "Pretty", "lname": "James"}';

GRANT Select ON test_db.orders TO 'test'@'localhost';
````
Используя таблицу INFORMATION_SCHEMA.USER_ATTRIBUTES получите данные по пользователю test и приведите в ответе к задаче.
````
mysql> SELECT * FROM INFORMATION_SCHEMA.USER_ATTRIBUTES WHERE USER='test';
+------+-----------+---------------------------------------+
| USER | HOST      | ATTRIBUTE                             |
+------+-----------+---------------------------------------+
| test | localhost | {"fname": "Pretty", "lname": "James"} |
+------+-----------+---------------------------------------+
1 row in set (0.00 sec)


````
Задача 3  
Установите профилирование SET profiling = 1. Изучите вывод профилирования команд SHOW PROFILES;.
````
SET profiling = 1;

mysql> SHOW PROFILES;
+----------+------------+----------------------+
| Query_ID | Duration   | Query                |
+----------+------------+----------------------+
|        1 | 0.00042200 | .                    |
|        2 | 0.01052525 | show tables          |
|        3 | 0.00185150 | select * from orders |
+----------+------------+----------------------+
3 rows in set, 1 warning (0.00 sec)

````
Исследуйте, какой engine используется в таблице БД test_db и приведите в ответе.
````
mysql> SELECT TABLE_NAME, ENGINE FROM information_schema.TABLES where TABLE_SCHEMA = 'test_db';
+------------+--------+
| TABLE_NAME | ENGINE |
+------------+--------+
| orders     | InnoDB |
+------------+--------+
1 row in set (0.00 sec)


````

Измените engine и приведите время выполнения и запрос на изменения из профайлера в ответе:

на MyISAM
````
 ALTER TABLE orders ENGINE = MyIsam;
 mysql> SELECT TABLE_NAME, ENGINE FROM information_schema.TABLES where TABLE_SCHEMA = 'test_db';
+------------+--------+
| TABLE_NAME | ENGINE |
+------------+--------+
| orders     | MyISAM |
+------------+--------+
1 row in set (0.00 sec)

mysql> SHOW PROFILES;
+----------+------------+-----------------------------------------------------------------------------------------+
| Query_ID | Duration   | Query                                                                                   |
+----------+------------+-----------------------------------------------------------------------------------------+
|        1 | 0.00042200 | .                                                                                       |
|        2 | 0.01052525 | show tables                                                                             |
|        3 | 0.00185150 | select * from orders                                                                    |
|        4 | 0.23730050 | ALTER TABLE orders ENGINE = MyIsam                                                      |
|        5 | 0.00458575 | SELECT TABLE_NAME, ENGINE FROM information_schema.TABLES where TABLE_SCHEMA = 'test_db' |
+----------+------------+-----------------------------------------------------------------------------------------+
5 rows in set, 1 warning (0.00 sec)


````
на InnoDB
````
ALTER TABLE orders ENGINE = InnoDB;
mysql> SHOW PROFILES;
+----------+------------+-----------------------------------------------------------------------------------------+
| Query_ID | Duration   | Query                                                                                   |
+----------+------------+-----------------------------------------------------------------------------------------+
|        1 | 0.00042200 | .                                                                                       |
|        2 | 0.01052525 | show tables                                                                             |
|        3 | 0.00185150 | select * from orders                                                                    |
|        4 | 0.23730050 | ALTER TABLE orders ENGINE = MyIsam                                                      |
|        5 | 0.00458575 | SELECT TABLE_NAME, ENGINE FROM information_schema.TABLES where TABLE_SCHEMA = 'test_db' |
|        6 | 0.30275075 | ALTER TABLE orders ENGINE = InnoDB                                                      |
+----------+------------+-----------------------------------------------------------------------------------------+
6 rows in set, 1 warning (0.00 sec)

mysql> SHOW PROFILE FOR QUERY 4;
+--------------------------------+----------+
| Status                         | Duration |
+--------------------------------+----------+
| starting                       | 0.000604 |
| Executing hook on transaction  | 0.000025 |
| starting                       | 0.000204 |
| checking permissions           | 0.000043 |
| checking permissions           | 0.000026 |
| init                           | 0.000182 |
| Opening tables                 | 0.002934 |
| setup                          | 0.000497 |
| creating table                 | 0.006041 |
| waiting for handler commit     | 0.000028 |
| waiting for handler commit     | 0.030928 |
| After create                   | 0.001302 |
| System lock                    | 0.000025 |
| copy to tmp table              | 0.000494 |
| waiting for handler commit     | 0.000038 |
| waiting for handler commit     | 0.000067 |
| waiting for handler commit     | 0.000106 |
| rename result table            | 0.000092 |
| waiting for handler commit     | 0.064875 |
| waiting for handler commit     | 0.000025 |
| waiting for handler commit     | 0.006123 |
| waiting for handler commit     | 0.000019 |
| waiting for handler commit     | 0.055455 |
| waiting for handler commit     | 0.000025 |
| waiting for handler commit     | 0.004812 |
| end                            | 0.038331 |
| query end                      | 0.023732 |
| closing tables                 | 0.000021 |
| waiting for handler commit     | 0.000037 |
| freeing items                  | 0.000095 |
| cleaning up                    | 0.000118 |
+--------------------------------+----------+
31 rows in set, 1 warning (0.01 sec)


````
Задача 4
Изучите файл my.cnf в директории /etc/mysql.

Измените его согласно ТЗ (движок InnoDB):

Скорость IO важнее сохранности данных
Нужна компрессия таблиц для экономии места на диске
Размер буффера с незакомиченными транзакциями 1 Мб
Буффер кеширования 30% от ОЗУ
Размер файла логов операций 100 Мб
Приведите в ответе измененный файл my.cnf.

````
default_storage_engine = InnoDB
innodb-flush-method = O_DSYNC
innodb-compression-level = 9
innodb_log_buffer_size = 1M
innodb_buffer_pool_size = 1G
innodb_log_file_size = 100M

````