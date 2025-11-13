# Команды JOIN в SQL: INNER JOIN, LEFT JOIN, RIGHT JOIN

Команды `JOIN`, `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN` используются для связывания таблиц по определённым полям связи.

## Синтаксис

```sql
SELECT поля 
FROM имя_таблицы
[LEFT/RIGHT/INNER] JOIN имя_связанной_таблицы ON условие_связи
WHERE условие_выборки
```

## Таблицы для примеров

**Таблица `countries`**

| id | name |
|----|------|
| 1 | Беларусь |
| 2 | Россия |
| 3 | Украина |

**Таблица `cities`**

| id | name | country_id |
|----|------|------------|
| 1 | Минск | 1 |
| 2 | Витебск | 1 |
| 3 | Москва | 2 |
| 4 | Питер | 2 |
| 5 | Лондон | 0 |

## Примеры использования

### Пример 1: LEFT JOIN

**Запрос:**
```sql
SELECT
    cities.id AS city_id,
    cities.name AS city_name,
    cities.country_id AS city_country_id,
    countries.id AS country_id,
    countries.name AS country_name
FROM cities
LEFT JOIN countries ON countries.id = cities.country_id
```

**Результат:**

| city_id | city_name | city_country_id | country_id | country_name |
|---------|-----------|-----------------|------------|--------------|
| 1 | Минск | 1 | 1 | Беларусь |
| 2 | Витебск | 1 | 1 | Беларусь |
| 3 | Москва | 2 | 2 | Россия |
| 4 | Питер | 2 | 2 | Россия |
| 5 | Лондон | 0 | NULL | NULL |

**Пояснение:**  
- Все строки из левой таблицы (`cities`) включены в результат.
- Для строки «Лондон» (country_id = 0) нет соответствующей записи в `countries` → поля из `countries` заполнены как `NULL`.

### Пример 2: RIGHT JOIN

**Запрос:**
```sql
SELECT
    cities.id AS city_id,
    cities.name AS city_name,
    cities.country_id AS city_country_id,
    countries.id AS country_id,
    countries.name AS country_name
FROM cities
RIGHT JOIN countries ON countries.id = cities.country_id
```

**Результат:**

| city_id | city_name | city_country_id | country_id | country_name |
|---------|-----------|-----------------|------------|--------------|
| 1 | Минск | 1 | 1 | Беларусь |
| 2 | Витебск | 1 | 1 | Беларусь |
| 3 | Москва | 2 | 2 | Россия |
| 4 | Питер | 2 | 2 | Россия |
| NULL | NULL | NULL | 3 | Украина |

**Пояснение:**  
- Все строки из правой таблицы (`countries`) включены в результат.
- Для страны «Украина» (id = 3) нет соответствующих городов в `cities` → поля из `cities` заполнены как `NULL`.
- Город «Лондон» (country_id = 0) не включён, так как нет соответствия в `countries`.

### Пример 3: INNER JOIN

**Запрос:**
```sql
SELECT
    cities.id AS city_id,
    cities.name AS city_name,
    cities.country_id AS city_country_id,
    countries.id AS country_id,
    countries.name AS country_name
FROM cities
INNER JOIN countries ON countries.id = cities.country_id
```

**Результат:**

| city_id | city_name | city_country_id | country_id | country_name |
|---------|-----------|-----------------|------------|--------------|
| 1 | Минск | 1 | 1 | Беларусь |
| 2 | Витебск | 1 | 1 | Беларусь |
| 3 | Москва | 2 | 2 | Россия |
| 4 | Питер | 2 | 2 | Россия |

**Пояснение:**  
- В результат включены только строки, где есть соответствие по условию `ON` (countries.id = cities.country_id).
- «Лондон» (country_id = 0) и «Украина» (нет городов) не попали в выборку.

## Краткие выводы

- **`LEFT JOIN`** — все строки из левой таблицы + совпадающие из правой (несовпадающие → `NULL`).
- **`RIGHT JOIN`** — все строки из правой таблицы + совпадающие из левой (несовпадающие → `NULL`).
- **`INNER JOIN`** — только строки с совпадениями по условию связи.
