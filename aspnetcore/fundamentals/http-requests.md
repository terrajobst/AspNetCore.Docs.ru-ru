---
title: Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core
author: stevejgordon
description: Сведения об использовании интерфейса IHttpClientFactory для управления логическими экземплярами HttpClient в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
uid: fundamentals/http-requests
ms.openlocfilehash: 482f8e28c23c621cecaf9ce111d89e9166ea6d85
ms.sourcegitcommit: da2fb2d78ce70accdba903ccbfdcfffdd0112123
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/07/2020
ms.locfileid: "75722730"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="1efed-103">Выполнения HTTP-запросов с помощью IHttpClientFactory в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1efed-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1efed-104">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak), [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon), [Рик Андерсон (Rick Anderson)](https://twitter.com/RickAndMSFT) и [Кирк Ларкин (Kirk Larkin)](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="1efed-104">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak),  [Steve Gordon](https://github.com/stevejgordon), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="1efed-105"><xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении.</span><span class="sxs-lookup"><span data-stu-id="1efed-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="1efed-106">`IHttpClientFactory` предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="1efed-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="1efed-107">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="1efed-108">Например, можно зарегистрировать и настроить клиент *github* для доступа к [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="1efed-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="1efed-109">Можно зарегистрировать клиент по умолчанию для общего доступа.</span><span class="sxs-lookup"><span data-stu-id="1efed-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="1efed-110">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="1efed-111">Предоставление расширений для ПО промежуточного слоя на основе Polly для делегирования обработчиков в `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="1efed-112">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="1efed-113">Автоматическое управление позволяет избежать обычных проблем со службой доменных имен (DNS), которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="1efed-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="1efed-114">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="1efed-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="1efed-115">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1efed-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="1efed-116">Пример кода в этой версии раздела использует <xref:System.Text.Json> для десериализации содержимого JSON, возвращаемого в ответах HTTP.</span><span class="sxs-lookup"><span data-stu-id="1efed-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="1efed-117">Для примеров, использующих `Json.NET` и `ReadAsAsync<T>`, воспользуйтесь средством выбора версии, чтобы выбрать версию 2.x этого раздела.</span><span class="sxs-lookup"><span data-stu-id="1efed-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="1efed-118">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="1efed-118">Consumption patterns</span></span>

<span data-ttu-id="1efed-119">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="1efed-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="1efed-120">Основное использование</span><span class="sxs-lookup"><span data-stu-id="1efed-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="1efed-121">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="1efed-122">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="1efed-123">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="1efed-124">Оптимальный подход зависит от требований приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="1efed-125">Основное использование</span><span class="sxs-lookup"><span data-stu-id="1efed-125">Basic usage</span></span>

<span data-ttu-id="1efed-126">`IHttpClientFactory` можно зарегистрировать, вызвав `AddHttpClient`:</span><span class="sxs-lookup"><span data-stu-id="1efed-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="1efed-127">`IHttpClientFactory` можно запросить с помощью [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="1efed-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1efed-128">Следующий код использует `IHttpClientFactory` для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="1efed-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="1efed-129">Подобное использование `IHttpClientFactory` — это хороший способ рефакторинга имеющегося приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="1efed-130">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="1efed-131">Там, где в существующем приложении создаются экземпляры `HttpClient`, используйте вызовы к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="1efed-132">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-132">Named clients</span></span>

<span data-ttu-id="1efed-133">Именованные клиенты являются хорошим выбором в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="1efed-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="1efed-134">Приложение требует много отдельных использований `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="1efed-135">Многие `HttpClient` имеют другую конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="1efed-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="1efed-136">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1efed-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="1efed-137">В приведенном выше коде клиент регистрируется с:</span><span class="sxs-lookup"><span data-stu-id="1efed-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="1efed-138">базовым адресом `https://api.github.com/`;</span><span class="sxs-lookup"><span data-stu-id="1efed-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="1efed-139">двумя заголовками, необходимыми для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="1efed-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="1efed-140">CreateClient</span><span class="sxs-lookup"><span data-stu-id="1efed-140">CreateClient</span></span>

<span data-ttu-id="1efed-141">При каждом вызове <xref:System.Net.Http.IHttpClientFactory.CreateClient*>:</span><span class="sxs-lookup"><span data-stu-id="1efed-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="1efed-142">создается новый экземпляр `HttpClient`;</span><span class="sxs-lookup"><span data-stu-id="1efed-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="1efed-143">вызывается действие настройки.</span><span class="sxs-lookup"><span data-stu-id="1efed-143">The configuration action is called.</span></span>

<span data-ttu-id="1efed-144">Чтобы создать именованный клиент, передайте его имя в `CreateClient`:</span><span class="sxs-lookup"><span data-stu-id="1efed-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="1efed-145">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="1efed-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="1efed-146">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="1efed-147">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-147">Typed clients</span></span>

<span data-ttu-id="1efed-148">Типизированные клиенты:</span><span class="sxs-lookup"><span data-stu-id="1efed-148">Typed clients:</span></span>

* <span data-ttu-id="1efed-149">предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="1efed-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="1efed-150">Это помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="1efed-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="1efed-151">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="1efed-152">Например, один типизированный клиент можно использовать:</span><span class="sxs-lookup"><span data-stu-id="1efed-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="1efed-153">для одной серверной конечной точки;</span><span class="sxs-lookup"><span data-stu-id="1efed-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="1efed-154">для инкапсуляции всей логики, связанной с конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="1efed-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="1efed-155">Поддерживаются работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="1efed-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="1efed-156">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="1efed-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="1efed-157">В приведенном выше коде:</span><span class="sxs-lookup"><span data-stu-id="1efed-157">In the preceding code:</span></span>

* <span data-ttu-id="1efed-158">Конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="1efed-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="1efed-159">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="1efed-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="1efed-160">Можно создать связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="1efed-161">Например, метод `GetAspNetDocsIssues` инкапсулирует код для получения открытых вопросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="1efed-162">Следующий код вызывает <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices` для регистрации класса типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="1efed-163">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="1efed-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="1efed-164">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="1efed-164">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="1efed-165">Конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-165">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="1efed-166">`HttpClient` может быть инкапсулирован в типизированном клиенте.</span><span class="sxs-lookup"><span data-stu-id="1efed-166">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="1efed-167">Вместо предоставления его как свойства определите метод для внутреннего вызова экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="1efed-167">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="1efed-168">В приведенном выше коде `HttpClient` хранится в закрытом поле.</span><span class="sxs-lookup"><span data-stu-id="1efed-168">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="1efed-169">Доступ к `HttpClient` осуществляется с помощью общедоступного метода `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="1efed-169">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="1efed-170">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-170">Generated clients</span></span>

<span data-ttu-id="1efed-171">`IHttpClientFactory` можно использовать в сочетании с библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="1efed-171">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="1efed-172">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="1efed-172">Refit is a REST library for .NET.</span></span> <span data-ttu-id="1efed-173">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="1efed-173">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="1efed-174">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="1efed-174">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="1efed-175">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="1efed-175">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="1efed-176">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="1efed-176">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="1efed-177">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="1efed-177">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="1efed-178">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="1efed-178">Outgoing request middleware</span></span>

<span data-ttu-id="1efed-179">В `HttpClient` существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-179">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="1efed-180">`IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="1efed-180">`IHttpClientFactory`:</span></span>

* <span data-ttu-id="1efed-181">Упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="1efed-181">Simplifies defining the handlers to apply for each named client.</span></span>
* <span data-ttu-id="1efed-182">Поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-182">Supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="1efed-183">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="1efed-183">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="1efed-184">Этот шаблон:</span><span class="sxs-lookup"><span data-stu-id="1efed-184">This pattern:</span></span>

  * <span data-ttu-id="1efed-185">похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="1efed-185">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
  * <span data-ttu-id="1efed-186">предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая:</span><span class="sxs-lookup"><span data-stu-id="1efed-186">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>

    * <span data-ttu-id="1efed-187">кэширование</span><span class="sxs-lookup"><span data-stu-id="1efed-187">caching</span></span>
    * <span data-ttu-id="1efed-188">обработку ошибок</span><span class="sxs-lookup"><span data-stu-id="1efed-188">error handling</span></span>
    * <span data-ttu-id="1efed-189">сериализацию</span><span class="sxs-lookup"><span data-stu-id="1efed-189">serialization</span></span>
    * <span data-ttu-id="1efed-190">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1efed-190">logging</span></span>

<span data-ttu-id="1efed-191">Чтобы создать делегированный обработчик, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="1efed-191">To create a delegating handler:</span></span>

* <span data-ttu-id="1efed-192">Создайте объект, производный от <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-192">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="1efed-193">Переопределите метод <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-193">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="1efed-194">Выполните код до передачи запроса следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="1efed-194">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="1efed-195">Предыдущий код проверяет, находится ли заголовок `X-API-KEY` в запросе.</span><span class="sxs-lookup"><span data-stu-id="1efed-195">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="1efed-196">Если `X-API-KEY` отсутствует, возвращается <xref:System.Net.HttpStatusCode.BadRequest>.</span><span class="sxs-lookup"><span data-stu-id="1efed-196">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="1efed-197">Можно добавить сразу несколько обработчиков в конфигурацию для `HttpClient` с использованием <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="1efed-197">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="1efed-198">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1efed-198">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="1efed-199">`IHttpClientFactory` создает отдельную область внедрения зависимостей для каждого обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-199">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="1efed-200">Обработчики могут зависеть от служб из любой области.</span><span class="sxs-lookup"><span data-stu-id="1efed-200">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="1efed-201">Службы, которые зависят от обработчиков, удаляются при удалении обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-201">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="1efed-202">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-202">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="1efed-203">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="1efed-203">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="1efed-204">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="1efed-204">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="1efed-205">Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:</span><span class="sxs-lookup"><span data-stu-id="1efed-205">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="1efed-206">Передайте данные в обработчик с помощью [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="1efed-206">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="1efed-207">Используйте <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> для доступа к текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="1efed-207">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="1efed-208">Создайте пользовательский объект хранилища <xref:System.Threading.AsyncLocal`1> для передачи данных.</span><span class="sxs-lookup"><span data-stu-id="1efed-208">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="1efed-209">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-209">Use Polly-based handlers</span></span>

<span data-ttu-id="1efed-210">`IHttpClientFactory` интегрируется с библиотекой сторонних разработчиков [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="1efed-210">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="1efed-211">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="1efed-211">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="1efed-212">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="1efed-212">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="1efed-213">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="1efed-213">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="1efed-214">Расширения Polly поддерживают добавление обработчиков на основе Polly клиентам.</span><span class="sxs-lookup"><span data-stu-id="1efed-214">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="1efed-215">Polly нужен пакет NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="1efed-215">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="1efed-216">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="1efed-216">Handle transient faults</span></span>

<span data-ttu-id="1efed-217">Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными.</span><span class="sxs-lookup"><span data-stu-id="1efed-217">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="1efed-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="1efed-218"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="1efed-219">Политики, настроенные с помощью `AddTransientHttpErrorPolicy`, обрабатывают следующие ответы:</span><span class="sxs-lookup"><span data-stu-id="1efed-219">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="1efed-220">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="1efed-220">HTTP 5xx</span></span>
* <span data-ttu-id="1efed-221">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="1efed-221">HTTP 408</span></span>

<span data-ttu-id="1efed-222">`AddTransientHttpErrorPolicy` предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="1efed-222">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="1efed-223">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="1efed-223">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="1efed-224">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="1efed-224">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="1efed-225">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="1efed-225">Dynamically select policies</span></span>

<span data-ttu-id="1efed-226">Предоставляются методы расширения для добавления обработчиков на основе Polly, например <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-226">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="1efed-227">Следующая перегрузка `AddPolicyHandler` проверяет запрос для определения применимой политики:</span><span class="sxs-lookup"><span data-stu-id="1efed-227">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="1efed-228">Если в приведенном выше коде исходящий запрос является запросом HTTP GET, применяется время ожидания 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="1efed-228">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="1efed-229">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="1efed-229">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="1efed-230">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-230">Add multiple Polly handlers</span></span>

<span data-ttu-id="1efed-231">Общепринятой практикой является вложение политик Polly:</span><span class="sxs-lookup"><span data-stu-id="1efed-231">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="1efed-232">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="1efed-232">In the preceding example:</span></span>

* <span data-ttu-id="1efed-233">Добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-233">Two handlers are added.</span></span>
* <span data-ttu-id="1efed-234">Первый обработчик использует <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*>, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="1efed-234">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="1efed-235">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="1efed-235">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="1efed-236">Второй вызов `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="1efed-236">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="1efed-237">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="1efed-237">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="1efed-238">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="1efed-238">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="1efed-239">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="1efed-239">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="1efed-240">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-240">Add policies from the Polly registry</span></span>

<span data-ttu-id="1efed-241">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="1efed-241">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="1efed-242">В приведенном ниже коде выполняется следующее:</span><span class="sxs-lookup"><span data-stu-id="1efed-242">In the following code:</span></span>

* <span data-ttu-id="1efed-243">Добавляются политики regular и long.</span><span class="sxs-lookup"><span data-stu-id="1efed-243">The "regular" and "long" polices are added.</span></span>
* <span data-ttu-id="1efed-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> добавляет политики regular и long из реестра.</span><span class="sxs-lookup"><span data-stu-id="1efed-244"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*>  adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="1efed-245">Дополнительные сведения о `IHttpClientFactory` и интеграции Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="1efed-245">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="1efed-246">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="1efed-246">HttpClient and lifetime management</span></span>

<span data-ttu-id="1efed-247">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-247">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="1efed-248"><xref:System.Net.Http.HttpMessageHandler> создается для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-248">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="1efed-249">Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-249">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="1efed-250">`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="1efed-250">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="1efed-251">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="1efed-251">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="1efed-252">Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями.</span><span class="sxs-lookup"><span data-stu-id="1efed-252">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="1efed-253">Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="1efed-253">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="1efed-254">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения службы доменных имен (DNS).</span><span class="sxs-lookup"><span data-stu-id="1efed-254">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="1efed-255">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="1efed-255">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="1efed-256">Значение по умолчанию можно переопределить для каждого именованного клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-256">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="1efed-257">Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, **не** требующие освобождения.</span><span class="sxs-lookup"><span data-stu-id="1efed-257">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="1efed-258">Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-258">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="1efed-259">`IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-259">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="1efed-260">До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени.</span><span class="sxs-lookup"><span data-stu-id="1efed-260">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="1efed-261">После перехода на `IHttpClientFactory` это уже не нужно.</span><span class="sxs-lookup"><span data-stu-id="1efed-261">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="1efed-262">Альтернативы интерфейсу IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="1efed-262">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="1efed-263">Использование `IHttpClientFactory` в приложении с внедрением зависимостей позволяет:</span><span class="sxs-lookup"><span data-stu-id="1efed-263">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="1efed-264">предотвращать проблемы нехватки ресурсов путем объединения экземпляров `HttpMessageHandler` в пулы;</span><span class="sxs-lookup"><span data-stu-id="1efed-264">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="1efed-265">предотвращать проблемы устаревания записей DNS путем регулярной утилизации экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-265">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="1efed-266">Существуют альтернативные способы решения указанных выше проблем с помощью долгосрочного экземпляра <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-266">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="1efed-267">Создайте экземпляр `SocketsHttpHandler` при запуске приложения и используйте его в течение всего жизненного цикла приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-267">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="1efed-268">Присвойте <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> соответствующее значение в соответствии со временем обновления записей DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-268">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="1efed-269">По мере необходимости создавайте экземпляры `HttpClient` с помощью `new HttpClient(handler, disposeHandler: false)`.</span><span class="sxs-lookup"><span data-stu-id="1efed-269">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="1efed-270">Описанные выше подходы решают проблемы, связанные с управлением ресурсами, которые в `IHttpClientFactory` решаются сходным образом.</span><span class="sxs-lookup"><span data-stu-id="1efed-270">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="1efed-271">`SocketsHttpHandler` обеспечивает совместное использование подключений экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-271">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="1efed-272">Этот позволяет предотвратить нехватку сокетов.</span><span class="sxs-lookup"><span data-stu-id="1efed-272">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="1efed-273">`SocketsHttpHandler` уничтожает подключения в соответствии со значением `PooledConnectionLifetime`, чтобы предотвратить проблемы устаревания записей DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-273">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="1efed-274">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="1efed-274">Cookies</span></span>

<span data-ttu-id="1efed-275">Объединение экземпляров `HttpMessageHandler` в пул приводит к совместному использованию объектов `CookieContainer`.</span><span class="sxs-lookup"><span data-stu-id="1efed-275">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="1efed-276">Непредвиденное совместное использование объектов `CookieContainer` часто приводит к ошибкам в коде.</span><span class="sxs-lookup"><span data-stu-id="1efed-276">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="1efed-277">Для приложений, которым требуются файлы cookie, рекомендуется один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="1efed-277">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="1efed-278">отключите автоматическую обработку файлов cookie;</span><span class="sxs-lookup"><span data-stu-id="1efed-278">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="1efed-279">не используйте `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="1efed-279">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="1efed-280">Чтобы отключить автоматическую обработку файлов cookie, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>:</span><span class="sxs-lookup"><span data-stu-id="1efed-280">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="1efed-281">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1efed-281">Logging</span></span>

<span data-ttu-id="1efed-282">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-282">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="1efed-283">Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1efed-283">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="1efed-284">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="1efed-284">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="1efed-285">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-285">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="1efed-286">Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="1efed-286">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="1efed-287">Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-287">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="1efed-288">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="1efed-288">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="1efed-289">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="1efed-289">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="1efed-290">Кроме того, журнал ведется в конвейере обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-290">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="1efed-291">В примере *MyNamedClient* эти сообщения регистрируются с категорией журнала "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="1efed-291">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="1efed-292">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса.</span><span class="sxs-lookup"><span data-stu-id="1efed-292">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="1efed-293">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="1efed-293">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="1efed-294">Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="1efed-294">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="1efed-295">Сюда входят изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="1efed-295">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="1efed-296">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам.</span><span class="sxs-lookup"><span data-stu-id="1efed-296">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="1efed-297">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="1efed-297">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="1efed-298">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="1efed-298">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="1efed-299">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1efed-299">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="1efed-300">Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-300">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="1efed-301">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="1efed-301">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="1efed-302">Использование IHttpClientFactory в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="1efed-302">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="1efed-303">В консольном приложении добавьте в проект следующие ссылки на пакеты:</span><span class="sxs-lookup"><span data-stu-id="1efed-303">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="1efed-304">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="1efed-304">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="1efed-305">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="1efed-305">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="1efed-306">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1efed-306">In the following example:</span></span>

* <span data-ttu-id="1efed-307"><xref:System.Net.Http.IHttpClientFactory> регистрируется в контейнере службы [универсального узла](xref:fundamentals/host/generic-host):</span><span class="sxs-lookup"><span data-stu-id="1efed-307"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="1efed-308">`MyService` создает экземпляр фабрики клиента из службы, который используется для создания `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-308">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="1efed-309">`HttpClient` используется для получения веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="1efed-309">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="1efed-310">`Main` создает область для выполнения метода `GetPage` службы и вывода первых 500 символов содержимого веб-страницы на консоль.</span><span class="sxs-lookup"><span data-stu-id="1efed-310">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="1efed-311">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1efed-311">Additional resources</span></span>

* [<span data-ttu-id="1efed-312">Использование HttpClientFactory для реализации устойчивых HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="1efed-312">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="1efed-313">Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-313">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="1efed-314">Реализация шаблона размыкателя цепи</span><span class="sxs-lookup"><span data-stu-id="1efed-314">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="1efed-315">Сериализация и десериализация JSON в .NET</span><span class="sxs-lookup"><span data-stu-id="1efed-315">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="1efed-316">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak) и [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="1efed-316">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="1efed-317"><xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении.</span><span class="sxs-lookup"><span data-stu-id="1efed-317">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="1efed-318">Так вы получите следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="1efed-318">It offers the following benefits:</span></span>

* <span data-ttu-id="1efed-319">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-319">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="1efed-320">Например, можно зарегистрировать и настроить клиент *github* для доступа к [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="1efed-320">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="1efed-321">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="1efed-321">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="1efed-322">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient` и предоставление расширений для ПО промежуточного слоя на основе Polly для использования этой возможности.</span><span class="sxs-lookup"><span data-stu-id="1efed-322">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="1efed-323">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="1efed-323">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="1efed-324">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="1efed-324">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="1efed-325">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1efed-325">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="1efed-326">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="1efed-326">Consumption patterns</span></span>

<span data-ttu-id="1efed-327">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="1efed-327">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="1efed-328">Основное использование</span><span class="sxs-lookup"><span data-stu-id="1efed-328">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="1efed-329">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-329">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="1efed-330">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-330">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="1efed-331">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-331">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="1efed-332">Все способы равноценны.</span><span class="sxs-lookup"><span data-stu-id="1efed-332">None of them are strictly superior to another.</span></span> <span data-ttu-id="1efed-333">Оптимальный подход зависит от ограничений приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-333">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="1efed-334">Основное использование</span><span class="sxs-lookup"><span data-stu-id="1efed-334">Basic usage</span></span>

<span data-ttu-id="1efed-335">`IHttpClientFactory` можно зарегистрировать путем вызова метода расширения `AddHttpClient` в `IServiceCollection` внутри метода `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1efed-335">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="1efed-336">После регистрации код может принимать `IHttpClientFactory` в любом месте, куда можно внедрить службу с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="1efed-336">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1efed-337">`IHttpClientFactory` можно использовать для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="1efed-337">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="1efed-338">Подобное использование `IHttpClientFactory` — это отличный способ рефакторинга имеющегося приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-338">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="1efed-339">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-339">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="1efed-340">Там, где в данный момент создаются экземпляры `HttpClient`, используйте вызов к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-340">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="1efed-341">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-341">Named clients</span></span>

<span data-ttu-id="1efed-342">Если для приложения предполагаются разные способы использования `HttpClient`, каждый со своей конфигурацией, можно применять **именованные клиенты**.</span><span class="sxs-lookup"><span data-stu-id="1efed-342">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="1efed-343">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1efed-343">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="1efed-344">В приведенном выше коде вызывается клиент `AddHttpClient`, предоставляющий имя *github*.</span><span class="sxs-lookup"><span data-stu-id="1efed-344">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="1efed-345">У клиента есть некоторые настройки по умолчанию &mdash; а именно: базовый адрес и два заголовка, необходимые для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="1efed-345">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="1efed-346">При каждом вызове `CreateClient` создается новый экземпляр `HttpClient` и вызывается действие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1efed-346">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="1efed-347">Для использования именованного клиента можно передать строковый параметр в `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-347">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="1efed-348">Укажите имя создаваемого клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-348">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="1efed-349">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="1efed-349">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="1efed-350">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-350">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="1efed-351">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-351">Typed clients</span></span>

<span data-ttu-id="1efed-352">Типизированные клиенты:</span><span class="sxs-lookup"><span data-stu-id="1efed-352">Typed clients:</span></span>

* <span data-ttu-id="1efed-353">предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="1efed-353">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="1efed-354">Это помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="1efed-354">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="1efed-355">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-355">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="1efed-356">Например, для конечной точки серверной части можно использовать один типизированный клиент, который будет содержать всю логику работы с этой конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="1efed-356">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="1efed-357">Поддерживаются работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="1efed-357">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="1efed-358">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="1efed-358">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="1efed-359">В приведенном выше коде конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="1efed-359">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="1efed-360">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="1efed-360">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="1efed-361">Можно определить связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-361">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="1efed-362">Метод `GetAspNetDocsIssues` инкапсулирует код, необходимый для запроса и анализа последнего открытого выпуска из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="1efed-362">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="1efed-363">Для регистрации типизированного клиента можно использовать универсальный метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices`, указав класс типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-363">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="1efed-364">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="1efed-364">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="1efed-365">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="1efed-365">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="1efed-366">При желании конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-366">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="1efed-367">Можно полностью инкапсулировать `HttpClient` внутри типизированного клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-367">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="1efed-368">Вместо предоставления его как свойства можно использовать открытые методы для внутреннего вызова экземпляра `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-368">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="1efed-369">В приведенном выше коде `HttpClient` хранится как закрытое поле.</span><span class="sxs-lookup"><span data-stu-id="1efed-369">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="1efed-370">Любой доступ для совершения внешних вызовов осуществляется через метод `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="1efed-370">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="1efed-371">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-371">Generated clients</span></span>

<span data-ttu-id="1efed-372">`IHttpClientFactory` можно использовать в сочетании с другими библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="1efed-372">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="1efed-373">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="1efed-373">Refit is a REST library for .NET.</span></span> <span data-ttu-id="1efed-374">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="1efed-374">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="1efed-375">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="1efed-375">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="1efed-376">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="1efed-376">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="1efed-377">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="1efed-377">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="1efed-378">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="1efed-378">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="1efed-379">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="1efed-379">Outgoing request middleware</span></span>

<span data-ttu-id="1efed-380">В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-380">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="1efed-381">Класс `IHttpClientFactory` упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="1efed-381">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="1efed-382">Он поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-382">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="1efed-383">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="1efed-383">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="1efed-384">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1efed-384">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="1efed-385">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="1efed-385">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="1efed-386">Чтобы создать обработчик, необходимо определить класс, производный от <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-386">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="1efed-387">Переопределите метод `SendAsync` для выполнения кода до передачи запросов следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="1efed-387">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="1efed-388">В предыдущем коде определяется базовый обработчик.</span><span class="sxs-lookup"><span data-stu-id="1efed-388">The preceding code defines a basic handler.</span></span> <span data-ttu-id="1efed-389">Он проверяет, включен ли в запрос заголовок `X-API-KEY`.</span><span class="sxs-lookup"><span data-stu-id="1efed-389">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="1efed-390">Если заголовок отсутствует, он может избежать вызовов HTTP и вернуть подходящий ответ.</span><span class="sxs-lookup"><span data-stu-id="1efed-390">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="1efed-391">Во время регистрации можно добавить один или несколько обработчиков в конфигурацию для `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-391">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="1efed-392">Эта задача выполняется через методы расширения в <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1efed-392">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="1efed-393">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1efed-393">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="1efed-394">`IHttpClientFactory` создает отдельную область внедрения зависимостей для каждого обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-394">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="1efed-395">Обработчики могут зависеть от служб из любой области.</span><span class="sxs-lookup"><span data-stu-id="1efed-395">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="1efed-396">Службы, которые зависят от обработчиков, удаляются при удалении обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-396">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="1efed-397">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-397">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="1efed-398">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="1efed-398">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="1efed-399">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="1efed-399">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="1efed-400">Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:</span><span class="sxs-lookup"><span data-stu-id="1efed-400">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="1efed-401">Передайте данные в обработчик с помощью `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="1efed-401">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="1efed-402">Используйте `IHttpContextAccessor` для доступа к текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="1efed-402">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="1efed-403">Создайте пользовательский объект хранилища `AsyncLocal` для передачи данных.</span><span class="sxs-lookup"><span data-stu-id="1efed-403">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="1efed-404">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-404">Use Polly-based handlers</span></span>

<span data-ttu-id="1efed-405">`IHttpClientFactory` интегрируется с популярной библиотекой сторонних разработчиков под названием [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="1efed-405">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="1efed-406">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="1efed-406">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="1efed-407">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="1efed-407">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="1efed-408">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="1efed-408">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="1efed-409">Расширения Polly:</span><span class="sxs-lookup"><span data-stu-id="1efed-409">The Polly extensions:</span></span>

* <span data-ttu-id="1efed-410">Поддерживает добавление обработчиков на основе Polly клиентам.</span><span class="sxs-lookup"><span data-stu-id="1efed-410">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="1efed-411">Можно использовать после установки пакета NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="1efed-411">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="1efed-412">Пакет не включен в общую платформу ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1efed-412">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="1efed-413">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="1efed-413">Handle transient faults</span></span>

<span data-ttu-id="1efed-414">Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными.</span><span class="sxs-lookup"><span data-stu-id="1efed-414">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="1efed-415">Используется удобный метод расширения `AddTransientHttpErrorPolicy`, который позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="1efed-415">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="1efed-416">Политики, заданные с помощью этого метода расширения, обрабатывают `HttpRequestException`, ответы HTTP 5xx и ответы HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="1efed-416">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="1efed-417">Расширение `AddTransientHttpErrorPolicy` может быть использовано в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1efed-417">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1efed-418">Данное расширение предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="1efed-418">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="1efed-419">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="1efed-419">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="1efed-420">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="1efed-420">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="1efed-421">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="1efed-421">Dynamically select policies</span></span>

<span data-ttu-id="1efed-422">Существуют дополнительные методы расширения, которые можно использовать для добавления обработчиков на основе Polly.</span><span class="sxs-lookup"><span data-stu-id="1efed-422">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="1efed-423">Одним из таких расширений является `AddPolicyHandler` с несколькими перегрузками.</span><span class="sxs-lookup"><span data-stu-id="1efed-423">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="1efed-424">Одна перегрузка разрешает проверку запроса для определения необходимой политики:</span><span class="sxs-lookup"><span data-stu-id="1efed-424">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="1efed-425">Если в приведенном выше коде исходящий запрос является запросом HTTP GET, применяется время ожидания 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="1efed-425">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="1efed-426">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="1efed-426">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="1efed-427">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-427">Add multiple Polly handlers</span></span>

<span data-ttu-id="1efed-428">Общепринятой практикой является вложение политик Polly для предоставления расширенной функциональности:</span><span class="sxs-lookup"><span data-stu-id="1efed-428">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="1efed-429">В приведенном выше примере добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-429">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="1efed-430">Первый использует расширение `AddTransientHttpErrorPolicy`, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="1efed-430">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="1efed-431">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="1efed-431">Failed requests are retried up to three times.</span></span> <span data-ttu-id="1efed-432">Второй вызов к `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="1efed-432">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="1efed-433">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="1efed-433">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="1efed-434">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="1efed-434">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="1efed-435">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="1efed-435">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="1efed-436">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-436">Add policies from the Polly registry</span></span>

<span data-ttu-id="1efed-437">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="1efed-437">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="1efed-438">Предоставляется метод расширения, разрешающий добавление обработчика с помощью политики из реестра:</span><span class="sxs-lookup"><span data-stu-id="1efed-438">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="1efed-439">В приведенном выше коде, когда `PolicyRegistry` добавляется в `ServiceCollection`, регистрируются две политики.</span><span class="sxs-lookup"><span data-stu-id="1efed-439">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="1efed-440">Чтобы использовать политику из реестра, применяется метод `AddPolicyHandlerFromRegistry`, который передает имя необходимой политики.</span><span class="sxs-lookup"><span data-stu-id="1efed-440">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="1efed-441">Дополнительные сведения об интеграции `IHttpClientFactory` и Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="1efed-441">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="1efed-442">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="1efed-442">HttpClient and lifetime management</span></span>

<span data-ttu-id="1efed-443">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-443">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="1efed-444">Для каждого названного клиента существует <xref:System.Net.Http.HttpMessageHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-444">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="1efed-445">Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-445">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="1efed-446">`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="1efed-446">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="1efed-447">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="1efed-447">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="1efed-448">Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями.</span><span class="sxs-lookup"><span data-stu-id="1efed-448">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="1efed-449">Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="1efed-449">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="1efed-450">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-450">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="1efed-451">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="1efed-451">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="1efed-452">Значение по умолчанию можно переопределить для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-452">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="1efed-453">Чтобы переопределить это значение, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> в `IHttpClientBuilder`, который возвращается при создании клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-453">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="1efed-454">Высвобождать клиент не требуется.</span><span class="sxs-lookup"><span data-stu-id="1efed-454">Disposal of the client isn't required.</span></span> <span data-ttu-id="1efed-455">Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-455">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="1efed-456">`IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-456">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="1efed-457">Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, не требующие высвобождения.</span><span class="sxs-lookup"><span data-stu-id="1efed-457">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="1efed-458">До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени.</span><span class="sxs-lookup"><span data-stu-id="1efed-458">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="1efed-459">После перехода на `IHttpClientFactory` это уже не нужно.</span><span class="sxs-lookup"><span data-stu-id="1efed-459">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="1efed-460">Альтернативы интерфейсу IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="1efed-460">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="1efed-461">Использование `IHttpClientFactory` в приложении с внедрением зависимостей позволяет:</span><span class="sxs-lookup"><span data-stu-id="1efed-461">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="1efed-462">предотвращать проблемы нехватки ресурсов путем объединения экземпляров `HttpMessageHandler` в пулы;</span><span class="sxs-lookup"><span data-stu-id="1efed-462">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="1efed-463">предотвращать проблемы устаревания записей DNS путем регулярной утилизации экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-463">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="1efed-464">Существуют альтернативные способы решения указанных выше проблем с помощью долгосрочного экземпляра <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-464">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="1efed-465">Создайте экземпляр `SocketsHttpHandler` при запуске приложения и используйте его в течение всего жизненного цикла приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-465">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="1efed-466">Присвойте <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> соответствующее значение в соответствии со временем обновления записей DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-466">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="1efed-467">По мере необходимости создавайте экземпляры `HttpClient` с помощью `new HttpClient(handler, disposeHandler: false)`.</span><span class="sxs-lookup"><span data-stu-id="1efed-467">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="1efed-468">Описанные выше подходы решают проблемы, связанные с управлением ресурсами, которые в `IHttpClientFactory` решаются сходным образом.</span><span class="sxs-lookup"><span data-stu-id="1efed-468">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="1efed-469">`SocketsHttpHandler` обеспечивает совместное использование подключений экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-469">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="1efed-470">Этот позволяет предотвратить нехватку сокетов.</span><span class="sxs-lookup"><span data-stu-id="1efed-470">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="1efed-471">`SocketsHttpHandler` уничтожает подключения в соответствии со значением `PooledConnectionLifetime`, чтобы предотвратить проблемы устаревания записей DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-471">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="1efed-472">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="1efed-472">Cookies</span></span>

<span data-ttu-id="1efed-473">Объединение экземпляров `HttpMessageHandler` в пул приводит к совместному использованию объектов `CookieContainer`.</span><span class="sxs-lookup"><span data-stu-id="1efed-473">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="1efed-474">Непредвиденное совместное использование объектов `CookieContainer` часто приводит к ошибкам в коде.</span><span class="sxs-lookup"><span data-stu-id="1efed-474">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="1efed-475">Для приложений, которым требуются файлы cookie, рекомендуется один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="1efed-475">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="1efed-476">отключите автоматическую обработку файлов cookie;</span><span class="sxs-lookup"><span data-stu-id="1efed-476">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="1efed-477">не используйте `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="1efed-477">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="1efed-478">Чтобы отключить автоматическую обработку файлов cookie, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>:</span><span class="sxs-lookup"><span data-stu-id="1efed-478">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="1efed-479">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1efed-479">Logging</span></span>

<span data-ttu-id="1efed-480">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-480">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="1efed-481">Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1efed-481">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="1efed-482">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="1efed-482">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="1efed-483">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-483">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="1efed-484">Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-484">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="1efed-485">Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-485">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="1efed-486">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="1efed-486">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="1efed-487">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="1efed-487">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="1efed-488">Кроме того, журнал ведется в конвейере обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-488">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="1efed-489">В примере *MyNamedClient* эти сообщения вносятся в журнал по категории журнала `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-489">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="1efed-490">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса по сети.</span><span class="sxs-lookup"><span data-stu-id="1efed-490">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="1efed-491">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="1efed-491">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="1efed-492">Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="1efed-492">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="1efed-493">Сюда входят, например, изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="1efed-493">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="1efed-494">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам при необходимости.</span><span class="sxs-lookup"><span data-stu-id="1efed-494">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="1efed-495">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="1efed-495">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="1efed-496">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="1efed-496">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="1efed-497">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1efed-497">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="1efed-498">Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-498">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="1efed-499">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="1efed-499">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="1efed-500">Использование IHttpClientFactory в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="1efed-500">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="1efed-501">В консольном приложении добавьте в проект следующие ссылки на пакеты:</span><span class="sxs-lookup"><span data-stu-id="1efed-501">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="1efed-502">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="1efed-502">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="1efed-503">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="1efed-503">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="1efed-504">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1efed-504">In the following example:</span></span>

* <span data-ttu-id="1efed-505"><xref:System.Net.Http.IHttpClientFactory> регистрируется в контейнере службы [универсального узла](xref:fundamentals/host/generic-host):</span><span class="sxs-lookup"><span data-stu-id="1efed-505"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="1efed-506">`MyService` создает экземпляр фабрики клиента из службы, который используется для создания `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-506">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="1efed-507">`HttpClient` используется для получения веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="1efed-507">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="1efed-508">`Main` создает область для выполнения метода `GetPage` службы и вывода первых 500 символов содержимого веб-страницы на консоль.</span><span class="sxs-lookup"><span data-stu-id="1efed-508">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="1efed-509">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1efed-509">Additional resources</span></span>

* [<span data-ttu-id="1efed-510">Использование HttpClientFactory для реализации устойчивых HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="1efed-510">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="1efed-511">Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-511">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="1efed-512">Реализация шаблона размыкателя цепи</span><span class="sxs-lookup"><span data-stu-id="1efed-512">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="1efed-513">Авторы: [Гленн Кондрон (Glenn Condron)](https://github.com/glennc), [Райан Новак (Ryan Nowak)](https://github.com/rynowak) и [Стив Гордон (Steve Gordon)](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="1efed-513">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="1efed-514"><xref:System.Net.Http.IHttpClientFactory> можно зарегистрировать и использовать для настройки и создания экземпляров <xref:System.Net.Http.HttpClient> в приложении.</span><span class="sxs-lookup"><span data-stu-id="1efed-514">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="1efed-515">Так вы получите следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="1efed-515">It offers the following benefits:</span></span>

* <span data-ttu-id="1efed-516">Центральное расположение для именования и настройки логических экземпляров `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-516">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="1efed-517">Например, можно зарегистрировать и настроить клиент *github* для доступа к [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="1efed-517">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="1efed-518">Можно зарегистрировать клиент по умолчанию для других целей.</span><span class="sxs-lookup"><span data-stu-id="1efed-518">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="1efed-519">Кодификация концепции исходящего ПО промежуточного слоя путем делегирования обработчиков в `HttpClient` и предоставление расширений для ПО промежуточного слоя на основе Polly для использования этой возможности.</span><span class="sxs-lookup"><span data-stu-id="1efed-519">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="1efed-520">Управление созданием пулов и временем существования базовых экземпляров `HttpClientMessageHandler` с целью избежать обычных проблем с DNS, которые возникают при управлении временем существования `HttpClient` вручную.</span><span class="sxs-lookup"><span data-stu-id="1efed-520">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="1efed-521">Настройка параметров ведения журнала (через `ILogger`) для всех запросов, отправленных через клиентов, созданных фабрикой.</span><span class="sxs-lookup"><span data-stu-id="1efed-521">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="1efed-522">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1efed-522">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1efed-523">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="1efed-523">Prerequisites</span></span>

<span data-ttu-id="1efed-524">Для проектов, предназначенных для .NET Framework, необходимо установить пакет NuGet [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/).</span><span class="sxs-lookup"><span data-stu-id="1efed-524">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="1efed-525">Пакет `Microsoft.Extensions.Http` уже включен в проекты, предназначенные для .NET Core и ссылающиеся на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1efed-525">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="1efed-526">Принципы использования</span><span class="sxs-lookup"><span data-stu-id="1efed-526">Consumption patterns</span></span>

<span data-ttu-id="1efed-527">Существует несколько способов использования `IHttpClientFactory` в приложении:</span><span class="sxs-lookup"><span data-stu-id="1efed-527">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="1efed-528">Основное использование</span><span class="sxs-lookup"><span data-stu-id="1efed-528">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="1efed-529">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-529">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="1efed-530">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-530">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="1efed-531">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-531">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="1efed-532">Все способы равноценны.</span><span class="sxs-lookup"><span data-stu-id="1efed-532">None of them are strictly superior to another.</span></span> <span data-ttu-id="1efed-533">Оптимальный подход зависит от ограничений приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-533">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="1efed-534">Основное использование</span><span class="sxs-lookup"><span data-stu-id="1efed-534">Basic usage</span></span>

<span data-ttu-id="1efed-535">`IHttpClientFactory` можно зарегистрировать путем вызова метода расширения `AddHttpClient` в `IServiceCollection` внутри метода `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1efed-535">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="1efed-536">После регистрации код может принимать `IHttpClientFactory` в любом месте, куда можно внедрить службу с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI).</span><span class="sxs-lookup"><span data-stu-id="1efed-536">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="1efed-537">`IHttpClientFactory` можно использовать для создания экземпляра `HttpClient`:</span><span class="sxs-lookup"><span data-stu-id="1efed-537">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="1efed-538">Подобное использование `IHttpClientFactory` — это отличный способ рефакторинга имеющегося приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-538">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="1efed-539">Он не оказывает влияния на использование `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-539">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="1efed-540">Там, где в данный момент создаются экземпляры `HttpClient`, используйте вызов к <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-540">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="1efed-541">Именованные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-541">Named clients</span></span>

<span data-ttu-id="1efed-542">Если для приложения предполагаются разные способы использования `HttpClient`, каждый со своей конфигурацией, можно применять **именованные клиенты**.</span><span class="sxs-lookup"><span data-stu-id="1efed-542">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="1efed-543">Конфигурацию для именованного клиента `HttpClient` можно указать во время регистрации в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1efed-543">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="1efed-544">В приведенном выше коде вызывается клиент `AddHttpClient`, предоставляющий имя *github*.</span><span class="sxs-lookup"><span data-stu-id="1efed-544">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="1efed-545">У клиента есть некоторые настройки по умолчанию &mdash; а именно: базовый адрес и два заголовка, необходимые для работы с API GitHub.</span><span class="sxs-lookup"><span data-stu-id="1efed-545">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="1efed-546">При каждом вызове `CreateClient` создается новый экземпляр `HttpClient` и вызывается действие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1efed-546">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="1efed-547">Для использования именованного клиента можно передать строковый параметр в `CreateClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-547">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="1efed-548">Укажите имя создаваемого клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-548">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="1efed-549">В приведенном выше коде в запросе не требуется указывать имя узла.</span><span class="sxs-lookup"><span data-stu-id="1efed-549">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="1efed-550">Достаточно передать только путь, так как используется базовый адрес, заданный для клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-550">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="1efed-551">Типизированные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-551">Typed clients</span></span>

<span data-ttu-id="1efed-552">Типизированные клиенты:</span><span class="sxs-lookup"><span data-stu-id="1efed-552">Typed clients:</span></span>

* <span data-ttu-id="1efed-553">предоставляют те же возможности, что и именованные клиенты, без необходимости использовать строки в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="1efed-553">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="1efed-554">Это помогает IntelliSense и компилятору при использовании клиентов.</span><span class="sxs-lookup"><span data-stu-id="1efed-554">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="1efed-555">Они предоставляют единое расположение для настройки и взаимодействия с конкретным клиентом `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-555">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="1efed-556">Например, для конечной точки серверной части можно использовать один типизированный клиент, который будет содержать всю логику работы с этой конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="1efed-556">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="1efed-557">Поддерживаются работа с внедрением зависимостей и возможность вставки в нужное место в приложении.</span><span class="sxs-lookup"><span data-stu-id="1efed-557">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="1efed-558">Типизированный клиент принимает параметр `HttpClient` в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="1efed-558">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="1efed-559">В приведенном выше коде конфигурация перемещается в типизированный клиент.</span><span class="sxs-lookup"><span data-stu-id="1efed-559">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="1efed-560">Объект `HttpClient` предоставляется в виде открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="1efed-560">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="1efed-561">Можно определить связанные с API методы, которые предоставляют функциональные возможности `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-561">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="1efed-562">Метод `GetAspNetDocsIssues` инкапсулирует код, необходимый для запроса и анализа последнего открытого выпуска из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="1efed-562">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="1efed-563">Для регистрации типизированного клиента можно использовать универсальный метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> в `Startup.ConfigureServices`, указав класс типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-563">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="1efed-564">Типизированный клиент регистрируется во внедрении зависимостей как временный.</span><span class="sxs-lookup"><span data-stu-id="1efed-564">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="1efed-565">Типизированный клиент можно внедрить и использовать напрямую:</span><span class="sxs-lookup"><span data-stu-id="1efed-565">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="1efed-566">При желании конфигурацию для типизированного клиента можно указать во время регистрации в `Startup.ConfigureServices`, а не в конструкторе типизированного клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-566">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="1efed-567">Можно полностью инкапсулировать `HttpClient` внутри типизированного клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-567">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="1efed-568">Вместо предоставления его как свойства можно использовать открытые методы для внутреннего вызова экземпляра `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-568">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="1efed-569">В приведенном выше коде `HttpClient` хранится как закрытое поле.</span><span class="sxs-lookup"><span data-stu-id="1efed-569">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="1efed-570">Любой доступ для совершения внешних вызовов осуществляется через метод `GetRepos`.</span><span class="sxs-lookup"><span data-stu-id="1efed-570">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="1efed-571">Созданные клиенты</span><span class="sxs-lookup"><span data-stu-id="1efed-571">Generated clients</span></span>

<span data-ttu-id="1efed-572">`IHttpClientFactory` можно использовать в сочетании с другими библиотеками сторонних разработчиков, например [Refit](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="1efed-572">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="1efed-573">Refit — это библиотека REST для .NET.</span><span class="sxs-lookup"><span data-stu-id="1efed-573">Refit is a REST library for .NET.</span></span> <span data-ttu-id="1efed-574">Она преобразует REST API в динамические интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="1efed-574">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="1efed-575">Реализация интерфейса формируется динамически с помощью `RestService` с использованием `HttpClient` для совершения внешних вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="1efed-575">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="1efed-576">Для представления внешнего API и его ответа определяются интерфейс и ответ:</span><span class="sxs-lookup"><span data-stu-id="1efed-576">An interface and a reply are defined to represent the external API and its response:</span></span>

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

<span data-ttu-id="1efed-577">Можно добавить типизированный клиент, используя Refit для создания реализации:</span><span class="sxs-lookup"><span data-stu-id="1efed-577">A typed client can be added, using Refit to generate the implementation:</span></span>

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

<span data-ttu-id="1efed-578">При необходимости можно использовать определенный интерфейс с реализацией, предоставленной внедрением зависимостей и Refit:</span><span class="sxs-lookup"><span data-stu-id="1efed-578">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

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

## <a name="outgoing-request-middleware"></a><span data-ttu-id="1efed-579">ПО промежуточного слоя для исходящих запросов</span><span class="sxs-lookup"><span data-stu-id="1efed-579">Outgoing request middleware</span></span>

<span data-ttu-id="1efed-580">В `HttpClient` уже существует концепция делегирования обработчиков, которые можно связать друг с другом для исходящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-580">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="1efed-581">Класс `IHttpClientFactory` упрощает определение обработчиков для применения к каждому именованному клиенту.</span><span class="sxs-lookup"><span data-stu-id="1efed-581">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="1efed-582">Он поддерживает регистрацию и объединение в цепочки нескольких обработчиков для создания конвейера ПО промежуточного слоя для исходящих запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-582">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="1efed-583">Каждый из этих обработчиков может выполнять работу до и после исходящего запроса.</span><span class="sxs-lookup"><span data-stu-id="1efed-583">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="1efed-584">Этот шаблон похож на входящий конвейер ПО промежуточного слоя в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1efed-584">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="1efed-585">Шаблон предоставляет механизм управления сквозной функциональностью HTTP-запросов, включая кэширование, обработку ошибок, сериализацию и ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="1efed-585">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="1efed-586">Чтобы создать обработчик, необходимо определить класс, производный от <xref:System.Net.Http.DelegatingHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-586">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="1efed-587">Переопределите метод `SendAsync` для выполнения кода до передачи запросов следующему обработчику в конвейере:</span><span class="sxs-lookup"><span data-stu-id="1efed-587">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="1efed-588">В предыдущем коде определяется базовый обработчик.</span><span class="sxs-lookup"><span data-stu-id="1efed-588">The preceding code defines a basic handler.</span></span> <span data-ttu-id="1efed-589">Он проверяет, включен ли в запрос заголовок `X-API-KEY`.</span><span class="sxs-lookup"><span data-stu-id="1efed-589">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="1efed-590">Если заголовок отсутствует, он может избежать вызовов HTTP и вернуть подходящий ответ.</span><span class="sxs-lookup"><span data-stu-id="1efed-590">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="1efed-591">Во время регистрации можно добавить один или несколько обработчиков в конфигурацию для `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-591">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="1efed-592">Эта задача выполняется через методы расширения в <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1efed-592">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="1efed-593">В приведенном выше коде `ValidateHeaderHandler` регистрируется с помощью внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1efed-593">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="1efed-594">Обработчик **должен** регистрироваться во внедрении зависимостей как временная служба, а не ограниченная.</span><span class="sxs-lookup"><span data-stu-id="1efed-594">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="1efed-595">Если обработчик зарегистрирован в качестве службы с областью действия и все службы, от которых зависит этот обработчик, освобождаются:</span><span class="sxs-lookup"><span data-stu-id="1efed-595">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="1efed-596">Службы обработчика могли быть удалены, прежде чем обработчик вышел из области действия.</span><span class="sxs-lookup"><span data-stu-id="1efed-596">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="1efed-597">Освобожденные службы обработчика приводят к его сбою.</span><span class="sxs-lookup"><span data-stu-id="1efed-597">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="1efed-598">После регистрации можно вызвать <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*>, передав тип обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-598">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="1efed-599">Можно зарегистрировать несколько обработчиков в порядке, в котором они должны выполняться.</span><span class="sxs-lookup"><span data-stu-id="1efed-599">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="1efed-600">Каждый обработчик содержит следующий обработчик, пока последний `HttpClientHandler` не выполнит запрос:</span><span class="sxs-lookup"><span data-stu-id="1efed-600">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="1efed-601">Используйте один из следующих методов для предоставления общего доступа к состоянию отдельных запросов с помощью обработчиков сообщений:</span><span class="sxs-lookup"><span data-stu-id="1efed-601">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="1efed-602">Передайте данные в обработчик с помощью `HttpRequestMessage.Properties`.</span><span class="sxs-lookup"><span data-stu-id="1efed-602">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="1efed-603">Используйте `IHttpContextAccessor` для доступа к текущему запросу.</span><span class="sxs-lookup"><span data-stu-id="1efed-603">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="1efed-604">Создайте пользовательский объект хранилища `AsyncLocal` для передачи данных.</span><span class="sxs-lookup"><span data-stu-id="1efed-604">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="1efed-605">Использование обработчиков на основе Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-605">Use Polly-based handlers</span></span>

<span data-ttu-id="1efed-606">`IHttpClientFactory` интегрируется с популярной библиотекой сторонних разработчиков под названием [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="1efed-606">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="1efed-607">Polly — это комплексная библиотека, обеспечивающая отказоустойчивость и обработку временных сбоев в .NET.</span><span class="sxs-lookup"><span data-stu-id="1efed-607">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="1efed-608">Она позволяет разработчикам выражать политики, например политику повтора, размыкателя цепи, времени ожидания, изоляции отсеков и отката, более эффективным и потокобезопасным образом.</span><span class="sxs-lookup"><span data-stu-id="1efed-608">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="1efed-609">Для использования политик Polly с настроенными экземплярами `HttpClient` предоставляются методы расширения.</span><span class="sxs-lookup"><span data-stu-id="1efed-609">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="1efed-610">Расширения Polly:</span><span class="sxs-lookup"><span data-stu-id="1efed-610">The Polly extensions:</span></span>

* <span data-ttu-id="1efed-611">Поддерживает добавление обработчиков на основе Polly клиентам.</span><span class="sxs-lookup"><span data-stu-id="1efed-611">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="1efed-612">Можно использовать после установки пакета NuGet [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/).</span><span class="sxs-lookup"><span data-stu-id="1efed-612">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="1efed-613">Пакет не включен в общую платформу ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1efed-613">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="1efed-614">Обработка временных сбоев</span><span class="sxs-lookup"><span data-stu-id="1efed-614">Handle transient faults</span></span>

<span data-ttu-id="1efed-615">Чаще всего ошибки происходят, когда внешние вызовы HTTP являются временными.</span><span class="sxs-lookup"><span data-stu-id="1efed-615">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="1efed-616">Используется удобный метод расширения `AddTransientHttpErrorPolicy`, который позволяет определить политику для обработки временных ошибок.</span><span class="sxs-lookup"><span data-stu-id="1efed-616">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="1efed-617">Политики, заданные с помощью этого метода расширения, обрабатывают `HttpRequestException`, ответы HTTP 5xx и ответы HTTP 408.</span><span class="sxs-lookup"><span data-stu-id="1efed-617">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="1efed-618">Расширение `AddTransientHttpErrorPolicy` может быть использовано в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="1efed-618">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="1efed-619">Данное расширение предоставляет доступ к объекту `PolicyBuilder`, настроенному для обработки ошибок, представляющих возможный временный сбой:</span><span class="sxs-lookup"><span data-stu-id="1efed-619">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="1efed-620">В приведенном выше коде определена политика `WaitAndRetryAsync`.</span><span class="sxs-lookup"><span data-stu-id="1efed-620">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="1efed-621">Неудачные запросы повторяются до трех раз с задержкой 600 мс между попытками.</span><span class="sxs-lookup"><span data-stu-id="1efed-621">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="1efed-622">Динамический выбор политик</span><span class="sxs-lookup"><span data-stu-id="1efed-622">Dynamically select policies</span></span>

<span data-ttu-id="1efed-623">Существуют дополнительные методы расширения, которые можно использовать для добавления обработчиков на основе Polly.</span><span class="sxs-lookup"><span data-stu-id="1efed-623">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="1efed-624">Одним из таких расширений является `AddPolicyHandler` с несколькими перегрузками.</span><span class="sxs-lookup"><span data-stu-id="1efed-624">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="1efed-625">Одна перегрузка разрешает проверку запроса для определения необходимой политики:</span><span class="sxs-lookup"><span data-stu-id="1efed-625">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="1efed-626">Если в приведенном выше коде исходящий запрос является запросом HTTP GET, применяется время ожидания 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="1efed-626">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="1efed-627">Для остальных методов HTTP время ожидания — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="1efed-627">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="1efed-628">Добавление нескольких обработчиков Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-628">Add multiple Polly handlers</span></span>

<span data-ttu-id="1efed-629">Общепринятой практикой является вложение политик Polly для предоставления расширенной функциональности:</span><span class="sxs-lookup"><span data-stu-id="1efed-629">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="1efed-630">В приведенном выше примере добавляются два обработчика.</span><span class="sxs-lookup"><span data-stu-id="1efed-630">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="1efed-631">Первый использует расширение `AddTransientHttpErrorPolicy`, чтобы добавить политику повтора.</span><span class="sxs-lookup"><span data-stu-id="1efed-631">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="1efed-632">Неудачные запросы выполняются повторно до трех раз.</span><span class="sxs-lookup"><span data-stu-id="1efed-632">Failed requests are retried up to three times.</span></span> <span data-ttu-id="1efed-633">Второй вызов к `AddTransientHttpErrorPolicy` добавляет политику размыкателя цепи.</span><span class="sxs-lookup"><span data-stu-id="1efed-633">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="1efed-634">Дополнительные внешние запросы блокируются в течение 30 секунд в случае пяти неудачных попыток подряд.</span><span class="sxs-lookup"><span data-stu-id="1efed-634">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="1efed-635">Политики размыкателя цепи отслеживают состояние.</span><span class="sxs-lookup"><span data-stu-id="1efed-635">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="1efed-636">Все вызовы через этот клиент имеют одинаковое состояние цепи.</span><span class="sxs-lookup"><span data-stu-id="1efed-636">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="1efed-637">Добавление политик из реестра Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-637">Add policies from the Polly registry</span></span>

<span data-ttu-id="1efed-638">Подход к управлению регулярно используемыми политиками заключается в их однократном определении и регистрации с помощью `PolicyRegistry`.</span><span class="sxs-lookup"><span data-stu-id="1efed-638">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="1efed-639">Предоставляется метод расширения, разрешающий добавление обработчика с помощью политики из реестра:</span><span class="sxs-lookup"><span data-stu-id="1efed-639">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="1efed-640">В приведенном выше коде, когда `PolicyRegistry` добавляется в `ServiceCollection`, регистрируются две политики.</span><span class="sxs-lookup"><span data-stu-id="1efed-640">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="1efed-641">Чтобы использовать политику из реестра, применяется метод `AddPolicyHandlerFromRegistry`, который передает имя необходимой политики.</span><span class="sxs-lookup"><span data-stu-id="1efed-641">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="1efed-642">Дополнительные сведения об интеграции `IHttpClientFactory` и Polly см. на [вики-сайте Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="1efed-642">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="1efed-643">Управление HttpClient и временем существования</span><span class="sxs-lookup"><span data-stu-id="1efed-643">HttpClient and lifetime management</span></span>

<span data-ttu-id="1efed-644">При каждом вызове `CreateClient` в `IHttpClientFactory` возвращается новый экземпляр `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-644">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="1efed-645">Для каждого названного клиента существует <xref:System.Net.Http.HttpMessageHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-645">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="1efed-646">Фабрика обеспечивает управление временем существования экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-646">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="1efed-647">`IHttpClientFactory` объединяет в пул все экземпляры `HttpMessageHandler`, созданные фабрикой, чтобы уменьшить потребление ресурсов.</span><span class="sxs-lookup"><span data-stu-id="1efed-647">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="1efed-648">Экземпляр `HttpMessageHandler` можно использовать повторно из пула при создании экземпляра `HttpClient`, если его время существования еще не истекло.</span><span class="sxs-lookup"><span data-stu-id="1efed-648">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="1efed-649">Создавать пулы обработчиков желательно, так как каждый обработчик обычно управляет собственными базовыми HTTP-подключениями.</span><span class="sxs-lookup"><span data-stu-id="1efed-649">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="1efed-650">Создание лишних обработчиков может привести к задержке подключения.</span><span class="sxs-lookup"><span data-stu-id="1efed-650">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="1efed-651">Некоторые обработчики поддерживают подключения открытыми в течение неопределенного периода, что может помешать обработчику отреагировать на изменения DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-651">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="1efed-652">Время существования обработчика по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="1efed-652">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="1efed-653">Значение по умолчанию можно переопределить для каждого именованного клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-653">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="1efed-654">Чтобы переопределить это значение, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> в `IHttpClientBuilder`, который возвращается при создании клиента:</span><span class="sxs-lookup"><span data-stu-id="1efed-654">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="1efed-655">Высвобождать клиент не требуется.</span><span class="sxs-lookup"><span data-stu-id="1efed-655">Disposal of the client isn't required.</span></span> <span data-ttu-id="1efed-656">Высвобождение отменяет исходящие запросы и гарантирует, что указанный экземпляр `HttpClient` не может использоваться после вызова <xref:System.IDisposable.Dispose*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-656">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="1efed-657">`IHttpClientFactory` отслеживает и высвобождает ресурсы, используемые экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-657">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="1efed-658">Экземпляры `HttpClient` обычно можно рассматривать как объекты .NET, не требующие высвобождения.</span><span class="sxs-lookup"><span data-stu-id="1efed-658">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="1efed-659">До появления `IHttpClientFactory` один экземпляр `HttpClient` часто сохраняли в активном состоянии в течение длительного времени.</span><span class="sxs-lookup"><span data-stu-id="1efed-659">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="1efed-660">После перехода на `IHttpClientFactory` это уже не нужно.</span><span class="sxs-lookup"><span data-stu-id="1efed-660">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="1efed-661">Альтернативы интерфейсу IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="1efed-661">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="1efed-662">Использование `IHttpClientFactory` в приложении с внедрением зависимостей позволяет:</span><span class="sxs-lookup"><span data-stu-id="1efed-662">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="1efed-663">предотвращать проблемы нехватки ресурсов путем объединения экземпляров `HttpMessageHandler` в пулы;</span><span class="sxs-lookup"><span data-stu-id="1efed-663">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="1efed-664">предотвращать проблемы устаревания записей DNS путем регулярной утилизации экземпляров `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-664">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="1efed-665">Существуют альтернативные способы решения указанных выше проблем с помощью долгосрочного экземпляра <xref:System.Net.Http.SocketsHttpHandler>.</span><span class="sxs-lookup"><span data-stu-id="1efed-665">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="1efed-666">Создайте экземпляр `SocketsHttpHandler` при запуске приложения и используйте его в течение всего жизненного цикла приложения.</span><span class="sxs-lookup"><span data-stu-id="1efed-666">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="1efed-667">Присвойте <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> соответствующее значение в соответствии со временем обновления записей DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-667">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="1efed-668">По мере необходимости создавайте экземпляры `HttpClient` с помощью `new HttpClient(handler, disposeHandler: false)`.</span><span class="sxs-lookup"><span data-stu-id="1efed-668">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="1efed-669">Описанные выше подходы решают проблемы, связанные с управлением ресурсами, которые в `IHttpClientFactory` решаются сходным образом.</span><span class="sxs-lookup"><span data-stu-id="1efed-669">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="1efed-670">`SocketsHttpHandler` обеспечивает совместное использование подключений экземплярами `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-670">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="1efed-671">Этот позволяет предотвратить нехватку сокетов.</span><span class="sxs-lookup"><span data-stu-id="1efed-671">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="1efed-672">`SocketsHttpHandler` уничтожает подключения в соответствии со значением `PooledConnectionLifetime`, чтобы предотвратить проблемы устаревания записей DNS.</span><span class="sxs-lookup"><span data-stu-id="1efed-672">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="1efed-673">Файлы cookie</span><span class="sxs-lookup"><span data-stu-id="1efed-673">Cookies</span></span>

<span data-ttu-id="1efed-674">Объединение экземпляров `HttpMessageHandler` в пул приводит к совместному использованию объектов `CookieContainer`.</span><span class="sxs-lookup"><span data-stu-id="1efed-674">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="1efed-675">Непредвиденное совместное использование объектов `CookieContainer` часто приводит к ошибкам в коде.</span><span class="sxs-lookup"><span data-stu-id="1efed-675">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="1efed-676">Для приложений, которым требуются файлы cookie, рекомендуется один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="1efed-676">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="1efed-677">отключите автоматическую обработку файлов cookie;</span><span class="sxs-lookup"><span data-stu-id="1efed-677">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="1efed-678">не используйте `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="1efed-678">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="1efed-679">Чтобы отключить автоматическую обработку файлов cookie, вызовите <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>:</span><span class="sxs-lookup"><span data-stu-id="1efed-679">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="1efed-680">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1efed-680">Logging</span></span>

<span data-ttu-id="1efed-681">Клиенты, созданные через `IHttpClientFactory`, записывают сообщения журнала для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-681">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="1efed-682">Установите соответствующий уровень информации в конфигурации ведения журнала, чтобы просматривать сообщения журнала по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1efed-682">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="1efed-683">Дополнительное ведение журнала, например запись заголовков запросов, включено только на уровне трассировки.</span><span class="sxs-lookup"><span data-stu-id="1efed-683">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="1efed-684">Категория журнала для каждого клиента включает в себя имя клиента.</span><span class="sxs-lookup"><span data-stu-id="1efed-684">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="1efed-685">Клиент с именем *MyNamedClient*, например, записывает в журнал сообщения с категорией `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-685">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="1efed-686">Сообщения с суффиксом *LogicalHandler* создаются за пределами конвейера обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-686">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="1efed-687">Во время запроса сообщения записываются в журнал до обработки запроса другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="1efed-687">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="1efed-688">Во время ответа сообщения записываются в журнал после получения ответа другими обработчиками в конвейере.</span><span class="sxs-lookup"><span data-stu-id="1efed-688">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="1efed-689">Кроме того, журнал ведется в конвейере обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="1efed-689">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="1efed-690">В примере *MyNamedClient* эти сообщения вносятся в журнал по категории журнала `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span><span class="sxs-lookup"><span data-stu-id="1efed-690">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="1efed-691">Во время запроса это происходит после выполнения всех обработчиков и непосредственно перед отправкой запроса по сети.</span><span class="sxs-lookup"><span data-stu-id="1efed-691">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="1efed-692">Во время ответа в журнале записывается состояние ответа перед его передачей обратно по конвейеру обработчиков.</span><span class="sxs-lookup"><span data-stu-id="1efed-692">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="1efed-693">Включив ведение журнала в конвейере и за его пределами, можно выполнять проверку изменений, внесенных другими обработчиками конвейера.</span><span class="sxs-lookup"><span data-stu-id="1efed-693">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="1efed-694">Сюда входят, например, изменения заголовков запросов или кода состояния ответов.</span><span class="sxs-lookup"><span data-stu-id="1efed-694">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="1efed-695">Включение имени клиента в категорию журнала позволяет фильтровать журналы по именованным клиентам при необходимости.</span><span class="sxs-lookup"><span data-stu-id="1efed-695">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="1efed-696">Настройка HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="1efed-696">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="1efed-697">Иногда необходимо контролировать конфигурацию внутреннего обработчика `HttpMessageHandler`, используемого клиентом.</span><span class="sxs-lookup"><span data-stu-id="1efed-697">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="1efed-698">При добавлении именованного или типизированного клиента возвращается `IHttpClientBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1efed-698">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="1efed-699">Для определения делегата можно использовать метод расширения <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>.</span><span class="sxs-lookup"><span data-stu-id="1efed-699">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="1efed-700">Делегат используется для создания и настройки основного обработчика `HttpMessageHandler`, используемого этим клиентом:</span><span class="sxs-lookup"><span data-stu-id="1efed-700">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="1efed-701">Использование IHttpClientFactory в консольном приложении</span><span class="sxs-lookup"><span data-stu-id="1efed-701">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="1efed-702">В консольном приложении добавьте в проект следующие ссылки на пакеты:</span><span class="sxs-lookup"><span data-stu-id="1efed-702">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="1efed-703">Microsoft.Extensions.Hosting</span><span class="sxs-lookup"><span data-stu-id="1efed-703">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="1efed-704">Microsoft.Extensions.Http</span><span class="sxs-lookup"><span data-stu-id="1efed-704">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="1efed-705">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1efed-705">In the following example:</span></span>

* <span data-ttu-id="1efed-706"><xref:System.Net.Http.IHttpClientFactory> регистрируется в контейнере службы [универсального узла](xref:fundamentals/host/generic-host):</span><span class="sxs-lookup"><span data-stu-id="1efed-706"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="1efed-707">`MyService` создает экземпляр фабрики клиента из службы, который используется для создания `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="1efed-707">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="1efed-708">`HttpClient` используется для получения веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="1efed-708">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="1efed-709">`Main` создает область для выполнения метода `GetPage` службы и вывода первых 500 символов содержимого веб-страницы на консоль.</span><span class="sxs-lookup"><span data-stu-id="1efed-709">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="1efed-710">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1efed-710">Additional resources</span></span>

* [<span data-ttu-id="1efed-711">Использование HttpClientFactory для реализации устойчивых HTTP-запросов</span><span class="sxs-lookup"><span data-stu-id="1efed-711">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="1efed-712">Реализация повторных попыток вызова HTTP с экспоненциальной задержкой с помощью HttpClientFactory и политик Polly</span><span class="sxs-lookup"><span data-stu-id="1efed-712">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="1efed-713">Реализация шаблона размыкателя цепи</span><span class="sxs-lookup"><span data-stu-id="1efed-713">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
