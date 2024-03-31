Ожидаемые (awaitable) методы - это асинхронные методы, завершение которых можно подождать, если необходим результат их работы в данный момент.
Ожидать завершения асинхронных методов и задач необходимо с помощью оператора await.

Другие способы ожидания:
- Свойство Result
- Методы ожидания (Wait(), WaitAll(), WaitAny())
- Метод GetResult(), вызванный на экземпляре структуры TaskAwaiter, которая получена через вызов метода GetAwaiter().

```C#
await example.Method2Async();
string result1 = await example.Method3Async();

example.Method2Async().Wait();
string result2 = example.Method3Async().Result;

example.Method2Async().GetAwaiter().GetResult();
string result3 = example.Method3Async().GetAwaiter().GetResult;
```

**НЕ рекомендуется прибегать к вызову  GetAwaiter(), пользуйтесь ключевым словом await**.
Тип TaskAwaiter и его члены предназначены для внутреннего использования компилятором.