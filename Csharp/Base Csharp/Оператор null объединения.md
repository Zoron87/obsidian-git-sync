![[Pasted image 20230904013633.png]]
Оператор null объединения "??" устанавливает значения по умолчанию для типов, которые допускают значения [[null]].
Аналогичен методу GetValueOrDefault() (В скобках указывается дефолтное значение или вернется дефолтное для указанного типа переменной)
Пример:

```C#
int? i = null;
Console.WriteLine(i.GetValueOrDefault()); // 0
Console.WriteLine(i.GetValueOrDefault(3)); // 3
Console.WriteLine(i ?? 3); // 3
```


#Csharp 