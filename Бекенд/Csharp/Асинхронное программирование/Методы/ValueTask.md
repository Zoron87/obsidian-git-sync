**`ValueTask<T>`:** Более легковесный аналог `Task<T>`, предназначенный для сценариев, где результат может быть получен синхронно, что уменьшает накладные расходы на создание объектов.

```C#
public async ValueTask<int> ПолучитьЧислоAsync(bool now)
{
    if (now)
    {
        // Возвращаем результат синхронно
        return 42;
    }
    else
    {
        // Выполняем асинхронную операцию
        await Task.Delay(1000);
        return 42;
    }
}
```
**Важно:** `ValueTask<T>` следует использовать только в тех случаях, когда есть реальная необходимость в оптимизации, так как он сложнее в использовании по сравнению с `Task<T>`