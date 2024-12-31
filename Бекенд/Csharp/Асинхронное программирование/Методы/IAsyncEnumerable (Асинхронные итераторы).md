Позволяют создавать асинхронные последовательности, которые можно перебирать с помощью `await foreach`.

```C#
public async IAsyncEnumerable<int> GenDigitAsync()
{
    for (int i = 0; i < 5; i++)
    {
        await Task.Delay(500);
        yield return i;
    }
}

public async Task UseIteratorAsync()
{
    await foreach (var число in GenDigitAsync())
    {
        Console.WriteLine($"Получено число: {число}");
    }
}
```