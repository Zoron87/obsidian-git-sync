AutoFixture - это библиотека, которая позволяет создавать фейковые объекты для использования в unit тестах. Предназначения для повышения производительности разработки на основе тестирования и повышения безопасности модульных тестов.
Также устраняет необходимость ручного кодирования анонимных переменных в рамках этапа настройки юнит тестов. 
Среди других ф-ций он предлагает универсальную реализацию шаблона Test Data Buider.

Примеры:

```C#
var fixture = new Fixture();
var randomDate = fixture.Create<DateTime>(); // генерация рандомной даты
var randomString = fixture.Create<string>(); // генерация рандомной строки
var randomDouble =   fixture.Create<double>(); // генерация рандомного числа
var randomPerson = fixture.Create<Person>(); // генерация рандомного экземпляра типа Person

var randomPerson = fixture.CreateMany<Person>(); // генерация 3 рандомных экземпляров типа Person
var random10Person = fixture.CreateMany<Person>(10); // генерация 10 рандомных экземпляров типа Person


fixture.Register(() => "example.net");
var rndString = fixture.Create<string>(); // rndString = example.net

// Наполнение списка 
var person = new List<Person>();
per.AddManyTo(person);

// Создание определенного пользователя с опред. настройками
var people = fixture
			.Build<Person>()
			.With(x => x.Id, 555)
			.With(x => x.DateOfBirth, new DateTime(2000, 10, 11))
			.With(x => x.LastName, "Пупкин")
			.Create();

```