## Базовое использование

```C#
// Создаем mock объект
var productRepositoryMock = new Moq.Mock<IProductRepository>();

// Настраиваем
productRepositoryMock.Setup(obj => obj.GetById("1")).Returns(new Product("1", "TEST123"));

// И проверяем
IProductRepository productRepository = productRepositoryMock.Object;
Console.WriteLine(productRepository.GetById("1").Name);  // TEST123
```

## LINQ to Mocks
```C#
// Имитируем объект в одну строчку
IProductRepostitory productRepository = Moq.Mock.Of<IProductRepository>(obj => obj.GetById("1") == new Product("1", "TEST123") && 
obj.GetById("2") == new Product("2", "TEST321"));

// Проверяем
Console.WriteLine(productRepository.GetById("2").Name); // TEST321
```
## Волшебное It

| Метод | Комментарий |
| ---- | ---- |
| It.IsAny | Ограничений нет, соответствует любому значению аргумента |
| It.IsNotNull | Все значения, кроме null |
| It.Is | Задает предикат для сопоставления возможных значений |
| It.IsInRange | Задает возможные минимальные и максимальные диапазоны значений |
| It.IsIn | Значение должно соответствовать одному из значений в перечислении |
| It.IsNotIn | Подойдет любое значение, которое не находится в перечислении |
| It.IsRegex | Значение должно соответствовать регулярному выражению. Только для строк |
| It.Ref.Any | Используется для ref-аргументов |
Пример
```C#
// Создаем mock Объект
var productRepositoryMock = new Moq.Mock<IProductRepository>();
productRepositoryMock.Setup(obj => obj.GetTotalCount(Moq.It.IsAny<string>())).Returns(2);

// Вызываем тестируемый метод
OutTestMethod(productRepositoryMock.Object);

// Верифицируем
productRepositoryMock.Verify(obj => obj.GetTotalCount(Moq.It.IsRegex(@"^Test".*")));
```

## Возможности: имитация классов
```C#
public class MyCompany
{
	public virtual decimal GiveMeSalary() => 1000;
	public virtual event EventHandler<decimal> OnReceive;
}
var MyCompany = new MyCompany();
Console.WriteLine(myCompany.GiveMeSalary().ToString("C")); // $1000

var myCompanyMock = new Moq.Mock<MyCompany>();
myCompanyMock.Setup(obj => obj.GiveMeSalary()).Returns(2000));

var myDreamsCompany = myCompanyMock.Object;
Console.WriteLine(myDreamCompany.GiveMeSalary().ToString("C")); // $2000 
```