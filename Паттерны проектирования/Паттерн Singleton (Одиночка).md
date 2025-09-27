
- Проблема: иногда нужен **только один объект** во всей программе (например, кассовый аппарат).
- Решение: создаём класс, у которого может быть **только один экземпляр**.
- Singleton = “Один объект для всех”.

---
## 2) Простое объяснение

Представь магазин игрушек. Есть только **одна касса**. Все покупатели должны платить именно там.  
Если кто-то захочет поставить вторую кассу — нельзя, потому что так решило руководство.  
Значит, всегда есть одна касса, и все её используют.

👉 Вопрос: Если в магазине только одна касса, сколько человек могут одновременно ей пользоваться?  
(Ответ: Все по очереди, но касса одна!)

---
## 2) Подробное объяснение

- **Когда использовать:**
    - Когда нужен один объект для всей программы (логгер, кэш, настройки).
    - Когда нельзя допустить создание копий.
        
- **Участники и роли:**
    - `Singleton`: класс, который контролирует количество экземпляров.
    - `Instance`: статическая ссылка на этот единственный объект.
        
- **Как работает:**
    1. Прячем конструктор (делаем `private`).
    2. Создаём статическое свойство `Instance`.
    3. Первый вызов — создаёт объект, дальше — возвращает тот же.
        
- **Связанность:**
    - Часто путают с `Static class`. Но отличие — Singleton можно передавать в интерфейсы и тестировать.
        
- **Подводные камни:**
    - ❌ Нарушение SRP (одна касса делает слишком много).
    - ❌ Не потокобезопасно — нужна синхронизация.
    - ❌ Сложно тестировать, если “вшить” внутрь кода.
        
- **В .NET:**
    - Часто встречается в `HttpClientFactory`, `Configuration`, `ILogger`.

```C#
using System;

namespace ToyStore
{
    // Простейший Singleton
    public class CashRegister
    {
        private static CashRegister instance; // хранит ссылку
        private CashRegister() { } // закрытый конструктор

        public static CashRegister Instance
        {
            get
            {
                if (instance == null)
                    instance = new CashRegister();
                return instance;
            }
        }

        public void Pay(string customer)
        {
            Console.WriteLine($"{customer} оплатил покупку в кассе");
        }
    }

    class Program
    {
        static void Main()
        {
            var c1 = CashRegister.Instance;
            var c2 = CashRegister.Instance;

            Console.WriteLine(c1 == c2); // True
            c1.Pay("Алиса");
            c2.Pay("Боб");
        }
    }
}

/*
Ожидаемый вывод:
True
Алиса оплатил покупку в кассе
Боб оплатил покупку в кассе
*/
```

```C#
using System;

namespace ToyStore
{
    public class CashRegister
    {
        private static readonly Lazy<CashRegister> instance =
            new(() => new CashRegister());

        private CashRegister() { }

        public static CashRegister Instance => instance.Value;

        public void Pay(string customer)
        {
            Console.WriteLine($"{customer} оплатил покупку в кассе");
        }
    }

    class Program
    {
        static void Main()
        {
            var c1 = CashRegister.Instance;
            var c2 = CashRegister.Instance;

            Console.WriteLine(object.ReferenceEquals(c1, c2)); // True
        }
    }
}

```



#Паттерны_Проектирования 