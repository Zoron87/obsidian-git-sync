
## Проблема

Дан класс `Student`:

```cs
public class Student
{
    public string Name { get; set; }
    public double AverageGrade { get; set; }

    public Student(string name, double averageGrade)
    {
        Name = name;
        AverageGrade = averageGrade;
    }
}
```

Допустим, у нас есть задача обработки данных о студентах. Нам необходимо написать функцию, которая из списка студентов извлекает **максимальный** и **минимальный** средние баллы. Эти данные нужно вычислить **1 раз** в одном месте программы.

## Решение 1

Так как с функции нельзя вернуть больше одного значения, то создадим класс `StudentsPoints`, в котором будут два свойства с максимальным и минимальным средним баллом:

```cs
public class StudentsPoints
{
    public double Max { get; set; }
    public double Min { get; set; }
}
```

Теперь реализуем функцию `GetStudentsPoints`, возвращающая объект класса `StudentsPoints`:

```cs
 public static StudentsPoints GetStudentsPoints(List<Student> students)
 {
     double min = 100;
     double max = 0;
     foreach (Student stud in students)
     {
         if (stud.AverageGrade > max) max = stud.AverageGrade;
         if (stud.AverageGrade < min) min = stud.AverageGrade;
     }
     return new StudentsPoints { Max = max, Min = min };
 }
```

Пример использования функции:

```cs
List<Student> students = new List<Student>
{
    new Student("Леша", 3),
    new Student("Петя", 2),
    new Student("Вася", 1),
};
var points = GetStudentsPoints(students);
Console.WriteLine(points.Min); // 1
Console.WriteLine(points.Max); // 3
```

Заметим, что мы создали НОВЫЙ класс `StudentsPoints` **ТОЛЬКО** для того, чтобы вернуть **два или более значения** с функции `GetStudentsPoints`.  Поэтому такое решение **НЕ** оптимальное. 

## Решение 2

**Кортежи** `(Tuple)` — это тип данных, который содержит последовательность из **N** элементов, где **N** не превышает восьми.

**Кортежи** предоставляют возможность для группировки нескольких элементов в упрощенной структуре данных. Как правило, они используются для группирования слабо связанных элементов, которые могут отличаться типом данным. **Кортежи** также могут передаваться в метод в качестве параметров или служить возвращаемым результатом. В частности, очень удобно использовать **кортеж** как возвращаемый из функции тип, так как функции возвращают **только одно** значение.

Теперь мы можем вернуть из функции **сразу оба значения** — максимальный и минимальный средний балл. И нам **не нужно** для этого создавать класс `StudentsPoints` с двумя полями. Этим и хороши **кортежи**!

Изменим нашу функцию `GetStudentsPoints`, чтобы она возвращала `Tuple` с максимальным и минимальным баллом:

```cs
public static Tuple<double, double> GetStudentsPoints(List<Student> students)
{
    double min = 100;
    double max = 0;
    foreach (Student stud in students)
    {
         if (stud.AverageGrade > max) max = stud.AverageGrade;
         if (stud.AverageGrade < min) min = stud.AverageGrade;
    }
    // Возвращаем кортеж из двух значений max и min
    return new Tuple<double, double>(max, min);
}
```

В угловых скобках указываются типы данных каждого элемента соответственно. В данном случае, это два вещественных числа `double`.

Пример использования:

```cs
List<Student> students = new List<Student> 
{
    new Student("Леша", 3),
    new Student("Петя", 2),
    new Student("Вася", 1),
};

Tuple<double, double> points = GetStudentsPoints(students);
Console.WriteLine(points); // (3, 1)
```

Здесь мы **явно** объявили кортеж с двумя `double` элементами и записали туда результат функции `GetStudentsPoints`

**Кортеж** может быть объявлен и более коротким способом, используя ключевое слово `var`:

```
var points = GetStudentsPoints(students);
Console.WriteLine(points); // (3, 1)
```

Кроме того `Tuple` можно создать помощью функции `Create`:

```
var tuple = Tuple.Create<int, int>(1, 3);
```

## Обращение к элементам кортежа

Мы можем обращаться к каждому из значений кортежа, используя имена по умолчанию:

`Item[порядковый_номер_элемента_в_кортеже]` **начиная с единицы**

Пример:

```cs
var points = GetStudentsPoints(students);
Console.WriteLine(points.Item1); // 3
Console.WriteLine(points.Item2); // 1
```

**Кортежи** `(Tuple)` — это ссылочный тип данных. То есть данные кортежей хранятся в `куче`. Кроме того `Tuple` — это неизменяемый тип данных. Мы не можем поменять значение любого из его элементов.

```cs
var points = GetStudentsPoints(students);
points.Item1 = 0; // Ошибка компиляции
```

## Переопределенные методы `Tuple`

1. `Equals(object obj)` — сопоставляет кортежу какой-либо объект и возвращает `true`, если:

- Это объект такого же типа
- Все его элементы имеют те же типы, что у данного кортежа.
    
- Все его элементы идентичны элементам текущего кортежа. 
    

Также вместо `Equals`, можно использовать `==`,  работать все будет точно так же, например:

```cs
var t1 = Tuple.Create<int, int>(0, 0);
var t2 = Tuple.Create<int, int>(1, 2);

Console.WriteLine(t1 == t2); // False
```

2. `ToString()` — возвращает строку,  вида `(Item1, Item2, ... )`

3. `Tuple.Create()`  — создает новый объект кортежа. Можно использовать для создания экземпляра кортежа без необходимости явно указывать типы элементов.