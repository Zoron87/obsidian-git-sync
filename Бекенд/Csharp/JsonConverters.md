1. Общая идея: что делают ReadJson и WriteJson

### `ReadJson` — JSON → объект

Твоя задача:

> Взять _то, как данные лежат в JSON_, разобрать их и **собрать объект нужного типа**.

### `WriteJson` — объект → JSON

Твоя задача:

> Взять _объект в C#_ и **записать в JSON в нужном формате**.

Ключевая мысль:

> **`ReadJson` и `WriteJson` — две стороны одного договора.**  
> Если ты сериализуешь объект через `WriteJson`, потом десериализуешь через `ReadJson` — всё должно корректно туда-сюда ехать (по крайней мере, для тех полей, которые важны).

## 2. Как думать про `ReadJson`

Сигнатура:
```C#
public override object? ReadJson(
    JsonReader reader,
    Type objectType,
    object? existingValue,
    JsonSerializer serializer)
```
### Что тебе дают:

- `reader` — поток, **уже стоящий на начале данных** для твоего типа:
    - это может быть строка `"test"`,
    - или объект `{ "Type": 1, "Action": 2, ... }`,
    - или число `123`,
    - или массив `[ ... ]`.
- `objectType` — тип, который нужно вернуть (обычно твой целевой тип).
- `serializer` — чтобы, при желании, поручить десериализацию каких-то частей **встроенной логике** (типа `serializer.Deserialize<T>(reader)`).
### Типичная логика шаг за шагом

1. **Обработать `null`:**
```C#
if (reader.TokenType == JsonToken.Null)
    return null;
```
- **Определить форму данных:**
    - ожидаем ли тут:
        - строку?
        - объект?
        - массив
    - если форма неожиданная → кинуть понятное исключение.
- **Прочитать данные:**
    Есть два подхода:
    #### a) Потоковый (читая `reader` напрямую)
    Подходит для простых случаев, как с `UpperString`:
    ```C#
    if (reader.TokenType != JsonToken.String)
    throw new JsonSerializationException("Ожидалась строка");

	string? s = (string?)reader.Value;
	return new UpperString { Value = s };
    ```
    b) Через `JObject/JArray/JToken` (как у тебя в большом конвертере)
    ```C#
    JObject jObj = JObject.Load(reader);

	JToken typeToken = jObj["Type"];
	JToken actionToken = jObj["Action"];
	// ...
    ```
    Это проще, когда JSON сложный и надо много полей доставать.

**Преобразовать в нужный тип:**

- разобрать нужные поля;
- понять, какие классы создавать (например, RequestGroup` или `RequestSingle`);
- вернуть **объект целевого типа**, а не примитив.

В твоём большом конвертере:
```C#
return new RequestsCommand
{
    Type = type,
    Action = action,
    ObjectId = objectId
};
```
**Важно:**  
`ReadJson` **всегда должен вернуть либо:**
- объект нужного типа
- либо `null` (если ты реально хочешь так интерпретировать `null`).