FluentAssertions — это библиотека, которая служит «надстройкой» над существующими тестовыми фреймворками (xUnit, NUnit, MSTest и т. д.). Она предоставляет более выразительный (fluent, «цепочечный») синтаксис для проверок в тестах. Примеры:

Стандартный способ на xUnit:

```C#
Assert.Equal(expected, actual);
```

С использованием FluentAssertions:

```C#
actual.Should().Be(expected);
```