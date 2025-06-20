-- 🧩 Основные элементы после SELECT (что именно мы выбираем)

SELECT
    name,                            -- обычный столбец
    salary * 1.2 AS adjusted_salary, -- выражение с псевдонимом (alias)
    department,
    COUNT(*) AS total,               -- агрегатная функция (кол-во строк)
    CASE                             -- условная логика (IF внутри SELECT)
        WHEN salary > 5000 THEN 'High'
        ELSE 'Low'
    END AS salary_level,
    DISTINCT city                   -- только уникальные значения городов

| Что указываем      | Обязательно? | Назначение                        | Пример                                           |
| ------------------ | ------------ | --------------------------------- | ------------------------------------------------ |
| Столбцы            | ✅ Да         | Какие поля выводить               | `SELECT name, age`                               |
| Выражения          | ⬜ Нет        | Вычисления на лету                | `salary * 1.2`                                   |
| Псевдонимы (`AS`)  | ⬜ Нет        | Переименование столбцов/выражений | `AS adjusted_salary`                             |
| Агрегатные функции | ⬜ Нет        | Сумма, среднее, количество и т.д. | `SUM(salary)`, `COUNT(*)`                        |
| `DISTINCT`         | ⬜ Нет        | Только уникальные значения        | `SELECT DISTINCT city`                           |
| `CASE`             | ⬜ Нет        | Условная логика (аналог IF)       | `CASE WHEN salary > 5000 THEN 'High' ELSE 'Low'` |

Примечания:
SELECT всегда идёт первым в запросе.

Всё, что после SELECT, влияет на то, что будет показано в выводе,
а не на то, откуда берутся данные (это делает FROM).

Порядок в SELECT важен, особенно если используется GROUP BY — тогда все поля
без агрегатных функций должны быть в GROUP BY.
SELECT
    employees.name,
    departments.name AS department,
    salary,
    salary * 1.2 AS adjusted_salary,
    CASE 
        WHEN salary > 5000 THEN 'High'
        ELSE 'Low'
    END AS salary_level,
    COUNT(*) OVER () AS total_count
FROM employees
JOIN departments ON employees.dept_id = departments.id;


FROM <таблица или подзапрос>
 
 
[JOIN ...]          -- объединение таблиц
[WHERE ...]         -- фильтрация строк
[GROUP BY ...]      -- группировка
[HAVING ...]        -- фильтрация групп
[ORDER BY ...]      -- сортировка
[LIMIT ...]         -- ограничение количества строк

| Команда    | Обязательно? | Что делает                          | Пример                                                   |
| ---------- | ------------ | ----------------------------------- | -------------------------------------------------------- |
| `FROM`     | ✅ Да         | Указывает таблицу или подзапрос     | `FROM employees`                                         |
| `JOIN`     | ⬜ Нет        | Объединяет с другими таблицами      | `JOIN departments ON employees.dept_id = departments.id` |
| `WHERE`    | ⬜ Нет        | Фильтрует строки (до группировки)   | `WHERE salary > 3000`                                    |
| `GROUP BY` | ⬜ Нет        | Группирует строки                   | `GROUP BY department`                                    |
| `HAVING`   | ⬜ Нет        | Фильтрует группы (после `GROUP BY`) | `HAVING COUNT(*) > 3`                                    |
| `ORDER BY` | ⬜ Нет        | Сортирует результат                 | `ORDER BY salary DESC`                                   |
| `LIMIT`    | ⬜ Нет        | Ограничивает количество строк       | `LIMIT 10`                                               |

Ключевые правила:
WHERE — фильтрует до группировки (GROUP BY).
HAVING — фильтрует после группировки.
ORDER BY и LIMIT — работают в конце, на уже отобранных строках.

FROM employees
JOIN departments ON employees.dept_id = departments.id
WHERE salary > 3000
GROUP BY department
HAVING COUNT(*) > 3
ORDER BY AVG(salary) DESC
LIMIT 5;
