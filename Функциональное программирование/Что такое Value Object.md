Value Object (VO) - это объект, который:
1. Определяется своими атрибутами (значениями), а не идентификатором.
2. Неизменяем (immutable) после создания.
3. Сравнивается по значению, а не по ссылке.

**Аналогия:**  
Денежная купюра. Две купюры по $10 равны, даже если их серийные номера разные. Их ценность определяется номиналом, а не физическим объектом.

**Отличие от Entity:**

- **Entity** имеет уникальный идентификатор (например, пользователь с `UserId`).
- **Value Object** не имеет идентификатора. Два VO равны, если все их поля совпадают.

Value Objects должны гарантировать корректность данных. Используйте приватный конструктор и фабричные методы.

```C#
public record Email
{
    public string Value { get; }

    private Email(string value) => Value = value;

    public static Email Create(string email)
    {
        if (string.IsNullOrEmpty(email) || !email.Contains('@'))
        {
            throw new ArgumentException("Invalid email");
        }
        return new Email(email);
    }
}
```

// Использование
var email = Email.Create("test@example.com");

#Функциональное_программирование 