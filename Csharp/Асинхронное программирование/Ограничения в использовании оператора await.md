Оператор await нельзя использовать в следующих ситуациях:

- В теле синхронного метода, лямбда-выражения, анонимного метода (не помеченных модификатором **async**)
- В блоке оператора **lock** (представляет использование класса Monitor и его методов Enter и Exit в блоках try и finally соответственно. Одно из правил с синхронизатором Monitor - блокировку должен снять и вернуть один и тот же поток, но при работе с оператором await вторая часть, находящаяся после оператора может быть выполнена в другом потоке. Соответственно блокировка будет возвращена другим потоком, А это приведет к ошибкам, поэтому компилятор такое отслеживает и запрещает)
- В выражении большинства запроса LINQ (но можно создать свои расширяющие методы для работы с async await)
- В небезопасном (unsafe) контексте (Т.к. оператор await напару с async раскладывается на конечный автомат, что нарушит работу в контексте unsafe кода)
- Нельзя создавать экземпляры ref struct типов в асинхронных методах (Из-за особенности ref структуры - она не может попасть на кучу)
- В блоке catch (начиная с версии C# 6.0 **РАЗРЕШЕНО ИСПОЛЬЗОВАТЬ**)
- В блоке finally (начиная с версии C# 6.0 **РАЗРЕШЕНО ИСПОЛЬЗОВАТЬ**)
- В блоке Main (начиная с версии C# 7.1 **РАЗРЕШЕНО ИСПОЛЬЗОВАТЬ**)