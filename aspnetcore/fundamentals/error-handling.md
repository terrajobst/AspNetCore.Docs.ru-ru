---
title: Обработка ошибок в ASP.NET Core
author: ardalis
description: Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: d1e94fdc89fbebc264dc001bbf35666af16f4799
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50208035"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="28c37-103">Обработка ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28c37-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="28c37-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra/) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="28c37-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="28c37-105">В этой статье рассматриваются основные методы обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="28c37-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="28c37-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="28c37-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="28c37-107">Страница исключений для разработчика</span><span class="sxs-lookup"><span data-stu-id="28c37-107">The Developer Exception Page</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="28c37-108">Чтобы настроить приложение для отображения страницы с подробными сведениями об исключениях, используйте *страницу исключений для разработчика*.</span><span class="sxs-lookup"><span data-stu-id="28c37-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="28c37-109">Эта страница доступна в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="28c37-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="28c37-110">Добавьте строку в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="28c37-110">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="28c37-111">Чтобы настроить приложение для отображения страницы с подробными сведениями об исключениях, используйте *страницу исключений для разработчика*.</span><span class="sxs-lookup"><span data-stu-id="28c37-111">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="28c37-112">Эта страница доступна в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="28c37-112">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span> <span data-ttu-id="28c37-113">Добавьте строку в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="28c37-113">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28c37-114">Чтобы настроить приложение для отображения страницы с подробными сведениями об исключениях, используйте *страницу исключений для разработчика*.</span><span class="sxs-lookup"><span data-stu-id="28c37-114">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="28c37-115">Чтобы использовать страницу, добавьте ссылку на пакет [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="28c37-115">The page is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span> <span data-ttu-id="28c37-116">Добавьте строку в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="28c37-116">Add a line to the `Startup.Configure` method:</span></span>

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="28c37-117">Поместите вызов к [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) перед другим ПО промежуточного слоя, где требуется перехватывать исключения, например `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="28c37-117">Place the call to [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) in front of any middleware where you want to catch exceptions, such as `app.UseMvc`.</span></span>

> [!WARNING]
> <span data-ttu-id="28c37-118">Включать страницу исключений для разработчика следует **только тогда, когда приложение выполняется в среде разработки**.</span><span class="sxs-lookup"><span data-stu-id="28c37-118">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="28c37-119">Подробные сведения об исключениях не должны быть общедоступными при выполнении приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="28c37-119">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="28c37-120">[Дополнительные сведения о настройке сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="28c37-120">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="28c37-121">Чтобы просмотреть страницу исключений для разработчика, запустите образец приложения, указав среду `Development`, и добавьте элемент `?throw=true` к базовому URL-адресу приложения.</span><span class="sxs-lookup"><span data-stu-id="28c37-121">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="28c37-122">Страница содержит несколько вкладок со сведениями об исключении и запросе.</span><span class="sxs-lookup"><span data-stu-id="28c37-122">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="28c37-123">На первой вкладке приводится трассировка стека.</span><span class="sxs-lookup"><span data-stu-id="28c37-123">The first tab includes a stack trace:</span></span>

![Трассировка стека](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="28c37-125">На следующей вкладке представлены параметры строки запроса, если они имеются.</span><span class="sxs-lookup"><span data-stu-id="28c37-125">The next tab shows the query string parameters, if any:</span></span>

![Параметры строки запроса](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="28c37-127">Если запрос содержит файлы cookie, они отображаются на вкладке **Cookie**. Заголовки отображаются на последней вкладке.</span><span class="sxs-lookup"><span data-stu-id="28c37-127">If the request has cookies, they appear on the **Cookies** tab. Headers are seen in the last tab:</span></span>

![Заголовки](error-handling/_static/developer-exception-page-headers.png)

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="28c37-129">Настройка пользовательской страницы обработки исключений</span><span class="sxs-lookup"><span data-stu-id="28c37-129">Configure a custom exception handling page</span></span>

<span data-ttu-id="28c37-130">Если приложение выполняется не в среде `Development`, настройте страницу обработчика исключений.</span><span class="sxs-lookup"><span data-stu-id="28c37-130">Configure an exception handler page to use when the app isn't running in the `Development` environment:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="28c37-131">В приложении Razor Pages шаблон Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) предоставляет страницу ошибок и класс ошибок `PageModel` в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="28c37-131">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and an error `PageModel` class in the *Pages* folder.</span></span>

<span data-ttu-id="28c37-132">В приложении MVC не следует добавлять в метод действия обработки ошибок атрибуты метода HTTP, например `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="28c37-132">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="28c37-133">Из-за использования явных команд некоторые запросы могут не передаваться в метод.</span><span class="sxs-lookup"><span data-stu-id="28c37-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="28c37-134">Разрешите анонимный доступ к методу, чтобы не прошедшие проверку подлинности пользователи могли открывать представление ошибок.</span><span class="sxs-lookup"><span data-stu-id="28c37-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="28c37-135">Например, следующий метод обработчика ошибок предоставляется шаблоном MVC [dotnet new](/dotnet/core/tools/dotnet-new) и отображается в контроллере Home:</span><span class="sxs-lookup"><span data-stu-id="28c37-135">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configure-status-code-pages"></a><span data-ttu-id="28c37-136">Настройка страниц с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="28c37-136">Configure status code pages</span></span>

<span data-ttu-id="28c37-137">По умолчанию приложение не предоставляет специальных страниц для кодов состояния HTTP, таких как код *404 Не найдено*.</span><span class="sxs-lookup"><span data-stu-id="28c37-137">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="28c37-138">Чтобы указать коды состояния, используйте ПО промежуточного слоя страниц с кодами состояния.</span><span class="sxs-lookup"><span data-stu-id="28c37-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="28c37-139">Это ПО промежуточного слоя доступно в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="28c37-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="28c37-140">Это ПО промежуточного слоя доступно в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="28c37-140">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="28c37-141">Чтобы сделать это ПО промежуточного слоя доступным, добавьте ссылку на пакет [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="28c37-141">The middleware is made available by adding a package reference for the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package in the project file.</span></span>

::: moniker-end

<span data-ttu-id="28c37-142">Добавьте строку в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="28c37-142">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="28c37-143"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> должен вызываться перед ПО промежуточного слоя, обрабатывающим запрос в конвейере (например, ПО промежуточного слоя статических файлов и ПО промежуточного слоя MVC).</span><span class="sxs-lookup"><span data-stu-id="28c37-143"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> should be called before request handling middlewares in the pipeline (for example, Static Files Middleware and MVC Middleware).</span></span>

<span data-ttu-id="28c37-144">По умолчанию это ПО промежуточного слоя добавляет текстовые обработчики для распространенных кодов состояния, например 404.</span><span class="sxs-lookup"><span data-stu-id="28c37-144">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as 404:</span></span>

![Страница 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="28c37-146">ПО промежуточного слоя поддерживает несколько методов расширения.</span><span class="sxs-lookup"><span data-stu-id="28c37-146">The middleware supports several extension methods.</span></span> <span data-ttu-id="28c37-147">Один метод принимает лямбда-выражение:</span><span class="sxs-lookup"><span data-stu-id="28c37-147">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="28c37-148">Другой метод принимает строку формата и типа содержимого:</span><span class="sxs-lookup"><span data-stu-id="28c37-148">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="28c37-149">Имеются также методы расширения для перенаправления и повторного выполнения.</span><span class="sxs-lookup"><span data-stu-id="28c37-149">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="28c37-150">Метод перенаправления отправляет клиенту код состояния *302 Найдено* и перенаправляет клиент к указанному шаблону URL-адреса расположения.</span><span class="sxs-lookup"><span data-stu-id="28c37-150">The redirect method sends a *302 Found* status code to the client and redirects the client to the provided location URL template.</span></span> <span data-ttu-id="28c37-151">Шаблон может содержать заполнитель `{0}` для кода состояния.</span><span class="sxs-lookup"><span data-stu-id="28c37-151">The template may include a `{0}` placeholder for the status code.</span></span> <span data-ttu-id="28c37-152">В начало URL-адресов, начинающихся с `~`, добавлен базовый путь.</span><span class="sxs-lookup"><span data-stu-id="28c37-152">URLs starting with `~` have the base path prepended.</span></span> <span data-ttu-id="28c37-153">URL-адрес, который не начинается с `~`, используется как есть.</span><span class="sxs-lookup"><span data-stu-id="28c37-153">A URL that doesn't start with `~` is used as is.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="28c37-154">Метод повторного выполнения возвращает клиенту исходный код состояния и указывает, что текст ответа должен создаваться путем повторного выполнения конвейера запросов с использованием другого пути.</span><span class="sxs-lookup"><span data-stu-id="28c37-154">The re-execute method returns the original status code to the client and specifies that the response body should be generated by re-executing the request pipeline using an alternate path.</span></span> <span data-ttu-id="28c37-155">Этот путь может содержать заполнитель `{0}` для кода состояния:</span><span class="sxs-lookup"><span data-stu-id="28c37-155">This path may contain a `{0}` placeholder for the status code:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="28c37-156">Можно отключить страницы кодов состояния для конкретных запросов в методе обработчика Razor Pages или в контроллере MVC.</span><span class="sxs-lookup"><span data-stu-id="28c37-156">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="28c37-157">Чтобы отключить страницы кодов состояния, попробуйте извлечь [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) из коллекции запроса [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) и отключить эту функцию, если она доступна:</span><span class="sxs-lookup"><span data-stu-id="28c37-157">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="28c37-158">Если вы используете перегрузку `UseStatusCodePages*`, которая указывает на конечную точку в приложении, создайте представление MVC или страницу Razor для конечной точки.</span><span class="sxs-lookup"><span data-stu-id="28c37-158">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="28c37-159">Например, шаблон [dotnet new](/dotnet/core/tools/dotnet-new) для приложения Razor Pages создает следующую страницу и класс модели страницы:</span><span class="sxs-lookup"><span data-stu-id="28c37-159">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="28c37-160">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="28c37-160">*Error.cshtml*:</span></span>

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="28c37-161">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="28c37-161">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="28c37-162">Код обработки исключений</span><span class="sxs-lookup"><span data-stu-id="28c37-162">Exception-handling code</span></span>

<span data-ttu-id="28c37-163">Код на страницах обработки исключений может создавать исключения.</span><span class="sxs-lookup"><span data-stu-id="28c37-163">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="28c37-164">Часто желательно, чтобы страницы ошибок в рабочей среде содержали только статическое содержимое.</span><span class="sxs-lookup"><span data-stu-id="28c37-164">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="28c37-165">Кроме того, имейте в виду, что после отправки заголовков для ответа нельзя изменить код состояния ответа, а также невозможно выполнение страниц или обработчиков исключений.</span><span class="sxs-lookup"><span data-stu-id="28c37-165">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="28c37-166">Необходимо завершить ответ или прервать подключение.</span><span class="sxs-lookup"><span data-stu-id="28c37-166">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="28c37-167">Обработка исключений на сервере</span><span class="sxs-lookup"><span data-stu-id="28c37-167">Server exception handling</span></span>

<span data-ttu-id="28c37-168">Помимо логики обработки исключений в приложении, [сервер](xref:fundamentals/servers/index), на котором размещается приложение, также выполняет ряд операций по обработке исключений.</span><span class="sxs-lookup"><span data-stu-id="28c37-168">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="28c37-169">Если сервер перехватывает исключение перед отправкой заголовков, он отсылает ответ *500 Внутренняя ошибка сервера* без текста.</span><span class="sxs-lookup"><span data-stu-id="28c37-169">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="28c37-170">Если сервер перехватывает исключение после отправки заголовков, он закрывает соединение.</span><span class="sxs-lookup"><span data-stu-id="28c37-170">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="28c37-171">Запросы, не обработанные приложением, обрабатываются сервером.</span><span class="sxs-lookup"><span data-stu-id="28c37-171">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="28c37-172">Все возникшие исключения обрабатываются с помощью механизма обработки исключений на сервере.</span><span class="sxs-lookup"><span data-stu-id="28c37-172">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="28c37-173">Настроенные пользовательские страницы ошибок, промежуточный слой обработки исключений и фильтры не влияют на этот процесс.</span><span class="sxs-lookup"><span data-stu-id="28c37-173">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="28c37-174">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="28c37-174">Startup exception handling</span></span>

<span data-ttu-id="28c37-175">Исключения, возникающие во время запуска приложения, могут обрабатываться только на уровне размещения.</span><span class="sxs-lookup"><span data-stu-id="28c37-175">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="28c37-176">При помощи [веб-узла](xref:fundamentals/host/web-host) вы можете [настроить реакцию узла на ошибки при запуске](xref:fundamentals/host/web-host#detailed-errors) с помощью ключей `captureStartupErrors` и `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="28c37-176">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="28c37-177">Узел может отображать страницу со сведениями о перехваченной ошибке при запуске, только если ошибка произошла после привязки адреса и порта узла.</span><span class="sxs-lookup"><span data-stu-id="28c37-177">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="28c37-178">Если выполнить привязку по какой-либо причине не удается, уровень размещения записывает в журнал критическое исключение, происходит аварийное завершение процесса dotnet и страница со сведениями об ошибке не выводится, когда приложение запущено на сервере [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="28c37-178">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="28c37-179">При работе в службе [IIS](/iis) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) возвращает ошибку *502.5 Сбой процесса*, если процесс невозможно запустить.</span><span class="sxs-lookup"><span data-stu-id="28c37-179">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="28c37-180">Сведения об устранении неполадок при запуске при размещении в службах IIS см. в статье <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="28c37-180">For information on troubleshooting startup issues when hosting with IIS, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="28c37-181">Сведения об устранении неполадок при запуске в службе приложений Azure см. в статье <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="28c37-181">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="28c37-182">Обработка ошибок в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="28c37-182">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="28c37-183">В приложениях [MVC](xref:mvc/overview) есть дополнительные возможности обработки ошибок, например настройка фильтров исключений и проведение проверки модели.</span><span class="sxs-lookup"><span data-stu-id="28c37-183">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="28c37-184">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="28c37-184">Exception filters</span></span>

<span data-ttu-id="28c37-185">Фильтры исключений можно настраивать глобально либо для отдельных контроллеров, либо для действий в приложении MVC.</span><span class="sxs-lookup"><span data-stu-id="28c37-185">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="28c37-186">Эти фильтры обрабатывают все необработанные исключения, которые возникают во время выполнения действия контроллера или другого фильтра.</span><span class="sxs-lookup"><span data-stu-id="28c37-186">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="28c37-187">Иным образом они не вызываются.</span><span class="sxs-lookup"><span data-stu-id="28c37-187">These filters aren't called otherwise.</span></span> <span data-ttu-id="28c37-188">Дополнительные сведения см. в разделе [Фильтры](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="28c37-188">To learn more, see [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="28c37-189">Фильтры исключений хорошо подходят для перехвата исключений, которые происходят в действиях MVC, однако они не так гибки, как ПО промежуточного слоя для обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="28c37-189">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="28c37-190">Лучше выбирать ПО промежуточного слоя и использовать фильтры только тогда, когда обработка ошибок должна осуществляться *по-разному* в зависимости от выбранного действия MVC.</span><span class="sxs-lookup"><span data-stu-id="28c37-190">Generally prefer the use of middleware, and use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="28c37-191">Обработка ошибок состояния модели</span><span class="sxs-lookup"><span data-stu-id="28c37-191">Handling model state errors</span></span>

<span data-ttu-id="28c37-192">[Проверка модели](xref:mvc/models/validation) проводится перед вызовом каждого действия контроллера. Метод действия отвечает за проверку свойства `ModelState.IsValid` и соответствующую реакцию.</span><span class="sxs-lookup"><span data-stu-id="28c37-192">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="28c37-193">В некоторых приложениях соблюдается стандартное соглашение об обработке ошибок проверки модели. В этом случае подходящим местом для реализации такой политики может быть [фильтр](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="28c37-193">Some apps choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="28c37-194">Следует проверить, как выполняются действия при недопустимых состояниях модели.</span><span class="sxs-lookup"><span data-stu-id="28c37-194">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="28c37-195">Дополнительные сведения см. в статье [Тестирование логики контроллера](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="28c37-195">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28c37-196">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="28c37-196">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
