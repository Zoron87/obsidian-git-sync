![[Pasted image 20230904013633.png]]
Оператор null объединения "??" устанавливает значения по умолчанию для типов, которые допускают значения [[null]].
Аналогичен методу GetValueOrDefault() (В скобках указывается дефолтное значение или вернется дефолтное для указанного типа переменной)
Пример:

```C#
int? i = null;
ConsoleWriteLine(i.GetValueOrDefault()) // 0
ConsoleWriteLine(i.GetValueOrDefault(3)) // 3
ConsoleWriteLine(i ?? 3) // 3
```


#Csharp 