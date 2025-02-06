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