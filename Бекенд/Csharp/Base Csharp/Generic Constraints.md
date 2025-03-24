
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

| СИНТАКСИС                           | ОПИСАНИЕ                                                 |        |
| ----------------------------------- | -------------------------------------------------------- | ------ |
| `where T : class`                   | T должен быть ссылочным типом                            |        |
| `where T : struct`                  | T должен быть значимым типом                             |        |
| `where T : new()`                   | T должен иметь публичный конструктор без параметров      |        |
| `where T : BaseClass`               | T должен наследовать от`BaseClass`                       |        |
| `where T : IInterface`              | T должен реализовывать интерфейс`IInterface`             |        |
| where T : unmanaged                 |                                                          |        |
| where T : Enum                      |                                                          |        |
| where T : Delegate                  |                                                          |        |
| where T : notnull                   |                                                          |        |
| where T : default                   |                                                          |        |
| where T : INumber<T>                |                                                          |        |
| where T : IAdditionOperators<T,T,T> | T должен поддерживать оператор сложения                  |        |
| where T : Span<T>                   | Ограничение на ref-структуры                             | C# 11+ |
| where T : U                         | T должен быть производным от другого generic-параметра U |        |

**Пример с несколькими ограничениями:**

```C#
void Example<T>() where T : class, IComparable, new() { }
```



#Csharp #Generics 