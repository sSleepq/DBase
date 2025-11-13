# Команда HAVING в SQL

Команда `HAVING` позволяет фильтровать результат группировки, выполненной с помощью команды `GROUP BY`.

## Синтаксис

```sql
GROUP BY поле HAVING условие
```

## Таблицы для примеров

**Таблица `employees`**

| id | name | age | salary |
|----|------|-----|--------|
| 1 | user1 | 23 | 400 |
| 2 | user2 | 25 | 500 |
| 3 | user3 | 23 | 500 |
| 4 | user4 | 30 | 900 |
| 5 | user5 | 27 | 500 |
| 6 | user6 | 28 | 900 |

## Примеры использования

### Пример 1: GROUP BY без HAVING

Демонстрация работы `GROUP BY` без условия `HAVING`.

**Запрос:**
```sql
SELECT age, SUM(salary) as sum FROM employees GROUP BY age
```

**Результат:**

| age | sum |
|-----|-----|
| 23 | 600 |
| 24 | 3000 |
| 25 | 900 |

### Пример 2: Фильтрация с HAVING по суммарной зарплате

Оставляем только те строки, где суммарная зарплата ≥ 1000.

**Запрос:**
```sql
SELECT age, SUM(salary) as sum FROM employees GROUP BY age HAVING sum >= 1000
```

**Результат:**

| age | sum |
|-----|-----|
| 24 | 3000 |
| 25 | 900 |

### Пример 3: Подсчёт количества записей в группе (без HAVING)

Используем функцию `COUNT` для подсчёта записей в каждой группе.

**Запрос:**
```sql
SELECT age, COUNT(*) as count FROM employees GROUP BY age
```

**Результат:**

| age | count |
|-----|-------|
| 23 | 3 |
| 24 | 2 |
| 25 | 1 |

### Пример 4: Фильтрация с HAVING по количеству записей

Оставляем только группы, где количество строк ≤ 2.

**Запрос:**
```sql
SELECT age, COUNT(*) as count FROM employees GROUP BY age HAVING count <= 2
```

**Результат:**

| age | count |
|-----|-------|
| 24 | 2 |
| 25 | 1 |

### Пример 5: Использование HAVING с IN

Достигаем аналогичного эффекта с помощью оператора `IN`.

**Запрос:**
```sql
SELECT age, COUNT(*) as count FROM employees GROUP BY age HAVING count IN (1, 2)
```

### Пример 6: Использование HAVING с BETWEEN

Фильтруем группы с помощью оператора `BETWEEN`.

**Запрос:**
```sql
SELECT age, COUNT(*) as count FROM employees GROUP BY age HAVING count BETWEEN 1 AND 2
```
