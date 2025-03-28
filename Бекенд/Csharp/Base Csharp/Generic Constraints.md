
Базовый синтаксис:

```C#
class MyClass<T> where T : BaseType {
    // implementation
}
```

Ограничения 'where T' определяют требования к типам, которые могут быть использованы в качестве аргументов для обобщенного типа.

Зачем нужны:

 - Ограничения возможных типов
 - Доступ к методам и свойствам базового типа
 - Обеспечение определенного поведения


### Виды ограничений типа (Constraints)

| СИНТАКСИС                           | ОПИСАНИЕ                                                                  |                  |
| ----------------------------------- | ------------------------------------------------------------------------- | ---------------- |
| `where T : class`                   | T должен быть ссылочным типом                                             |                  |
| `where T : struct`                  | T должен быть значимым типом                                              |                  |
| `where T : new()`                   | T должен иметь публичный конструктор без параметров                       |                  |
| `where T : BaseClass`               | T должен наследовать от`BaseClass`                                        |                  |
| `where T : IInterface`              | T должен реализовывать интерфейс`IInterface`                              |                  |
| where T : unmanaged                 | T должен быть неуправляемым типом (примитивы, struct без ссылочных полей) |                  |
| where T : Enum                      | T должен быть типом перечисления                                          |                  |
| where T : Delegate                  | T должен быть типом делегата                                              |                  |
| where T : notnull                   | T должен быть не-nullable типом (ссылочным или значимым)                  | C# 8.0+          |
| where T : default                   | Разрешает переопределение с менее строгими ограничениями                  | C# 9.0+          |
| where T : INumber<T>                | T должен реализовывать интерфейс числовых операций                        | C# 11+ (.NET 7+) |
| where T : IAdditionOperators<T,T,T> | T должен поддерживать оператор сложения                                   | C# 11+ (.NET 7+) |
| where T : Span<T>                   | Ограничение на ref-структуры                                              | C# 11+           |
| where T : U                         | T должен быть производным от другого generic-параметра U                  |                  |

**Пример с несколькими ограничениями:**

```C#
void Example<T>() where T : class, IComparable, new() { }
```

 **unmanaged:**

```C#
unsafe void Process<T>(T value) where T : unmanaged 
{
    byte* ptr = &value;
    // Работа с указателем
}
```

#Csharp #Generics 