Позволяет задать действие, которое будет выполнено после завершения задачи.


```C#
public void TaskWithContinuos()
{
    Task task = Task.Run(() =>
    {
        Thread.Sleep(2000);
        Console.WriteLine("Основная задача завершена.");
    });

    task.ContinueWith((lastTask) =>
    {
        Console.WriteLine("Продолжение задачи.");
    });
}
```