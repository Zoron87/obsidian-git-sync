**CLR** (Common language runtime) — общеязыковая исполняющая среда. Она обеспечивает интеграцию языков и позволяет объектам благодаря стандартному набору типов и метаданным), созданным на одном языке, быть «равноправными гражданами» кода, написанного на другом.

Другими словами CLR этот тот самый механизм, который позволяет программе выполняться в нужном нам порядке, вызывая функции, управляя данными. И все это для разных языков (c#, VisualBasic, Fortran). Да, CLR действительно управляет процессом выполнения команд (машинного кода, если хотите) и решает, какой кусок кода (функцию) от куда взять и куда подставить прямо в момент работы программы. Процесс компиляции представлен на рисунке:  
[![CLR](https://habrastorage.org/r/w1560/getpro/habr/post_images/c16/b9d/5c2/c16b9d5c218c0b445e5a119d3f280f74.jpg)

**IL** (Intermediate Language) — код на специальном языке, напоминающим ассемблер, но написанном для .NET. В него преобразуется код из других языков верхнего уровня (c#, VisualBasic). Вот тогда-то и пропадает зависимость от выбранного языка. Ведь все преобразуется в IL (правда тут есть оговорки соответствия общей языковой спецификации CLS, что не входит в рамки данной статьи)  
Вот как он выглядит для функции SomeClass::GetAge()  
[![IL](https://habrastorage.org/r/w1560/getpro/habr/post_images/d74/489/e68/d74489e68d5cdd93ad46682e50960afc.jpg)]

# Работа JIT
  
И так, что же происходит, когда запускается впервые программа?  
Сперва происходит анализ заголовка, чтобы узнать какой процесс запустить (32 или 64 разрядный). Затем загружается выбранная версия файла MSCorEE.dll ( _C:\Windows\System32\MSCorEE.dll_ для 32разрядных процессоров)  
После чего вызывается метод, расположенный MSCorEE.dll, который и инициализирует CLR, сборки и точку входа функции Main() нашей программы.  

```C#
static void Main()  
{  
  System.Console.WriteLine("Hello ");  
  System.Console.WriteLine("Goodbye");  
}
```

  Для выполнения какого-либо метода, например System.Console.WriteLine(«Hello „), IL должен быть преобразован в машинные команды (те самые нули и единицы) Этим занимается Jiter или just-in-time compiler.  
  
Сперва, перед выполнением Main() среда CLR находит все объявленные типы (например тип Console).  
Затем определяет методы, объединяя их в записи внутри единой “структуры» (по одному методу определенному в типе Console).  
Записи содержат адреса, по которым можно найти реализации методов (т.е. те преобразования, которые выполняет метод).  
  
[![Jit](https://habrastorage.org/r/w1560/getpro/habr/post_images/d97/c0a/cd1/d97c0acd1c164af2e91b81ea2d70087e.jpg)](http://www.flickr.com/photos/49055286@N07/4506840113/ "Jit by asArtem, on Flickr")  
  
При первом обращение к функции WriteLine вызывается JiT-compiler.  
JiTer 'у известны вызываемый метод и тип, которым определен этот метод.  
JiTer ищет в метаданных соответствующей сборки — реализацию кода метода (код реализации метода WriteLine(string str) ).  
Затем, он проверяет и компилирует IL в машинный код (собственные команды), сохраняя его в динамической памяти.  
После JIT Compiler возвращается к внутренней «структуре» данных типа (Console) и заменяет адрес вызываемого метода, на адрес блока памяти с исполняемыми процессорными командами.  
После этого метод Main() обращается к методу WriteLine(string str) повторно. Т.к. код уже скомпилирован, обращение производится минуя JiT Compiler. Выполнив метод WriteLine(string str) управление возвращается методу Main().  
  
Из описания следует, что «медленно» работает функция только в момент первого вызова, когда JIT переводит IL код в инструкции процессора. Во всех остальных случаях код уже находится в памяти и подставляется как оптимизированный для данного процессора. Однако если будет запущена еще одна программа в другом процессе, то Jiter будет вызван снова для того же метода. Для приложений выполняемых в х86 среде JIT генерируется 32-разрядные инструкции, в х64 или IA64 средах — соответственно 64-разрядные

https://habr.com/ru/articles/90426/

------------------------------------------------------

**JIT-компиляция** – это метод повышения производительности интерпретируемых программ. JIT расшифровывается как _Just-in-time_. Во время выполнения программа может быть скомпилирована в машинный код для повышения ее производительности. Также этот метод известен как динамическая компиляция.