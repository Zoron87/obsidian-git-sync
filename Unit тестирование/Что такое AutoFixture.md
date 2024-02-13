AutoFixture - это библиотека, которая позволяет создавать фейковые объекты для использования в unit тестах. Предназначения для повышения производительности разработки на основе тестирования и повышения безопасности модульных тестов.
Также устраняет необходимость ручного кодирования анонимных переменных в рамках этапа настройки юнит тестов. 
Среди других ф-ций он предлагает универсальную реализацию шаблона Test Data Buider.

Пример:

```C#
var fixture = new Fixture();
var randomDate = fixture.Create<DateTime>(); // генерация рандомной даты
var randomString = fixture.Create<string>(); // генерация рандомной строки
var randomDouble =   fixture.Create<double>(); // генерация рандомного числа
var randomPerson = fixture.Create<Person>(); // генерация рандомного экземпляра типа Person

var randomPerson = fixture.CreateMany<Person>(); // генерация 3 рандомных экземпляров типа Person
var random10Person = fixture.CreateMany<Person>(10); // генерация 10 рандомных экземпляров типа Person
```