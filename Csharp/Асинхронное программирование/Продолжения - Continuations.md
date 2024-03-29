Продолжения (Continuations) - это асинхронная задача, вызываемая другой задачей (предшествующей) при своем завершении. Это некий вариант обратного вызова (Callback method).

Для добавления продолжения используют метод ContinueWith()
```C#
Task task =  new Task(new Action(Download));
Task Continuation = task.ContinueWith(new Action<Task>(ShowData));
```
Метод ContinueWith возвращает новый экземпляр класса Task, что позволяет выстраивать цепочки продолжений.