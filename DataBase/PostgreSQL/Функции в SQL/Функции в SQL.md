---
создал заметку: 2024-08-27
---
### Зачем нужны функции в PostgreSQL? 

1) Функция - объект БД, принимающий аргументы и возвращающий результат
2) Функции (хранимые процедуры) - компилируемы и хранятся на стороне БД.
	А потому их вызов стоит дёшево
3) Разграничение работы Frontend-dev и Server-side developer  
   То есть Frontend разработчик пишет код который просто будет дергать уже скомпилированные функции, а Server-side разработчик будет писать функции который дергает этот самый Frontend разработчик
4) Хранить код который работает с кортежами логичнее ближе к данным (SRP)
5) Переиспользуемость функций разными клиентскими приложениями
6) Управление безопасностью через регулирование доступа к функциям
7) Уменьшение трафика на сеть
8) Модульное программирование. Что если нужна генерация случайных чисел от 1 до
	10 в разных SQL-скриптах?


### Функции в PostgerSQL  

- Состоят из набора утверждений, возвращая результат последнего
- Могут содержать SELECT, INSERT, UPDATE, DELETE (CRUD)
- Не могут содержать COMMIT, SAVEPOINT (TCL), VACUUM (utility)
	хоть и написано что фк.не могут содержать keyword-s для таранзакций. Но функции являются ==авто-транзакционными==. То есть если что произойдет на сервере. Бд все откатит назад


Делятся на:

==SQL-функции (будем рассматривать)==
==Процедурные (PL/pgSQL - основной диалект)(будем рассматривать)==
Серверные функции (написанные на С) (скорее всего никогда не сталкнемся)
Собственные С-функции (скорее всего никогда не сталкнемся)


### Первая функция в SQL

```sql
SELECT * 
INTO tmp_customestomer
FROM customers;

SELECT *  
FROM tmp_customers;  
  
-- Например, нам нужно сделать вот такой код:  
UPDATE tmp_customers  
SET region = 'unknown'  
WHERE region IS NULL;  
  
-- Как бы он выглядел если бы нам нужно было написать функцию?  
--Например у нас есть какой то клиенский код, который нуждается в этой функции. Для того  
--чтобы клиенту не писать код из нутри. Мы можем просто создать функцию которую он потом вызовет.  
  
  
CREATE OR REPLACE FUNCTION fix_customer_region() RETURNS void AS $$  
    UPDATE tmp_customers  
    SET region = 'unknown'  
    WHERE region IS NULL  
    $$ language SQL;  
  
 SELECT fix_customer_region();
```

Здесь мы создали новую постоянную таблицу, в которую скопировали все данные из customer.

Далее мы создали функцию которая ничего не принемали и нечего не возвращала.
И благодаря этой функции мы исправили все Null значения в таблице на 'unknown'

### Скалярные функции

Скалярные функции, которые имеют в качестве возврата какое ==1 значение==

Пример нескольких скалярных функций

```sql
-- Сумма кол-во всех продуктов  
CREATE OR REPLACE FUNCTION get_total_of_goods()  
    RETURNS BIGINT AS $$  
SELECT SUM(units_in_stock)  
FROM products;  
$$ LANGUAGE SQL;  
  
  
SELECT get_total_of_goods() AS total_goods;  
```

```sql
CREATE OR REPLACE FUNCTION get_avg_price()  
    RETURNS float8 AS $$  
SELECT AVG(unit_price)  
FROM products;  
$$ LANGUAGE SQL;  
  
SELECT get_avg_price() AS avg_price;
```

Вот как можно написать скалярную функцию.

Важно понимать. Если функция возвращает 1 значение то Select не может выбрать несколько значений


### Функции которые что то принимают и возвращают

Аргументы функций:

IN - входящие аргументы
OUT - исходящие аргументы
IN OUT - и входящий и исходящий аргумент
VARIADIC - массив входящих параметров
DEFAULT - value


### Примеры



<font color="#2DC26B">Функция которая фильтрует продукты по имени</font>
```sql
CREATE OR REPLACE FUNCTION get_product_price_by_name(prod_name varchar) RETURNS real AS $$ --Здесь не явно IN параметр (IN prod_name varchar)  
    SELECT unit_price  
    FROM products  
    WHERE product_name = prod_name  
    $$ LANGUAGE SQL;  
  
SELECT get_product_price_by_name('Chocolade') as price;  
```


<font color="#2DC26B">Границы цен, среди всех товаров (мин-цена | мак-цена)</font>

```sql
CREATE OR REPLACE FUNCTION get_price_boundaries (OUT max_price real, OUT min_price real) AS $$  
    SELECT MAX(unit_price), MIN(unit_price)  
    FROM products  
    $$ LANGUAGE SQL;  
  
SELECT get_price_boundaries(); -- Получим значения который будут в одной коллонке (263.5,2.5)  
SELECT * FROM get_price_boundaries(); -- По разным коллонкам 263.5 | 2.5
```

<font color="#2DC26B">Добавили входные данные</font>

```sql
CREATE OR REPLACE FUNCTION get_price_boundaries_by_discontinued(IN is_discontinued int, OUT max_price real, OUT min_price real) AS $$  
SELECT MAX(unit_price), MIN(unit_price)  
FROM products  
WHERE discontinued = is_discontinued  
$$ LANGUAGE SQL;  
  
SELECT * FROM get_price_boundaries_by_discontinued(0);
```


Default
Мы можем еще больше улучшить код. Например, клиенский код чаще всего передает параметр is_discontinued в качестве 1
Мы можем еще больше снизить симантическую награзку с клиетна, и просто становить 1 дефелтным значением

```sql
CREATE OR REPLACE FUNCTION get_price_boundaries_by_discontinued_default(IN is_discontinued int DEFAULT 1, OUT max_price real, OUT min_price real) AS $$  
SELECT MAX(unit_price), MIN(unit_price)  
FROM products  
WHERE discontinued = is_discontinued  
$$ LANGUAGE SQL;

SELECT * FROM get_price_boundaries_by_discontinued_default();
```
Получилось следующее, если клиент передал что то, то будет вызвано то что хотел клиент.  
Если не передал ни чего то будет вызвано значение по умолчанию, то есть "1"


### Возврат множества строк

- RETURNS SETOF data_type - возврат n-значений типа data_type

- RETURNS SETOF table - если нужно вернуть все столбцы из таблицы или
пользовательского типа

- RETURNS SETOF record
(только когда типы колонок в результирующем наборе заранее неизвестны)

- RETURNS TABLE (column_name data_type, …) - тоже что и set of table,
но имеем возможность явно указать возвращаемые столбцы

- Возврат через out-параметры

И так, как вернуть набор примитивного типа

### Например 

Нужно вернуть средние цены, разбитые по категориям продуктов. 
```sql
CREATE or replace function get_average_prices_by_prod_categories()  
    RETURNS SETOF double precision AS  $$  
    SELECT AVG(unit_price)  
    FROM products  
    GROUP BY category_id  
    $$ LANGUAGE SQL;  
  
SELECT get_average_prices_by_prod_categories() AS avarage_prices;
```

Вернули тип double precision 

---
Средние цены по категориям продуктов
SUM(units_price) и AVG(units_price)




