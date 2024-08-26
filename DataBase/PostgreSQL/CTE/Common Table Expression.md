---
создал заметку: 2024-08-20
---

---
# PostgreSQL CTE

### Краткие сведения
В этом руководстве вы узнаете, как использовать PostgreSQL Common Table Expression (CTE) для упрощения сложных запросов.

### Введение в PostgreSQL CTE
Общее табличное выражение (CTE) позволяет создать временный результирующий набор в запросе. CTE помогает улучшить читаемость сложного запроса, разбивая его на более мелкие и пригодные для повторного использования части.

**Базовый синтаксис для создания CTE:**
```sql
WITH cte_name (column1, column2, ...) AS (
    -- CTE query
    SELECT ...
)
-- Main query using the CTE
SELECT ...
FROM cte_name;
```

**В этом синтаксисе:**
- **WITH**: Вводит CTE. За ним следует название CTE и список имен столбцов в круглых скобках (необязательно).
- **Имя CTE**: Укажите имя CTE. Оно существует только в пределах текущего запроса.
- **Список столбцов (необязательно)**: Укажите список имен столбцов, если хотите явно определить их.
- **AS**: Указывает начало определения CTE.
- **Запрос CTE**: Определяет CTE. Могут использоваться JOIN, WHERE, GROUP BY и другие конструкции SQL.
- **Основной запрос**: После определения CTE его можно использовать в основном запросе, как обычную таблицу.

### Примеры PostgreSQL CTE

1. **Базовый пример общего табличного выражения**
   ```sql
   WITH action_films AS (
     SELECT 
       f.title, 
       f.length 
     FROM 
       film f 
       INNER JOIN film_category fc USING (film_id) 
       INNER JOIN category c USING (category_id) 
     WHERE 
       c.name = 'Action'
   ) 
   SELECT * FROM action_films;
   ```
   **Вывод:**
   ```
             title          | length
   -------------------------+--------
    Amadeus Holy            |    113
    American Circus         |    129
    Antitrust Tomatoes      |    168
    Ark Ridgemont           |     68
   ```

2. **Присоединение CTE к таблице**
   ```sql
   WITH cte_rental AS (
     SELECT 
       staff_id, 
       COUNT(rental_id) rental_count 
     FROM 
       rental 
     GROUP BY 
       staff_id
   ) 
   SELECT 
     s.staff_id, 
     first_name, 
     last_name, 
     rental_count 
   FROM 
     staff s 
     INNER JOIN cte_rental USING (staff_id);
   ```
   **Вывод:**
   ```
    staff_id | first_name | last_name | rental_count
   ----------+------------+-----------+--------------
          1 | Mike       | Hillyer   |         8040
          2 | Jon        | Stephens  |         8004
   ```

3. **Пример нескольких CTE**
   ```sql
   WITH film_stats AS (
       -- CTE 1: Calculate film statistics
       SELECT
           AVG(rental_rate) AS avg_rental_rate,
           MAX(length) AS max_length,
           MIN(length) AS min_length
       FROM film
   ),
   customer_stats AS (
       -- CTE 2: Calculate customer statistics
       SELECT
           COUNT(DISTINCT customer_id) AS total_customers,
           SUM(amount) AS total_payments
       FROM payment
   )
   -- Main query using the CTEs
   SELECT
       ROUND((SELECT avg_rental_rate FROM film_stats), 2) AS avg_film_rental_rate,
       (SELECT max_length FROM film_stats) AS max_film_length,
       (SELECT min_length FROM film_stats) AS min_film_length,
       (SELECT total_customers FROM customer_stats) AS total_customers,
       (SELECT total_payments FROM customer_stats) AS total_payments;
   ```
   **Вывод:**
   ```
    avg_film_rental_rate | max_film_length | min_film_length | total_customers | total_payments
   ----------------------+-----------------+-----------------+-----------------+----------------
                    2.98 |             185 |              46 |             599 |       61312.04
   ```

### Преимущества PostgreSQL CTE
- **Улучшение читаемости**: CTE упрощает организацию сложных запросов.
- **Рекурсивные запросы**: Позволяет создавать рекурсивные запросы для работы с иерархическими данными.
- **Использование с оконными функциями**: Можно использовать CTE совместно с оконными функциями для создания промежуточных результирующих наборов.

### Краткие сведения
Используйте CTE для создания временного результирующего набора в запросе. CTE упрощает сложные запросы и улучшает их читаемость.

---
