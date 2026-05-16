# Почему контейнер не запрещает transient в singleton

Потому что transient — это не “короткоживущий scope-bound объект”.

Он означает:

> Создай новый экземпляр каждый раз, когда меня запрашивают.

Если singleton запрашивают один раз, то transient тоже будет создан один раз.

Это не нарушение правила DI-контейнера.

Но это может быть нарушением твоего **архитектурного ожидания**, если ты думал:

> Transient будет новым на каждую итерацию background worker-а.

Не будет.

Он будет новым на каждый resolve, а не на каждый вызов метода.

# 11. Когда нужен новый transient на каждую итерацию

Если тебе реально нужен новый transient каждый цикл, делай resolve внутри scope/операции:

```
protected override async Task ExecuteAsync(CancellationToken stoppingToken){    while (!stoppingToken.IsCancellationRequested)    {        await using var scope = _scopeFactory.CreateAsyncScope();        var processor = scope.ServiceProvider            .GetRequiredService<InvoiceProcessor>();        await processor.ProcessAsync(stoppingToken);        await Task.Delay(TimeSpan.FromMinutes(1), stoppingToken);    }}
```

Тогда на каждой итерации:

```
- создаётся новый scope;- создаётся новый InvoiceProcessor;- создаётся scoped DbContext;- всё корректно dispose-ится в конце итерации.
```