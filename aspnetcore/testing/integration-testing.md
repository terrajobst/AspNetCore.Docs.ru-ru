---
title: "Интеграция тестирования в ASP.NET Core"
author: ardalis
description: "Инструкции по использованию интеграции ASP.NET Core, тестирование, чтобы гарантировать правильную работу компонентов приложения."
keywords: "ASP.NET Core интеграционного тестирования"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 40d534f2-89b3-4b09-9c2c-3494bf9991c9
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: d30af6edc31fbce2ebb77c57be3fd78231c54b50
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2017
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="d39d4-104">Интеграция тестирования в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d39d4-104">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="d39d4-105">По [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="d39d4-105">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="d39d4-106">Интеграционного тестирования гарантирует, что компоненты приложения работают корректно, когда собраны вместе.</span><span class="sxs-lookup"><span data-stu-id="d39d4-106">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="d39d4-107">ASP.NET Core предоставляет возможности интеграции тестирования с помощью платформы модульного тестирования и встроенных тестов веб-узел, который может использоваться для обработки запросов без нагрузки на сеть.</span><span class="sxs-lookup"><span data-stu-id="d39d4-107">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

[<span data-ttu-id="d39d4-108">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="d39d4-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample)

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="d39d4-109">Общие сведения о интеграционного тестирования</span><span class="sxs-lookup"><span data-stu-id="d39d4-109">Introduction to integration testing</span></span>

<span data-ttu-id="d39d4-110">Интеграционные тесты проверки различных частях приложения исправной работы друг с другом.</span><span class="sxs-lookup"><span data-stu-id="d39d4-110">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="d39d4-111">В отличие от [модульное тестирование](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), тесты интеграции часто включают проблемы инфраструктуры приложения, такие как базы данных, файловая система, сетевые ресурсы, или веб-запросов и ответов.</span><span class="sxs-lookup"><span data-stu-id="d39d4-111">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="d39d4-112">Модульные тесты использовать fakes или макетов объектов вместо этих проблем, но интеграционные тесты предназначен для убедитесь, что система работает должным образом с этими системами.</span><span class="sxs-lookup"><span data-stu-id="d39d4-112">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="d39d4-113">Интеграционные тесты, поскольку они охватывают больше сегменты кода, и они используют элементы инфраструктуры, как правило порядков медленнее, чем модульные тесты.</span><span class="sxs-lookup"><span data-stu-id="d39d4-113">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="d39d4-114">Таким образом рекомендуется ограничить количество тестов интеграции, можно написать, особенно в том случае, если то же самое можно протестировать с модульным тестом.</span><span class="sxs-lookup"><span data-stu-id="d39d4-114">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="d39d4-115">Если некоторое поведение можно проверить при помощи модульного теста или интеграционного теста, предпочтительно модульный тест, так как он будет почти всегда будет выполняться быстрее.</span><span class="sxs-lookup"><span data-stu-id="d39d4-115">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="d39d4-116">Может потребоваться десятки и сотни модульных тестов с помощью многих различных входных данных, но лишь небольшое число тестов интеграции, охватывающие наиболее важных сценариев.</span><span class="sxs-lookup"><span data-stu-id="d39d4-116">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="d39d4-117">Тестирование логики в свои собственные методы обычно совпадает с доменом модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="d39d4-117">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="d39d4-118">Тестирование, как приложение работает в его структуре, например с помощью ASP.NET Core или с базой данных где тесты интеграции начинают действовать.</span><span class="sxs-lookup"><span data-stu-id="d39d4-118">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="d39d4-119">Он не требует слишком много интеграции тестов, чтобы убедиться, что вы можете записать строку в базе данных и их чтения.</span><span class="sxs-lookup"><span data-stu-id="d39d4-119">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="d39d4-120">Не требуется для проверки каждого возможные перестановки вашего кода доступа к данным — необходимо протестировать достаточно, чтобы дает уверенность в том, когда приложение работает правильно.</span><span class="sxs-lookup"><span data-stu-id="d39d4-120">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="d39d4-121">Интеграция тестирования ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d39d4-121">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="d39d4-122">Настройке до выполнения интеграционных тестов, необходимо будет создать тестовый проект, добавить ссылку на веб-проекта ASP.NET Core и установить средства выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="d39d4-122">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="d39d4-123">Этот процесс описан в [модульное тестирование](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) документации, а также более подробные сведения о выполнении тестов и рекомендации по именованию тестов и тестовых классов.</span><span class="sxs-lookup"><span data-stu-id="d39d4-123">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="d39d4-124">Отдельные модульные тесты и тесты интеграции с помощью разных проектах.</span><span class="sxs-lookup"><span data-stu-id="d39d4-124">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="d39d4-125">Это гарантирует, что не случайно вводят проблем инфраструктуры, связанных в модульные тесты и позволяет легко выбирать конкретный набор тестов для запуска.</span><span class="sxs-lookup"><span data-stu-id="d39d4-125">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="d39d4-126">Узел тестирования</span><span class="sxs-lookup"><span data-stu-id="d39d4-126">The Test Host</span></span>

<span data-ttu-id="d39d4-127">ASP.NET Core включает тест узла, который можно добавить к интеграции тестовые проекты и используется для узла ASP.NET Core приложений, тестирования обслуживанием запросов без необходимости на реальных веб-узел.</span><span class="sxs-lookup"><span data-stu-id="d39d4-127">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="d39d4-128">Предоставленный образец включает проект интеграционного теста, который был настроен для использования [xUnit](https://xunit.github.io) и проверки узла.</span><span class="sxs-lookup"><span data-stu-id="d39d4-128">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="d39d4-129">Она использует `Microsoft.AspNetCore.TestHost` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="d39d4-129">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="d39d4-130">Один раз `Microsoft.AspNetCore.TestHost` пакет включен в проект, вы сможете создать и настроить `TestServer` в тестах.</span><span class="sxs-lookup"><span data-stu-id="d39d4-130">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="d39d4-131">Следующий тест показано, как проверить, что запрос к корню сайта возвращает «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="d39d4-131">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="d39d4-132">и должны выполняться успешно по умолчанию шаблон пустой веб-узел ASP.NET Core, созданные с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d39d4-132">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

<span data-ttu-id="d39d4-133">[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]</span><span class="sxs-lookup"><span data-stu-id="d39d4-133">[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]</span></span>

<span data-ttu-id="d39d4-134">Этот тест использует шаблон упорядочить Act Assert.</span><span class="sxs-lookup"><span data-stu-id="d39d4-134">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="d39d4-135">На этапе компоновки выполняется в конструктор, который создает экземпляр `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="d39d4-135">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="d39d4-136">Настроенный `WebHostBuilder` будет использоваться для создания `TestHost`; в этом примере `Configure` метода из тестируемой системы (SUT) `Startup` передается класс `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d39d4-136">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="d39d4-137">Этот метод будет использоваться для настройки конвейера запросов из `TestServer` идентично как будет настроена SUT сервера.</span><span class="sxs-lookup"><span data-stu-id="d39d4-137">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="d39d4-138">В Act части теста сделан запрос на `TestServer` экземпляр путь «/» и ответ считывается обратно в строку.</span><span class="sxs-lookup"><span data-stu-id="d39d4-138">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="d39d4-139">Эта строка сравнивается с ожидаемую строку «Hello World!».</span><span class="sxs-lookup"><span data-stu-id="d39d4-139">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="d39d4-140">Если они совпадают, тест выполняется; в противном случае происходит сбой.</span><span class="sxs-lookup"><span data-stu-id="d39d4-140">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="d39d4-141">Теперь можно добавить несколько дополнительных интеграции тестов, чтобы убедиться, что простые, функции проверки работает через веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="d39d4-141">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

<span data-ttu-id="d39d4-142">[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]</span><span class="sxs-lookup"><span data-stu-id="d39d4-142">[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]</span></span>

<span data-ttu-id="d39d4-143">Обратите внимание, который вы пытаетесь не совсем проверить правильность проверки простых чисел с помощью этих тестов, но вместо веб-приложение выполняется как ожидалось.</span><span class="sxs-lookup"><span data-stu-id="d39d4-143">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="d39d4-144">Уже имеется покрытия модульного теста, который дает уверенность в `PrimeService`, как видно из:</span><span class="sxs-lookup"><span data-stu-id="d39d4-144">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Обозреватель тестов](integration-testing/_static/test-explorer.png)

<span data-ttu-id="d39d4-146">Дополнительные сведения о модульных тестах в [модульное тестирование](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) статьи.</span><span class="sxs-lookup"><span data-stu-id="d39d4-146">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>

## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="d39d4-147">Рефакторинг для использования по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="d39d4-147">Refactoring to use middleware</span></span>

<span data-ttu-id="d39d4-148">Рефакторинг — это процесс изменения кода приложения для повышения его разработки без изменения его поведения.</span><span class="sxs-lookup"><span data-stu-id="d39d4-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="d39d4-149">Это следует делать в идеале при наличии набора передачи тесты, так как эти справки убедитесь, что поведение системы остается неизменным до и после изменения.</span><span class="sxs-lookup"><span data-stu-id="d39d4-149">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="d39d4-150">Просмотрев способом, в котором простых, логика проверки реализуется в веб-приложении `Configure` метода, вы видите:</span><span class="sxs-lookup"><span data-stu-id="d39d4-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

<span data-ttu-id="d39d4-151">Этот код работает, но это далеко от способ реализации в приложении ASP.NET Core даже простые, так как этот функциональность этого типа.</span><span class="sxs-lookup"><span data-stu-id="d39d4-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="d39d4-152">Предположим, что `Configure` метод выглядит следующим образом, при необходимости добавлять в него такой большой объем кода, каждый раз при добавлении другой URL-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="d39d4-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="d39d4-153">Добавление попробуйте [MVC](xref:mvc/overview) в приложение и создание контроллера для обработки простых проверки.</span><span class="sxs-lookup"><span data-stu-id="d39d4-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="d39d4-154">Тем не менее при условии, что в настоящее время не требуются другие MVC функциональные возможности, который немного overkill.</span><span class="sxs-lookup"><span data-stu-id="d39d4-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="d39d4-155">Тем не менее, можно воспользоваться преимуществами ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware), которое поможет нам инкапсуляции простых, логика в отдельный класс проверки и добиться лучшего [Разделение областей ответственности](http://deviq.com/separation-of-concerns/) в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="d39d4-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="d39d4-156">Вы хотите разрешить путь по промежуточного слоя использует, чтобы указать в качестве параметра, поэтому класс по промежуточного слоя ожидает `RequestDelegate` и `PrimeCheckerOptions` экземпляра в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="d39d4-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="d39d4-157">Если путь запроса не соответствует — это по промежуточного слоя настроен следует ожидать, просто вызвать следующее по промежуточного слоя в цепочке и в дальнейшем ничего.</span><span class="sxs-lookup"><span data-stu-id="d39d4-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="d39d4-158">Остальная часть код реализации, которая была `Configure` теперь `Invoke` метод.</span><span class="sxs-lookup"><span data-stu-id="d39d4-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="d39d4-159">Так как зависит от по промежуточного слоя `PrimeService` службы запрашивается экземпляра данной службы с помощью конструктора.</span><span class="sxs-lookup"><span data-stu-id="d39d4-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="d39d4-160">Платформа будет предоставлять эту службу через [внедрения зависимостей](xref:fundamentals/dependency-injection), при условии, что он был настроен, например в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d39d4-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

<span data-ttu-id="d39d4-161">[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]</span><span class="sxs-lookup"><span data-stu-id="d39d4-161">[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]</span></span>

<span data-ttu-id="d39d4-162">Так как это по промежуточного слоя выступает в качестве конечной точки в цепи делегатов запроса при соответствует пути, не используется без вызова `_next.Invoke` при этом по промежуточного слоя обрабатывает запрос.</span><span class="sxs-lookup"><span data-stu-id="d39d4-162">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="d39d4-163">С помощью этого по промежуточного слоя и некоторые полезные методы расширения для упрощения его настройки, созданные рефакторингу `Configure` метод выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d39d4-163">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

<span data-ttu-id="d39d4-164">[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]</span><span class="sxs-lookup"><span data-stu-id="d39d4-164">[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]</span></span>

<span data-ttu-id="d39d4-165">После этого рефакторинга, вы уверены, как и раньше, работает веб-приложение, поскольку тесты интеграции всех завершается успешно.</span><span class="sxs-lookup"><span data-stu-id="d39d4-165">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="d39d4-166">Рекомендуется для фиксации изменений в систему управления версиями после завершения рефакторинг и тесты успешно выполнены.</span><span class="sxs-lookup"><span data-stu-id="d39d4-166">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="d39d4-167">Если занятии краткое руководство. Разработка, [рассмотрите возможность добавления фиксации ли цикл красный-зеленый — рефакторинг](http://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="d39d4-167">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](http://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="d39d4-168">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="d39d4-168">Resources</span></span>

* [<span data-ttu-id="d39d4-169">Модульное тестирование</span><span class="sxs-lookup"><span data-stu-id="d39d4-169">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="d39d4-170">По промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="d39d4-170">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="d39d4-171">Контроллеры тестирования</span><span class="sxs-lookup"><span data-stu-id="d39d4-171">Testing controllers</span></span>](xref:mvc/controllers/testing)
