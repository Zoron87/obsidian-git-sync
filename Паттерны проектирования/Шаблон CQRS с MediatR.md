
Сегодня я хочу показать вам, как использовать шаблон **CQRS** для создания быстрых и масштабируемых приложений.

Шаблон CQRS разделяет операции записи и чтения в приложении.

Такое разделение может быть логическим или физическим и имеет множество преимуществ:

- Управление сложностью
- Улучшенная производительность
- Масштабируемость
- Гибкость
- Безопасность

Я также собираюсь показать вам, как реализовать CQRS в вашем приложении с помощью MediatR.

Но сначала нам нужно понять, что такое CQRS.

## [Что такое CQRS?](https://www.milanjovanovic.tech/blog/cqrs-pattern-with-mediatr#what-exactly-is-cqrs)

[CQRS](https://learn.microsoft.com/en-us/azure/architecture/patterns/cqrs) означает **разделение ответственности за запросы команд** . Шаблон CQRS использует отдельные модели для чтения и обновления данных. Преимущества использования CQRS — управление сложностью, повышение производительности, масштабируемости и безопасности.

Стандартный подход к работе с базой данных заключается в использовании одной и той же модели для запроса и обновления данных. Это просто и отлично работает для большинства операций CRUD. Однако в более сложных приложениях его становится сложно поддерживать. Что касается записи, у вас может быть сложная бизнес-логика и проверка в модели. На стороне чтения вам может потребоваться выполнить множество различных запросов.

Также рассмотрим, как мы создаем модель данных. Применение лучших практик моделирования данных SQL даст вам нормализованную базу данных. В целом это нормально, но оно оптимизировано для записи.

Наличие отдельных моделей для команд и запросов позволяет масштабировать их независимо. Разделение может быть логичным при использовании одной и той же базы данных. Вы можете разделить подсистемы для команд и запросов на отдельные службы. И вы даже можете иметь несколько баз данных, оптимизированных для записи или чтения данных.

## [Чем он отличается от CQS?](https://www.milanjovanovic.tech/blog/cqrs-pattern-with-mediatr#how-is-it-different-from-cqs)

[CQS](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation) означает **разделение командных запросов** . Этот термин придумал Бертран Мейер в своей книге « [Объектно-ориентированное построение программного обеспечения».](https://en.wikipedia.org/wiki/Object-Oriented_Software_Construction)

Основная предпосылка CQS — разделение методов объекта на **команды** и **запросы** .

- **Команды** : изменить состояние системы, но не возвращать значение.
- **Запросы** : возвращают значение и не меняют состояние системы (без побочных эффектов).

Это не означает, что команда никогда не сможет вернуть значение. Типичный пример — извлечение значения из стека. Он возвращает значение и изменяет состояние системы. Но здесь важно намерение.

CQS — это _принцип._ Вы можете следовать этому принципу, если это имеет смысл, но будьте прагматичны.

CQRS — это эволюция CQS. CQRS работает на архитектурном уровне. В то же время CQS работает на уровне метода (или класса).

## [Множество разновидностей CQRS](https://www.milanjovanovic.tech/blog/cqrs-pattern-with-mediatr#many-flavors-of-cqrs)

Вот общий обзор системы CQRS, использующей несколько баз данных. Команды обновляют базу данных записи. Затем вам необходимо синхронизировать обновления с прочитанной базой данных. Это обеспечивает конечную согласованность систем CQRS.

Окончательная согласованность значительно увеличивает сложность вашего приложения. Вы должны учитывать, что произойдет в случае сбоя процесса синхронизации, и иметь стратегию отказоустойчивости.
![Схема системы, использующей CQRS, с двумя базами данных.](https://www.milanjovanovic.tech/blogs/mnw_060/cqrs.png?imwidth=3840)

Существует множество разновидностей этого подхода:

- База данных SQL на стороне записи и база данных NoSQL (например, [RavenDB](https://ravendb.net/) ) на стороне чтения.
- Источник событий на стороне записи и база данных NoSQL на стороне чтения.
- Использование Redis или другого распределенного кеша на стороне чтения.

Разделение моделей обновления и чтения данных позволяет выбрать лучшую базу данных, соответствующую вашим требованиям.

## [Логическая архитектура CQRS](https://www.milanjovanovic.tech/blog/cqrs-pattern-with-mediatr#logical-cqrs-architecture)

Как применить шаблон CQRS к своей системе? Я предпочитаю использовать [MediatR.](https://github.com/jbogard/MediatR)

MediatR реализует [шаблон посредника](https://refactoring.guru/design-patterns/mediator) для решения простой проблемы — отделения внутрипроцессной отправки сообщений от обработки сообщений.

Вы можете расширить интерфейс MediatR `IRequest`с помощью пользовательской `ICommand`абстракции `IQuery`. Это позволяет вам явно определять команды и запросы в вашей системе.

Что касается записи, я обычно использую [EF Core](https://learn.microsoft.com/en-us/ef/core/) и богатую модель предметной области для инкапсуляции бизнес-логики. Поток команд использует EF для загрузки объекта в память, выполнения логики предметной области и сохранения изменений в базе данных.

Что касается чтения, я хочу как можно меньше косвенности. Использование [Dapper](https://github.com/DapperLib/Dapper) с необработанными SQL-запросами — отличный выбор. Вы также можете создавать представления в базе данных и запрашивать их. Альтернативно вы можете использовать EF Core для выполнения запросов с прогнозами.
![Схема приложения, использующего CQRS на архитектурном уровне.](https://www.milanjovanovic.tech/blogs/mnw_060/cqrs_application.png?imwidth=3840)

## [Реализация CQRS с помощью MediatR](https://www.milanjovanovic.tech/blog/cqrs-pattern-with-mediatr#implementing-cqrs-with-mediatr)

Реализация CQRS с помощью MediatR состоит из двух компонентов:

- Определение класса команды или запроса
- Реализация соответствующей команды или обработчика запроса

Я снял подробное видео, объясняющее этот процесс, и вы можете [посмотреть его здесь.](https://youtu.be/vdi-p9StmG0)

Вы используете `ISender`интерфейс для `Send`команды или запроса. MediatR заботится о маршрутизации команды или запроса соответствующему обработчику.

Запрос пройдет через _конвейер запросов_ . Это оболочка каждого запроса, и вы можете использовать ее для решения сквозных проблем с помощью `IPipelineBehavior`. Например, вы можете реализовать [проверку команд с помощью FluentValidation.](https://www.milanjovanovic.tech/blog/cqrs-validation-with-mediatr-pipeline-and-fluentvalidation)

```csharp
[ApiController]
[Route("api/bookings")]
public class BookingsController : ControllerBase
{
    private readonly ISender _sender;

    public BookingsController(ISender sender)
    {
        _sender = sender;
    }

    [HttpPut("{id}/confirm")]
    public async Task<IActionResult> ConfirmBooking(
        Guid id,
        CancellationToken cancellationToken)
    {
        var command = new ConfirmBookingCommand(id);

        var result = await _sender.Send(command, cancellationToken);

        if (result.IsFailure)
        {
            return BadRequest(result.Error);
        }

        return NoContent();
    }
}

```

Вот пример обработчика команд с репозиториями и богатой моделью предметной области:

```csharp
internal sealed class ConfirmBookingCommandHandler
    : ICommandHandler<ConfirmBookingCommand>
{
    private readonly IDateTimeProvider _dateTimeProvider;
    private readonly IBookingRepository _bookingRepository;
    private readonly IUnitOfWork _unitOfWork;

    public ConfirmBookingCommandHandler(
        IDateTimeProvider dateTimeProvider,
        IBookingRepository bookingRepository,
        IUnitOfWork unitOfWork)
    {
        _dateTimeProvider = dateTimeProvider;
        _bookingRepository = bookingRepository;
        _unitOfWork = unitOfWork;
    }

    public async Task<Result> Handle(
        ConfirmBookingCommand request,
        CancellationToken cancellationToken)
    {
        var booking = await _bookingRepository.GetByIdAsync(
            request.BookingId,
            cancellationToken);

        if (booking is null)
        {
            return Result.Failure(BookingErrors.NotFound);
        }

        var result = booking.Confirm(_dateTimeProvider.UtcNow);

        if (result.IsFailure)
        {
            return result;
        }

        await _unitOfWork.SaveChangesAsync(cancellationToken);

        return Result.Success();
    }
}
```

Вот пример обработчика запросов, использующего Dapper и необработанный SQL:

```csharp
internal sealed class SearchApartmentsQueryHandler
    : IQueryHandler<SearchApartmentsQuery, IReadOnlyList<ApartmentResponse>>
{
    private static readonly int[] ActiveBookingStatuses =
    {
        (int)BookingStatus.Reserved,
        (int)BookingStatus.Confirmed,
        (int)BookingStatus.Completed
    };

    private readonly ISqlConnectionFactory _sqlConnectionFactory;

    public SearchApartmentsQueryHandler(
        ISqlConnectionFactory sqlConnectionFactory)
    {
        _sqlConnectionFactory = sqlConnectionFactory;
    }

    public async Task<Result<IReadOnlyList<ApartmentResponse>>> Handle(
        SearchApartmentsQuery request,
        CancellationToken cancellationToken)
    {
        if (request.StartDate > request.EndDate)
        {
            return new List<ApartmentResponse>();
        }

        using var connection = _sqlConnectionFactory.CreateConnection();

        const string sql = """
            SELECT
                a.id AS Id,
                a.name AS Name,
                a.description AS Description,
                a.price_amount AS Price,
                a.price_currency AS Currency,
                a.address_country AS Country,
                a.address_state AS State,
                a.address_zip_code AS ZipCode,
                a.address_city AS City,
                a.address_street AS Street
            FROM apartments AS a
            WHERE NOT EXISTS
            (
                SELECT 1
                FROM bookings AS b
                WHERE
                    b.apartment_id = a.id AND
                    b.duration_start <= @EndDate AND
                    b.duration_end >= @StartDate AND
                    b.status = ANY(@ActiveBookingStatuses)
            )
            """;

        var apartments = await connection
            .QueryAsync<ApartmentResponse, AddressResponse, ApartmentResponse>(
                sql,
                (apartment, address) =>
                {
                    apartment.Address = address;

                    return apartment;
                },
                new
                {
                    request.StartDate,
                    request.EndDate,
                    ActiveBookingStatuses
                },
                splitOn: "Country");

        return apartments.ToList();
    }
}
```

## [Заключительные мысли](https://www.milanjovanovic.tech/blog/cqrs-pattern-with-mediatr#closing-thoughts)

Разделение команд и запросов может улучшить производительность и масштабируемость в долгосрочной перспективе. Вы можете оптимизировать команды и запросы по-разному в зависимости от ваших требований.

Команды инкапсулируют сложную бизнес-логику и проверку. Использование EF Core и богатой модели предметной области — отличное решение.

Запросы зависят от производительности, поэтому вам нужно использовать самое быстрое. Это могут быть необработанные SQL-запросы с помощью Dapper, проекций EF Core или Redis.

Если вам нужна система, которую я использую для создания масштабируемых приложений с помощью CQRS и MediatR, ознакомьтесь с [**Pragmatic Clean Architecture.**](https://www.milanjovanovic.tech/pragmatic-clean-architecture)