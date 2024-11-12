
Ранее было рассмотрена два способа изменения функциональности методов, унаследованных от базового класса - скрытие и переопределение. В чем разница между двумя этими способами?

### Переопределение

Возьмем пример с переопределением методов:

```C#
class Person
{
    public string Name { get; set; }
    public Person(string name)
    {
        Name = name;
    }
    public virtual void Print()
    {
        Console.WriteLine(Name);
    }
}
class Employee : Person
{
    public string Company { get; set; }
    public Employee(string name, string company)
        : base(name)
    {
        Company = company;
    }
 
    public override void Print()
    {
        Console.WriteLine($"{Name} работает в {Company}");
    }
}
```

Используем классы в программе:

```C#
`Person tom =` `new` `Employee(``"Tom"``,` `"Microsoft"``);`

`tom.Print();`        `// Tom работает в Microsoft`
```

При вызове `tom.Print()` выполняется реализация метода Print из класса Employee, несмотря на то, что переменная `tom` - переменная типа Person.

Для работы с виртуальными методами компилятор формирует таблицу виртуальных методов (Virtual Method Table или VMT). В нее записывается адреса виртуальных методов. Для каждого класса создается своя таблица.

Когда создается объект класса, то компилятор передает в конструктор объекта специальный код, который связывает объект и таблицу VMT.

А при вызове виртуального метода из объекта берется адрес его таблицы VMT. Затем из VMT извлекается адрес метода и ему передается управление. То есть процесс выбора реализации метода производится во время выполнения программы. Собственно так и выполняется виртуальный метод. Следует учитывать, что так как среде выполнения вначале необходимо получить из таблицы VMT адрес нужного метода, то это немного замедляет выполнение программы.

### Скрытие

Теперь возьмем те же классы Person и Employee, но вместо переопределения используем сокрытие:

```C#
class Person
{
    public string Name { get; set; }
    public Person(string name)
    {
        Name = name;
    }
 
    public void Print()
    {
        Console.WriteLine(Name);
    }
}
 
class Employee : Person
{
    public string Company { get; set; }
    public Employee(string name, string company)
            : base(name)
    {
        Company = company;
    }
    public new void Print()
    {
        Console.WriteLine($"{Name} работает в {Company}");
    }
}
```

И посмотрим, что будет в следующем случае:

```C#
Person tom = new Employee("Tom", "Microsoft");
tom.Print();        // Tom
```

Переменная tom представляет тип Person, но хранит ссылку на объект Employee. Однако при вызове метода Print будет выполняться та версия метода, которая определена именно в классе Person, а не в классе Employee. Почему? Класс Employee никак не переопределяет метод Print, унаследованный от базового класса, а фактически определяет новый метод. Поэтому при вызове `tom.Print()` вызывается метод Print из класса Person.