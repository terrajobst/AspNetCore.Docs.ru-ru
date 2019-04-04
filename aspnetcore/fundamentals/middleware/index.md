---
title: ПО промежуточного слоя ASP.NET Core
author: rick-anderson
description: Сведения о ПО промежуточного слоя ASP.NET Core и конвейере запросов.
ms.author: riande
ms.custom: mvc
ms.date: 02/17/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: bac121441d6856ca79affe1a3130e5cbc76debd9
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665393"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="de9a1-103">ПО промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de9a1-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="de9a1-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="de9a1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="de9a1-105">ПО промежуточного слоя — это программное обеспечение, выстраиваемое в виде конвейера приложения для обработки запросов и откликов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="de9a1-106">Каждый компонент:</span><span class="sxs-lookup"><span data-stu-id="de9a1-106">Each component:</span></span>

* <span data-ttu-id="de9a1-107">определяет, нужно ли передать запрос следующему компоненту в конвейере;</span><span class="sxs-lookup"><span data-stu-id="de9a1-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="de9a1-108">может выполнять работу как до, так и после вызова следующего компонента в конвейере.</span><span class="sxs-lookup"><span data-stu-id="de9a1-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="de9a1-109">Для построения конвейера запросов используются делегаты запроса.</span><span class="sxs-lookup"><span data-stu-id="de9a1-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="de9a1-110">Они обрабатывают каждый HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="de9a1-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="de9a1-111">Для их настройки служат методы расширения <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> и <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="de9a1-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="de9a1-112">Отдельный делегат запроса можно указать встроенным в качестве анонимного метода (называемого встроенным ПО промежуточного слоя) либо определить в многоразовом классе.</span><span class="sxs-lookup"><span data-stu-id="de9a1-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="de9a1-113">Эти многоразовые классы и встроенные анонимные методы являются *ПО промежуточного слоя* или *компонентами промежуточного слоя*.</span><span class="sxs-lookup"><span data-stu-id="de9a1-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="de9a1-114">Каждый компонент ПО промежуточного слоя в конвейере запросов отвечает за вызов следующего компонента в конвейере или замыкает конвейер.</span><span class="sxs-lookup"><span data-stu-id="de9a1-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="de9a1-115">Когда промежуточный слой замыкает конвейер, он становится *терминальным промежуточным слоем*, так как препятствует обработке запроса дальнейшими компонентами промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="de9a1-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="de9a1-116">Статья <xref:migration/http-modules> поясняет различия между конвейерами запросов в ASP.NET Core и ASP.NET 4.x, а также содержит дополнительные примеры ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="de9a1-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="de9a1-117">Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder</span><span class="sxs-lookup"><span data-stu-id="de9a1-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="de9a1-118">Конвейер запросов ASP.NET Core состоит из последовательности делегатов запроса, вызываемых один за другим.</span><span class="sxs-lookup"><span data-stu-id="de9a1-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="de9a1-119">На следующей схеме демонстрируется этот принцип.</span><span class="sxs-lookup"><span data-stu-id="de9a1-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="de9a1-120">Поток выполнения показан черными стрелками.</span><span class="sxs-lookup"><span data-stu-id="de9a1-120">The thread of execution follows the black arrows.</span></span>

![Шаблон обработки запросов, показывающий входящий запрос, обработку в трех компонентах промежуточного слоя и запрос, покидающий приложение.](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="de9a1-124">Каждый из делегатов может выполнять операции до и после следующего делегата.</span><span class="sxs-lookup"><span data-stu-id="de9a1-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="de9a1-125">Делегаты обработки исключений должны вызываться в начале конвейера, чтобы перехватывать исключения, возникающие на более поздних этапах.</span><span class="sxs-lookup"><span data-stu-id="de9a1-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="de9a1-126">Простейшее приложение ASP.NET Core задает один делегат запроса, обрабатывающий все запросы.</span><span class="sxs-lookup"><span data-stu-id="de9a1-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="de9a1-127">В этом случае конвейер запросов как таковой отсутствует.</span><span class="sxs-lookup"><span data-stu-id="de9a1-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="de9a1-128">Вместо этого в ответ на каждый HTTP-запрос вызывается одна анонимная функция.</span><span class="sxs-lookup"><span data-stu-id="de9a1-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="de9a1-129">Первый делегат <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> завершает конвейер.</span><span class="sxs-lookup"><span data-stu-id="de9a1-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="de9a1-130">Несколько делегатов запроса можно соединить в цепочку с помощью <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span><span class="sxs-lookup"><span data-stu-id="de9a1-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="de9a1-131">Параметр `next` представляет следующий делегат в конвейере.</span><span class="sxs-lookup"><span data-stu-id="de9a1-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="de9a1-132">Замыкать конвейер можно*не* вызывая параметр *next*.</span><span class="sxs-lookup"><span data-stu-id="de9a1-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="de9a1-133">Обычно действия можно выполнять как до, так и после следующего делегата, как показано в этом примере.</span><span class="sxs-lookup"><span data-stu-id="de9a1-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

<span data-ttu-id="de9a1-134">Если делегат не передает запрос следующему делегату, это называется *замыканием конвейера запросов*.</span><span class="sxs-lookup"><span data-stu-id="de9a1-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="de9a1-135">Замыкание часто является предпочтительным, так как позволяет избежать ненужной работы.</span><span class="sxs-lookup"><span data-stu-id="de9a1-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="de9a1-136">Например, [компонент промежуточного слоя для статических файлов](xref:fundamentals/static-files) может выступать в роли *терминального промежуточного слоя*, обрабатывая запрос статического файла и замыкая оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="de9a1-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="de9a1-137">Компоненты промежуточного слоя, предшествующие терминальному промежуточному слою, по-прежнему обрабатывают код после их инструкций `next.Invoke`.</span><span class="sxs-lookup"><span data-stu-id="de9a1-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="de9a1-138">Но учитывайте следующее предупреждение о попытке записи в ответ, который уже был отправлен.</span><span class="sxs-lookup"><span data-stu-id="de9a1-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="de9a1-139">Не вызывайте `next.Invoke` после отправки отклика клиенту.</span><span class="sxs-lookup"><span data-stu-id="de9a1-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="de9a1-140">Изменения <xref:Microsoft.AspNetCore.Http.HttpResponse> после запуска отклика приведут к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="de9a1-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="de9a1-141">Например, таким изменением может быть задание HTTP-заголовков и кода состояния.</span><span class="sxs-lookup"><span data-stu-id="de9a1-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="de9a1-142">Запись в тело отклика после вызова `next`:</span><span class="sxs-lookup"><span data-stu-id="de9a1-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="de9a1-143">может вызвать нарушение протокола,</span><span class="sxs-lookup"><span data-stu-id="de9a1-143">May cause a protocol violation.</span></span> <span data-ttu-id="de9a1-144">например, при записи больше указанного значения `Content-Length`;</span><span class="sxs-lookup"><span data-stu-id="de9a1-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="de9a1-145">может привести к нарушению формата,</span><span class="sxs-lookup"><span data-stu-id="de9a1-145">May corrupt the body format.</span></span> <span data-ttu-id="de9a1-146">например, при записи нижнего колонтитула HTML в CSS-файл.</span><span class="sxs-lookup"><span data-stu-id="de9a1-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="de9a1-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> удобно использовать для обозначения того, были ли отправлены заголовки или выполнена запись в тело отклика.</span><span class="sxs-lookup"><span data-stu-id="de9a1-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="de9a1-148">Номер</span><span class="sxs-lookup"><span data-stu-id="de9a1-148">Order</span></span>

<span data-ttu-id="de9a1-149">Порядок, в котором компоненты промежуточного слоя добавляются в метод `Startup.Configure`, определяет порядок их вызова при запросах и обратный порядок для отклика.</span><span class="sxs-lookup"><span data-stu-id="de9a1-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="de9a1-150">Соблюдение этого порядка крайне важно для обеспечения безопасности, производительности и функциональности.</span><span class="sxs-lookup"><span data-stu-id="de9a1-150">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="de9a1-151">Метод `Startup.Configure` добавляет компоненты ПО промежуточного слоя для распространенных сценариев приложений:</span><span class="sxs-lookup"><span data-stu-id="de9a1-151">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="de9a1-152">Обработка исключений/ошибок</span><span class="sxs-lookup"><span data-stu-id="de9a1-152">Exception/error handling</span></span>
1. <span data-ttu-id="de9a1-153">Протокол HTTP Strict Transport Security.</span><span class="sxs-lookup"><span data-stu-id="de9a1-153">HTTP Strict Transport Security Protocol</span></span>
1. <span data-ttu-id="de9a1-154">Перенаправление HTTPS</span><span class="sxs-lookup"><span data-stu-id="de9a1-154">HTTPS redirection</span></span>
1. <span data-ttu-id="de9a1-155">Статический файловый сервер</span><span class="sxs-lookup"><span data-stu-id="de9a1-155">Static file server</span></span>
1. <span data-ttu-id="de9a1-156">Применение политик в отношении файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="de9a1-156">Cookie policy enforcement</span></span>
1. <span data-ttu-id="de9a1-157">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="de9a1-157">Authentication</span></span>
1. <span data-ttu-id="de9a1-158">Сеанс</span><span class="sxs-lookup"><span data-stu-id="de9a1-158">Session</span></span>
1. <span data-ttu-id="de9a1-159">MVC</span><span class="sxs-lookup"><span data-stu-id="de9a1-159">MVC</span></span>

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

1. <span data-ttu-id="de9a1-160">Обработка исключений/ошибок</span><span class="sxs-lookup"><span data-stu-id="de9a1-160">Exception/error handling</span></span>
1. <span data-ttu-id="de9a1-161">Статические файлы</span><span class="sxs-lookup"><span data-stu-id="de9a1-161">Static files</span></span>
1. <span data-ttu-id="de9a1-162">Проверка подлинности</span><span class="sxs-lookup"><span data-stu-id="de9a1-162">Authentication</span></span>
1. <span data-ttu-id="de9a1-163">Сеанс</span><span class="sxs-lookup"><span data-stu-id="de9a1-163">Session</span></span>
1. <span data-ttu-id="de9a1-164">MVC</span><span class="sxs-lookup"><span data-stu-id="de9a1-164">MVC</span></span>

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

<span data-ttu-id="de9a1-165">В предыдущем примере кода каждый метод расширения ПО промежуточного слоя представляется в <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> с использованием пространства имен <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="de9a1-165">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="de9a1-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> — это первый компонент промежуточного слоя, добавленный в конвейер.</span><span class="sxs-lookup"><span data-stu-id="de9a1-166"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="de9a1-167">Таким образом, обработчик исключений ПО промежуточного слоя перехватывает все исключения, возникающие в последующих вызовах.</span><span class="sxs-lookup"><span data-stu-id="de9a1-167">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="de9a1-168">Компонент промежуточного слоя для статических файлов вызывается на раннем этапе конвейера, чтобы он мог обработать запросы и выполнить замыкание, минуя остальные компоненты.</span><span class="sxs-lookup"><span data-stu-id="de9a1-168">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="de9a1-169">Этот компонент **не** выполняет проверки авторизации.</span><span class="sxs-lookup"><span data-stu-id="de9a1-169">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="de9a1-170">Все обрабатываемые им файлы, включая расположенные в *wwwroot*, находятся в открытом доступе.</span><span class="sxs-lookup"><span data-stu-id="de9a1-170">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="de9a1-171">Сведения о защите статических файлов см. в статье <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="de9a1-171">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="de9a1-172">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для проверки подлинности (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="de9a1-172">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="de9a1-173">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="de9a1-173">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="de9a1-174">Хотя ПО промежуточного слоя для проверки подлинности проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанную страницу Razor Page или контроллер MVC и действие.</span><span class="sxs-lookup"><span data-stu-id="de9a1-174">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="de9a1-175">Если запрос не обрабатывается компонентом промежуточного слоя для статических файлов, он передается в компонент промежуточного слоя для удостоверений (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), который выполняет проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="de9a1-175">If the request isn't handled by Static File Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="de9a1-176">Этот компонент не замыкает запросы, не прошедшие проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="de9a1-176">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="de9a1-177">Хотя он проверяет подлинность запросов, авторизация (и отклонение) выполняются только после того, как MVC выберет указанный контроллер и действие.</span><span class="sxs-lookup"><span data-stu-id="de9a1-177">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="de9a1-178">Следующий пример показывает порядок компонентов промежуточного слоя, где запросы для статических файлов обрабатываются компонентом для статических файлов до компонента для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-178">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="de9a1-179">Статические файлы не сжимаются с этим порядком ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="de9a1-179">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="de9a1-180">Отклики MVC из <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> можно сжать.</span><span class="sxs-lookup"><span data-stu-id="de9a1-180">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static File Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="de9a1-181">Методы Use, Run и Map</span><span class="sxs-lookup"><span data-stu-id="de9a1-181">Use, Run, and Map</span></span>

<span data-ttu-id="de9a1-182">Для настройки конвейера HTTP служат методы `Use`, `Run` и `Map`.</span><span class="sxs-lookup"><span data-stu-id="de9a1-182">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="de9a1-183">Метод `Use` может замыкать конвейер (это происходит, если он не вызывает делегат запроса `next`).</span><span class="sxs-lookup"><span data-stu-id="de9a1-183">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="de9a1-184">`Run` является соглашением, и некоторые компоненты промежуточного слоя могут предоставлять методы `Run[Middleware]`, которые выполняются в конце конвейера.</span><span class="sxs-lookup"><span data-stu-id="de9a1-184">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="de9a1-185">Расширения <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> используются в качестве соглашения для ветвления конвейера.</span><span class="sxs-lookup"><span data-stu-id="de9a1-185"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="de9a1-186">`Map*` осуществляет ветвление конвейера запросов на основе совпадений для заданного пути запроса.</span><span class="sxs-lookup"><span data-stu-id="de9a1-186">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="de9a1-187">Если путь запроса начинается с заданного пути, данная ветвь выполняется.</span><span class="sxs-lookup"><span data-stu-id="de9a1-187">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="de9a1-188">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="de9a1-188">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="de9a1-189">Запрос</span><span class="sxs-lookup"><span data-stu-id="de9a1-189">Request</span></span>             | <span data-ttu-id="de9a1-190">Ответ</span><span class="sxs-lookup"><span data-stu-id="de9a1-190">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="de9a1-191">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="de9a1-191">localhost:1234</span></span>      | <span data-ttu-id="de9a1-192">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="de9a1-192">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="de9a1-193">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="de9a1-193">localhost:1234/map1</span></span> | <span data-ttu-id="de9a1-194">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="de9a1-194">Map Test 1</span></span>                   |
| <span data-ttu-id="de9a1-195">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="de9a1-195">localhost:1234/map2</span></span> | <span data-ttu-id="de9a1-196">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="de9a1-196">Map Test 2</span></span>                   |
| <span data-ttu-id="de9a1-197">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="de9a1-197">localhost:1234/map3</span></span> | <span data-ttu-id="de9a1-198">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="de9a1-198">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="de9a1-199">Когда используется `Map`, соответствующие сегменты путей удаляются из `HttpRequest.Path` и добавляются к `HttpRequest.PathBase` для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="de9a1-199">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="de9a1-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) осуществляет ветвление конвейера запросов на основе результата заданного предиката.</span><span class="sxs-lookup"><span data-stu-id="de9a1-200">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="de9a1-201">Любой предикат типа `Func<HttpContext, bool>` можно использовать для сопоставления запросов с новой ветвью конвейера.</span><span class="sxs-lookup"><span data-stu-id="de9a1-201">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="de9a1-202">В следующем примере предикат служит для определения наличия переменной строки запроса `branch`.</span><span class="sxs-lookup"><span data-stu-id="de9a1-202">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="de9a1-203">Ниже приведены запросы и отклики `http://localhost:1234` на базе предыдущего кода.</span><span class="sxs-lookup"><span data-stu-id="de9a1-203">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="de9a1-204">Запрос</span><span class="sxs-lookup"><span data-stu-id="de9a1-204">Request</span></span>                       | <span data-ttu-id="de9a1-205">Ответ</span><span class="sxs-lookup"><span data-stu-id="de9a1-205">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="de9a1-206">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="de9a1-206">localhost:1234</span></span>                | <span data-ttu-id="de9a1-207">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="de9a1-207">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="de9a1-208">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="de9a1-208">localhost:1234/?branch=master</span></span> | <span data-ttu-id="de9a1-209">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="de9a1-209">Branch used = master</span></span>         |

<span data-ttu-id="de9a1-210">`Map` поддерживает вложение, например:</span><span class="sxs-lookup"><span data-stu-id="de9a1-210">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="de9a1-211">`Map` также может сопоставить несколько сегментов одновременно:</span><span class="sxs-lookup"><span data-stu-id="de9a1-211">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="de9a1-212">Встроенное ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="de9a1-212">Built-in middleware</span></span>

<span data-ttu-id="de9a1-213">ASP.NET Core содержит следующие компоненты промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="de9a1-213">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="de9a1-214">В столбце *Порядок* указаны сведения о размещении ПО промежуточного слоя в конвейере обработки запросов и условия, в соответствии с которыми ПО промежуточного слоя может прервать обработку запроса.</span><span class="sxs-lookup"><span data-stu-id="de9a1-214">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="de9a1-215">Если промежуточный слой замыкает конвейер обработки запроса и препятствует обработке запроса дальнейшими компонентами промежуточного слоя, он называется *терминальным промежуточным слоем*.</span><span class="sxs-lookup"><span data-stu-id="de9a1-215">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="de9a1-216">Дополнительные сведения о замыкании конвейера см. в разделе [Создание конвейера ПО промежуточного слоя с помощью IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder).</span><span class="sxs-lookup"><span data-stu-id="de9a1-216">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="de9a1-217">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="de9a1-217">Middleware</span></span> | <span data-ttu-id="de9a1-218">Описание</span><span class="sxs-lookup"><span data-stu-id="de9a1-218">Description</span></span> | <span data-ttu-id="de9a1-219">Номер</span><span class="sxs-lookup"><span data-stu-id="de9a1-219">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="de9a1-220">Authentication</span><span class="sxs-lookup"><span data-stu-id="de9a1-220">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="de9a1-221">Обеспечивает поддержку проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="de9a1-221">Provides authentication support.</span></span> | <span data-ttu-id="de9a1-222">Ставится перед тем, как потребуется `HttpContext.User`.</span><span class="sxs-lookup"><span data-stu-id="de9a1-222">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="de9a1-223">Является конечным для обратных вызовов OAuth.</span><span class="sxs-lookup"><span data-stu-id="de9a1-223">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="de9a1-224">Cookie Policy</span><span class="sxs-lookup"><span data-stu-id="de9a1-224">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="de9a1-225">Позволяет отслеживать согласие пользователей на хранение личных сведений и применять минимальные стандарты для полей файлов cookie, таких как `secure` и `SameSite`.</span><span class="sxs-lookup"><span data-stu-id="de9a1-225">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="de9a1-226">Перед ПО промежуточного слоя, которое использует файлы cookie.</span><span class="sxs-lookup"><span data-stu-id="de9a1-226">Before middleware that issues cookies.</span></span> <span data-ttu-id="de9a1-227">Примеры Authentication, Session, MVC (TempData).</span><span class="sxs-lookup"><span data-stu-id="de9a1-227">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="de9a1-228">CORS</span><span class="sxs-lookup"><span data-stu-id="de9a1-228">CORS</span></span>](xref:security/cors) | <span data-ttu-id="de9a1-229">Настраивает общий доступ к ресурсам независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="de9a1-229">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="de9a1-230">Ставится перед компонентами, использующими CORS.</span><span class="sxs-lookup"><span data-stu-id="de9a1-230">Before components that use CORS.</span></span> |
| [<span data-ttu-id="de9a1-231">Обработка исключений</span><span class="sxs-lookup"><span data-stu-id="de9a1-231">Exception Handling</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="de9a1-232">Обрабатывает исключения.</span><span class="sxs-lookup"><span data-stu-id="de9a1-232">Handles exceptions.</span></span> | <span data-ttu-id="de9a1-233">Ставится перед компонентами, выдающими ошибки.</span><span class="sxs-lookup"><span data-stu-id="de9a1-233">Before components that generate errors.</span></span> |
| [<span data-ttu-id="de9a1-234">Forwarded Headers</span><span class="sxs-lookup"><span data-stu-id="de9a1-234">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="de9a1-235">Пересылает заголовки, переданные через прокси-сервер, в текущий запрос.</span><span class="sxs-lookup"><span data-stu-id="de9a1-235">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="de9a1-236">Перед компонентами, использующими обновленные поля.</span><span class="sxs-lookup"><span data-stu-id="de9a1-236">Before components that consume the updated fields.</span></span> <span data-ttu-id="de9a1-237">Например: схема, узел, IP-адрес клиента, метод.</span><span class="sxs-lookup"><span data-stu-id="de9a1-237">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="de9a1-238">Проверка работоспособности</span><span class="sxs-lookup"><span data-stu-id="de9a1-238">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="de9a1-239">Проверяет работоспособность приложения ASP.NET Core и его зависимостей, таких как проверка доступности базы данных.</span><span class="sxs-lookup"><span data-stu-id="de9a1-239">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="de9a1-240">Является конечным, если запрос соответствует конечной точке проверки работоспособности.</span><span class="sxs-lookup"><span data-stu-id="de9a1-240">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="de9a1-241">HTTP Method Override</span><span class="sxs-lookup"><span data-stu-id="de9a1-241">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="de9a1-242">Разрешает входящий запрос POST для переопределения этого метода.</span><span class="sxs-lookup"><span data-stu-id="de9a1-242">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="de9a1-243">Ставится перед компонентами, использующими обновленный метод.</span><span class="sxs-lookup"><span data-stu-id="de9a1-243">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="de9a1-244">HTTPS Redirection</span><span class="sxs-lookup"><span data-stu-id="de9a1-244">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="de9a1-245">Перенаправление всех запросов HTTP на HTTPS (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="de9a1-245">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="de9a1-246">Ставится перед компонентами, использующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="de9a1-246">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="de9a1-247">HTTP Strict Transport Security (HSTS)</span><span class="sxs-lookup"><span data-stu-id="de9a1-247">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="de9a1-248">ПО промежуточного слоя для усовершенствования безопасности, которое добавляет специальный заголовок ответа (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="de9a1-248">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="de9a1-249">Перед отправкой ответов и после компонентов, изменяющих запросы.</span><span class="sxs-lookup"><span data-stu-id="de9a1-249">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="de9a1-250">Примеры Forwarded Headers, URL Rewriting.</span><span class="sxs-lookup"><span data-stu-id="de9a1-250">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="de9a1-251">MVC</span><span class="sxs-lookup"><span data-stu-id="de9a1-251">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="de9a1-252">Обрабатывает запросы с помощью MVC/Razor Pages (ASP.NET Core 2.0 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="de9a1-252">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="de9a1-253">Является конечным, если запрос соответствует маршруту.</span><span class="sxs-lookup"><span data-stu-id="de9a1-253">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="de9a1-254">OWIN</span><span class="sxs-lookup"><span data-stu-id="de9a1-254">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="de9a1-255">Взаимодействие с приложениями, серверами и ПО промежуточного слоя на основе OWIN.</span><span class="sxs-lookup"><span data-stu-id="de9a1-255">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="de9a1-256">Является конечным, если ПО промежуточного слоя OWIN полностью обрабатывает запрос.</span><span class="sxs-lookup"><span data-stu-id="de9a1-256">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="de9a1-257">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="de9a1-257">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="de9a1-258">Обеспечивает поддержку для кэширования откликов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-258">Provides support for caching responses.</span></span> | <span data-ttu-id="de9a1-259">Ставится перед компонентами, требующими кэширование.</span><span class="sxs-lookup"><span data-stu-id="de9a1-259">Before components that require caching.</span></span> |
| [<span data-ttu-id="de9a1-260">Сжатие откликов</span><span class="sxs-lookup"><span data-stu-id="de9a1-260">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="de9a1-261">Обеспечивает поддержку для сжатия откликов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-261">Provides support for compressing responses.</span></span> | <span data-ttu-id="de9a1-262">Ставится перед компонентами, требующими сжатие.</span><span class="sxs-lookup"><span data-stu-id="de9a1-262">Before components that require compression.</span></span> |
| [<span data-ttu-id="de9a1-263">Localization</span><span class="sxs-lookup"><span data-stu-id="de9a1-263">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="de9a1-264">Обеспечивает поддержку локализации.</span><span class="sxs-lookup"><span data-stu-id="de9a1-264">Provides localization support.</span></span> | <span data-ttu-id="de9a1-265">Ставится перед компонентами, для которых важна локализация.</span><span class="sxs-lookup"><span data-stu-id="de9a1-265">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="de9a1-266">Routing</span><span class="sxs-lookup"><span data-stu-id="de9a1-266">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="de9a1-267">Определяет и ограничивает маршруты запросов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-267">Defines and constrains request routes.</span></span> | <span data-ttu-id="de9a1-268">Является конечным для совпадающих маршрутов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-268">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="de9a1-269">Session</span><span class="sxs-lookup"><span data-stu-id="de9a1-269">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="de9a1-270">Обеспечивает поддержку для управления пользовательскими сеансами.</span><span class="sxs-lookup"><span data-stu-id="de9a1-270">Provides support for managing user sessions.</span></span> | <span data-ttu-id="de9a1-271">Ставится перед компонентами, требующими сеанс.</span><span class="sxs-lookup"><span data-stu-id="de9a1-271">Before components that require Session.</span></span> |
| [<span data-ttu-id="de9a1-272">Static Files</span><span class="sxs-lookup"><span data-stu-id="de9a1-272">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="de9a1-273">Обеспечивает поддержку для обработки статических файлов и просмотра каталогов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-273">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="de9a1-274">Является конечным, если запрос соответствует файлу.</span><span class="sxs-lookup"><span data-stu-id="de9a1-274">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="de9a1-275">URL Rewriting</span><span class="sxs-lookup"><span data-stu-id="de9a1-275">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="de9a1-276">Обеспечивает поддержку для переопределения URL-адресов и перенаправления запросов.</span><span class="sxs-lookup"><span data-stu-id="de9a1-276">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="de9a1-277">Ставится перед компонентами, использующими URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="de9a1-277">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="de9a1-278">WebSockets</span><span class="sxs-lookup"><span data-stu-id="de9a1-278">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="de9a1-279">Обеспечивает поддержку протокола WebSockets.</span><span class="sxs-lookup"><span data-stu-id="de9a1-279">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="de9a1-280">Ставится перед компонентами, которым нужно принимать запросы WebSocket.</span><span class="sxs-lookup"><span data-stu-id="de9a1-280">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="de9a1-281">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="de9a1-281">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
