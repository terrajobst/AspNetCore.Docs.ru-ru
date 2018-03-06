---
title: "ПО промежуточного слоя ASP.NET Core"
author: rick-anderson
description: "Сведения о ПО промежуточного слоя ASP.NET Core и конвейере запросов."
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 5d236c79120d79195c1970cc87d164002b56d0f1
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="74672-103">ПО промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="74672-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="74672-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="74672-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="74672-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="74672-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="74672-106">Что такое ПО промежуточного слоя?</span><span class="sxs-lookup"><span data-stu-id="74672-106">What is middleware?</span></span>

<span data-ttu-id="74672-107">ПО промежуточного слоя — это программное обеспечение, выстраиваемое в виде конвейера приложения ASP.NET для обработки запросов и откликов.</span><span class="sxs-lookup"><span data-stu-id="74672-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="74672-108">Каждый компонент:</span><span class="sxs-lookup"><span data-stu-id="74672-108">Each component:</span></span>

* <span data-ttu-id="74672-109">определяет, нужно ли передать запрос следующему компоненту в конвейере;</span><span class="sxs-lookup"><span data-stu-id="74672-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="74672-110">может выполнять работу как до, так и после вызова следующего компонента в конвейере.</span><span class="sxs-lookup"><span data-stu-id="74672-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="74672-111">Для построения конвейера запросов используются делегаты запроса.</span><span class="sxs-lookup"><span data-stu-id="74672-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="74672-112">Они обрабатывают каждый HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="74672-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="74672-113">Для их настройки служат методы расширения [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) и [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="74672-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="74672-114">Отдельный делегат запроса можно указать встроенным в качестве анонимного метода (называемого встроенным ПО промежуточного слоя) либо определить в многоразовом классе.</span><span class="sxs-lookup"><span data-stu-id="74672-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="74672-115">Эти многоразовые классы и встроенные анонимные методы являются *ПО промежуточного слоя* или *компонентами промежуточного слоя*.</span><span class="sxs-lookup"><span data-stu-id="74672-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="74672-116">Каждый компонент промежуточного слоя представляет собой конвейер запросов, отвечающий за вызов следующего компонента в конвейере и замыкающий цепочку, если это необходимо.</span><span class="sxs-lookup"><span data-stu-id="74672-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="74672-117">Статья о [миграции модулей HTTP на ПО промежуточного слоя](xref:migration/http-modules) поясняет различия между конвейерами запросов в ASP.NET Core и ASP.NET 4.x, а также содержит дополнительные примеры ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="74672-117">[Migrating HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="74672-118">Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="74672-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="74672-119">Конвейер запросов ASP.NET Core состоит из последовательности делегатов запроса, вызываемых один за другим, как показано на этой схеме (поток выполнения обозначен черными стрелками).</span><span class="sxs-lookup"><span data-stu-id="74672-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![Шаблон обработки запросов, показывающий входящий запрос, обработку в трех компонентах промежуточного слоя и запрос, покидающий приложение.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="74672-123">Каждый из делегатов может выполнять операции до и после следующего делегата.</span><span class="sxs-lookup"><span data-stu-id="74672-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="74672-124">Делегат также может принять решение не передавать запрос следующему делегату, что называется замыканием конвейера запросов.</span><span class="sxs-lookup"><span data-stu-id="74672-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="74672-125">Замыкание часто является предпочтительным, так как позволяет избежать ненужной работы.</span><span class="sxs-lookup"><span data-stu-id="74672-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="74672-126">Например, компонент промежуточного слоя для статических файлов может возвратить запрос статического файла и выполнить замыкание, чтобы обойти оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="74672-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="74672-127">Делегаты обработки исключений должны вызываться в начале конвейера, чтобы они могли перехватывать исключения, возникающие в более поздних стадиях.</span><span class="sxs-lookup"><span data-stu-id="74672-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="74672-128">Простейшее приложение ASP.NET Core задает один делегат запроса, обрабатывающий все запросы.</span><span class="sxs-lookup"><span data-stu-id="74672-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="74672-129">В этом случае конвейер запросов как таковой отсутствует.</span><span class="sxs-lookup"><span data-stu-id="74672-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="74672-130">Вместо этого в ответ на каждый HTTP-запрос вызывается одна анонимная функция.</span><span class="sxs-lookup"><span data-stu-id="74672-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="74672-131">Первый делегат [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) завершает конвейер.</span><span class="sxs-lookup"><span data-stu-id="74672-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="74672-132">Несколько делегатов запроса можно соединить в цепочку с помощью [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span><span class="sxs-lookup"><span data-stu-id="74672-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="74672-133">Параметр `next` представляет следующий делегат в конвейере.</span><span class="sxs-lookup"><span data-stu-id="74672-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="74672-134">(Помните, что замкнуть конвейер, *не* вызывая параметр *next*, невозможно.) Обычно действия можно выполнять как до, так и после следующего делегата, как показано в этом примере.</span><span class="sxs-lookup"><span data-stu-id="74672-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="74672-135">Не вызывайте `next.Invoke` после отправки отклика клиенту.</span><span class="sxs-lookup"><span data-stu-id="74672-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="74672-136">Изменения `HttpResponse` после запуска отклика приведут к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="74672-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="74672-137">Например, таким изменением может быть задание заголовков, кода состояние и т. п.</span><span class="sxs-lookup"><span data-stu-id="74672-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="74672-138">Запись в тело отклика после вызова `next`:</span><span class="sxs-lookup"><span data-stu-id="74672-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="74672-139">может вызвать нарушение протокола,</span><span class="sxs-lookup"><span data-stu-id="74672-139">May cause a protocol violation.</span></span> <span data-ttu-id="74672-140">например, при записи больше указанного значения `content-length`;</span><span class="sxs-lookup"><span data-stu-id="74672-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="74672-141">может привести к нарушению формата,</span><span class="sxs-lookup"><span data-stu-id="74672-141">May corrupt the body format.</span></span> <span data-ttu-id="74672-142">например, при записи нижнего колонтитула HTML в CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="74672-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="74672-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) удобное использовать для обозначения того, были ли отправлены заголовки и (или) выполнена запись в тело отклика.</span><span class="sxs-lookup"><span data-stu-id="74672-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="74672-144">Упорядочение</span><span class="sxs-lookup"><span data-stu-id="74672-144">Ordering</span></span>

<span data-ttu-id="74672-145">Порядок, в котором компоненты промежуточного слоя добавляются в метод `Configure`, определяет порядок их вызова при запросах и обратный порядок для отклика.</span><span class="sxs-lookup"><span data-stu-id="74672-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="74672-146">Соблюдение этого порядка крайне важно для обеспечения безопасности, производительности и функциональности.</span><span class="sxs-lookup"><span data-stu-id="74672-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="74672-147">Метод Configure (см. ниже) добавляет следующие компоненты промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="74672-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="74672-148">Обработка исключений/ошибок</span><span class="sxs-lookup"><span data-stu-id="74672-148">Exception/error handling</span></span>
2. <span data-ttu-id="74672-149">Статический файловый сервер</span><span class="sxs-lookup"><span data-stu-id="74672-149">Static file server</span></span>
3. <span data-ttu-id="74672-150">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="74672-150">Authentication</span></span>
4. <span data-ttu-id="74672-151">MVC</span><span class="sxs-lookup"><span data-stu-id="74672-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="74672-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="74672-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="74672-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="74672-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

<span data-ttu-id="74672-154">В представленном выше коде `UseExceptionHandler` — это первый компонент промежуточного слоя, добавленный в конвейер, поэтому он перехватывает все исключения, возникающие в последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="74672-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="74672-155">Компонент промежуточного слоя для статических файлов вызывается на раннем этапе конвейера, чтобы он мог обработать запросы и выполнить замыкание, минуя остальные компоненты.</span><span class="sxs-lookup"><span data-stu-id="74672-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="74672-156">Этот компонент **не** выполняет проверки авторизации.</span><span class="sxs-lookup"><span data-stu-id="74672-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="74672-157">Все обрабатываемые им файлы, включая расположенные в *wwwroot*, находятся в открытом доступе.</span><span class="sxs-lookup"><span data-stu-id="74672-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="74672-158">Сведения о защите статических файлов см. в статье [Работа со статическими файлами](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="74672-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="74672-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="74672-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="74672-160">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (`app.UseAuthentication`), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="74672-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="74672-161">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="74672-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="74672-162">Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанную страницу Razor Page или указанный контроллер и действие.</span><span class="sxs-lookup"><span data-stu-id="74672-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="74672-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="74672-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="74672-164">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (`app.UseIdentity`), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="74672-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="74672-165">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="74672-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="74672-166">Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанный контроллер и действие.</span><span class="sxs-lookup"><span data-stu-id="74672-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="74672-167">Следующий пример показывает порядок компонентов промежуточного слоя, где запросы для статических файлов обрабатываются компонентом для статических файлов до компонента для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="74672-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="74672-168">При таком порядке сжатие статических файлов не выполняется.</span><span class="sxs-lookup"><span data-stu-id="74672-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="74672-169">Отклики MVC из [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) могут сжиматься.</span><span class="sxs-lookup"><span data-stu-id="74672-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="74672-170">Методы Use, Run и Map</span><span class="sxs-lookup"><span data-stu-id="74672-170">Use, Run, and Map</span></span>

<span data-ttu-id="74672-171">Для настройки конвейера HTTP служат методы `Use`, `Run` и `Map`.</span><span class="sxs-lookup"><span data-stu-id="74672-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="74672-172">Метод `Use` может замыкать конвейер (это происходит, если он не вызывает делегат запроса `next`).</span><span class="sxs-lookup"><span data-stu-id="74672-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="74672-173">`Run` является соглашением, и некоторые компоненты промежуточного слоя могут предоставлять методы `Run[Middleware]`, которые выполняются в конце конвейера.</span><span class="sxs-lookup"><span data-stu-id="74672-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="74672-174">Расширения `Map*` используются в качестве соглашения для ветвления конвейера.</span><span class="sxs-lookup"><span data-stu-id="74672-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="74672-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) осуществляет ветвление конвейера запросов на основе совпадений для заданного пути запроса.</span><span class="sxs-lookup"><span data-stu-id="74672-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="74672-176">Если путь запроса начинается с заданного пути, данная ветвь выполняется.</span><span class="sxs-lookup"><span data-stu-id="74672-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="74672-177">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="74672-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="74672-178">Запрос</span><span class="sxs-lookup"><span data-stu-id="74672-178">Request</span></span> | <span data-ttu-id="74672-179">Ответ</span><span class="sxs-lookup"><span data-stu-id="74672-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="74672-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="74672-180">localhost:1234</span></span> | <span data-ttu-id="74672-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="74672-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="74672-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="74672-182">localhost:1234/map1</span></span> | <span data-ttu-id="74672-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="74672-183">Map Test 1</span></span> |
| <span data-ttu-id="74672-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="74672-184">localhost:1234/map2</span></span> | <span data-ttu-id="74672-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="74672-185">Map Test 2</span></span> |
| <span data-ttu-id="74672-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="74672-186">localhost:1234/map3</span></span> | <span data-ttu-id="74672-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="74672-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="74672-188">Когда используется `Map`, соответствующие сегменты путей удаляются из `HttpRequest.Path` и добавляются к `HttpRequest.PathBase` для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="74672-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="74672-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) осуществляет ветвление конвейера запросов на основе результата заданного предиката.</span><span class="sxs-lookup"><span data-stu-id="74672-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="74672-190">Любой предикат типа `Func<HttpContext, bool>` можно использовать для сопоставления запросов с новой ветвью конвейера.</span><span class="sxs-lookup"><span data-stu-id="74672-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="74672-191">В следующем примере предикат служит для определения наличия переменной строки запроса `branch`.</span><span class="sxs-lookup"><span data-stu-id="74672-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="74672-192">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="74672-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="74672-193">Запрос</span><span class="sxs-lookup"><span data-stu-id="74672-193">Request</span></span> | <span data-ttu-id="74672-194">Ответ</span><span class="sxs-lookup"><span data-stu-id="74672-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="74672-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="74672-195">localhost:1234</span></span> | <span data-ttu-id="74672-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="74672-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="74672-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="74672-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="74672-198">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="74672-198">Branch used = master</span></span>|

<span data-ttu-id="74672-199">`Map` поддерживает вложение, например:</span><span class="sxs-lookup"><span data-stu-id="74672-199">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="74672-200">`Map` также может сопоставить несколько сегментов одновременно, например:</span><span class="sxs-lookup"><span data-stu-id="74672-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="74672-201">Встроенное ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="74672-201">Built-in middleware</span></span>

<span data-ttu-id="74672-202">ASP.NET Core содержит следующие компоненты промежуточного слоя, а также описание порядка их добавления.</span><span class="sxs-lookup"><span data-stu-id="74672-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="74672-203">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="74672-203">Middleware</span></span> | <span data-ttu-id="74672-204">Описание:</span><span class="sxs-lookup"><span data-stu-id="74672-204">Description</span></span> | <span data-ttu-id="74672-205">Номер</span><span class="sxs-lookup"><span data-stu-id="74672-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="74672-206">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="74672-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="74672-207">Обеспечивает поддержку проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="74672-207">Provides authentication support.</span></span> | <span data-ttu-id="74672-208">Ставится перед тем, как потребуется `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="74672-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="74672-209">Является конечным для обратных вызовов OAuth.</span><span class="sxs-lookup"><span data-stu-id="74672-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="74672-210">CORS</span><span class="sxs-lookup"><span data-stu-id="74672-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="74672-211">Настраивает общий доступ к ресурсам независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="74672-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="74672-212">Ставится перед компонентами, использующими CORS.</span><span class="sxs-lookup"><span data-stu-id="74672-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="74672-213">Диагностика</span><span class="sxs-lookup"><span data-stu-id="74672-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="74672-214">Настраивает диагностику.</span><span class="sxs-lookup"><span data-stu-id="74672-214">Configures diagnostics.</span></span> | <span data-ttu-id="74672-215">Ставится перед компонентами, выдающими ошибки.</span><span class="sxs-lookup"><span data-stu-id="74672-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="74672-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="74672-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="74672-217">Пересылает заголовки, переданные через прокси-сервер, в текущий запрос.</span><span class="sxs-lookup"><span data-stu-id="74672-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="74672-218">Ставится перед компонентами, использующими обновленные поля (например: Scheme, Host, ClientIP, Method).</span><span class="sxs-lookup"><span data-stu-id="74672-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="74672-219">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="74672-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="74672-220">Обеспечивает поддержку для кэширования откликов.</span><span class="sxs-lookup"><span data-stu-id="74672-220">Provides support for caching responses.</span></span> | <span data-ttu-id="74672-221">Ставится перед компонентами, требующими кэширование.</span><span class="sxs-lookup"><span data-stu-id="74672-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="74672-222">Сжатие откликов</span><span class="sxs-lookup"><span data-stu-id="74672-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="74672-223">Обеспечивает поддержку для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="74672-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="74672-224">Ставится перед компонентами, требующими сжатие.</span><span class="sxs-lookup"><span data-stu-id="74672-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="74672-225">Локализация запросов</span><span class="sxs-lookup"><span data-stu-id="74672-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="74672-226">Обеспечивает поддержку локализации.</span><span class="sxs-lookup"><span data-stu-id="74672-226">Provides localization support.</span></span> | <span data-ttu-id="74672-227">Ставится перед компонентами, для которых важна локализация.</span><span class="sxs-lookup"><span data-stu-id="74672-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="74672-228">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="74672-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="74672-229">Определяет и ограничивает маршруты запросов.</span><span class="sxs-lookup"><span data-stu-id="74672-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="74672-230">Является конечным для совпадающих маршрутов.</span><span class="sxs-lookup"><span data-stu-id="74672-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="74672-231">Сеанс</span><span class="sxs-lookup"><span data-stu-id="74672-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="74672-232">Обеспечивает поддержку для управления пользовательскими сеансами.</span><span class="sxs-lookup"><span data-stu-id="74672-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="74672-233">Ставится перед компонентами, требующими сеанс.</span><span class="sxs-lookup"><span data-stu-id="74672-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="74672-234">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="74672-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="74672-235">Обеспечивает поддержку для обработки статических файлов и просмотра каталогов.</span><span class="sxs-lookup"><span data-stu-id="74672-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="74672-236">Является конечным, если запрос соответствует файлам.</span><span class="sxs-lookup"><span data-stu-id="74672-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="74672-237">Переопределение адреса URL</span><span class="sxs-lookup"><span data-stu-id="74672-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="74672-238">Обеспечивает поддержку для переопределения URL-адресов и перенаправления запросов.</span><span class="sxs-lookup"><span data-stu-id="74672-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="74672-239">Ставится перед компонентами, использующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="74672-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="74672-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="74672-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="74672-241">Обеспечивает поддержку протокола WebSockets.</span><span class="sxs-lookup"><span data-stu-id="74672-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="74672-242">Ставится перед компонентами, которым нужно принимать запросы WebSocket.</span><span class="sxs-lookup"><span data-stu-id="74672-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="74672-243">Написание ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="74672-243">Writing middleware</span></span>

<span data-ttu-id="74672-244">ПО промежуточного слоя обычно инкапсулируется в класс и предоставляется с помощью метода расширения.</span><span class="sxs-lookup"><span data-stu-id="74672-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="74672-245">Рассмотрим следующее ПО промежуточного слоя, которое задает язык и региональные параметры для текущего запроса из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="74672-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="74672-246">Примечание. Приведенный выше пример кода демонстрирует создание компонента промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="74672-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="74672-247">Сведения о встроенной поддержке локализации ASP.NET Core см. в разделе [ Глобализация и локализация](xref:fundamentals/localization).</span><span class="sxs-lookup"><span data-stu-id="74672-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="74672-248">Это ПО промежуточного слоя можно проверить, передав язык и региональные параметры, например `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="74672-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="74672-249">Следующий код перемещает делегат ПО промежуточного слоя в класс.</span><span class="sxs-lookup"><span data-stu-id="74672-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="74672-250">Следующий метод расширения предоставляет ПО промежуточного слоя посредством [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="74672-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="74672-251">Следующий код вызывает ПО промежуточного слоя из `Configure`.</span><span class="sxs-lookup"><span data-stu-id="74672-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="74672-252">ПО промежуточного слоя должно соответствовать [принципу явных зависимостей](http://deviq.com/explicit-dependencies-principle/), предоставляя свои зависимости в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="74672-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="74672-253">ПО промежуточного слоя создается один раз за *время существования приложения*.</span><span class="sxs-lookup"><span data-stu-id="74672-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="74672-254">В разделе *Зависимости отдельных запросов* ниже приведены сведения о том, как использовать службы совместно с ПО промежуточного слоя внутри запроса.</span><span class="sxs-lookup"><span data-stu-id="74672-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="74672-255">Компоненты промежуточного слоя могут разрешать свои зависимости, возникшие в результате введения зависимостей, за счет параметров конструктора.</span><span class="sxs-lookup"><span data-stu-id="74672-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="74672-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) также может принимать дополнительные параметры напрямую.</span><span class="sxs-lookup"><span data-stu-id="74672-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="74672-257">Зависимости отдельных запросов</span><span class="sxs-lookup"><span data-stu-id="74672-257">Per-request dependencies</span></span>

<span data-ttu-id="74672-258">Так как ПО промежуточного слоя создается при запуске приложения, а не для отдельных запросов, службы времени существования *scoped*, используемые конструкторами ПО промежуточного слоя, не являются общими с другими типами, возникшими в результате введения зависимостей, в каждом из запросов.</span><span class="sxs-lookup"><span data-stu-id="74672-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="74672-259">Если необходимо предоставить службу *scoped* для совместного использования ПО промежуточного слоя и другими типами, добавьте ее в сигнатуру метода `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="74672-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="74672-260">Метод `Invoke` может принимать дополнительные параметры, заполняемые при введении зависимостей.</span><span class="sxs-lookup"><span data-stu-id="74672-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="74672-261">Пример:</span><span class="sxs-lookup"><span data-stu-id="74672-261">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="74672-262">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="74672-262">Additional resources</span></span>

* [<span data-ttu-id="74672-263">Миграция с модулей HTTP на ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="74672-263">Migrating HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="74672-264">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="74672-264">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="74672-265">Параметры запроса</span><span class="sxs-lookup"><span data-stu-id="74672-265">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="74672-266">Активация фабричного ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="74672-266">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="74672-267">Активация ПО промежуточного слоя со сторонним контейнером</span><span class="sxs-lookup"><span data-stu-id="74672-267">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
