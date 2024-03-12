Синтаксис создания таблиц

```sql
CREATE TABLE table_name (
    column_name column_type,
    ...
);
```
_<create table - создать таблицу>_, где:

- _`CREATE TABLE` - команда, создающая пустую таблицу в текущей базе данных_
- _`table_name` - название таблицы_
- _`column_name` - название столбца_
- _`column_type` - тип данных столбца_

также, _при создании таблиц_, к команде `CREATE TABLE` добавляют команду `IF NOT EXISTS` **_-_** _<если не существует>_, для предотвращения ошибки, в случае создания уже существующей таблицы в базе данных ⤵

```sql
CREATE TABLE pokemons (
    id INT,
    name VARCHAR(50),
    type VARCHAR(50),
    hp INT,
    attack INT,
    defence INT 
);

/* результат:
ERROR:  relation "pokemons" already exists
SQL state: 42P07
*/
```

без ошибки, но с уведомлением ⤵

```sql
CREATE TABLE IF NOT EXISTS pokemons (
    id INT,
    name VARCHAR(50),
    type VARCHAR(50),
    hp INT,
    attack INT,
    defence INT 
);

/* результат:
NOTICE:  relation "pokemons" already exists, skipping
CREATE TABLE
Query returned successfully in 41 msec.
*/
```