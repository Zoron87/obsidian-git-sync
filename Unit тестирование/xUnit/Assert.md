## Строки:
```csharp
// читается однозначно и даёт понятную диагностику при падении.
Assert.Equal(expected, actual)` 
```

```csharp
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