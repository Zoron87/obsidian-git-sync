## Строки:

```csharp
// Вместо ручных `Contains` и т.п. лучше использовать строковые assert’ы:  
- `Assert.Contains(sub, str)`  
- `Assert.StartsWith(prefix, str)`  
- `Assert.Matches(regex, str)`  
//Это повышает выразительность теста.

// указываем ожидаемый результат и фактический 
Assert.Equal(expected, actual)`
 
//А если сравнение должно быть **без учёта регистра**, всё равно предпочитаем `Equal`, но с comparer/опцией:

`Assert.Equal("user@mail.com", actual, ignoreCase: true); 
// или 
Assert.Equal("user@mail.com", actual, StringComparer.OrdinalIgnoreCase);`
```

```csharp
// проверка результата на равенство (получим по итоу False/True)
Assert.True(result == 5);
```
```csharp
// проверка nullability.
`Assert.NotNull(obj)` / `Assert.Null(obj)` 

// объект точно типа T
Assert.IsType<T>(obj)

// объект типа `T` **или наследник/реализация**.
`Assert.IsAssignableFrom<T>(obj)` 

//  одна и та же ссылка (reference equality), а не “одинаковые значения”.
`Assert.Same(a, b)`
```
## Числа и диапазоны  

```csharp
// — когда важен диапазон (например, проценты, лимиты).
Assert.InRange(value, min, max)` 
```
## Коллекции
```csharp
// сравнивает элементы по порядку (это удобно для массивов/списков).
`Assert.Equal(expectedEnumerable, actualEnumerable)` 
```

## Assert.Collection — когда хочешь точечно проверить структуру
```csharp
Assert.Collection(
    result,
    first => Assert.Equal(10, first),
    second => Assert.Equal(7, second),
    third => Assert.Equal(3, third)
);
```
Это спасает от «assertion roulette» (много непонятных assert’ов подряд), потому что структура ожиданий видна сразу. 
- `Assert.Collection(actual, …)` лучше, когда ты хочешь:  
- **проверять каждый элемент разными условиями** (не только `Equal`),  
- сделать тест **самодокументируемым** (“первый элемент такой-то, второй такой-то…”),  
- избежать “каши” из множества Assert’ов.
Пример:
```csharp
`Assert.Collection( users, u => Assert.StartsWith("admin", u.Login), u => Assert.True(u.Age >= 18), u => Assert.Contains("@", u.Email) );`
```
**Assertion Roulette** — это когда в тесте много `Assert` подряд (или в цикле), и при падении непонятно, _какое именно ожидание сломалось_ и _почему это важно_.
## Assert.All / Assert.Contains
```csharp
// проверка инварианта для каждого элемента
`Assert.All(collection, item => ...)` 
```

# Исключения (Assert.Throws<T)
```csharp
Assert.Throws<DivideByZeroException>(() => Calculator.Divide(10, 0));
```
В xUnit есть перегрузки Throws, которые проверяют имя параметра у `ArgumentException`-наследников:
```csharp
Assert.Throws<ArgumentException>("email", () => NormalizeEmail(null!));
```

## Async (ThrowsAsync)

```csharp
Assert.ThrowsAsync<T>(...)
```