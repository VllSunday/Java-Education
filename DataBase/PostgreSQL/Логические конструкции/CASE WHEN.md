---
создал заметку: 2024-08-16
---
---
# Конспект по `CASE WHEN`

`CASE WHEN` является важной конструкцией в SQL, позволяющей выполнять условные логические проверки и возвращать разные результаты в зависимости от условий. Вот основные моменты:

## 1. Основное применение

`CASE WHEN` используется для создания вычисляемых столбцов или для выполнения условий в SQL-запросах. Это позволяет вернуть различные значения или выполнять разные действия в зависимости от условий.

## 2. Синтаксис

### Синтаксис `CASE` в SQL

Есть два типа синтаксиса для `CASE WHEN`:

#### 2.1. Синтаксис с условием

```sql
CASE 
    WHEN условие1 THEN результат1
    WHEN условие2 THEN результат2
    ELSE результат_по_умолчанию
END
```

- **WHEN условие**: Условие, которое проверяется.
- **THEN результат**: Значение, возвращаемое, если условие истинно.
- **ELSE результат_по_умолчанию**: Значение, возвращаемое, если ни одно из условий не выполнено. `ELSE` является необязательным.

#### Пример

```sql
SELECT
    имя,
    CASE
        WHEN возраст < 18 THEN 'Молодежь'
        WHEN возраст BETWEEN 18 AND 65 THEN 'Взрослый'
        ELSE 'Пенсионер'
    END AS категория
FROM пользователи;
```

#### 2.2. Синтаксис поиска

```sql
CASE выражение
    WHEN значение1 THEN результат1
    WHEN значение2 THEN результат2
    ELSE результат_по_умолчанию
END
```

- **CASE выражение**: Выражение, значение которого проверяется.
- **WHEN значение**: Значение, которое сравнивается с результатом выражения.

#### Пример

```sql
SELECT
    имя,
    CASE возраст
        WHEN 18 THEN 'Только взрослый'
        WHEN 65 THEN 'На пенсии'
        ELSE 'Другой возраст'
    END AS статус
FROM пользователи;
```

## 3. Использование в разных частях запроса

### 3.1. В выборке (SELECT)

```sql
SELECT
    имя,
    CASE
        WHEN зарплата > 100000 THEN 'Высокая'
        WHEN зарплата BETWEEN 50000 AND 100000 THEN 'Средняя'
        ELSE 'Низкая'
    END AS уровень_зарплаты
FROM сотрудники;
```

### 3.2. В условиях (WHERE)

```sql
SELECT *
FROM заказы
WHERE
    CASE
        WHEN статус = 'Отправлено' THEN 1
        ELSE 0
    END = 1;
```

### 3.3. В группировке (GROUP BY)

```sql
SELECT
    CASE
        WHEN AVG(зарплата) > 100000 THEN 'Высокий уровень зарплаты'
        ELSE 'Низкий уровень зарплаты'
    END AS категория
FROM сотрудники
GROUP BY категория;
```

## 4. Порядок выполнения

1. Выражение в `CASE` вычисляется.
2. Сравнивается с условиями `WHEN`.
3. Возвращается значение `THEN`, если условие истинно.
4. Если ни одно условие не истинно и есть `ELSE`, возвращается значение `ELSE`.

## 5. Ограничения

- `CASE` не поддерживает логические операторы `AND`, `OR` напрямую в `WHEN` (требуется использование вложенных конструкций `CASE` или дополнительных условий).
- `CASE` не может использоваться напрямую для сравнения текстовых значений в некоторых базах данных, где необходима строгая проверка типа.

---