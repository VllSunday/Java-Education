---
создал заметку: 2024-08-11
---
Ограничение `CHECK` в PostgreSQL позволяет задать условия, которые должны выполняться для данных в таблице. Если данные не соответствуют этим условиям, то попытка вставки или обновления строки будет отклонена.

### Основные аспекты ограничения `CHECK`:

1. **Синтаксис**:
   ```sql
   CREATE TABLE table_name (
       column_name data_type,
       CONSTRAINT constraint_name CHECK (condition)
   );
   ```
   - `constraint_name`: Опциональное имя ограничения.
   - `condition`: Условие, которому должны соответствовать данные.

2. **Примеры использования**:
   - **Проверка на диапазон значений**:
     ```sql
     CREATE TABLE employees (
         id SERIAL PRIMARY KEY,
         age INT,
         CONSTRAINT check_age CHECK (age >= 18 AND age <= 65)
     );
     ```
     В данном примере, столбец `age` должен содержать значения от 18 до 65. Любое значение вне этого диапазона будет отклонено.

   - **Проверка на положительные значения**:
     ```sql
     CREATE TABLE products (
         product_id SERIAL PRIMARY KEY,
         price NUMERIC,
         CONSTRAINT check_price CHECK (price > 0)
     );
     ```
     В данном случае, цена продукта должна быть больше нуля.

   - **Использование логических выражений**:
     ```sql
     CREATE TABLE orders (
         order_id SERIAL PRIMARY KEY,
         amount NUMERIC,
         discount NUMERIC,
         CONSTRAINT check_discount CHECK (discount >= 0 AND discount <= amount)
     );
     ```
     Здесь скидка не может превышать сумму заказа и должна быть неотрицательной.

3. **Особенности**:
   - Ограничение `CHECK` может проверять только значения, относящиеся к одной строке, и не может выполнять проверку между строками.
   - Можно создать несколько ограничений `CHECK` для одной таблицы.
   - `CHECK` не может ссылаться на данные в других таблицах.

4. **Обновление существующих таблиц**:
   Если нужно добавить ограничение `CHECK` в уже существующую таблицу, это делается с помощью команды `ALTER TABLE`:
   ```sql
   ALTER TABLE table_name ADD CONSTRAINT constraint_name CHECK (condition);
   ```

5. **Ограничения и производительность**:
   - `CHECK`-ограничения могут незначительно замедлить операции вставки и обновления, так как требуется дополнительная проверка условий.
   - Но они обеспечивают целостность данных, предотвращая некорректные вставки.

### Заключение:
Ограничение `CHECK` в PostgreSQL является мощным инструментом для поддержания целостности данных на уровне базы данных. Оно позволяет задать условия, которым должны соответствовать данные при их вставке или обновлении, тем самым предотвращая появление некорректных данных в таблице.