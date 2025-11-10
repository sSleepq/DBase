# Внешние ключи в SQL

Внешние ключи применяются для установки связи между таблицами. Внешний ключ:
- устанавливается для столбцов из **зависимой (подчиненной) таблицы**;
- указывает на один из столбцов из **главной таблицы**.

Хотя обычно внешний ключ ссылается на первичный ключ главной таблицы, это не обязательное условие. Он также может указывать на любой другой столбец с уникальными значениями.

## Синтаксис установки внешнего ключа

### На уровне столбца

```sql
[FOREIGN KEY] REFERENCES главная_таблица (столбец_главной_таблицы)
    [ON DELETE {CASCADE|NO ACTION|SET NULL|SET DEFAULT}]
    [ON UPDATE {CASCADE|NO ACTION|SET NULL|SET DEFAULT}]
```

**Пояснения:**
- После `REFERENCES` указывается имя связанной таблицы и в скобках — имя связанного столбца.
- Ключевое слово `FOREIGN KEY` можно не указывать.
- `ON DELETE` и `ON UPDATE` — дополнительные выражения для определения действий при изменении данных.

### На уровне таблицы

```sql
FOREIGN KEY (столбец1, столбец2, ..., столбецN)
    REFERENCES главная_таблица (столбец_главной_таблицы1, столбец_главной_таблицы2, ..., столбец_главной_таблицыN)
    [ON DELETE {CASCADE|NO ACTION|SET NULL|SET DEFAULT}]
    [ON UPDATE {CASCADE|NO ACTION|SET NULL|SET DEFAULT}]
```

## Пример связи двух таблиц

```sql
CREATE TABLE Customers
(
    Id INT PRIMARY KEY IDENTITY,
    Age INT DEFAULT 18, 
    FirstName VARCHAR(20) NOT NULL,
    LastName VARCHAR(20) NOT NULL,
    Email VARCHAR(30) UNIQUE,
    Phone VARCHAR(20) UNIQUE
);

CREATE TABLE Orders
(
    Id INT PRIMARY KEY IDENTITY,
    CustomerId INT REFERENCES Customers (Id),
    CreatedAt Date
);
```

**Описание:**
- `Customers` — главная таблица (клиент).
- `Orders` — зависимая таблица (заказ).
- Столбец `CustomerId` в таблице `Orders` является внешним ключом, указывающим на столбец `Id` в таблице `Customers`.

## Альтернативный вариант: определение внешнего ключа на уровне таблицы

```sql
CREATE TABLE Orders
(
    Id INT PRIMARY KEY IDENTITY,
    CustomerId INT,
    CreatedAt Date,
    FOREIGN KEY (CustomerId) REFERENCES Customers (Id)
);
```

## Задание имени ограничения внешнего ключа

С помощью оператора `CONSTRAINT` можно задать имя для ограничения (обычно с префиксом `FK_`):

```sql
CREATE TABLE Orders
(
    Id INT PRIMARY KEY IDENTITY,
    CustomerId INT,
    CreatedAt Date,
    CONSTRAINT FK_Orders_To_Customers FOREIGN KEY (CustomerId) REFERENCES Customers (Id)
);
```

В этом примере ограничение внешнего ключа `CustomerId` называется `FK_Orders_To_Customers`.

## Действия при изменении данных (ON DELETE и ON UPDATE)

Эти выражения задают действия при удалении или изменении связанных строк в главной таблице. Доступные опции:

- **`CASCADE`** — автоматически удаляет или изменяет строки в зависимой таблице при изменении главной.
- **`NO ACTION`** — предотвращает любые действия в зависимой таблице (фактически блокирует операцию).
- **`SET NULL`** — устанавливает значение `NULL` для столбца внешнего ключа при удалении связанной строки.
- **`SET DEFAULT`** — устанавливает значение по умолчанию для столбца внешнего ключа (если значение не задано — используется `NULL`).
