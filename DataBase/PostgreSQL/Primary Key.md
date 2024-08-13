---
создал заметку: 2024-08-07
---
## Primary Key
# Primary KEY - это ограничение.  
Он накладывает ограничение уникальности. Например если все колонки будут разные, но одно поле PRIMARY KEY будет одинаковое, мы не сможем добавить

Также он накладывает ограничения на NULL. То есть мы не можем в поле PRIMARY KEY вставить NULL 
### Что такое Primary Key?

**Primary Key** (первичный ключ) — это столбец (или набор столбцов) в таблице, который однозначно идентифицирует каждую строку в этой таблице. Первичный ключ должен быть уникальным и не должен содержать значения `NULL`.

### Зачем нужен Primary Key?

1. **Уникальная идентификация**: Каждая запись в таблице будет иметь уникальный идентификатор.
2. **Интегритет данных**: Обеспечивает целостность данных, предотвращая дублирование строк.
3. **Поиск и индексирование**: Ускоряет поиск данных и операции JOIN.

### Синтаксис для создания Primary Key

При создании таблицы:
```SQL
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    position VARCHAR(50)
);

```

Для добавления Primary Key к уже существующей таблице:
```SQL
ALTER TABLE employees ADD PRIMARY KEY (id);
```

### Composite Primary Key

Иногда первичный ключ состоит из нескольких столбцов. Такой ключ называется составным (composite key).
```SQL
CREATE TABLE orders (
    order_id INT,
    product_id INT,
    quantity INT,
    PRIMARY KEY (order_id, product_id)
);
```

## Constraints (Ограничения)

### Что такое Constraints?

**Constraints** (ограничения) — это правила, которые применяются к данным в таблице для обеспечения корректности и целостности данных. Они помогают предотвратить ввод некорректных данных.

### Основные типы Constraints

1. **NOT NULL**: Гарантирует, что столбец не будет содержать значения `NULL`.
```SQL
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    position VARCHAR(50) NOT NULL
);
```
2) **UNIQUE**: Обеспечивает уникальность значений в столбце (или наборе столбцов).
```SQL 
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    name VARCHAR(100)
);
```

3) **CHECK**: Устанавливает условие, которому должны удовлетворять данные в столбце
```SQL 
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    salary DECIMAL(10, 2),
    CHECK (salary > 0)
);
```

4) **FOREIGN KEY**: Создаёт связь между столбцом в одной таблице и столбцом (или набором столбцов) в другой таблице, обеспечивая целостность ссылок.
```SQL
CREATE TABLE departments (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);
```

### Зачем нужны Constraints?

1. **Целостность данных**: Обеспечивают целостность и корректность данных.
2. **Предотвращение ошибок**: Предотвращают ввод некорректных данных.
3. **Упрощение поддержки данных**: Упрощают поддержку и работу с данными, обеспечивая их соответствие заданным правилам.


### Good Practices при использовании Constraints

1. **Используйте PRIMARY KEY для каждой таблицы**: Это гарантирует уникальную идентификацию каждой строки.
2. **Используйте NOT NULL там, где это применимо**: Это предотвратит ввод `NULL` в столбцы, где это не допустимо.
3. **Применяйте UNIQUE для уникальных значений**: Например, для email-адресов.
4. **Используйте FOREIGN KEY для связей между таблицами**: Это обеспечит целостность ссылок.
5. **CHECK Constraints для дополнительных условий**: Используйте для проверки сложных бизнес-правил.


### Пример создания таблицы с различными Constraints

```SQL
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    salary DECIMAL(10, 2) CHECK (salary > 0),
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);
```

Этот пример включает:

- `PRIMARY KEY` на столбце `id`.
- `NOT NULL` на столбце `name`.
- `UNIQUE` на столбце `email`.
- `CHECK` на столбце `salary`, чтобы зарплата была положительной.
- `FOREIGN KEY` на столбце `department_id`, ссылающийся на таблицу `departments`.
### Заключение

Использование Primary Key и различных Constraints помогает поддерживать целостность и корректность данных в базе данных. Эти ограничения предотвращают ошибки, обеспечивают уникальность и устанавливают связи между таблицами. Следование лучшим практикам при использовании Constraints поможет создать надёжную и эффективную структуру базы данных.

Если у тебя возникнут дополнительные вопросы или нужны примеры по конкретным случаям, не стесняйся задавать!