**ExecuteNonQuery** - Выполняет `SQL`-запрос и возвращает количество измененных записей. Подходит для выражений `INSERT`, `UPDATE`, `DELETE`, `CREATE`.
```C#
// Переопределяем SQL-выражение, вставляем данные в таблицу 
command.CommandText = $@"INSERT INTO users (first_name, last_name, email, age) VALUES ({firstName}, {lastName}, {email}, {age}); "; 

// Выполнение команды на вставку данных
execute = command.ExecuteNonQuery();
```

Казалось бы все хорошо, но при попытке выполнить `SQL`-запрос при наличии в `email` символа `@`, мы получим исключение:
![[Pasted image 20241115210454.png]]
Проблема с использованием символа `@` в строке `SQL` связана с тем, что символ `@` используется для указания параметров в `SQL`-запросах. Для решения этой проблемы лучше использовать **параметризованные запросы, что также позволит избежать проблем с `SQL`-инъекциями**. Параметры добавляются с помощью метода `AddWithValue()`.
```C#
// Переопределяем SQL-выражение, вставляем данные в таблицу 
command.CommandText = $@"INSERT INTO users (first_name, last_name, email, age) VALUES (@firstName, @lastName, @email, @age); "; 

// Добавляем параметры запроса 
command.Parameters.AddWithValue("@firstName", firstName); command.Parameters.AddWithValue("@lastName", lastName); command.Parameters.AddWithValue("@email", email); command.Parameters.AddWithValue("@age", age);
```