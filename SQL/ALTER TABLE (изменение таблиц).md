```sql
ALTER TABLE old_table_name
RENAME TO new_table_name;
```

- _`old_table_name` - актуальное имя таблицы в базе данных_
- _`RENAME TO` - <rename to - переименовать в> команда, указывающая на изменение имени таблицы_
- _`new_table_name` - новое имя таблицы_

### _`-- rename a column`_ 

```sql
ALTER TABLE table_name
RENAME COLUMN old_column_name TO new_column_name;
```

- _`table_name` - имя таблицы в базе данных_
- _`RENAME COLUMN TO` - <rename column to - переименовать столбец в> команда, указывающая на изменение имени столбца_
- _`old_column_name` - актуальное название столбца таблицы_
- _`new_column_name` - новое название столбца таблицы_

**_примерчики_**

```sql
-- создание таблички profile
CREATE TABLE profile (
	id INT,
	username VARCHAR(255),
	email VARCHAR(255),
	reg_dt TIMESTAMP
);

-- изменение названия таблицы
ALTER TABLE profile
RENAME TO users;

-- изменение названия столбца таблицы users
ALTER TABLE users
RENAME COLUMN reg_dt TO registration_date;

-- выборка наименований атрибутов таблицы users
SELECT column_name FROM information_schema.columns
WHERE table_name = 'users';

/* результат:
column_name      |
-----------------|
id               |
registration_date|
username         |
email            |
*/
```

_next_, рассмотрим добавление нового атрибута в таблицу и удаление существующего ⤵

### _`-- add a new column`_

```sql
ALTER TABLE table_name
ADD column_name column_type;
```

- _`table_name` - название таблицы в базе данных_
- _`ADD` - <add - добавить> команда, указывающая на добавление столбца_
- _`column_name` - название добавляемого столбца (не схожее с уже существующими названиями столбцов в таблице)_
- _`column_type` - тип данных добавляемого столбца_

### _`-- drop a column`_

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

- _`table_name` - название таблицы в базе данных_
- _`DROP COLUMN` - <drop column - отбросить столбец> команда, указывающая на удаление столбца_
- _`column_name` - название удаляемого столбца_

**_примерчики_**

❗❔ _также (для PostgreSQL), как и в случае с_ `CREATE TABLE IF NOT EXISTS`_, к команде_ `ALTER TABLE` _можно добавить_ `IF EXISTS`_, т.е._

```sql
ALTER TABLE IF EXISTS table_name
...
```

_что предотвратит ошибку в случае несуществующей таблицы_