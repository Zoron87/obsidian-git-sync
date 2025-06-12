Иногда типы неизвестны заранее и их нужно создавать динамически.

### 🔸 **Создание Generic-типа с помощью рефлексии:**

```C#
Type genericTypeDefinition = typeof(List<>);
Type constructedType = genericTypeDefinition.MakeGenericType(typeof(int));
```

- `List<>` — открытый Generic-тип (без указания типа).
- `MakeGenericType` — создаёт закрытый тип (`List<int>`).

### **Полный пример создания и использования Generic-типа:**
```C#
Type openType = typeof(List<>);
Type closedType = openType.MakeGenericType(typeof(string));

object instance = Activator.CreateInstance(closedType);

MethodInfo addMethod = closedType.GetMethod("Add");
addMethod.Invoke(instance, new object[] { "Hello" });
addMethod.Invoke(instance, new object[] { "World" });

// Вывод содержимого списка
foreach (var item in (IEnumerable)instance)
{
	Console.WriteLine(item);
}
```