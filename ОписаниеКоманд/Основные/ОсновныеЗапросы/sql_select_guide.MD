# Команда SELECT

Команда SELECT получает записи из базы данных по определенному условию, которое задается с помощью команды WHERE.

Эти записи можно отсортировать с помощью команды ORDER BY, а также можно ограничить их количество с помощью LIMIT.

## Синтаксис

### Базовый синтаксис:
```sql
SELECT * FROM имя_таблицы
SELECT * FROM имя_таблицы WHERE условие
SELECT поле1, поле2... FROM имя_таблицы WHERE условие
```

### Полный синтаксис:
```sql
SELECT
    [STRAIGHT_JOIN] [SQL_SMALL_RESULT] [SQL_BIG_RESULT] [SQL_BUFFER_RESULT]
    [SQL_CACHE | SQL_NO_CACHE] [SQL_CALC_FOUND_ROWS] [HIGH_PRIORITY]
    [DISTINCT | DISTINCTROW | ALL]
    select_expression,...
[INTO {OUTFILE | DUMPFILE} 'file_name' export_options]
[FROM table_references
[WHERE where_definition]
[GROUP BY {unsigned_integer | col_name | formula} [ASC | DESC], ...]
[HAVING where_definition]
[ORDER BY {unsigned_integer | col_name | formula} [ASC | DESC], ...]
[LIMIT [offset,] rows | rows OFFSET offset]
[PROCEDURE procedure_name(argument_list)]
[FOR UPDATE | LOCK IN SHARE MODE]]
```

## Таблица для примеров

таблица employees
| id | name  | age | salary |
|----|-------|-----|--------|
| 1  | user1 | 23  | 400    |
| 2  | user2 | 25  | 500    |
| 3  | user3 | 23  | 500    |
| 4  | user4 | 30  | 900    |
| 5  | user5 | 27  | 500    |
| 6  | user6 | 28  | 900    |

## Примеры

### Пример 1
Выбор записей, у которых поле id больше 3-х:
```sql
SELECT * FROM employees WHERE id>3
```

Результат:
| id | name  | age | salary
|----|-------|-----|--------|
4  | user4 | 30  | 900
5  | user5 | 27  | 500
6  | user6 | 28  | 900

### Пример 2
Выбор записей, у которых поле id меньше 3-х:
```sql
SELECT * FROM employees WHERE id<=3
```

Результат:
| id | name  | age | salary
|--- |-------|-----|--------|
1  | user1 | 23  | 400
2  | user2 | 25  | 500
3  | user3 | 23  | 500

### Пример 3
Выбор записей, у которых поле id равно 3-м:
```sql
SELECT * FROM employees WHERE id=3
```

Результат:
| id | name  | age | salary
|--- |-------|-----|--------|
3  | user3 | 23  | 500

### Пример 4
Выбор записей, у которых поле id НЕ равно 3-м:
```sql
SELECT * FROM employees WHERE id!=3
```

Результат:
| id | name  | age | salary
|--- |-------|-----|--------|
1  | user1 | 23  | 400
2  | user2 | 25  | 500
4  | user4 | 30  | 900
5  | user5 | 27  | 500
6  | user6 | 28  | 900

Альтернативная запись:
```sql
SELECT * FROM employees WHERE id<>3
```

### Пример 5
Выбор всех записей из таблицы (без условия WHERE):
```sql
SELECT * FROM employees
```

Результат:
| id | name  | age | salary
|--- |-------|-----|--------|
1  | user1 | 23  | 400
2  | user2 | 25  | 500
3  | user3 | 23  | 500
4  | user4 | 30  | 900
5  | user5 | 27  | 500
6  | user6 | 28  | 900

### Пример 6
Выбор конкретных полей (id, name, age):
```sql
SELECT id, name, age FROM employees
```

Результат:
| id | name  | age
|--- |-------|-----
1  | user1 | 23
2  | user2 | 25
3  | user3 | 23
4  | user4 | 30
5  | user5 | 27
6  | user6 | 28

### Пример 7
Выбор только одного поля - name:
```sql
SELECT name FROM employees
```

Результат:
| name
| --- |
user1
user2
user3
user4
user5
user6