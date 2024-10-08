Тип Parallel - это класс, который упрощает параллельное вычисление кода. Благодаря его метода можно быстро распараллелить вычислительный процесс.

У класса Parallel доступно всего 3 метода для параллельной обработки:

- **Invoke** - параллельное выполнение делегатов в Action.
- **For** - параллельное выполнение итераций.
- **ForEach** - параллельный перебор коллекций.

При этом параллельное выполнение этих методов можно настраивать с помощью выбора перегрузки, которая принимает в качестве параметров класс ParallelOptions.

## ParallelOptions

С помощью класса **ParallelOptions** можно настроить выполнение параллельных методов. Для этого необходимо создать экземпляр класса **ParallelOptions** и задать необходимые параметры.

Класс **ParallelOptions** имеет 3 различных настройки:

1. **CancellationToken** - указание **CancellationToken** для возможности отмены выполнения параллельных операций.
2. **MaxDegreeOfParallelism** - возможность для установки или получения максимального кол-ва одновременных задач.
3. **TaskScheduler** - установка своего планировщика задач для параллельного выполнения.