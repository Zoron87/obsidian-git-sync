#### Метод Migrate

В некоторых случаях, например, в приложениях с локальной базой данных (SQLite в UWP), мы можем выполнять миграции в процессе выполнения приложения. Для этого определен метод Database.Migrate(), который можно вызвать через объект контекста:


```C#
	myDbContext.Database.Migrate();
```

Стоит учитывать, что перед вызовом этого метода не следует вызывать метод EnsureCreated, который обходит миграции при создании базы данных, что вызывает ошибку при выполнении метода Migrate.

Чтобы задействовать этот метод, необходимо подключить пространство имен 
```C#
Microsoft.EntityFrameworkCore`
```

#### Миграция, если конструктор контекста принимает параметр DbContextOptions

Выше была рассмотрена миграция для контекста данных, который имеет конструктор без параметров и устанавливает настройки подключения в методе `OnConfiguring()`. Однако мы можем также передавать параметры подключения в контекст данных извне через конструктор с параметром типа DbContextOptions:

```C#
public class ApplicationContext : DbContext
{
    public DbSet<User> Users { get; set; }
    public ApplicationContext(DbContextOptions<ApplicationContext> options) : base(options)
    {
    }
}
```

Например, у нас в проекте есть файл конфигурации appsettings.json:

```C#
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\mssqllocaldb;Database=helloappdb32;Trusted_Connection=True;"
  }
}
```
Из которого в процессе выполнения приложения мы извлекаем строку подключения и передаем в контекст данных:

```C#
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using System;
using System.IO;
using System.Linq;
 
namespace HelloApp
{
    class Program
    {
        static void Main(string[] args)
        {
 
            var builder = new ConfigurationBuilder();
            // установка пути к текущему каталогу
            builder.SetBasePath(Directory.GetCurrentDirectory());
            // получаем конфигурацию из файла appsettings.json
            builder.AddJsonFile("appsettings.json");
            // создаем конфигурацию
            var config = builder.Build();
            // получаем строку подключения
            string connectionString = config.GetConnectionString("DefaultConnection");
 
            var optionsBuilder = new DbContextOptionsBuilder<ApplicationContext>();
            var options = optionsBuilder
                .UseSqlServer(connectionString)
                .Options;
 
            using (ApplicationContext db = new ApplicationContext(options))
            {
                var users = db.Users.ToList();
                foreach (User u in users)
                {
                    Console.WriteLine(u.Name);
                }
            }
        }
    }
}
```

Как получать конфигурацию подключения из файла, описывалось в статье [Конфигурация подключения](https://metanit.com/sharp/entityframeworkcore/2.2.php)

При выполнении миграции для такого контекста данных мы получим ошибку:

```C#
PM> Add-Migration InitialCreate
Build started...
Build succeeded.
Unable to create an object of type 'ApplicationContext'. For the different patterns supported at design time, see https://go.microsoft.com/fwlink/?linkid=851728
```

Дело в том, что, если единственный конструктор класса контекста принимает параметр DbContext:

```C#
public ApplicationContext(DbContextOptions<ApplicationContext> options) : base(options){ }
```

В этом случае при выполнении миграции инструментарий Entity Frameworkа ищет класс, который реализует интерфейс IDesignTimeDbContextFactory и который задает конфигурацию контекста.

Поэтому в этом случае нам необходимо добавить в проект подобный класс. Например:

```C#
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
using Microsoft.Extensions.Configuration;
using System;
using System.IO;
 
namespace HelloApp
{
    public class SampleContextFactory : IDesignTimeDbContextFactory<ApplicationContext>
    {
        public ApplicationContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<ApplicationContext>();
             
            // получаем конфигурацию из файла appsettings.json
            ConfigurationBuilder builder = new ConfigurationBuilder();
            builder.SetBasePath(Directory.GetCurrentDirectory());
            builder.AddJsonFile("appsettings.json");
            IConfigurationRoot config = builder.Build();
 
            // получаем строку подключения из файла appsettings.json
            string connectionString = config.GetConnectionString("DefaultConnection");
            optionsBuilder.UseSqlServer(connectionString, opts => opts.CommandTimeout((int)TimeSpan.FromMinutes(10).TotalSeconds));
            return new ApplicationContext(optionsBuilder.Options);
        }
    }
}
```

Класс SampleContextFactory применяет интерфейс IDesignTimeDbContextFactory, который типизируется типом контекста данных - в данном случае класс ApplicationContext. Данный интерфейс содержит один метод CreateDbContext(), который должен возвращать созданный объект контекста данных.

В данном случае также получаем конфигурацию из файла appsettings.json и извлекаем из ее строку подключения и таким образом создаем контекст.

Хотя этот класс формально нигде не вызывается и никак не используется, фактически он вызывается инфраструктурой Entity Framework при создании миграции.

Исходная статья:
https://metanit.com/sharp/entityframeworkcore/2.15.php?ysclid=lp6ukb7wnv67112164


#ASP-NET #Migrations #МиграцииБД
