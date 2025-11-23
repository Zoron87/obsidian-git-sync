Сигнатура:
```C#
public override void WriteJson(
    JsonWriter writer,
    object? value,
    JsonSerializer serializer)
```

### Что тебе дают:

- `writer` — «ручка», которой ты рисуешь JSON:
    - `WriteStartObject`, `WritePropertyName`, `WriteValue`, `WriteStartArray`, ...
- `value` — объект, который надо сериализовать (обычно твоего типа).
- `serializer` — чтобы, если нужно, deleгировать сериализацию вложенных объектов.
### Типичная логика шаг за шагом

1. **Обработать `null`:**
2. ```C#
   if (value == null)
{
    writer.WriteNull();
    return;
}
   ```
   или
   ```C#
   if (value is not RequestsCommand cmd)
    throw new JsonSerializationException("Ожидался RequestsCommand");
   ```
- **Решить, какой JSON ты хочешь**
    Это ключевой шаг: **ты сам придумываешь формат**.
    - Для `UpperString` → просто строка `"HELLO"`.
    - Для `ImportRequestsCommand` → объект `{ "Type": ..., "Action": ..., ... }`.
        
- **Записать JSON в нужной форме**
    Для простого типа:
    
   ```C#
    writer.WriteValue(typed.Value.ToUpper());
    ```
    Для сложного:
    ```C#
    writer.WriteStartObject();

	writer.WritePropertyName("Type");
	serializer.Serialize(writer, cmd.Type);
	
	writer.WritePropertyName("Action");
	serializer.Serialize(writer, cmd.Action);
	
	writer.WritePropertyName("ImportRequests");
	serializer.Serialize(writer, cmd.ImportRequests);
	
	// ...
	
	writer.WriteEndObject();
    ```