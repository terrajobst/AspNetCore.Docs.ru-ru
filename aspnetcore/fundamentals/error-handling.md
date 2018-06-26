---
title: Обработка ошибок в ASP.NET Core
author: ardalis
description: Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273713"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="71e8e-103">Обработка ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="71e8e-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="71e8e-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra/) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="71e8e-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="71e8e-105">В этой статье рассматриваются основные методы обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="71e8e-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="71e8e-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="71e8e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="71e8e-107">Страница со сведениями об исключении для разработчика</span><span class="sxs-lookup"><span data-stu-id="71e8e-107">The developer exception page</span></span>

<span data-ttu-id="71e8e-108">Чтобы настроить вывод в приложении страницы с подробными сведениями об исключениях, установите пакет NuGet `Microsoft.AspNetCore.Diagnostics` и добавьте строку в [метод Configure в классе Startup](xref:fundamentals/startup):</span><span class="sxs-lookup"><span data-stu-id="71e8e-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="71e8e-109">Поместите метод `UseDeveloperExceptionPage` перед каждым промежуточным слоем, в котором нужно перехватывать исключения, например `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="71e8e-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="71e8e-110">Включать страницу со сведениями об исключении для разработчика следует **только тогда, когда приложение выполняется в среде разработки**.</span><span class="sxs-lookup"><span data-stu-id="71e8e-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="71e8e-111">Подробные сведения об исключениях не должны быть общедоступными при выполнении приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="71e8e-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="71e8e-112">[Дополнительные сведения о настройке сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="71e8e-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="71e8e-113">Чтобы увидеть страницу со сведениями об исключении для разработчика, запустите образец приложения, указав среду `Development`, и добавьте элемент `?throw=true` к базовому URL-адресу приложения.</span><span class="sxs-lookup"><span data-stu-id="71e8e-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="71e8e-114">Страница содержит несколько вкладок со сведениями об исключении и запросе.</span><span class="sxs-lookup"><span data-stu-id="71e8e-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="71e8e-115">На первой вкладке приводится трассировка стека.</span><span class="sxs-lookup"><span data-stu-id="71e8e-115">The first tab includes a stack trace.</span></span> 

![Трассировка стека](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="71e8e-117">На следующей вкладке представлены параметры строки запроса, если они имеются.</span><span class="sxs-lookup"><span data-stu-id="71e8e-117">The next tab shows the query string parameters, if any.</span></span>

![Параметры строки запроса](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="71e8e-119">У этого запроса не было файлов cookie, но если бы они были, они бы отображались на вкладке **Cookies** (Файлы cookie). На последней вкладке можно просмотреть переданные заголовки.</span><span class="sxs-lookup"><span data-stu-id="71e8e-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Заголовки](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="71e8e-121">Настройка пользовательской страницы обработки исключений</span><span class="sxs-lookup"><span data-stu-id="71e8e-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="71e8e-122">Если приложение выполняется не в среде `Development`, настройте страницу обработчика исключений.</span><span class="sxs-lookup"><span data-stu-id="71e8e-122">Configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="71e8e-123">В приложении Razor Pages шаблон Razor Pages [dotnet new](/dotnet/core/tools/dotnet-new) предоставляет страницу ошибок и класс модели страницы `ErrorModel` в папке *Pages*.</span><span class="sxs-lookup"><span data-stu-id="71e8e-123">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="71e8e-124">В приложении MVC не следует добавлять в метод действия обработки ошибок атрибуты метода HTTP, например `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="71e8e-124">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="71e8e-125">Из-за использования явных команд некоторые запросы могут не передаваться в метод.</span><span class="sxs-lookup"><span data-stu-id="71e8e-125">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="71e8e-126">Разрешите анонимный доступ к методу, чтобы не прошедшие проверку подлинности пользователи могли открывать представление ошибок.</span><span class="sxs-lookup"><span data-stu-id="71e8e-126">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="71e8e-127">Например, следующий метод обработчика ошибок предоставляется шаблоном MVC [dotnet new](/dotnet/core/tools/dotnet-new) и отображается в контроллере Home:</span><span class="sxs-lookup"><span data-stu-id="71e8e-127">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="71e8e-128">Настройка страниц с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="71e8e-128">Configuring status code pages</span></span>

<span data-ttu-id="71e8e-129">По умолчанию приложение не предоставляет специальных страниц для кодов состояния HTTP, таких как код *404 Не найдено*.</span><span class="sxs-lookup"><span data-stu-id="71e8e-129">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="71e8e-130">Чтобы предоставить страницы кодов состояния, настройте ПО промежуточного слоя страниц кода состояния, добавив строку в метод `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="71e8e-130">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="71e8e-131">По умолчанию это ПО промежуточного слоя добавляет простые текстовые обработчики для распространенных кодов состояния, например 404:</span><span class="sxs-lookup"><span data-stu-id="71e8e-131">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![Страница 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="71e8e-133">ПО промежуточного слоя поддерживает несколько методов расширения.</span><span class="sxs-lookup"><span data-stu-id="71e8e-133">The middleware supports several extension methods.</span></span> <span data-ttu-id="71e8e-134">Один метод принимает лямбда-выражение:</span><span class="sxs-lookup"><span data-stu-id="71e8e-134">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="71e8e-135">Другой метод принимает строку формата и типа содержимого:</span><span class="sxs-lookup"><span data-stu-id="71e8e-135">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="71e8e-136">Имеются также методы расширения для перенаправления и повторного выполнения.</span><span class="sxs-lookup"><span data-stu-id="71e8e-136">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="71e8e-137">Метод перенаправления отправляет клиенту код состояния 302:</span><span class="sxs-lookup"><span data-stu-id="71e8e-137">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="71e8e-138">Метод повторного выполнения возвращает исходный код состояния клиенту, а также выполняет обработчик для URL-адреса перенаправления:</span><span class="sxs-lookup"><span data-stu-id="71e8e-138">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="71e8e-139">Можно отключить страницы кодов состояния для конкретных запросов в методе обработчика Razor Pages или в контроллере MVC.</span><span class="sxs-lookup"><span data-stu-id="71e8e-139">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="71e8e-140">Чтобы отключить страницы кодов состояния, попробуйте извлечь [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) из коллекции запроса [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) и отключить эту функцию, если она доступна:</span><span class="sxs-lookup"><span data-stu-id="71e8e-140">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="71e8e-141">Если вы используете перегрузку `UseStatusCodePages*`, которая указывает на конечную точку в приложении, создайте представление MVC или страницу Razor для конечной точки.</span><span class="sxs-lookup"><span data-stu-id="71e8e-141">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="71e8e-142">Например, шаблон [dotnet new](/dotnet/core/tools/dotnet-new) для приложения Razor Pages создает следующую страницу и класс модели страницы:</span><span class="sxs-lookup"><span data-stu-id="71e8e-142">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="71e8e-143">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="71e8e-143">*Error.cshtml*:</span></span>

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
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="71e8e-144">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="71e8e-144">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="71e8e-145">Код обработки исключений</span><span class="sxs-lookup"><span data-stu-id="71e8e-145">Exception-handling code</span></span>

<span data-ttu-id="71e8e-146">Код на страницах обработки исключений может создавать исключения.</span><span class="sxs-lookup"><span data-stu-id="71e8e-146">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="71e8e-147">Часто желательно, чтобы страницы ошибок в рабочей среде содержали только статическое содержимое.</span><span class="sxs-lookup"><span data-stu-id="71e8e-147">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="71e8e-148">Кроме того, имейте в виду, что после отправки заголовков для ответа нельзя изменить код состояния ответа, а также невозможно выполнение страниц или обработчиков исключений.</span><span class="sxs-lookup"><span data-stu-id="71e8e-148">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="71e8e-149">Необходимо завершить ответ или прервать подключение.</span><span class="sxs-lookup"><span data-stu-id="71e8e-149">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="71e8e-150">Обработка исключений на сервере</span><span class="sxs-lookup"><span data-stu-id="71e8e-150">Server exception handling</span></span>

<span data-ttu-id="71e8e-151">Помимо логики обработки исключений в приложении, [сервер](xref:fundamentals/servers/index), на котором размещается приложение, также выполняет ряд операций по обработке исключений.</span><span class="sxs-lookup"><span data-stu-id="71e8e-151">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="71e8e-152">Если сервер перехватывает исключение перед отправкой заголовков, он отсылает ответ *500 Внутренняя ошибка сервера* без текста.</span><span class="sxs-lookup"><span data-stu-id="71e8e-152">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="71e8e-153">Если сервер перехватывает исключение после отправки заголовков, он закрывает соединение.</span><span class="sxs-lookup"><span data-stu-id="71e8e-153">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="71e8e-154">Запросы, не обработанные приложением, обрабатываются сервером.</span><span class="sxs-lookup"><span data-stu-id="71e8e-154">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="71e8e-155">Все возникшие исключения обрабатываются с помощью механизма обработки исключений на сервере.</span><span class="sxs-lookup"><span data-stu-id="71e8e-155">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="71e8e-156">Настроенные пользовательские страницы ошибок, промежуточный слой обработки исключений и фильтры не влияют на этот процесс.</span><span class="sxs-lookup"><span data-stu-id="71e8e-156">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="71e8e-157">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="71e8e-157">Startup exception handling</span></span>

<span data-ttu-id="71e8e-158">Исключения, возникающие во время запуска приложения, могут обрабатываться только на уровне размещения.</span><span class="sxs-lookup"><span data-stu-id="71e8e-158">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="71e8e-159">При помощи [веб-узла](xref:fundamentals/host/web-host) вы можете [настроить реакцию узла на ошибки при запуске](xref:fundamentals/host/web-host#detailed-errors) с помощью ключей `captureStartupErrors` и `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="71e8e-159">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="71e8e-160">Узел может отображать страницу со сведениями о перехваченной ошибке при запуске, только если ошибка произошла после привязки адреса и порта узла.</span><span class="sxs-lookup"><span data-stu-id="71e8e-160">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="71e8e-161">Если выполнить привязку по какой-либо причине не удается, уровень размещения записывает в журнал критическое исключение, происходит аварийное завершение процесса dotnet и страница со сведениями об ошибке не выводится, когда приложение запущено на сервере [Kestrel](xref:fundamentals/servers/kestrel).</span><span class="sxs-lookup"><span data-stu-id="71e8e-161">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="71e8e-162">При работе в службе [IIS](/iis) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) возвращает ошибку *502.5 Сбой процесса*, если процесс невозможно запустить.</span><span class="sxs-lookup"><span data-stu-id="71e8e-162">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="71e8e-163">Следуйте рекомендациям по устранению неполадок из раздела [Устранение неполадок ASP.NET Core в службах IIS](xref:host-and-deploy/iis/troubleshoot).</span><span class="sxs-lookup"><span data-stu-id="71e8e-163">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="71e8e-164">Обработка ошибок ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="71e8e-164">ASP.NET MVC error handling</span></span>

<span data-ttu-id="71e8e-165">В приложениях [MVC](xref:mvc/overview) есть дополнительные возможности обработки ошибок, например настройка фильтров исключений и проведение проверки модели.</span><span class="sxs-lookup"><span data-stu-id="71e8e-165">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="71e8e-166">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="71e8e-166">Exception Filters</span></span>

<span data-ttu-id="71e8e-167">Фильтры исключений можно настраивать глобально либо для отдельных контроллеров, либо для действий в приложении MVC.</span><span class="sxs-lookup"><span data-stu-id="71e8e-167">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="71e8e-168">Эти фильтры обрабатывают все необработанные исключения, которые возникают во время выполнения действия контроллера или другого фильтра. Иным образом они не вызываются.</span><span class="sxs-lookup"><span data-stu-id="71e8e-168">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="71e8e-169">Дополнительные сведения о фильтрах исключений см. в статье [Фильтры](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="71e8e-169">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="71e8e-170">Фильтры исключений хорошо подходят для перехвата исключений, которые происходят в действиях MVC, однако они не так гибки, как ПО промежуточного слоя для обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="71e8e-170">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="71e8e-171">ПО промежуточного слоя предпочтительнее для общих случаев. Фильтры следует использовать только тогда, когда обработка ошибок должна осуществляться *по-разному* в зависимости от выбранного действия MVC.</span><span class="sxs-lookup"><span data-stu-id="71e8e-171">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="71e8e-172">Обработка ошибок состояния модели</span><span class="sxs-lookup"><span data-stu-id="71e8e-172">Handling Model State Errors</span></span>

<span data-ttu-id="71e8e-173">[Проверка модели](xref:mvc/models/validation) проводится перед вызовом каждого действия контроллера. Метод действия отвечает за проверку свойства `ModelState.IsValid` и соответствующую реакцию.</span><span class="sxs-lookup"><span data-stu-id="71e8e-173">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="71e8e-174">В некоторых приложениях соблюдается стандартное соглашение об обработке ошибок проверки модели. В этом случае подходящим местом для реализации такой политики может быть [фильтр](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="71e8e-174">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="71e8e-175">Следует проверить, как выполняются действия при недопустимых состояниях модели.</span><span class="sxs-lookup"><span data-stu-id="71e8e-175">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="71e8e-176">Дополнительные сведения см. в статье [Тестирование логики контроллера](xref:mvc/controllers/testing).</span><span class="sxs-lookup"><span data-stu-id="71e8e-176">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>
