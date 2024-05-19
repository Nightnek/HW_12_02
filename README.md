# 12-02 Стасенко Григорий Домашнее задание к занятию «Работа с данными (DDL/DML)» 12-02

## Задание 1
1.1. Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.

1.2. Создайте учётную запись sys_temp.

1.3. Выполните запрос на получение списка пользователей в базе данных. (скриншот)
![image](https://github.com/Nightnek/HW_12_02/assets/127677631/b7cea5d4-d9a5-4fe1-a1ec-8d707bd66d66)


1.4. Дайте все права для пользователя sys_temp.

1.5. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)
![image](https://github.com/Nightnek/HW_12_02/assets/127677631/e88e20e8-5828-4da9-9012-7051ba55bd2d)


1.6. Переподключитесь к базе данных от имени sys_temp.

Для смены типа аутентификации с sha2 используйте запрос:

ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
1.6. По ссылке https://downloads.mysql.com/docs/sakila-db.zip скачайте дамп базы данных.

1.7. Восстановите дамп в базу данных.

1.8. При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)
![image](https://github.com/Nightnek/HW_12_02/assets/127677631/5e77d6f5-c91e-4ec9-af86-2da799a1665f)


Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

````
Создаем пользователя sys_temp:
create user 'sys_temp'@'localhost' identified by 'example';

mysql> select User, host, plugin from mysql.user;
+------------------+-----------+-----------------------+
| User             | host      | plugin                |
+------------------+-----------+-----------------------+
| root             | %         | caching_sha2_password |
| mysql.infoschema | localhost | caching_sha2_password |
| mysql.session    | localhost | caching_sha2_password |
| mysql.sys        | localhost | caching_sha2_password |
| root             | localhost | caching_sha2_password |
| sys_temp         | localhost | caching_sha2_password |
+------------------+-----------+-----------------------+

Даем права на все пользователю sys_temp:
grant all on mysql.* to 'sys_temp'@'localhost';

show grants for 'sys_temp'@'localhost';
+-------------------------------------------------------------+
| Grants for sys_temp@localhost                               |
+-------------------------------------------------------------+
| GRANT USAGE ON *.* TO `sys_temp`@`localhost`                |
| GRANT ALL PRIVILEGES ON `mysql`.* TO `sys_temp`@`localhost` |
+-------------------------------------------------------------+

Заходим:
bin/mysql -h localhost -u sys_temp -p

Подключаем sakila:
/bin/mysql -u root -p
source /var/lib/mysql/sakila-db/sakila-schema.sql
source /var/lib/mysql/sakila-db/sakila-data.sql
use sakila
show full tables;
+----------------------------+------------+
| Tables_in_sakila           | Table_type |
+----------------------------+------------+
| actor                      | BASE TABLE |
| actor_info                 | VIEW       |
| address                    | BASE TABLE |
| category                   | BASE TABLE |
| city                       | BASE TABLE |
| country                    | BASE TABLE |
| customer                   | BASE TABLE |
| customer_list              | VIEW       |
| film                       | BASE TABLE |
| film_actor                 | BASE TABLE |
| film_category              | BASE TABLE |
| film_list                  | VIEW       |
| film_text                  | BASE TABLE |
| inventory                  | BASE TABLE |
| language                   | BASE TABLE |
| nicer_but_slower_film_list | VIEW       |
| payment                    | BASE TABLE |
| rental                     | BASE TABLE |
| sales_by_film_category     | VIEW       |
| sales_by_store             | VIEW       |
| staff                      | BASE TABLE |
| staff_list                 | VIEW       |
| store                      | BASE TABLE |
+----------------------------+------------+
````

## Задание 2
Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца: в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц. Пример: (скриншот/текст)

Название таблицы | Название первичного ключа
customer         | customer_id


````
+-----------------+--------------------------+
|Название таблицы | Название первичного ключа|
+-----------------+--------------------------+
|  customer       | customer_id              |
+-----------------+--------------------------+
|  actor          |  actor_id                |
+-----------------+--------------------------+
|  address        |  address_id              |
+-----------------+--------------------------+
|  category       |  category_id             |
+-----------------+--------------------------+
|  city           |  city_id                 |
+-----------------+--------------------------+
|  country        |  country_id              |
+-----------------+--------------------------+
|  film           |  film_id                 |
+-----------------+--------------------------+
|  film_actor     |  actor_id, film_id       |
+-----------------+--------------------------+
|  film_category  |  film_id, category_id    |
+-----------------+--------------------------+
|  film_text      |  film_id                 |
+-----------------+--------------------------+
|  inventory      |  inventory_id            |
+-----------------+--------------------------+
|  language       |  language_id             |
+-----------------+--------------------------+
|  payment        |  payment_id              |
+-----------------+--------------------------+
|  rental         |  rental_id               |
+-----------------+--------------------------+
|  staff          |  staff_id                |
+-----------------+--------------------------+
|  store          |  store_id                |
+-----------------+--------------------------+

````

## Задание 3*
3.1. Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.
![image](https://github.com/Nightnek/HW_12_02/assets/127677631/43effa48-36b4-41ac-87ea-173ea21bc5cf)

3.2. Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)
![image](https://github.com/Nightnek/HW_12_02/assets/127677631/b653d5f3-1f02-4cb9-8885-accebe997435)

Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

````
mysql> revoke insert, update, drop on sakila.* from 'sys_temp'@'localhost';
Query OK, 0 rows affected, 1 warning (0.00 sec)

mysql> show grants for 'sys_temp'@'localhost';
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Grants for sys_temp@localhost                                                                                                                                                                                        |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO `sys_temp`@`localhost`                                                                                                                                                                         |
| GRANT ALL PRIVILEGES ON `mysql`.* TO `sys_temp`@`localhost`                                                                                                                                                          |
| GRANT SELECT, DELETE, CREATE, REFERENCES, INDEX, ALTER, CREATE TEMPORARY TABLES, LOCK TABLES, EXECUTE, CREATE VIEW, SHOW VIEW, CREATE ROUTINE, ALTER ROUTINE, EVENT, TRIGGER ON `sakila`.* TO `sys_temp`@`localhost` |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
3 rows in set (0.00 sec)
````
