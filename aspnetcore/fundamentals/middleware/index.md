---
title: ПО промежуточного слоя ASP.NET Core
author: rick-anderson
description: Сведения о ПО промежуточного слоя ASP.NET Core и конвейере запросов.
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 9ba77561ab4f6a8668c480d6e81f2ce7e0193c73
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870951"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="a5fcb-103">ПО промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a5fcb-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="a5fcb-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="a5fcb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a5fcb-105">ПО промежуточного слоя — это программное обеспечение, выстраиваемое в виде конвейера приложения для обработки запросов и откликов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="a5fcb-106">Каждый компонент:</span><span class="sxs-lookup"><span data-stu-id="a5fcb-106">Each component:</span></span>

* <span data-ttu-id="a5fcb-107">определяет, нужно ли передать запрос следующему компоненту в конвейере;</span><span class="sxs-lookup"><span data-stu-id="a5fcb-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="a5fcb-108">может выполнять работу как до, так и после вызова следующего компонента в конвейере.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="a5fcb-109">Для построения конвейера запросов используются делегаты запроса.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="a5fcb-110">Они обрабатывают каждый HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="a5fcb-111">Для их настройки служат методы расширения <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> и <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="a5fcb-112">Отдельный делегат запроса можно указать встроенным в качестве анонимного метода (называемого встроенным ПО промежуточного слоя) либо определить в многоразовом классе.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="a5fcb-113">Эти многоразовые классы и встроенные анонимные методы являются *ПО промежуточного слоя* или *компонентами промежуточного слоя*.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="a5fcb-114">Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает конвейер.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="a5fcb-115">Статья <xref:migration/http-modules> поясняет различия между конвейерами запросов в ASP.NET Core и ASP.NET 4.x, а также содержит дополнительные примеры ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="a5fcb-116">Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="a5fcb-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="a5fcb-117">Конвейер запросов ASP.NET Core состоит из последовательности делегатов запроса, вызываемых один за другим.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="a5fcb-118">На следующей схеме демонстрируется этот принцип.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="a5fcb-119">Поток выполнения показан черными стрелками.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-119">The thread of execution follows the black arrows.</span></span>

![Шаблон обработки запросов, показывающий входящий запрос, обработку в трех компонентах промежуточного слоя и запрос, покидающий приложение.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="a5fcb-123">Каждый из делегатов может выполнять операции до и после следующего делегата.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="a5fcb-124">Делегат также может принять решение не передавать запрос следующему делегату, что называется *замыканием конвейера запросов*.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="a5fcb-125">Замыкание часто является предпочтительным, так как позволяет избежать ненужной работы.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="a5fcb-126">Например, компонент промежуточного слоя для статических файлов может возвратить запрос статического файла и выполнить замыкание, чтобы обойти оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="a5fcb-127">Делегаты обработки исключений вызываются в начале конвейера, чтобы они могли перехватывать исключения, возникающие в более поздних стадиях.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="a5fcb-128">Простейшее приложение ASP.NET Core задает один делегат запроса, обрабатывающий все запросы.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="a5fcb-129">В этом случае конвейер запросов как таковой отсутствует.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="a5fcb-130">Вместо этого в ответ на каждый HTTP-запрос вызывается одна анонимная функция.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="a5fcb-131">Первый делегат <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> завершает конвейер.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="a5fcb-132">Несколько делегатов запроса можно соединить в цепочку с помощью <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="a5fcb-133">Параметр `next` представляет следующий делегат в конвейере.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="a5fcb-134">Замыкать конвейер можно*не* вызывая параметр *next*.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="a5fcb-135">Обычно действия можно выполнять как до, так и после следующего делегата, как показано в этом примере.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="a5fcb-136">Не вызывайте `next.Invoke` после отправки отклика клиенту.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="a5fcb-137">Изменения <xref:Microsoft.AspNetCore.Http.HttpResponse> после запуска отклика приведут к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="a5fcb-138">Например, таким изменением может быть задание заголовков и кода состояния.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="a5fcb-139">Запись в тело отклика после вызова `next`:</span><span class="sxs-lookup"><span data-stu-id="a5fcb-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="a5fcb-140">может вызвать нарушение протокола,</span><span class="sxs-lookup"><span data-stu-id="a5fcb-140">May cause a protocol violation.</span></span> <span data-ttu-id="a5fcb-141">например, при записи больше указанного значения `Content-Length`;</span><span class="sxs-lookup"><span data-stu-id="a5fcb-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="a5fcb-142">может привести к нарушению формата,</span><span class="sxs-lookup"><span data-stu-id="a5fcb-142">May corrupt the body format.</span></span> <span data-ttu-id="a5fcb-143">например, при записи нижнего колонтитула HTML в CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="a5fcb-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> удобно использовать для обозначения того, были ли отправлены заголовки или выполнена запись в тело отклика.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="a5fcb-145">Номер</span><span class="sxs-lookup"><span data-stu-id="a5fcb-145">Order</span></span>

<span data-ttu-id="a5fcb-146">Порядок, в котором компоненты промежуточного слоя добавляются в метод `Startup.Configure`, определяет порядок их вызова при запросах и обратный порядок для отклика.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="a5fcb-147">Соблюдение этого порядка крайне важно для обеспечения безопасности, производительности и функциональности.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="a5fcb-148">Следующий метод `Configure` добавляет следующие компоненты промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="a5fcb-149">Обработка исключений/ошибок</span><span class="sxs-lookup"><span data-stu-id="a5fcb-149">Exception/error handling</span></span>
2. <span data-ttu-id="a5fcb-150">Статический файловый сервер</span><span class="sxs-lookup"><span data-stu-id="a5fcb-150">Static file server</span></span>
3. <span data-ttu-id="a5fcb-151">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="a5fcb-151">Authentication</span></span>
4. <span data-ttu-id="a5fcb-152">MVC</span><span class="sxs-lookup"><span data-stu-id="a5fcb-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="a5fcb-153">В приведенном выше коде <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> — это первый компонент промежуточного слоя, добавленный в конвейер.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-153">In the code above, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="a5fcb-154">Таким образом, обработчик исключений ПО промежуточного слоя перехватывает все исключения, возникающие в последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-154">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="a5fcb-155">Компонент промежуточного слоя для статических файлов вызывается на раннем этапе конвейера, чтобы он мог обработать запросы и выполнить замыкание, минуя остальные компоненты.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-155">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="a5fcb-156">Этот компонент **не** выполняет проверки авторизации.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-156">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="a5fcb-157">Все обрабатываемые им файлы, включая расположенные в *wwwroot*, находятся в открытом доступе.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="a5fcb-158">Сведения о защите статических файлов см. в статье <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-158">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a5fcb-159">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для проверки подлинности (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-159">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="a5fcb-160">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-160">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a5fcb-161">Хотя ПО промежуточного слоя для проверки подлинности проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанную страницу Razor Page или контроллер MVC и действие.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-161">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a5fcb-162">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-162">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="a5fcb-163">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-163">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a5fcb-164">Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанный контроллер и действие.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-164">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="a5fcb-165">Следующий пример показывает порядок компонентов промежуточного слоя, где запросы для статических файлов обрабатываются компонентом для статических файлов до компонента для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-165">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="a5fcb-166">Статические файлы не сжимаются с этим порядком ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-166">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="a5fcb-167">Отклики MVC из <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> можно сжать.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-167">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="a5fcb-168">Методы Use, Run и Map</span><span class="sxs-lookup"><span data-stu-id="a5fcb-168">Use, Run, and Map</span></span>

<span data-ttu-id="a5fcb-169">Для настройки конвейера HTTP служат методы `Use`, `Run` и `Map`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-169">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="a5fcb-170">Метод `Use` может замыкать конвейер (это происходит, если он не вызывает делегат запроса `next`).</span><span class="sxs-lookup"><span data-stu-id="a5fcb-170">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="a5fcb-171">`Run` является соглашением, и некоторые компоненты промежуточного слоя могут предоставлять методы `Run[Middleware]`, которые выполняются в конце конвейера.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-171">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="a5fcb-172">Расширения <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> используются в качестве соглашения для ветвления конвейера.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-172"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="a5fcb-173">`Map*` осуществляет ветвление конвейера запросов на основе совпадений для заданного пути запроса.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-173">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="a5fcb-174">Если путь запроса начинается с заданного пути, данная ветвь выполняется.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-174">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="a5fcb-175">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-175">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="a5fcb-176">Запрос</span><span class="sxs-lookup"><span data-stu-id="a5fcb-176">Request</span></span>             | <span data-ttu-id="a5fcb-177">Ответ</span><span class="sxs-lookup"><span data-stu-id="a5fcb-177">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="a5fcb-178">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a5fcb-178">localhost:1234</span></span>      | <span data-ttu-id="a5fcb-179">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-179">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="a5fcb-180">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="a5fcb-180">localhost:1234/map1</span></span> | <span data-ttu-id="a5fcb-181">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="a5fcb-181">Map Test 1</span></span>                   |
| <span data-ttu-id="a5fcb-182">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="a5fcb-182">localhost:1234/map2</span></span> | <span data-ttu-id="a5fcb-183">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="a5fcb-183">Map Test 2</span></span>                   |
| <span data-ttu-id="a5fcb-184">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="a5fcb-184">localhost:1234/map3</span></span> | <span data-ttu-id="a5fcb-185">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-185">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="a5fcb-186">Когда используется `Map`, соответствующие сегменты путей удаляются из `HttpRequest.Path` и добавляются к `HttpRequest.PathBase` для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-186">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="a5fcb-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) осуществляет ветвление конвейера запросов на основе результата заданного предиката.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-187">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="a5fcb-188">Любой предикат типа `Func<HttpContext, bool>` можно использовать для сопоставления запросов с новой ветвью конвейера.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-188">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="a5fcb-189">В следующем примере предикат служит для определения наличия переменной строки запроса `branch`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-189">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="a5fcb-190">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-190">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="a5fcb-191">Запрос</span><span class="sxs-lookup"><span data-stu-id="a5fcb-191">Request</span></span>                       | <span data-ttu-id="a5fcb-192">Ответ</span><span class="sxs-lookup"><span data-stu-id="a5fcb-192">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="a5fcb-193">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a5fcb-193">localhost:1234</span></span>                | <span data-ttu-id="a5fcb-194">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-194">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="a5fcb-195">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="a5fcb-195">localhost:1234/?branch=master</span></span> | <span data-ttu-id="a5fcb-196">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="a5fcb-196">Branch used = master</span></span>         |

<span data-ttu-id="a5fcb-197">`Map` поддерживает вложение, например:</span><span class="sxs-lookup"><span data-stu-id="a5fcb-197">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="a5fcb-198">`Map` также может сопоставить несколько сегментов одновременно:</span><span class="sxs-lookup"><span data-stu-id="a5fcb-198">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="a5fcb-199">Встроенное ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="a5fcb-199">Built-in middleware</span></span>

<span data-ttu-id="a5fcb-200">ASP.NET Core содержит следующие компоненты промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-200">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="a5fcb-201">В столбце *Порядок* указаны сведения о размещении ПО промежуточного слоя в конвейере запросов и условия, в соответствии с которыми ПО промежуточного слоя может прервать запрос и предотвратить обработку запроса в других ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-201">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="a5fcb-202">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="a5fcb-202">Middleware</span></span> | <span data-ttu-id="a5fcb-203">Описание:</span><span class="sxs-lookup"><span data-stu-id="a5fcb-203">Description</span></span> | <span data-ttu-id="a5fcb-204">Номер</span><span class="sxs-lookup"><span data-stu-id="a5fcb-204">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="a5fcb-205">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="a5fcb-205">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="a5fcb-206">Обеспечивает поддержку проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-206">Provides authentication support.</span></span> | <span data-ttu-id="a5fcb-207">Ставится перед тем, как потребуется `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-207">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="a5fcb-208">Является конечным для обратных вызовов OAuth.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-208">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="a5fcb-209">CORS</span><span class="sxs-lookup"><span data-stu-id="a5fcb-209">CORS</span></span>](xref:security/cors) | <span data-ttu-id="a5fcb-210">Настраивает общий доступ к ресурсам независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-210">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="a5fcb-211">Ставится перед компонентами, использующими CORS.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-211">Before components that use CORS.</span></span> |
| [<span data-ttu-id="a5fcb-212">Диагностика</span><span class="sxs-lookup"><span data-stu-id="a5fcb-212">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="a5fcb-213">Настраивает диагностику.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-213">Configures diagnostics.</span></span> | <span data-ttu-id="a5fcb-214">Ставится перед компонентами, выдающими ошибки.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-214">Before components that generate errors.</span></span> |
| [<span data-ttu-id="a5fcb-215">Перенаправленные заголовки</span><span class="sxs-lookup"><span data-stu-id="a5fcb-215">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="a5fcb-216">Пересылает заголовки, переданные через прокси-сервер, в текущий запрос.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-216">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="a5fcb-217">Ставится перед компонентами, использующими обновленные поля (например: схема, узел, IP-адрес клиента, метод).</span><span class="sxs-lookup"><span data-stu-id="a5fcb-217">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="a5fcb-218">Переопределение метода HTTP</span><span class="sxs-lookup"><span data-stu-id="a5fcb-218">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="a5fcb-219">Разрешает входящий запрос POST для переопределения этого метода.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-219">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="a5fcb-220">Ставится перед компонентами, использующими обновленный метод.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-220">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="a5fcb-221">Перенаправление HTTPS</span><span class="sxs-lookup"><span data-stu-id="a5fcb-221">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="a5fcb-222">Перенаправление всех запросов HTTP на HTTPS (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="a5fcb-222">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="a5fcb-223">Ставится перед компонентами, использующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-223">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="a5fcb-224">HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a5fcb-224">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="a5fcb-225">ПО промежуточного слоя для усовершенствования безопасности, которое добавляет специальный заголовок ответа (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="a5fcb-225">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="a5fcb-226">Перед отправкой ответов и после компонентов, изменяющих запросы (например, перенаправленные заголовки, переопределение URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="a5fcb-226">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="a5fcb-227">MVC</span><span class="sxs-lookup"><span data-stu-id="a5fcb-227">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="a5fcb-228">Обрабатывает запросы с помощью MVC/Razor Pages (ASP.NET Core 2.0 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="a5fcb-228">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="a5fcb-229">Является конечным, если запрос соответствует маршруту.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-229">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="a5fcb-230">OWIN</span><span class="sxs-lookup"><span data-stu-id="a5fcb-230">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="a5fcb-231">Взаимодействие с приложениями, серверами и ПО промежуточного слоя на основе OWIN.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-231">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="a5fcb-232">Является конечным, если ПО промежуточного слоя OWIN полностью обрабатывает запрос.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-232">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="a5fcb-233">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="a5fcb-233">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="a5fcb-234">Обеспечивает поддержку для кэширования откликов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-234">Provides support for caching responses.</span></span> | <span data-ttu-id="a5fcb-235">Ставится перед компонентами, требующими кэширование.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-235">Before components that require caching.</span></span> |
| [<span data-ttu-id="a5fcb-236">Сжатие откликов</span><span class="sxs-lookup"><span data-stu-id="a5fcb-236">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="a5fcb-237">Обеспечивает поддержку для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-237">Provides support for compressing responses.</span></span> | <span data-ttu-id="a5fcb-238">Ставится перед компонентами, требующими сжатие.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-238">Before components that require compression.</span></span> |
| [<span data-ttu-id="a5fcb-239">Локализация запроса</span><span class="sxs-lookup"><span data-stu-id="a5fcb-239">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="a5fcb-240">Обеспечивает поддержку локализации.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-240">Provides localization support.</span></span> | <span data-ttu-id="a5fcb-241">Ставится перед компонентами, для которых важна локализация.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-241">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="a5fcb-242">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="a5fcb-242">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="a5fcb-243">Определяет и ограничивает маршруты запросов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-243">Defines and constrains request routes.</span></span> | <span data-ttu-id="a5fcb-244">Является конечным для совпадающих маршрутов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-244">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="a5fcb-245">Сеанс</span><span class="sxs-lookup"><span data-stu-id="a5fcb-245">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="a5fcb-246">Обеспечивает поддержку для управления пользовательскими сеансами.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-246">Provides support for managing user sessions.</span></span> | <span data-ttu-id="a5fcb-247">Ставится перед компонентами, требующими сеанс.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-247">Before components that require Session.</span></span> |
| [<span data-ttu-id="a5fcb-248">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="a5fcb-248">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="a5fcb-249">Обеспечивает поддержку для обработки статических файлов и просмотра каталогов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-249">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="a5fcb-250">Является конечным, если запрос соответствует файлу.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-250">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="a5fcb-251">Переопределение URL-адресов</span><span class="sxs-lookup"><span data-stu-id="a5fcb-251">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="a5fcb-252">Обеспечивает поддержку для переопределения URL-адресов и перенаправления запросов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-252">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="a5fcb-253">Ставится перед компонентами, использующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-253">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="a5fcb-254">WebSockets</span><span class="sxs-lookup"><span data-stu-id="a5fcb-254">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="a5fcb-255">Обеспечивает поддержку протокола WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-255">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="a5fcb-256">Ставится перед компонентами, которым нужно принимать запросы WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-256">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="a5fcb-257">Написание ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="a5fcb-257">Write middleware</span></span>

<span data-ttu-id="a5fcb-258">ПО промежуточного слоя обычно инкапсулируется в класс и предоставляется с помощью метода расширения.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-258">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="a5fcb-259">Рассмотрим следующее ПО промежуточного слоя, которое задает язык и региональные параметры для текущего запроса из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-259">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="a5fcb-260">Приведенный выше пример кода демонстрирует создание компонента промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-260">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="a5fcb-261">Дополнительные сведения о встроенной поддержке локализации в ASP.NET Core см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-261">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="a5fcb-262">Это ПО промежуточного слоя можно проверить, передав язык и региональные параметры, например `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-262">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="a5fcb-263">Следующий код перемещает делегат ПО промежуточного слоя в класс.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-263">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a5fcb-264">Метод ПО промежуточного слоя `Task` должен иметь имя `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-264">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="a5fcb-265">В ASP.NET Core 2.0 и более поздних версиях имя может быть `Invoke` или `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-265">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="a5fcb-266">Следующий метод расширения предоставляет ПО промежуточного слоя посредством <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="a5fcb-266">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="a5fcb-267">Следующий код вызывает ПО промежуточного слоя из `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-267">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="a5fcb-268">ПО промежуточного слоя должно соответствовать [принципу явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies), предоставляя свои зависимости в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-268">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="a5fcb-269">ПО промежуточного слоя создается один раз за *время существования приложения*.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-269">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="a5fcb-270">В разделе [Зависимости отдельных запросов](#per-request-dependencies) приведены сведения о том, как использовать службы совместно с ПО промежуточного слоя внутри запроса.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-270">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="a5fcb-271">Компоненты промежуточного слоя могут разрешать свои зависимости, возникшие в результате [внедрения зависимостей](xref:fundamentals/dependency-injection), за счет параметров конструктора.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-271">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="a5fcb-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) также может принимать дополнительные параметры напрямую.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-272">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="a5fcb-273">Зависимости отдельных запросов</span><span class="sxs-lookup"><span data-stu-id="a5fcb-273">Per-request dependencies</span></span>

<span data-ttu-id="a5fcb-274">Так как ПО промежуточного слоя создается при запуске приложения, а не для отдельных запросов, службы времени существования *scoped*, используемые конструкторами ПО промежуточного слоя, не являются общими с другими типами, возникшими в результате внедрения зависимостей, в каждом из запросов.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-274">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="a5fcb-275">Если необходимо предоставить службу *scoped* для совместного использования ПО промежуточного слоя и другими типами, добавьте ее в сигнатуру метода `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-275">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="a5fcb-276">Метод `Invoke` может принимать дополнительные параметры, заполняемые при внедрении зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a5fcb-276">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="a5fcb-277">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a5fcb-277">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
