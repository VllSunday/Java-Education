---
создал заметку: 2024-08-10
---
### SELF JOIN - чаще всего нужен для того что бы построить какую то иерархию. Когда есть некоторое иерархическое взаимоотношение между данными

Таким образом, если у вас есть связь между данными в таблице, и если есть внешний ключ который ссылается на первичный ключ в той же таблице. Скорее всего в таблице предусмотрена работа с SELF JOIN

Вот пример: 

```SQL 
CREATE TABLE employee  
(  
    employee_id INT PRIMARY KEY,  
    first_name  VARCHAR(255) NOT NULL,  
    last_name   VARCHAR(225) NOT NULL,  
    manager_id  INT,  
    FOREIGN KEY (manager_id) REFERENCES employee (employee_id)  
);
```

Таблица с работниками, где manager_id ссылается на employee_id
То есть мы можем сказать что: менеджер тоже работник. 

Далее можно заполнить таблицу данными: 

```SQL 
INSERT INTO employee  
VALUES (1, 'Windy', 'Hays', NULL),  
       (2, 'Ava', 'Christensen', 1),  
       (3, 'Hassan', 'Conner', 1),  
       (4, 'Anna', 'Reeves', 2),  
       (5, 'Sau', 'Norman', 2),  
       (6, 'Kelsie', 'Hays', 3),  
       (7, 'Tory', 'Goff', 3),  
       (8, 'Salley', 'Lester', 3);
```

Теперь давайте найдем всех работников, у для каждого работника выведем его менеджера: 

```SQL 
SELECT e.first_name || ' ' || e.last_name AS employee,  
       m.first_name || ' ' || m.last_name AS manager  
FROM employee e  
         LEFT JOIN employee m ON m.employee_id = e.manager_id  
ORDER BY manager;
```

Мы сопоставляем `manager_id` сотрудника (указывающего на его менеджера) с `employee_id` менеджера. Это означает, что мы связываем сотрудника с его менеджером на основе того, что `employee_id` менеджера должен совпадать с `manager_id` сотрудника.

Таким образом, разные ключи равняются друг другу, потому что они связаны логически: один(`manager_id`) указывает на другой (`employee_id`).


### Конкатенация строк в SQL
Здесь оператор `||` используется для конкатенации (объединения) строк в SQL. Что происходит в этих строках:

- `e.first_name || ' ' || e.last_name AS employee`: объединяются имя (`first_name`) и фамилия (`last_name`) сотрудника из таблицы `employee`, чтобы получить полное имя сотрудника. Результат будет назван `employee`.
- `m.first_name || ' ' || m.last_name AS manager`: аналогично объединяются имя и фамилия менеджера, результат будет назван `manager`.

