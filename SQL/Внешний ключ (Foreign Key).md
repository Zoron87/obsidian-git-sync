**Внешний ключ**, для связи между таблицами. Синтаксис ограничения внешнего ключа следующий:  
`FOREIGN KEY` `(` имя поля — внешний ключ дочерней таблицы `)` `REFERENCES` Имя родительской таблицы `(` поле родительской таблицы — первичный ключ `)`

Рассмотрим пример на двух таблицах.

Первая — родительская таблица `client` содержит в себе:

- `id` — id клиента, первичный ключ с автоинкрементом;
- `created` — дата регистрации клиента;
- `mobile` — мобильный номер клиента;
- `email` — электронная почта клиента;
- `is_active` — активна ли учетная запись клиента.

Вторая - дочерняя таблица `sms_logs` содержит в себе:

- `id` — идентификатор сообщения, первичный ключ с автоинкрементом;
- `client_id` — внешний ключ к полю id из таблицы `client`
- `created` — дата создания СМС;
- `phone_number` — номер телефона клиента;
- `message` — текст СМС;

```sql
CREATE TABLE clients
(
    id     	INT PRIMARY KEY AUTO_INCREMENT,
    created  	DATETIME,
    mobile 	VARCHAR(20),
    email 	VARCHAR(50),
    is_active 	TINYINT(1)
);

CREATE TABLE sms_logs
(
    id                  INT PRIMARY KEY AUTO_INCREMENT,
    client_id     	INT,
    created  		DATETIME,
    phone_number 	VARCHAR(20),
    message 		VARCHAR(50),
    FOREIGN KEY (client_id) REFERENCES clients (id)
);
```

В дочерней таблице `sms_logs` мы создали поле `client_id` - внешний ключ, которое указывает на id клиента в родительской таблице clients. Таким образом образовалась связь один ко многим. У одного клиента может быть много СМС, а одна конкретная СМС может принадлежать только одному клиенту. На схеме в СУБД MySQL это будет выглядеть следующим образом:

![](https://ucarecdn.com/43153775-8b18-4985-ae49-a8e302a6ee74/)

Внешний ключ и поле, на которое он ссылается должны быть одинаковых типов. Чаще всего это `INT` либо `GUID`. 

В табличном виде, структура полей будет иметь следующий вид:  
**Table:** **clients**  
**Columns:**

|   |   |
|---|---|
|**id**|int AI PK|
|created|datetime|
|mobile|varchar(20)|
|email|varchar(50)|
|is_active|tinyint(1)|

**Table:** **sms_logs**  
**Columns:**

|   |   |
|---|---|
|**id**|int AI PK|
|**client_id**|int|
|created|datetime|
|phone_number|varchar(20)|
|message|varchar(50)|