`MySqlDataReader`  используется для чтения данных из базы данных.
```C#
public static void Main() { 
	// Строка подключения к базе данных MySQL 
	string connectionString = "Server=localhost;Database=test;Uid=root;Pwd=;"; 
	// Создание подключения с автоматическим закрытием соединения 
	using (var connection = new MySqlConnection(connectionString)) 
	{ 
		// Открытие соединения 
		connection.Open(); 
		
		// Составление SQL-выражения для чтения данных из таблицы
		string sqlQuery = "SELECT * FROM USERS;"; 
		
		// Создание объекта для инкапсуляции выполняемого SQL-выражения 
		using (MySqlCommand command = new MySqlCommand(sqlQuery, connection)) 
		{ 
			// Создание объекта, который используется для чтения данных 
			using (MySqlDataReader reader = command.ExecuteReader()) 
			{ } 
		};
	}
}
```