# План заданий для проекта

1) Пользователь. Регистрация
2) Пользователь. Авторизация
3) Пользователь. Поиск прямого маршрута по заданной дате, отправлению и прибытию
4) Пользователь Поиск маршрута с пересадками
5) Пользователь. Просмотр свободных мест на маршрут.
6) Пользователь. Покупка билета.
7) Администратор. Добавлению новых маршрутов.
8) Пользователь. Просмотр собственной статистики перелетов с пагинацией, поиском по дате, по точке отправлению, по точке прибытия, сортировка по дате
9) Пользователь. Подписка на изменение цены на заданный маршрут
10) Пользователь. Подписка на появление билетов по заданному маршруту на заданные даты.

### Пользователь
1) Регистрация
```SQL
INSERT INTO users (name, surname, access, login, password)
VALUES ($1, $2, $3, $4, $5);
```
2) авторизация
```SQL
CREATE TABLE users(id INTEGER PRIMARY KEY,                      
 name VARCHAR(50),
   surname VARCHAR(50),
   pwd VARCHAR(128),
   is_admin BOOLEAN
   );
```
### Пользователи

```SQL
CREATE TABLE users (
    id serial PRIMARY KEY,
	name varchar(255) not null,
	surname varchar(255) not null,
	access int not null 
    login varchar(255),
    password varchar(255)
);
```

```SQL
CREATE TABLE trip(
    trip_id serial PRIMARY KEY,
    trip_start varchar(255) not null,
    trip_end varchar(255) not null,
    trip_cost decimal not null,
    trip_start_date datetime not null,
    trip_end_date datetime not null,
    transfer
);
```
```SQL
CREATE TABLE airport(id INTEGER PRIMARY KEY,
    country VARCHAR (255)
    );   
```            

3) Пользователь. Поиск прямого маршрута по заданной дате, отправлению и прибытию

```SQL
SELECT  trip.id
FROM  trip 
WHERE trip_start=var1 AND 
	  trip_start=var2 AND
	  trip_end=var3
```


4)     Пользователь. Поиск маршрута с пересадками
```SQL
CREATE OR REPLACE FUNCTION search_trip(trip INTEGER) RETURNS BIGINT AS 
	IF (SELECT COUNT( trip_start)
	FROM  trip 
	WHERE  trip.airport_end=trip) >var1 THEN
		...
```

5)     Пользователь. Просмотр свободных мест на маршрут.
```SQL
SELECT planes.places-trip.book_places
FROM trip
INNER JOIN planes
ON planes.id=trip.plane_id
WHERE trip.id=var4
```

6)     Пользователь. Покупка билета
```SQL
INSERT INTO users_trip (id, user_id, trip_id) VALUES (var5, var6, var7);
INSERT INTO users_trip (id, user_id, trip_id) VALUES (var5, var6, var8);
```

7)     Администратор. Добавление новых маршрутов
```SQL

CREATE OR REPLACE FUNCTION add_trip(i INTEGER) RETURNS void AS 
	IF (SELECT users.id FROM users)=i AND (SELECT users.is_admin FROM users WHERE users.id=i)=true THEN
		INSERT INTO  trip (id, trip_star, trip_end, trip_start_date, trip_start, trip_end, trip_id, book_places, trip_cost) VALUES (var1, var2, var3, var4, var5,var6, var7, var8, var9);
	END IF;
```
8)     Пользователь. Просмотр собственной статистики перелётов с
пагинацией, поиском по дате, по точке отправления, по точке прибытия,
сортировка по дате
```SQL
SELECT *
FROM trips
INNER JOIN
	(SELECT ut.trip_id
	FROM users_trip ut
	WHERE ut.user_id=var1) ut
ON ut.trip_id=trip.id
ORDER BY trip_start_date
```
