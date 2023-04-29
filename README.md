# Домашнее задание №4

1 создайте новый кластер PostgresSQL 14
> sudo apt update && sudo DEBIAN_FRONTEND=noninteractive apt upgrade -y -q && sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' && wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add - && sudo apt-get update && sudo DEBIAN_FRONTEND=noninteractive apt -y install postgresql-14
 
 ![image](https://user-images.githubusercontent.com/130083589/235295797-d32e8fcc-9f1a-4e11-9aeb-df619d80f121.png)


2 зайдите в созданный кластер под пользователем postgres
sudo -u postgres psql
 
 ![image](https://user-images.githubusercontent.com/130083589/235295829-ab6cfd76-ff69-4c5b-b2ce-a10365676ca7.png)


3 создайте новую базу данных testdb
CREATE DATABASE testdb;
 
 ![image](https://user-images.githubusercontent.com/130083589/235295832-52312c19-3163-46ac-acda-d5d554640516.png)


4 зайдите в созданную базу данных под пользователем postgres
sudo -u postgres psql testdb
 
 ![image](https://user-images.githubusercontent.com/130083589/235295843-aa76fe13-6748-486b-97be-6a488421e053.png)


5 создайте новую схему testnm
CREATE SCHEMA testnm;
 
 ![image](https://user-images.githubusercontent.com/130083589/235295848-cece0b4f-3a82-47d2-b87a-d683b68c596b.png)


6 создайте новую таблицу t1 с одной колонкой c1 типа integer
CREATE TABLE t1 (c1 integer);
 
 ![image](https://user-images.githubusercontent.com/130083589/235295853-04fe5aee-f25d-4840-a79c-a9a87857fb5b.png)


7 вставьте строку со значением c1=1
INSERT INTO t1 (c1) VALUES (1);
 
 ![image](https://user-images.githubusercontent.com/130083589/235295864-b7030261-87f1-4cd9-9d5e-e09f21b08b48.png)


8 создайте новую роль readonly
CREATE ROLE readonly;

![image](https://user-images.githubusercontent.com/130083589/235295873-4d4ca65f-fbb7-4755-92d0-3d248df902e3.png)


9 дайте новой роли право на подключение к базе данных testdb
GRANT CONNECT ON DATABASE testdb TO readonly;
 
 ![image](https://user-images.githubusercontent.com/130083589/235295899-3ec3995d-2764-46eb-9ed2-a80999e502f3.png)


10 дайте новой роли право на использование схемы testnm
GRANT USAGE ON SCHEMA testnm TO readonly;
 
 ![image](https://user-images.githubusercontent.com/130083589/235295922-ca66ed20-c084-40bf-b694-0149f668c0c1.png)


11 дайте новой роли право на select для всех таблиц схемы testnm
GRANT SELECT ON ALL TABLES IN SCHEMA testnm TO readonly;
 
 ![image](https://user-images.githubusercontent.com/130083589/235295971-51d6ab92-cfff-4479-a240-f5555b7fcae7.png)


12 создайте пользователя testread с паролем test123
CREATE USER testread WITH PASSWORD 'test123';
 
 ![image](https://user-images.githubusercontent.com/130083589/235296015-bdeca8d2-9c6b-4c78-8129-6c22cbd505c6.png)


13 дайте роль readonly пользователю testread
GRANT readonly TO testread;
 
 ![image](https://user-images.githubusercontent.com/130083589/235296025-8a610d6e-5384-491e-aa47-04ef28e7c37b.png)


14 зайдите под пользователем testread в базу данных testdb
![image](https://user-images.githubusercontent.com/130083589/235296053-587bc0e2-766a-47c1-b73b-e1bc27d8e770.png)
 
> Т.к. используется «peer» аутентификация, необходимо явно указать, что мы хотим подключиться к БД по сети.

15 сделайте select * from t1;
 
 ![image](https://user-images.githubusercontent.com/130083589/235296152-56a64446-a493-4e73-909c-ecbfb2778e9c.png)


16 получилось? (могло если вы делали сами не по шпаргалке и не упустили один существенный момент про который позже)

> нет

17 напишите что именно произошло в тексте домашнего задания

> нет прав на чтение таблицы

18 у вас есть идеи почему? ведь права то дали?

> попытались выбрать данные из таблицы, принадлежащей схеме public, на которую права не выдавались

19 посмотрите на список таблиц
 
 ![image](https://user-images.githubusercontent.com/130083589/235296219-c010edc7-d2e5-4e67-8242-aadb8825a74a.png)

20 подсказка в шпаргалке под пунктом 20
21 а почему так получилось с таблицей (если делали сами и без шпаргалки то может у вас все нормально)
 ![image](https://user-images.githubusercontent.com/130083589/235296233-97fe5d69-92af-41d9-9621-b163688a5187.png)

22 вернитесь в базу данных testdb под пользователем postgres
23 удалите таблицу t1
 ![image](https://user-images.githubusercontent.com/130083589/235296252-972fb60c-28c0-46fe-bf7c-972f6eae1bf3.png)

24 создайте ее заново но уже с явным указанием имени схемы testnm
 ![image](https://user-images.githubusercontent.com/130083589/235296257-72e6c434-77e6-4cc8-97c7-1698582492e3.png)

25 вставьте строку со значением c1=1
 ![image](https://user-images.githubusercontent.com/130083589/235296270-6242c702-d662-43ee-82f5-03e38ff24828.png)

26 зайдите под пользователем testread в базу данных testdb
27 сделайте select * from testnm.t1;
 ![image](https://user-images.githubusercontent.com/130083589/235296275-ff7ea813-0e4e-47b6-93d9-aba484946643.png)

28 получилось?

> нет

29 есть идеи почему? если нет - смотрите шпаргалку

> таблица t1 была создана после того, как были выданы права на чтение таблиц схемы testnm

30 как сделать так чтобы такое больше не повторялось? если нет идей - смотрите шпаргалку
 ![image](https://user-images.githubusercontent.com/130083589/235296304-81f66889-be9c-42e6-a612-3466966757a8.png)

31 сделайте select * from testnm.t1;
 ![image](https://user-images.githubusercontent.com/130083589/235296317-78708a72-bbd6-44d4-94cc-4dd04a57bd7f.png)

32 получилось?

> да, т.к. мы пересоздали таблицу. Если бы не пересоздали, то нужно было бы явно выдать грант на селект данной таблицы

33 есть идеи почему? если нет - смотрите шпаргалку
31 сделайте select * from testnm.t1;

32 получилось?
33 ура!
34 теперь попробуйте выполнить команду create table t2(c1 integer); insert into t2 values (2);
 ![image](https://user-images.githubusercontent.com/130083589/235296470-c80c68b5-34dc-4c87-94b8-26fed4b49450.png)

35 а как так? нам же никто прав на создание таблиц и insert в них под ролью readonly?

> не отозвали права у public

36 есть идеи как убрать эти права? если нет - смотрите шпаргалку
 ![image](https://user-images.githubusercontent.com/130083589/235296500-15b437b9-7869-4815-95e6-bf1ef745948c.png)


37 если вы справились сами то расскажите что сделали и почему, если смотрели шпаргалку - объясните что сделали и почему выполнив указанные в ней команды

> Не учел, что по умолчанию новые пользователи имеют права PUBLIC

38 теперь попробуйте выполнить команду create table t3(c1 integer); insert into t2 values (2);
 ![image](https://user-images.githubusercontent.com/130083589/235296538-30f43c9e-97cf-4f5f-a75a-fb85c26c45a0.png)

39 расскажите что получилось и почему

> мы отобрали права на создание таблиц, поэтому не можем создать новую. 
