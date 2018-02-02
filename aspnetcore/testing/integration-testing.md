---
title: "Интеграция тестирования в ASP.NET Core"
author: ardalis
description: "Инструкции по использованию интеграции ASP.NET Core, тестирование, чтобы гарантировать правильную работу компонентов приложения."
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: 4a5f14e11de6ed91f67808c3ea8c78a7b1d43b03
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="9da26-103">Интеграция тестирования в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9da26-103">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="9da26-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="9da26-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="9da26-105">Интеграционного тестирования гарантирует, что компоненты приложения работают корректно, когда собраны вместе.</span><span class="sxs-lookup"><span data-stu-id="9da26-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="9da26-106">ASP.NET Core предоставляет возможности интеграции тестирования с помощью платформы модульного тестирования и встроенных тестов веб-узел, который может использоваться для обработки запросов без нагрузки на сеть.</span><span class="sxs-lookup"><span data-stu-id="9da26-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="9da26-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9da26-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="9da26-108">Общие сведения о интеграционного тестирования</span><span class="sxs-lookup"><span data-stu-id="9da26-108">Introduction to integration testing</span></span>

<span data-ttu-id="9da26-109">Интеграционные тесты проверки различных частях приложения исправной работы друг с другом.</span><span class="sxs-lookup"><span data-stu-id="9da26-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="9da26-110">В отличие от [модульное тестирование](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), тесты интеграции часто включают проблемы инфраструктуры приложения, такие как базы данных, файловая система, сетевые ресурсы, или веб-запросов и ответов.</span><span class="sxs-lookup"><span data-stu-id="9da26-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="9da26-111">Модульные тесты использовать fakes или макетов объектов вместо этих проблем, но интеграционные тесты предназначен для убедитесь, что система работает должным образом с этими системами.</span><span class="sxs-lookup"><span data-stu-id="9da26-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="9da26-112">Интеграционные тесты, поскольку они охватывают больше сегменты кода, и они используют элементы инфраструктуры, как правило порядков медленнее, чем модульные тесты.</span><span class="sxs-lookup"><span data-stu-id="9da26-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="9da26-113">Таким образом рекомендуется ограничить количество тестов интеграции, можно написать, особенно в том случае, если то же самое можно протестировать с модульным тестом.</span><span class="sxs-lookup"><span data-stu-id="9da26-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="9da26-114">Если некоторое поведение можно проверить при помощи модульного теста или интеграционного теста, предпочтительно модульный тест, так как он будет почти всегда будет выполняться быстрее.</span><span class="sxs-lookup"><span data-stu-id="9da26-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="9da26-115">Может потребоваться десятки и сотни модульных тестов с помощью многих различных входных данных, но лишь небольшое число тестов интеграции, охватывающие наиболее важных сценариев.</span><span class="sxs-lookup"><span data-stu-id="9da26-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="9da26-116">Тестирование логики в свои собственные методы обычно совпадает с доменом модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="9da26-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="9da26-117">Тестирование, как приложение работает в его структуре, например с помощью ASP.NET Core или с базой данных где тесты интеграции начинают действовать.</span><span class="sxs-lookup"><span data-stu-id="9da26-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="9da26-118">Он не требует слишком много интеграции тестов, чтобы убедиться, что вы можете записать строку в базе данных и их чтения.</span><span class="sxs-lookup"><span data-stu-id="9da26-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="9da26-119">Не требуется для проверки каждого возможные перестановки вашего кода доступа к данным — необходимо протестировать достаточно, чтобы дает уверенность в том, когда приложение работает правильно.</span><span class="sxs-lookup"><span data-stu-id="9da26-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="9da26-120">Интеграция тестирования ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9da26-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="9da26-121">Настройке до выполнения интеграционных тестов, необходимо будет создать тестовый проект, добавить ссылку на веб-проекта ASP.NET Core и установить средства выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="9da26-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="9da26-122">Этот процесс описан в [модульное тестирование](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) документации, а также более подробные сведения о выполнении тестов и рекомендации по именованию тестов и тестовых классов.</span><span class="sxs-lookup"><span data-stu-id="9da26-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="9da26-123">Отдельные модульные тесты и тесты интеграции с помощью разных проектах.</span><span class="sxs-lookup"><span data-stu-id="9da26-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="9da26-124">Это гарантирует, что не случайно вводят проблем инфраструктуры, связанных в модульные тесты и позволяет легко выбирать конкретный набор тестов для запуска.</span><span class="sxs-lookup"><span data-stu-id="9da26-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="9da26-125">Узел тестирования</span><span class="sxs-lookup"><span data-stu-id="9da26-125">The Test Host</span></span>

<span data-ttu-id="9da26-126">ASP.NET Core включает тест узла, который можно добавить к интеграции тестовые проекты и используется для узла ASP.NET Core приложений, тестирования обслуживанием запросов без необходимости на реальных веб-узел.</span><span class="sxs-lookup"><span data-stu-id="9da26-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="9da26-127">Предоставленный образец включает проект интеграционного теста, который был настроен для использования [xUnit](https://xunit.github.io) и проверки узла.</span><span class="sxs-lookup"><span data-stu-id="9da26-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="9da26-128">Она использует `Microsoft.AspNetCore.TestHost` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="9da26-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="9da26-129">Один раз `Microsoft.AspNetCore.TestHost` пакет включен в проект, вы сможете создать и настроить `TestServer` в тестах.</span><span class="sxs-lookup"><span data-stu-id="9da26-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="9da26-130">Следующий тест показано, как проверить, что запрос к корню сайта возвращает «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="9da26-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="9da26-131">и должны выполняться успешно по умолчанию шаблон пустой веб-узел ASP.NET Core, созданные с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9da26-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="9da26-132">Этот тест использует шаблон упорядочить Act Assert.</span><span class="sxs-lookup"><span data-stu-id="9da26-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="9da26-133">На этапе компоновки выполняется в конструктор, который создает экземпляр `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="9da26-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="9da26-134">Настроенный `WebHostBuilder` будет использоваться для создания `TestHost`; в этом примере `Configure` метода из тестируемой системы (SUT) `Startup` передается класс `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9da26-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="9da26-135">Этот метод будет использоваться для настройки конвейера запросов из `TestServer` идентично как будет настроена SUT сервера.</span><span class="sxs-lookup"><span data-stu-id="9da26-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="9da26-136">В Act части теста сделан запрос на `TestServer` экземпляр путь «/» и ответ считывается обратно в строку.</span><span class="sxs-lookup"><span data-stu-id="9da26-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="9da26-137">Эта строка сравнивается с ожидаемую строку «Hello World!».</span><span class="sxs-lookup"><span data-stu-id="9da26-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="9da26-138">Если они совпадают, тест выполняется; в противном случае происходит сбой.</span><span class="sxs-lookup"><span data-stu-id="9da26-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="9da26-139">Теперь можно добавить несколько дополнительных интеграции тестов, чтобы убедиться, что простые, функции проверки работает через веб-приложения:</span><span class="sxs-lookup"><span data-stu-id="9da26-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="9da26-140">Обратите внимание, который вы пытаетесь не совсем проверить правильность проверки простых чисел с помощью этих тестов, но вместо веб-приложение выполняется как ожидалось.</span><span class="sxs-lookup"><span data-stu-id="9da26-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="9da26-141">Уже имеется покрытия модульного теста, который дает уверенность в `PrimeService`, как видно из:</span><span class="sxs-lookup"><span data-stu-id="9da26-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Обозреватель тестов](integration-testing/_static/test-explorer.png)

<span data-ttu-id="9da26-143">Дополнительные сведения о модульных тестах в [модульное тестирование](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) статьи.</span><span class="sxs-lookup"><span data-stu-id="9da26-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="9da26-144">Тестирование Mvc-Razor интеграции</span><span class="sxs-lookup"><span data-stu-id="9da26-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="9da26-145">Тестовые проекты, содержащие представления Razor требуют `<PreserveCompilationContext>` присвоено значение true в *.csproj* файла:</span><span class="sxs-lookup"><span data-stu-id="9da26-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="9da26-146">Проекты, этот элемент отсутствует, приведет к ошибке следующего вида:</span><span class="sxs-lookup"><span data-stu-id="9da26-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="9da26-147">Рефакторинг для использования по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="9da26-147">Refactoring to use middleware</span></span>

<span data-ttu-id="9da26-148">Рефакторинг — это процесс изменения кода приложения для повышения его разработки без изменения его поведения.</span><span class="sxs-lookup"><span data-stu-id="9da26-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="9da26-149">Это следует делать в идеале при наличии набора передачи тесты, так как эти справки убедитесь, что поведение системы остается неизменным до и после изменения.</span><span class="sxs-lookup"><span data-stu-id="9da26-149">It should ideally be done when there's a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="9da26-150">Просмотрев способом, в котором простых, логика проверки реализуется в веб-приложении `Configure` метода, вы видите:</span><span class="sxs-lookup"><span data-stu-id="9da26-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

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

<span data-ttu-id="9da26-151">Этот код работает, но это далеко от способ реализации в приложении ASP.NET Core даже простые, так как этот функциональность этого типа.</span><span class="sxs-lookup"><span data-stu-id="9da26-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="9da26-152">Предположим, что `Configure` метод выглядит следующим образом, при необходимости добавлять в него такой большой объем кода, каждый раз при добавлении другой URL-адрес конечной точки.</span><span class="sxs-lookup"><span data-stu-id="9da26-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="9da26-153">Добавление попробуйте [MVC](xref:mvc/overview) в приложение и создание контроллера для обработки простых проверки.</span><span class="sxs-lookup"><span data-stu-id="9da26-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="9da26-154">Тем не менее при условии, что в настоящее время не требуются другие MVC функциональные возможности, который немного overkill.</span><span class="sxs-lookup"><span data-stu-id="9da26-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="9da26-155">Тем не менее, можно воспользоваться преимуществами ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index), которое поможет нам инкапсуляции простых, логика в отдельный класс проверки и добиться лучшего [Разделение областей ответственности](http://deviq.com/separation-of-concerns/) в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="9da26-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware/index), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="9da26-156">Вы хотите разрешить путь по промежуточного слоя использует, чтобы указать в качестве параметра, поэтому класс по промежуточного слоя ожидает `RequestDelegate` и `PrimeCheckerOptions` экземпляра в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="9da26-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="9da26-157">Если путь запроса не соответствует — это по промежуточного слоя настроен следует ожидать, просто вызвать следующее по промежуточного слоя в цепочке и в дальнейшем ничего.</span><span class="sxs-lookup"><span data-stu-id="9da26-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="9da26-158">Остальная часть код реализации, которая была `Configure` теперь `Invoke` метод.</span><span class="sxs-lookup"><span data-stu-id="9da26-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="9da26-159">Так как зависит от по промежуточного слоя `PrimeService` службы запрашивается экземпляра данной службы с помощью конструктора.</span><span class="sxs-lookup"><span data-stu-id="9da26-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="9da26-160">Платформа будет предоставлять эту службу через [внедрения зависимостей](xref:fundamentals/dependency-injection), при условии, что он был настроен, например в `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9da26-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="9da26-161">Так как это по промежуточного слоя выступает в качестве конечной точки в цепи делегатов запроса при соответствует пути, не используется без вызова `_next.Invoke` при этом по промежуточного слоя обрабатывает запрос.</span><span class="sxs-lookup"><span data-stu-id="9da26-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there's no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="9da26-162">С помощью этого по промежуточного слоя и некоторые полезные методы расширения для упрощения его настройки, созданные рефакторингу `Configure` метод выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9da26-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="9da26-163">После этого рефакторинга, вы уверены, как и раньше, работает веб-приложение, поскольку тесты интеграции всех завершается успешно.</span><span class="sxs-lookup"><span data-stu-id="9da26-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="9da26-164">Рекомендуется для фиксации изменений в систему управления версиями после завершения рефакторинг и тесты успешно выполнены.</span><span class="sxs-lookup"><span data-stu-id="9da26-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="9da26-165">Если занятии краткое руководство. Разработка, [рассмотрите возможность добавления фиксации ли цикл красный-зеленый — рефакторинг](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="9da26-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="9da26-166">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="9da26-166">Resources</span></span>

* [<span data-ttu-id="9da26-167">Модульное тестирование</span><span class="sxs-lookup"><span data-stu-id="9da26-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="9da26-168">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="9da26-168">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="9da26-169">Контроллеры тестирования</span><span class="sxs-lookup"><span data-stu-id="9da26-169">Testing controllers</span></span>](xref:mvc/controllers/testing)
