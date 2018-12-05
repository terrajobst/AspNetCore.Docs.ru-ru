---
title: ПО промежуточного слоя ASP.NET Core
author: rick-anderson
description: Сведения о ПО промежуточного слоя ASP.NET Core и конвейере запросов.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: 4e5da1036b77e876899ccdea48bdec69454e1657
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861489"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="be4e2-103">ПО промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be4e2-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="be4e2-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="be4e2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="be4e2-105">ПО промежуточного слоя — это программное обеспечение, выстраиваемое в виде конвейера приложения для обработки запросов и откликов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="be4e2-106">Каждый компонент:</span><span class="sxs-lookup"><span data-stu-id="be4e2-106">Each component:</span></span>

* <span data-ttu-id="be4e2-107">определяет, нужно ли передать запрос следующему компоненту в конвейере;</span><span class="sxs-lookup"><span data-stu-id="be4e2-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="be4e2-108">может выполнять работу как до, так и после вызова следующего компонента в конвейере.</span><span class="sxs-lookup"><span data-stu-id="be4e2-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="be4e2-109">Для построения конвейера запросов используются делегаты запроса.</span><span class="sxs-lookup"><span data-stu-id="be4e2-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="be4e2-110">Они обрабатывают каждый HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="be4e2-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="be4e2-111">Для их настройки служат методы расширения <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> и <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="be4e2-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="be4e2-112">Отдельный делегат запроса можно указать встроенным в качестве анонимного метода (называемого встроенным ПО промежуточного слоя) либо определить в многоразовом классе.</span><span class="sxs-lookup"><span data-stu-id="be4e2-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="be4e2-113">Эти многоразовые классы и встроенные анонимные методы являются *ПО промежуточного слоя* или *компонентами промежуточного слоя*.</span><span class="sxs-lookup"><span data-stu-id="be4e2-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="be4e2-114">Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает конвейер.</span><span class="sxs-lookup"><span data-stu-id="be4e2-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="be4e2-115">Статья <xref:migration/http-modules> поясняет различия между конвейерами запросов в ASP.NET Core и ASP.NET 4.x, а также содержит дополнительные примеры ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="be4e2-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="be4e2-116">Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="be4e2-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="be4e2-117">Конвейер запросов ASP.NET Core состоит из последовательности делегатов запроса, вызываемых один за другим.</span><span class="sxs-lookup"><span data-stu-id="be4e2-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="be4e2-118">На следующей схеме демонстрируется этот принцип.</span><span class="sxs-lookup"><span data-stu-id="be4e2-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="be4e2-119">Поток выполнения показан черными стрелками.</span><span class="sxs-lookup"><span data-stu-id="be4e2-119">The thread of execution follows the black arrows.</span></span>

![Шаблон обработки запросов, показывающий входящий запрос, обработку в трех компонентах промежуточного слоя и запрос, покидающий приложение.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="be4e2-123">Каждый из делегатов может выполнять операции до и после следующего делегата.</span><span class="sxs-lookup"><span data-stu-id="be4e2-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="be4e2-124">Делегат также может принять решение не передавать запрос следующему делегату, что называется *замыканием конвейера запросов*.</span><span class="sxs-lookup"><span data-stu-id="be4e2-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="be4e2-125">Замыкание часто является предпочтительным, так как позволяет избежать ненужной работы.</span><span class="sxs-lookup"><span data-stu-id="be4e2-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="be4e2-126">Например, компонент промежуточного слоя для статических файлов может возвратить запрос статического файла и выполнить замыкание, чтобы обойти оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="be4e2-126">For example, Static File Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="be4e2-127">Делегаты обработки исключений вызываются в начале конвейера, чтобы они могли перехватывать исключения, возникающие в более поздних стадиях.</span><span class="sxs-lookup"><span data-stu-id="be4e2-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="be4e2-128">Простейшее приложение ASP.NET Core задает один делегат запроса, обрабатывающий все запросы.</span><span class="sxs-lookup"><span data-stu-id="be4e2-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="be4e2-129">В этом случае конвейер запросов как таковой отсутствует.</span><span class="sxs-lookup"><span data-stu-id="be4e2-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="be4e2-130">Вместо этого в ответ на каждый HTTP-запрос вызывается одна анонимная функция.</span><span class="sxs-lookup"><span data-stu-id="be4e2-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="be4e2-131">Первый делегат <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> завершает конвейер.</span><span class="sxs-lookup"><span data-stu-id="be4e2-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="be4e2-132">Несколько делегатов запроса можно соединить в цепочку с помощью <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="be4e2-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="be4e2-133">Параметр `next` представляет следующий делегат в конвейере.</span><span class="sxs-lookup"><span data-stu-id="be4e2-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="be4e2-134">Замыкать конвейер можно*не* вызывая параметр *next*.</span><span class="sxs-lookup"><span data-stu-id="be4e2-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="be4e2-135">Обычно действия можно выполнять как до, так и после следующего делегата, как показано в этом примере.</span><span class="sxs-lookup"><span data-stu-id="be4e2-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="be4e2-136">Не вызывайте `next.Invoke` после отправки отклика клиенту.</span><span class="sxs-lookup"><span data-stu-id="be4e2-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="be4e2-137">Изменения <xref:Microsoft.AspNetCore.Http.HttpResponse> после запуска отклика приведут к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="be4e2-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="be4e2-138">Например, таким изменением может быть задание HTTP-заголовков и кода состояния.</span><span class="sxs-lookup"><span data-stu-id="be4e2-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="be4e2-139">Запись в тело отклика после вызова `next`:</span><span class="sxs-lookup"><span data-stu-id="be4e2-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="be4e2-140">может вызвать нарушение протокола,</span><span class="sxs-lookup"><span data-stu-id="be4e2-140">May cause a protocol violation.</span></span> <span data-ttu-id="be4e2-141">например, при записи больше указанного значения `Content-Length`;</span><span class="sxs-lookup"><span data-stu-id="be4e2-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="be4e2-142">может привести к нарушению формата,</span><span class="sxs-lookup"><span data-stu-id="be4e2-142">May corrupt the body format.</span></span> <span data-ttu-id="be4e2-143">например, при записи нижнего колонтитула HTML в CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="be4e2-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="be4e2-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> удобно использовать для обозначения того, были ли отправлены заголовки или выполнена запись в тело отклика.</span><span class="sxs-lookup"><span data-stu-id="be4e2-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="be4e2-145">Номер</span><span class="sxs-lookup"><span data-stu-id="be4e2-145">Order</span></span>

<span data-ttu-id="be4e2-146">Порядок, в котором компоненты промежуточного слоя добавляются в метод `Startup.Configure`, определяет порядок их вызова при запросах и обратный порядок для отклика.</span><span class="sxs-lookup"><span data-stu-id="be4e2-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="be4e2-147">Соблюдение этого порядка крайне важно для обеспечения безопасности, производительности и функциональности.</span><span class="sxs-lookup"><span data-stu-id="be4e2-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="be4e2-148">Метод `Startup.Configure` добавляет компоненты ПО промежуточного слоя для распространенных сценариев приложений:</span><span class="sxs-lookup"><span data-stu-id="be4e2-148">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="be4e2-149">Обработка исключений/ошибок</span><span class="sxs-lookup"><span data-stu-id="be4e2-149">Exception/error handling</span></span>
1. <span data-ttu-id="be4e2-150">Протокол HTTP Strict Transport Security.</span><span class="sxs-lookup"><span data-stu-id="be4e2-150">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="be4e2-151">Перенаправление HTTPS</span><span class="sxs-lookup"><span data-stu-id="be4e2-151">HTTPS redirection</span></span>
1. <span data-ttu-id="be4e2-152">Статический файловый сервер</span><span class="sxs-lookup"><span data-stu-id="be4e2-152">Static file server</span></span>
1. <span data-ttu-id="be4e2-153">Применение политик в отношении файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="be4e2-153">Cookie policy enforcement</span></span>
1. <span data-ttu-id="be4e2-154">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="be4e2-154">Authentication</span></span>
1. <span data-ttu-id="be4e2-155">Сеанс</span><span class="sxs-lookup"><span data-stu-id="be4e2-155">Session</span></span>
1. <span data-ttu-id="be4e2-156">MVC</span><span class="sxs-lookup"><span data-stu-id="be4e2-156">MVC</span></span>

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
    // Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // If the app uses session state, call Session Middleware after Cookie 
    // Policy Middleware and before MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. <span data-ttu-id="be4e2-157">Обработка исключений/ошибок</span><span class="sxs-lookup"><span data-stu-id="be4e2-157">Exception/error handling</span></span>
1. <span data-ttu-id="be4e2-158">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="be4e2-158">Static files</span></span>
1. <span data-ttu-id="be4e2-159">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="be4e2-159">Authentication</span></span>
1. <span data-ttu-id="be4e2-160">Сеанс</span><span class="sxs-lookup"><span data-stu-id="be4e2-160">Session</span></span>
1. <span data-ttu-id="be4e2-161">MVC</span><span class="sxs-lookup"><span data-stu-id="be4e2-161">MVC</span></span>

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

    // If the app uses session state, call UseSession before 
    // MVC Middleware.
    app.UseSession();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="be4e2-162">В предыдущем примере кода каждый метод расширения ПО промежуточного слоя представляется в <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> с использованием пространства имен <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="be4e2-162">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="be4e2-163"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> — это первый компонент промежуточного слоя, добавленный в конвейер.</span><span class="sxs-lookup"><span data-stu-id="be4e2-163"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="be4e2-164">Таким образом, обработчик исключений ПО промежуточного слоя перехватывает все исключения, возникающие в последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="be4e2-164">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="be4e2-165">Компонент промежуточного слоя для статических файлов вызывается на раннем этапе конвейера, чтобы он мог обработать запросы и выполнить замыкание, минуя остальные компоненты.</span><span class="sxs-lookup"><span data-stu-id="be4e2-165">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="be4e2-166">Этот компонент **не** выполняет проверки авторизации.</span><span class="sxs-lookup"><span data-stu-id="be4e2-166">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="be4e2-167">Все обрабатываемые им файлы, включая расположенные в *wwwroot*, находятся в открытом доступе.</span><span class="sxs-lookup"><span data-stu-id="be4e2-167">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="be4e2-168">Сведения о защите статических файлов см. в статье <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="be4e2-168">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="be4e2-169">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для проверки подлинности (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="be4e2-169">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="be4e2-170">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="be4e2-170">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="be4e2-171">Хотя ПО промежуточного слоя для проверки подлинности проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанную страницу Razor Page или контроллер MVC и действие.</span><span class="sxs-lookup"><span data-stu-id="be4e2-171">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="be4e2-172">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="be4e2-172">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="be4e2-173">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="be4e2-173">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="be4e2-174">Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанный контроллер и действие.</span><span class="sxs-lookup"><span data-stu-id="be4e2-174">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="be4e2-175">Следующий пример показывает порядок компонентов промежуточного слоя, где запросы для статических файлов обрабатываются компонентом для статических файлов до компонента для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-175">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="be4e2-176">Статические файлы не сжимаются с этим порядком ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="be4e2-176">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="be4e2-177">Отклики MVC из <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> можно сжать.</span><span class="sxs-lookup"><span data-stu-id="be4e2-177">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="be4e2-178">Методы Use, Run и Map</span><span class="sxs-lookup"><span data-stu-id="be4e2-178">Use, Run, and Map</span></span>

<span data-ttu-id="be4e2-179">Для настройки конвейера HTTP служат методы `Use`, `Run` и `Map`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-179">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="be4e2-180">Метод `Use` может замыкать конвейер (это происходит, если он не вызывает делегат запроса `next`).</span><span class="sxs-lookup"><span data-stu-id="be4e2-180">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="be4e2-181">`Run` является соглашением, и некоторые компоненты промежуточного слоя могут предоставлять методы `Run[Middleware]`, которые выполняются в конце конвейера.</span><span class="sxs-lookup"><span data-stu-id="be4e2-181">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="be4e2-182">Расширения <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> используются в качестве соглашения для ветвления конвейера.</span><span class="sxs-lookup"><span data-stu-id="be4e2-182"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="be4e2-183">`Map*` осуществляет ветвление конвейера запросов на основе совпадений для заданного пути запроса.</span><span class="sxs-lookup"><span data-stu-id="be4e2-183">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="be4e2-184">Если путь запроса начинается с заданного пути, данная ветвь выполняется.</span><span class="sxs-lookup"><span data-stu-id="be4e2-184">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="be4e2-185">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="be4e2-185">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="be4e2-186">Запрос</span><span class="sxs-lookup"><span data-stu-id="be4e2-186">Request</span></span>             | <span data-ttu-id="be4e2-187">Ответ</span><span class="sxs-lookup"><span data-stu-id="be4e2-187">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="be4e2-188">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="be4e2-188">localhost:1234</span></span>      | <span data-ttu-id="be4e2-189">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="be4e2-189">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="be4e2-190">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="be4e2-190">localhost:1234/map1</span></span> | <span data-ttu-id="be4e2-191">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="be4e2-191">Map Test 1</span></span>                   |
| <span data-ttu-id="be4e2-192">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="be4e2-192">localhost:1234/map2</span></span> | <span data-ttu-id="be4e2-193">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="be4e2-193">Map Test 2</span></span>                   |
| <span data-ttu-id="be4e2-194">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="be4e2-194">localhost:1234/map3</span></span> | <span data-ttu-id="be4e2-195">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="be4e2-195">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="be4e2-196">Когда используется `Map`, соответствующие сегменты путей удаляются из `HttpRequest.Path` и добавляются к `HttpRequest.PathBase` для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="be4e2-196">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="be4e2-197">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) осуществляет ветвление конвейера запросов на основе результата заданного предиката.</span><span class="sxs-lookup"><span data-stu-id="be4e2-197">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="be4e2-198">Любой предикат типа `Func<HttpContext, bool>` можно использовать для сопоставления запросов с новой ветвью конвейера.</span><span class="sxs-lookup"><span data-stu-id="be4e2-198">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="be4e2-199">В следующем примере предикат служит для определения наличия переменной строки запроса `branch`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-199">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="be4e2-200">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="be4e2-200">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="be4e2-201">Запрос</span><span class="sxs-lookup"><span data-stu-id="be4e2-201">Request</span></span>                       | <span data-ttu-id="be4e2-202">Ответ</span><span class="sxs-lookup"><span data-stu-id="be4e2-202">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="be4e2-203">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="be4e2-203">localhost:1234</span></span>                | <span data-ttu-id="be4e2-204">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="be4e2-204">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="be4e2-205">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="be4e2-205">localhost:1234/?branch=master</span></span> | <span data-ttu-id="be4e2-206">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="be4e2-206">Branch used = master</span></span>         |

<span data-ttu-id="be4e2-207">`Map` поддерживает вложение, например:</span><span class="sxs-lookup"><span data-stu-id="be4e2-207">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="be4e2-208">`Map` также может сопоставить несколько сегментов одновременно:</span><span class="sxs-lookup"><span data-stu-id="be4e2-208">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="be4e2-209">Встроенное ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="be4e2-209">Built-in middleware</span></span>

<span data-ttu-id="be4e2-210">ASP.NET Core содержит следующие компоненты промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="be4e2-210">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="be4e2-211">В столбце *Порядок* указаны сведения о размещении ПО промежуточного слоя в конвейере запросов и условия, в соответствии с которыми ПО промежуточного слоя может прервать запрос и предотвратить обработку запроса в других ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="be4e2-211">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="be4e2-212">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="be4e2-212">Middleware</span></span> | <span data-ttu-id="be4e2-213">Описание:</span><span class="sxs-lookup"><span data-stu-id="be4e2-213">Description</span></span> | <span data-ttu-id="be4e2-214">Номер</span><span class="sxs-lookup"><span data-stu-id="be4e2-214">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="be4e2-215">Authentication</span><span class="sxs-lookup"><span data-stu-id="be4e2-215">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="be4e2-216">Обеспечивает поддержку проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="be4e2-216">Provides authentication support.</span></span> | <span data-ttu-id="be4e2-217">Ставится перед тем, как потребуется `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-217">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="be4e2-218">Является конечным для обратных вызовов OAuth.</span><span class="sxs-lookup"><span data-stu-id="be4e2-218">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="be4e2-219">Cookie Policy</span><span class="sxs-lookup"><span data-stu-id="be4e2-219">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="be4e2-220">Позволяет отслеживать согласие пользователей на хранение личных сведений и применять минимальные стандарты для полей файлов cookie, таких как `secure` и `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-220">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="be4e2-221">Перед ПО промежуточного слоя, которое использует файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="be4e2-221">Before middleware that issues cookies.</span></span> <span data-ttu-id="be4e2-222">Например: Authentication, Session, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="be4e2-222">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="be4e2-223">CORS</span><span class="sxs-lookup"><span data-stu-id="be4e2-223">CORS</span></span>](xref:security/cors) | <span data-ttu-id="be4e2-224">Настраивает общий доступ к ресурсам независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="be4e2-224">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="be4e2-225">Ставится перед компонентами, использующими CORS.</span><span class="sxs-lookup"><span data-stu-id="be4e2-225">Before components that use CORS.</span></span> |
| [<span data-ttu-id="be4e2-226">Error Handling</span><span class="sxs-lookup"><span data-stu-id="be4e2-226">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="be4e2-227">Настраивает диагностику.</span><span class="sxs-lookup"><span data-stu-id="be4e2-227">Configures diagnostics.</span></span> | <span data-ttu-id="be4e2-228">Ставится перед компонентами, выдающими ошибки.</span><span class="sxs-lookup"><span data-stu-id="be4e2-228">Before components that generate errors.</span></span> |
| [<span data-ttu-id="be4e2-229">Forwarded Headers</span><span class="sxs-lookup"><span data-stu-id="be4e2-229">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="be4e2-230">Пересылает заголовки, переданные через прокси-сервер, в текущий запрос.</span><span class="sxs-lookup"><span data-stu-id="be4e2-230">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="be4e2-231">Перед компонентами, использующими обновленные поля.</span><span class="sxs-lookup"><span data-stu-id="be4e2-231">Before components that consume the updated fields.</span></span> <span data-ttu-id="be4e2-232">Например: схема, узел, IP-адрес клиента, метод.</span><span class="sxs-lookup"><span data-stu-id="be4e2-232">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="be4e2-233">Проверка работоспособности</span><span class="sxs-lookup"><span data-stu-id="be4e2-233">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="be4e2-234">Проверяет работоспособность приложения ASP.NET Core и его зависимостей, таких как проверка доступности базы данных.</span><span class="sxs-lookup"><span data-stu-id="be4e2-234">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="be4e2-235">Является конечным, если запрос соответствует конечной точке проверки работоспособности.</span><span class="sxs-lookup"><span data-stu-id="be4e2-235">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="be4e2-236">HTTP Method Override</span><span class="sxs-lookup"><span data-stu-id="be4e2-236">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="be4e2-237">Разрешает входящий запрос POST для переопределения этого метода.</span><span class="sxs-lookup"><span data-stu-id="be4e2-237">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="be4e2-238">Ставится перед компонентами, использующими обновленный метод.</span><span class="sxs-lookup"><span data-stu-id="be4e2-238">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="be4e2-239">HTTPS Redirection</span><span class="sxs-lookup"><span data-stu-id="be4e2-239">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="be4e2-240">Перенаправление всех запросов HTTP на HTTPS (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="be4e2-240">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="be4e2-241">Ставится перед компонентами, использующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="be4e2-241">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="be4e2-242">HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="be4e2-242">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="be4e2-243">ПО промежуточного слоя для усовершенствования безопасности, которое добавляет специальный заголовок ответа (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="be4e2-243">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="be4e2-244">Перед отправкой ответов и после компонентов, изменяющих запросы.</span><span class="sxs-lookup"><span data-stu-id="be4e2-244">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="be4e2-245">Например: Forwarded Headers и URL Rewriting.</span><span class="sxs-lookup"><span data-stu-id="be4e2-245">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="be4e2-246">MVC</span><span class="sxs-lookup"><span data-stu-id="be4e2-246">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="be4e2-247">Обрабатывает запросы с помощью MVC/Razor Pages (ASP.NET Core 2.0 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="be4e2-247">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="be4e2-248">Является конечным, если запрос соответствует маршруту.</span><span class="sxs-lookup"><span data-stu-id="be4e2-248">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="be4e2-249">OWIN</span><span class="sxs-lookup"><span data-stu-id="be4e2-249">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="be4e2-250">Взаимодействие с приложениями, серверами и ПО промежуточного слоя на основе OWIN.</span><span class="sxs-lookup"><span data-stu-id="be4e2-250">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="be4e2-251">Является конечным, если ПО промежуточного слоя OWIN полностью обрабатывает запрос.</span><span class="sxs-lookup"><span data-stu-id="be4e2-251">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="be4e2-252">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="be4e2-252">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="be4e2-253">Обеспечивает поддержку для кэширования откликов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-253">Provides support for caching responses.</span></span> | <span data-ttu-id="be4e2-254">Ставится перед компонентами, требующими кэширование.</span><span class="sxs-lookup"><span data-stu-id="be4e2-254">Before components that require caching.</span></span> |
| [<span data-ttu-id="be4e2-255">Сжатие откликов</span><span class="sxs-lookup"><span data-stu-id="be4e2-255">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="be4e2-256">Обеспечивает поддержку для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-256">Provides support for compressing responses.</span></span> | <span data-ttu-id="be4e2-257">Ставится перед компонентами, требующими сжатие.</span><span class="sxs-lookup"><span data-stu-id="be4e2-257">Before components that require compression.</span></span> |
| [<span data-ttu-id="be4e2-258">Localization</span><span class="sxs-lookup"><span data-stu-id="be4e2-258">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="be4e2-259">Обеспечивает поддержку локализации.</span><span class="sxs-lookup"><span data-stu-id="be4e2-259">Provides localization support.</span></span> | <span data-ttu-id="be4e2-260">Ставится перед компонентами, для которых важна локализация.</span><span class="sxs-lookup"><span data-stu-id="be4e2-260">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="be4e2-261">Routing</span><span class="sxs-lookup"><span data-stu-id="be4e2-261">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="be4e2-262">Определяет и ограничивает маршруты запросов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-262">Defines and constrains request routes.</span></span> | <span data-ttu-id="be4e2-263">Является конечным для совпадающих маршрутов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-263">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="be4e2-264">Session</span><span class="sxs-lookup"><span data-stu-id="be4e2-264">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="be4e2-265">Обеспечивает поддержку для управления пользовательскими сеансами.</span><span class="sxs-lookup"><span data-stu-id="be4e2-265">Provides support for managing user sessions.</span></span> | <span data-ttu-id="be4e2-266">Ставится перед компонентами, требующими сеанс.</span><span class="sxs-lookup"><span data-stu-id="be4e2-266">Before components that require Session.</span></span> |
| [<span data-ttu-id="be4e2-267">Static Files</span><span class="sxs-lookup"><span data-stu-id="be4e2-267">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="be4e2-268">Обеспечивает поддержку для обработки статических файлов и просмотра каталогов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-268">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="be4e2-269">Является конечным, если запрос соответствует файлу.</span><span class="sxs-lookup"><span data-stu-id="be4e2-269">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="be4e2-270">URL Rewriting</span><span class="sxs-lookup"><span data-stu-id="be4e2-270">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="be4e2-271">Обеспечивает поддержку для переопределения URL-адресов и перенаправления запросов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-271">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="be4e2-272">Ставится перед компонентами, использующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="be4e2-272">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="be4e2-273">WebSockets</span><span class="sxs-lookup"><span data-stu-id="be4e2-273">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="be4e2-274">Обеспечивает поддержку протокола WebSockets.</span><span class="sxs-lookup"><span data-stu-id="be4e2-274">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="be4e2-275">Ставится перед компонентами, которым нужно принимать запросы WebSocket.</span><span class="sxs-lookup"><span data-stu-id="be4e2-275">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="be4e2-276">Написание ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="be4e2-276">Write middleware</span></span>

<span data-ttu-id="be4e2-277">ПО промежуточного слоя обычно инкапсулируется в класс и предоставляется с помощью метода расширения.</span><span class="sxs-lookup"><span data-stu-id="be4e2-277">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="be4e2-278">Рассмотрим следующее ПО промежуточного слоя, которое задает язык и региональные параметры для текущего запроса из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="be4e2-278">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="be4e2-279">Приведенный выше пример кода демонстрирует создание компонента промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="be4e2-279">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="be4e2-280">Дополнительные сведения о встроенной поддержке локализации в ASP.NET Core см. в разделе <xref:fundamentals/localization>.</span><span class="sxs-lookup"><span data-stu-id="be4e2-280">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="be4e2-281">Это ПО промежуточного слоя можно проверить, передав язык и региональные параметры, например `http://localhost:7997/?culture=no`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-281">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="be4e2-282">Следующий код перемещает делегат ПО промежуточного слоя в класс.</span><span class="sxs-lookup"><span data-stu-id="be4e2-282">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="be4e2-283">Метод ПО промежуточного слоя `Task` должен иметь имя `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-283">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="be4e2-284">В ASP.NET Core 2.0 и более поздних версиях имя может быть `Invoke` или `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-284">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="be4e2-285">Следующий метод расширения предоставляет ПО промежуточного слоя посредством <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="be4e2-285">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="be4e2-286">Следующий код вызывает ПО промежуточного слоя из `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-286">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="be4e2-287">ПО промежуточного слоя должно соответствовать [принципу явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies), предоставляя свои зависимости в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="be4e2-287">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="be4e2-288">ПО промежуточного слоя создается один раз за *время существования приложения*.</span><span class="sxs-lookup"><span data-stu-id="be4e2-288">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="be4e2-289">В разделе [Зависимости отдельных запросов](#per-request-dependencies) приведены сведения о том, как использовать службы совместно с ПО промежуточного слоя внутри запроса.</span><span class="sxs-lookup"><span data-stu-id="be4e2-289">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="be4e2-290">Компоненты промежуточного слоя могут разрешать свои зависимости, возникшие в результате [внедрения зависимостей](xref:fundamentals/dependency-injection), за счет параметров конструктора.</span><span class="sxs-lookup"><span data-stu-id="be4e2-290">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="be4e2-291">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) также может принимать дополнительные параметры напрямую.</span><span class="sxs-lookup"><span data-stu-id="be4e2-291">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="be4e2-292">Зависимости отдельных запросов</span><span class="sxs-lookup"><span data-stu-id="be4e2-292">Per-request dependencies</span></span>

<span data-ttu-id="be4e2-293">Так как ПО промежуточного слоя создается при запуске приложения, а не для отдельных запросов, службы времени существования *scoped*, используемые конструкторами ПО промежуточного слоя, не являются общими с другими типами, возникшими в результате внедрения зависимостей, в каждом из запросов.</span><span class="sxs-lookup"><span data-stu-id="be4e2-293">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="be4e2-294">Если необходимо предоставить службу *scoped* для совместного использования ПО промежуточного слоя и другими типами, добавьте ее в сигнатуру метода `Invoke`.</span><span class="sxs-lookup"><span data-stu-id="be4e2-294">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="be4e2-295">Метод `Invoke` может принимать дополнительные параметры, заполняемые при внедрении зависимостей.</span><span class="sxs-lookup"><span data-stu-id="be4e2-295">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="be4e2-296">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="be4e2-296">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
