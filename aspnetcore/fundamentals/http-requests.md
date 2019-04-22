---
title: Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core
author: stevejgordon
description: Сведения об использовании интерфейса IHttpClientFactory для управления логическими экземплярами HttpClient в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 270727443f091306ac3e4ce4e2ceb99b88bbc609
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "58809212"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a>Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core

Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak) и [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon)

<xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении. Так вы получите следующие преимущества:

* Центральное расположение для именования и настройки логических экземпляров `HttpClient`. Например, можно зарегистрировать и использовать клиент *github* для доступа к GitHub. Можно зарегистрировать клиент по умолчанию для других целей.
* Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient` и предоставление расширений для ПО промежуточного слоя на основе Polly для использования этой возможности.
* Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.
* Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

Для проектов, предназначенных для .NET Framework, необходимо установить пакет NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/). Пакет `Microsoft.Extensions.Http` уже включен в проекты, предназначенные для .NET Core и ссылающиеся на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

## <a name="consumption-patterns"></a>Принципы использования

Существует несколько способов использования `IHttpClientFactory` в приложении:

* [Основное использование](#basic-usage)
* [Именованные клиенты](#named-clients)
* [Типизированные клиенты](#typed-clients)
* [Созданные клиенты](#generated-clients)

Все способы равноценны. Оптимальный подход зависит от ограничений приложения.

### <a name="basic-usage"></a>Основное использование

`IHttpClientFactory` можно зарегистрировать путем вызова метода расширения `AddHttpClient` в `IServiceCollection` внутри метода `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

После регистрации код может принимать `IHttpClientFactory` в любом месте, куда можно внедрить службу с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI). `IHttpClientFactory` можно использовать для создания экземпляра `HttpClient`:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

Подобное использование `IHttpClientFactory` — это отличный способ рефакторинга имеющегося приложения. Он не оказывает влияния на использование `HttpClient`. Там, где в данный момент создаются экземпляры `HttpClient`, используйте вызов к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.

### <a name="named-clients"></a>Именованные клиенты

Если для приложения предполагаются разные способы использования `HttpClient`, каждый со своей конфигурацией, можно применять **именованные клиенты**. Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

В приведенном выше коде вызывается клиент `AddHttpClient`, предоставляющий имя *github*. У клиента есть некоторые настройки по умолчанию &mdash; а именно: базовый адрес и два заголовка, необходимые для работы с API GitHub.

При каждом вызове `CreateClient` создается новый экземпляр `HttpClient` и вызывается действие конфигурации.

Для использования именованного клиента можно передать строковый параметр в `CreateClient`. Укажите имя создаваемого клиента:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

В приведенном выше коде в запросе не требуется указывать имя узла. Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.

### <a name="typed-clients"></a>Типизированные клиенты

Типизированные клиенты предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей. Метод типизированных клиентов помогает IntelliSense и компилятору при использовании клиентов. Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`. Например, для конечной точки серверной части можно использовать один типизированный клиент, который будет содержать всю логику работы с этой конечной точкой. Еще одно преимущество — работа с внедрением зависимостей и возможность вставки в нужное место в приложении.

Типизированный клиент принимает параметр `HttpClient` в конструкторе:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

В приведенном выше коде конфигурация перемещается в типизированный клиент. Объект `HttpClient` предоставляется в виде открытого свойства. Можно определить связанные с API методы, которые предоставляют функциональные возможности `HttpClient`. Метод `GetAspNetDocsIssues` инкапсулирует код, необходимый для запроса и анализа последнего открытого выпуска из репозитория GitHub.

Для регистрации типизированного клиента можно использовать универсальный метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices`, указав класс типизированного клиента:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

Типизированный клиент регистрируется во внедрении зависимостей как временный. Типизированный клиент можно внедрить и использовать напрямую:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

При желании конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

Можно полностью инкапсулировать `HttpClient` внутри типизированного клиента. Вместо предоставления его как свойства можно использовать открытые методы для внутреннего вызова экземпляра `HttpClient`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

В приведенном выше коде `HttpClient` хранится как закрытое поле. Любой доступ для совершения внешних вызовов осуществляется через метод `GetRepos`.

### <a name="generated-clients"></a>Созданные клиенты

`IHttpClientFactory` можно использовать в сочетании с другими библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit). Refit — это библиотека REST для .NET. Она преобразует REST API в динамические интерфейсы. Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.

Для представления внешнего API и его ответа определяются интерфейс и ответ:

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

Можно добавить типизированный клиент, используя Refit для создания реализации:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a>ПО промежуточного слоя для исходящих запросов

В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов. Класс `IHttpClientFactory` упрощает определение обработчиков для применения к каждому именованному клиенту. Он поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов. Каждый из этих обработчиков может выполнять работу до и после исходящего запроса. Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core. Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.

Чтобы создать обработчик, необходимо определить класс, производный от <xref:System.Net.Http.DelegatingHandler>. Переопределите метод `SendAsync` для выполнения кода до передачи запросов следующему обработчику в конвейере:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

В предыдущем коде определяется базовый обработчик. Он проверяет, включен ли в запрос заголовок `X-API-KEY`. Если заголовок отсутствует, он может избежать вызовов HTTP и вернуть подходящий ответ.

Во время регистрации можно добавить один или несколько обработчиков в конфигурацию для `HttpClient`. Эта задача выполняется через методы расширения в <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

::: moniker range=">= aspnetcore-2.2"

В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей. `IHttpClientFactory` создает отдельную область внедрения зависимостей для каждого обработчика. Обработчики могут зависеть от служб из любой области. Службы, которые зависят от обработчиков, удаляются при удалении обработчика.

После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей. Обработчик **должен** регистрироваться во внедрении зависимостей как временная служба, а не ограниченная. Если обработчик регистрируется как ограниченная служба и все службы, от которых зависит обработчик, можно удалить, службы обработчика можно удалить до того, как обработчик выйдет за пределы области, что приведет к его сбою.

После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.

::: moniker-end

Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться. Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:

* Передайте данные в обработчик с помощью `HttpRequestMessage.Properties`.
* Используйте `IHttpContextAccessor` для доступа к текущему запросу.
* Создайте пользовательский объект хранилища `AsyncLocal` для передачи данных.

## <a name="use-polly-based-handlers"></a>Использование обработчиков на основе Polly

`IHttpClientFactory` интегрируется с популярной библиотекой сторонних разработчиков под названием [Polly](https://github.com/App-vNext/Polly). Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET. Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.

Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения. Расширения Polly доступны в пакете NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/). Этот пакет не входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Для использования расширений в проект необходимо включить явную ссылку `<PackageReference />`.

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/HttpClientFactorySample.csproj?highlight=10)]

После восстановления этого пакета будут доступны методы расширения для поддержки добавления обработчиков на основе Polly в клиенты.

### <a name="handle-transient-faults"></a>Обработка временных сбоев

Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными. Используется удобный метод расширения `AddTransientHttpErrorPolicy`, который позволяет определить политику для обработки временных ошибок. Политики, заданные с помощью этого метода расширения, обрабатывают `HttpRequestException`, ответы HTTP 5xx и ответы HTTP 408.

Расширение `AddTransientHttpErrorPolicy` может быть использовано в `Startup.ConfigureServices`. Данное расширение предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

В приведенном выше коде определена политика `WaitAndRetryAsync`. Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.

### <a name="dynamically-select-policies"></a>Динамический выбор политик

Существуют дополнительные методы расширения, которые можно использовать для добавления обработчиков на основе Polly. Одним из таких расширений является `AddPolicyHandler` с несколькими перегрузками. Одна перегрузка разрешает проверку запроса для определения необходимой политики:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

В приведенном выше коде для запроса GET время ожидания составляет 10 секунд. Для остальных методов HTTP время ожидания — 30 секунд.

### <a name="add-multiple-polly-handlers"></a>Добавление нескольких обработчиков Polly

Вложение политик Polly для расширения функциональных возможностей — это распространенная практика:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

В приведенном выше примере добавляются два обработчика. Первый использует расширение `AddTransientHttpErrorPolicy`, чтобы добавить политику повтора. Неудачные запросы выполняются повторно до трех раз. Второй вызов к `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи. Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд. Политики размыкателя цепи отслеживают состояние. Все вызовы через этот клиент имеют одинаковое состояние цепи.

### <a name="add-policies-from-the-polly-registry"></a>Добавление политик из реестра Polly

Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`. Предоставляется метод расширения, разрешающий добавление обработчика с помощью политики из реестра:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

В приведенном выше коде, когда `PolicyRegistry` добавляется в `ServiceCollection`, регистрируются две политики. Чтобы использовать политику из реестра, применяется метод `AddPolicyHandlerFromRegistry`, который передает имя необходимой политики.

Дополнительные сведения об интеграции `IHttpClientFactory` и Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).

## <a name="httpclient-and-lifetime-management"></a>Управление HttpClient и временем существования

При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`. Для каждого названного клиента существует <xref:System.Net.Http.HttpMessageHandler>. Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.

`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов. Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.

Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями. Создание лишних обработчиков может привести к задержке подключения. Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения DNS.

Время существования обработчика по умолчанию — две минуты. Значение по умолчанию можно переопределить для каждого именованного клиента. Чтобы переопределить это значение, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> в `IHttpClientBuilder`, который возвращается при создании клиента:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

Высвобождать клиент не требуется. Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>. `IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`. Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, не требующие высвобождения.

До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени. После перехода на `IHttpClientFactory` это уже не нужно.

## <a name="logging"></a>Ведение журнала

Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов. Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию. Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.

Категория журнала для каждого клиента включает в себя имя клиента. Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`. Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов. Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере. Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.

Кроме того, журнал ведется в конвейере обработчиков запросов. В примере *MyNamedClient* эти сообщения вносятся в журнал по категории журнала `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`. Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса по сети. Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.

Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера. Сюда входят, например, изменения заголовков запросов или кода состояния ответов.

Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам при необходимости.

## <a name="configure-the-httpmessagehandler"></a>Настройка HttpMessageHandler

Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.

При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`. Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>. Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:

[!code-csharp[Main](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Использование HttpClientFactory для реализации устойчивых HTTP-запросов](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [Реализация шаблона размыкателя цепи](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)