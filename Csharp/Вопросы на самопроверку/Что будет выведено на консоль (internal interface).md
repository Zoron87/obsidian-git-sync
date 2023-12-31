![[Pasted image 20231213152131.png]]

При реализации метода интерфейса в классе данный метод может иметь только модификатор public. Определение реализации с модификатором internal некорректно.

**Комментарий с форумов:**

**1) Члены интерфейса видны только коду вне интерфейса на основании правил соответствующего уровня видимости.**

- `public`: Члены интерфейса в C# по умолчанию являются общедоступными, поэтому это работает.
- `internal`: Если бы отдельные члены интерфейса могли быть объявлены как `internal`, это означало бы, что часть интерфейса может быть реализована только классами из сборки, в которой находится интерфейс. Аналогично ситуация уже существует в тех случаях, когда члены абстрактного класса или конструкторы абстрактных классов объявлены как `internal` — это эффективно предотвращает создание подклассов за пределами сборки базового класса.  
    Что касается сценариев, в которых это можно использовать, подумайте об API, которые возвращают объекты, которые позже снова используются указанным API. Если по какой-то причине API может использовать только объекты, экземпляры которых он создал сам, имея возможность объявить базовый класс таким образом, чтобы сторонний код не мог наследовать свои пользовательские классы, а использовал только то, что он получает от API, не будучи при этом возможность сделать то же самое с интерфейсами кажется странно асимметричной.
- `protected`: Защищенные члены видны только внутри своего класса и любого из его подклассов. Если применить те же предположения, что и выше, это будет означать, что только код внутри самого объявления интерфейса (ну, интерфейс может почти не содержать кода, где это могло бы иметь значение1), производных интерфейсов (так же, как и раньше) и, возможно, классов, реализующих интерфейс, могли видеть эти защищенные члены.  
    В последнем случае все может стать интересным, хотя бы в довольно надуманных ситуациях: метод внутри этого класса может захотеть использовать все, что реализует интерфейс, будь то текущий экземпляр самого класса или любой другой вложенный в него тип, в то время как получение полуэксклюзивного доступа к защищенным методам.
- `private`: все, что имеет видимость `private`, видно только самому объявляющему интерфейсу. Поскольку на члены интерфейса вряд ли можно1 ссылаться изнутри самого интерфейса, это, вероятно, будет не очень полезно. Тем не менее, все же могут быть крайние случаи, например, определения `const` внутри интерфейса, на которые ссылаются атрибуты в этом интерфейсе, но которые не должны быть доступны для внешнего кода – однако [C# не допускает использования `const` значений в интерфейсах](http://social.msdn.microsoft.com/Forums/en-US/71e8d202-b249-49bb-a85d-b0ef8207463d/static-const-members-in-interface?forum=csharplanguage), хотя [подобная функция не является беспрецедентной в CLI мир](https://stackoverflow.com/questions/12752364/how-to-associate-constants-with-an-interface-in-c).

**2) Члены интерфейса должны быть реализованы (по крайней мере?) с видимостью, указанной в интерфейсе, и придерживаться этой видимости при доступе к члену через ссылку, введенную в класс, но обычно они публично видимы при доступе к члену через ссылку, введенную в класс. интерфейс.**

Опять же, не будет никаких изменений в случае членов `public`, в то время как `internal`, `protected` и `private` позволит использовать интерфейсы, допускающие, по-видимому, частичную реализацию. Подобный эффект в настоящее время может быть достигнут путем явной реализации интерфейса, поэтому в некотором смысле это было бы синтаксическим сахаром, позволяющим избежать написания одного и того же метода дважды: один раз с предполагаемой видимостью и один раз с явным объявлением реализации, специфичным для интерфейса (даже если последний часто только перенаправляет вызовы).

Подробнее - https://stackoverflow.com/questions/25619009/why-interface-have-only-public-member-and-methods
------------------------------


интерфейс - это считай контракт.  
публикуя его (даже самому себе) программист говорит, какие внешние методы/св-ва имеет объект.  
Это нужно для планирования взаимодействия ещё не существующих будущих частей.  
  
Протектеды приваты и прочее совершенно не интересуют на этом этапе никого, так как это будет касаться только классов реализующих конкретный интерфейс.  
Главное что во вне вытащены необходимые заранее спланированные ниточки, которые гарантировано присутсвуют в реализующих интерфейс классах.  
  
Как только вы думаете про протектеды в интерфесе, вы начинаете спускаться вниз по абстракциям к реализации. Если вам так нужна кодовая дисциплина, можете сделать базовый класс с пустыми запланированными протектед методами, а-ля интерфейс девелопер тулкит, но только после того как спланировали работающий интерфейс и без протектедов. И после этого просить использовать только его в разработке как базовый, но это противоречит всей парадигме, где жёстко прописывается взаимодействие, и никак не ограничивается реализация и область применения

#вопросы_Csharp #задачи_Csharp #interface
