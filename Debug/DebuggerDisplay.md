### **Что такое `[DebuggerDisplay]` и зачем это нужно?**

При отладке объектов классов Visual Studio по умолчанию выводит только имя типа. Это не всегда информативно. Атрибут `[DebuggerDisplay]` позволяет указать, какие поля или свойства объекта отображать при отладке.

Это значительно упрощает просмотр значений и экономит время на открытии и раскрытии сложных объектов.

### 🔍 **Как использовать атрибут `[DebuggerDisplay]` пошагово:**

- Добавь атрибут `[DebuggerDisplay]` над определением класса.
    
- Укажи внутри атрибута строку с полями или свойствами класса, которые должны отображаться.
    

### 📗 **Практический пример:**

Без атрибута:

```C#
public class User
{
    public string Name { get; set; }
    public int Age { get; set; }
    public bool IsActive { get; set; }
}

// При отладке увидишь просто: {Namespace.User}
```

С атрибутом:

```C#
[DebuggerDisplay("Имя={Name}, Возраст={Age}, Активен={IsActive}")]
public class User
{
    public string Name { get; set; }
    public int Age { get; set; }
    public bool IsActive { get; set; }
}
```

```C#
Имя=Алексей, Возраст=27, Активен=True
```