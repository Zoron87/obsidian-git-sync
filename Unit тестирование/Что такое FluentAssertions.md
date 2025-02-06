FluentAssertions — это библиотека, которая служит «надстройкой» над существующими тестовыми фреймворками (xUnit, NUnit, MSTest и т. д.). Она предоставляет более выразительный (fluent, «цепочечный») синтаксис для проверок в тестах. Примеры:

Стандартный способ на xUnit:

```C#
Assert.Equal(expected, actual);
```

С использованием FluentAssertions:

```C#
actual.Should().Be(expected);
```

Как видно, второй вариант читается почти как обычное предложение на английском: «actual ДОЛЖЕН быть expected».

Другие ключевые примеры:

```C#
actual.Should().NotBeNull();
actual.Should().BeOfType<MyType>();
list.Should().Contain(x => x.Id == 5);
action.Should().Throw<InvalidOperationException>();
```

## Базовый синтаксис и основные методы


### Метод Should().Be()

Самый простой способ проверки равенства двух значений — это:
```C#
actualValue.Should().Be(expectedValue);
```
Обратите внимание: мы можем добавить `because: "..."` (или использовать перегрузку `Be(expected, "причина")`) для того, чтобы при падении теста в сообщении появилась причина. Например:

```C#
sum.Should().Be(12, "потому что 5 плюс 7 должно дать 12");
```

Это делает логику теста самодокументированной.

- Should().NotBe(expected) — проверка, что значение НЕ равно ожидаемому:
```C#
actual.Should().NotBe(10);
```
    
Should().BeNull() / `Should().NotBeNull() — проверка на null:
```C#
string s = null; s.Should().BeNull();
```
    
Should().BeTrue()` / `Should().BeFalse() — проверка булевых значений:
```C#
bool result = (5 > 3); result.Should().BeTrue();
```
    
Should().BeInRange(min, max) — проверка, что число лежит в заданном диапазоне:
```C#
int number = 5; number.Should().BeInRange(1, 10);
```
    
Should().Match(...) — проверяет соответствие условию (делает «гибкую» проверку). Можно использовать лямбда-выражения с `Match` (или `MatchRegex` для проверки по регулярному выражению).

### Проверка строк

FluentAssertions предлагает обширный набор методов для проверки строк:

```C#
str.Should().BeNullOrEmpty();
str.Should().BeNullOrWhiteSpace();
str.Should().Contain("text");
str.Should().StartWith("Hello");
str.Should().EndWith("World");`
str.Should().Match("*some pattern*");
```

### Основные методы для коллекций

FluentAssertions особенно силён в проверках списков и массивов. Ниже перечислены лишь некоторые основные методы:

```C#
collection.Should().BeEmpty();
collection.Should().NotBeEmpty();
collection.Should().HaveCount(int expectedCount);
collection.Should().Contain(item);
collection.Should().ContainSingle();
collection.Should().OnlyHaveUniqueItems();
collection.Should().Equal(params object[] items);
collection.Should().BeInAscendingOrder();
collection.Should().BeInDescendingOrder();
```

### Настройка правила сравнения

Часто нам нужно сравнить только некоторые поля. В таком случае можно воспользоваться дополнительными опциями:

```C#
actual.Should().BeEquivalentTo(expected, options => 
    options
        .Excluding(person => person.Age)
        .Including(person => person.Name),
    "важно сравнить только имя");

```

### Проверка ссылочной равности (ReferenceEquality)

Если вам важно, чтобы объекты в памяти были одной и той же ссылкой, используйте:
```C#
actual.Should().BeSameAs(expectedObject);
```

## Проверка исключений

### Общая идея

FluentAssertions упрощает проверку того, что некоторый вызов выбросил определённое исключение (или наоборот, **не** выбросил). Логика типовая: вы оборачиваете вызов в делегат (чаще всего лямбда `Action`) и вызываете метод `Should().Throw<TException>()` или `Should().NotThrow<TException>()`.
```C#
[Fact]
public void Should_ThrowException_When_DividingByZero()
{
    // Arrange
    var calculator = new Calculator();
    // Act
    Action act = () => calculator.Divide(10, 0);
    // Assert
    act.Should().Throw<DivideByZeroException>("делить на ноль нельзя");
}
```

### Проверка сообщения или свойств исключения

Дополнительно можно проверить сообщение исключения или другие свойства:
```C#
act.Should().Throw<ArgumentException>()
   .WithMessage("*cannot be empty*");
```
Символ `*` здесь обозначает «любой текст до или после строки `cannot be empty`». Или если нужно полное соответствие:

### Проверка на отсутствие исключений

Если вы хотите удостовериться, что никакого исключения _не_ происходит, используйте:
```C#
act.Should().NotThrow();
```
Или:
```C#
act.Should().NotThrowAnyException();
```