Value Task (Значимая задача) - представляет собой оберткой (wrapper) над обыкновенной задачей (Task). Была создана для уменьшения потребления ресурсов управляемой кучи.

Представляет собой структуру, которая появилась в C# 7.0. Была ведена для предотвращения выделения Task  объектов в  случае если результат нашей операции уже доступен синхронно, т.к. если это будет происходит слишком часто, то это приведет к расходам на новые объекты Task, поэтому и были введены  структуры ValueTask и ValueTask с указателем вместо заполнения типа TResult. Эти структуры являются обычными обертками над задачами, но в некоторых случаях эти структуры могут просто не получить ссылочный объект Task, а сразу результат. 
Эти структуры полноценно присутствуют в .Net Core, а для .NET Framework существует специальная сборка с именем System.Threading.Tasks.Extensions, она расширяет стандартные пространства имен по работе с нашими задачами, ее можно получить с помощью менеджера управления пакетами Nuget.  

**В некоторых случаях является сомнительной оптимизацией!**
![[GOM_4uMwfPdiDQ.png]]