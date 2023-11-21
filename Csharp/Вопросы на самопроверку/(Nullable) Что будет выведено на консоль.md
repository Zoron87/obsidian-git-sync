
```C#
static void NullableTest() 
{ 
	int? a = null; 
	object aObj = a; 
	
	int? b = new int?(); 
	object bObj = b; 
	
	Console.WriteLine(Object.ReferenceEquals(aObj, bObj)); // True or False? 
}
```
Что ж, давайте немного порассуждаем. Возьмём несколько основных направлений мысли, которые, как мне кажется, могут возникнуть.  
  
**1. Исходим из того, что _int?_ — ссылочный тип.**  
  
Давайте рассуждать так, что _int?_ — это ссылочный тип. В таком случае в _а_ будет записано значение _null_, оно же будет записано и в _aObj_ после присвоения. В _b_ будет записана ссылка на какой-то объект. Она также будет записана и в _bObj_ после присвоения. В итоге, _Object.ReferenceEquals_ примет в качестве аргументов значение _null_ и ненулевую ссылку на объект, так что…  
  
**Казалось бы, Всё очевидно, ответ — False!**

**Разбираемся**
  
Есть два пути — простой и интересный.  

### Простой путь

  
_int?_ — это _Nullable<int>_. Открываем [документацию по _Nullable<T>_](https://docs.microsoft.com/en-us/dotnet/api/system.nullable-1?view=netcore-3.1), где смотрим раздел "Boxing and Unboxing". В принципе, на этом всё — поведение там описано. 

Источник статьи:
[https://habr.com/ru/companies/pvs-studio/articles/525818/](https://habr.com/ru/companies/pvs-studio/articles/525818/)

#Nullable #задачи_Csharp 

