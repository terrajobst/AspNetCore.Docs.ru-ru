---
title: Обработка ошибок в ASP.NET Core
author: tdykstra
description: Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345509"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="d0170-103">Обработка ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d0170-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="d0170-104">Авторы: [Том Дикстра](https://github.com/tdykstra/) (Tom Dykstra), [Люк Лэтем](https://github.com/guardrex) (Luke Latham) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="d0170-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d0170-105">В этой статье рассматриваются основные методы обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d0170-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="d0170-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d0170-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="d0170-107">Страница со сведениями об исключении для разработчика</span><span class="sxs-lookup"><span data-stu-id="d0170-107">Developer Exception Page</span></span>

<span data-ttu-id="d0170-108">Чтобы настроить приложение для отображения страницы с подробными сведениями об исключениях, используйте *страницу исключений для разработчика*.</span><span class="sxs-lookup"><span data-stu-id="d0170-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="d0170-109">Эта страница доступна в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d0170-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="d0170-110">Добавьте строку в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d0170-110">Add a line to the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

<span data-ttu-id="d0170-111">Поместите вызов к <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> перед другим ПО промежуточного слоя, где требуется перехватывать исключения.</span><span class="sxs-lookup"><span data-stu-id="d0170-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="d0170-112">Включать страницу исключений для разработчика следует **только тогда, когда приложение выполняется в среде разработки**.</span><span class="sxs-lookup"><span data-stu-id="d0170-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="d0170-113">Подробные сведения об исключениях не должны быть общедоступными при выполнении приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d0170-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="d0170-114">Дополнительные сведения о настройке среды см. в статье <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="d0170-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="d0170-115">Чтобы просмотреть страницу исключений для разработчика, запустите образец приложения, указав среду `Development`, и добавьте элемент `?throw=true` к базовому URL-адресу приложения.</span><span class="sxs-lookup"><span data-stu-id="d0170-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="d0170-116">Страница содержит следующие сведения об исключении и запросе:</span><span class="sxs-lookup"><span data-stu-id="d0170-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="d0170-117">Трассировка стека</span><span class="sxs-lookup"><span data-stu-id="d0170-117">Stack trace</span></span>
* <span data-ttu-id="d0170-118">параметры строки запроса (при наличии);</span><span class="sxs-lookup"><span data-stu-id="d0170-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="d0170-119">файлы cookie (при наличии);</span><span class="sxs-lookup"><span data-stu-id="d0170-119">Cookies (if any)</span></span>
* <span data-ttu-id="d0170-120">Заголовки</span><span class="sxs-lookup"><span data-stu-id="d0170-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="d0170-121">Настройка пользовательской страницы обработки исключений</span><span class="sxs-lookup"><span data-stu-id="d0170-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="d0170-122">Когда приложение не выполняется в среде разработки, вызовите метод расширения <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>, чтобы добавить ПО промежуточного слоя для обработки исключений.</span><span class="sxs-lookup"><span data-stu-id="d0170-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="d0170-123">ПО промежуточного слоя выполняет такие функции:</span><span class="sxs-lookup"><span data-stu-id="d0170-123">The middleware:</span></span>

* <span data-ttu-id="d0170-124">Перехватывает исключения.</span><span class="sxs-lookup"><span data-stu-id="d0170-124">Catches exceptions.</span></span>
* <span data-ttu-id="d0170-125">Ведет журнал исключений.</span><span class="sxs-lookup"><span data-stu-id="d0170-125">Logs exceptions.</span></span>
* <span data-ttu-id="d0170-126">Повторно выполняет запрос в альтернативном конвейере для указанной страницы или контроллера.</span><span class="sxs-lookup"><span data-stu-id="d0170-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="d0170-127">Запрос не выполняется повторно, если запущен отклик.</span><span class="sxs-lookup"><span data-stu-id="d0170-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="d0170-128">В следующем примере приложения с помощью <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> добавляется ПО промежуточного слоя для обработки исключений в средах, не предназначенных для разработки.</span><span class="sxs-lookup"><span data-stu-id="d0170-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="d0170-129">Метод расширения определяет страницу ошибки или контроллер в конечной точке `/Error` для повторного выполнения запросов после перехвата исключений и их регистрации в журнале.</span><span class="sxs-lookup"><span data-stu-id="d0170-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

<span data-ttu-id="d0170-130">Шаблон приложения Razor Pages предоставляет страницу ошибки (*.cshtml*) и <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> класс (`ErrorModel`) в папке Pages.</span><span class="sxs-lookup"><span data-stu-id="d0170-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="d0170-131">В приложении MVC приведенный ниже метод обработчика ошибок добавлен в шаблон приложения MVC. Он отображается в контроллере Home.</span><span class="sxs-lookup"><span data-stu-id="d0170-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="d0170-132">Не следует добавлять в метод действия обработки ошибок атрибуты метода HTTP, например `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="d0170-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="d0170-133">Из-за использования явных команд некоторые запросы могут не передаваться в метод.</span><span class="sxs-lookup"><span data-stu-id="d0170-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="d0170-134">Разрешите анонимный доступ к методу, чтобы не прошедшие проверку подлинности пользователи могли открывать представление ошибок.</span><span class="sxs-lookup"><span data-stu-id="d0170-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="d0170-135">Настройка страниц с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="d0170-135">Configure status code pages</span></span>

<span data-ttu-id="d0170-136">По умолчанию приложение ASP.NET Core не предоставляет страницы для кодов состояния HTTP, таких как код *404 Not Found* (не найдено).</span><span class="sxs-lookup"><span data-stu-id="d0170-136">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="d0170-137">Приложение возвращает код состояния без текста ответа.</span><span class="sxs-lookup"><span data-stu-id="d0170-137">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="d0170-138">Чтобы указать коды состояния, используйте ПО промежуточного слоя страниц с кодами состояния.</span><span class="sxs-lookup"><span data-stu-id="d0170-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="d0170-139">Это ПО промежуточного слоя доступно в пакете [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) в [метапакете Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="d0170-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="d0170-140">Добавьте строку в метод `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d0170-140">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="d0170-141">Вызовите метод <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> перед ПО промежуточного слоя для обработки запросов (например ПО промежуточного слоя для статических файлов и ПО промежуточного слоя MVC).</span><span class="sxs-lookup"><span data-stu-id="d0170-141">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="d0170-142">По умолчанию это ПО промежуточного слоя добавляет текстовые обработчики для распространенных кодов состояния, например *404 Not Found* (не найдено).</span><span class="sxs-lookup"><span data-stu-id="d0170-142">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="d0170-143">Это ПО промежуточного слоя поддерживает несколько методов расширения, которые позволяют настроить его поведение.</span><span class="sxs-lookup"><span data-stu-id="d0170-143">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="d0170-144">Перегрузка <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> принимает лямбда-выражение, которое можно использовать для отработки пользовательской логики обработки ошибок и записи ответа вручную.</span><span class="sxs-lookup"><span data-stu-id="d0170-144">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="d0170-145">Перегрузка <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> принимает тип содержимого и строку формата, которые можно использовать для настройки типа содержимого и текста ответа.</span><span class="sxs-lookup"><span data-stu-id="d0170-145">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="d0170-146">Методы расширения для перенаправления и повторного выполнения</span><span class="sxs-lookup"><span data-stu-id="d0170-146">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="d0170-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span><span class="sxs-lookup"><span data-stu-id="d0170-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="d0170-148">Отправляет клиенту код состояния *302 — Found*.</span><span class="sxs-lookup"><span data-stu-id="d0170-148">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="d0170-149">Перенаправляет клиента к расположению, предоставленному в шаблоне URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="d0170-149">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="d0170-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> обычно используется, если приложение:</span><span class="sxs-lookup"><span data-stu-id="d0170-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="d0170-151">Должно перенаправлять клиент в другую конечную точку, что обычно бывает в случаях, когда другое приложение обрабатывает ошибку.</span><span class="sxs-lookup"><span data-stu-id="d0170-151">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="d0170-152">Для веб-приложений в адресной строке браузера клиента отображается конечная точка перенаправления.</span><span class="sxs-lookup"><span data-stu-id="d0170-152">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="d0170-153">Не должно сохранять и возвращать исходный код состояния с ответом первичного перенаправления.</span><span class="sxs-lookup"><span data-stu-id="d0170-153">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="d0170-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span><span class="sxs-lookup"><span data-stu-id="d0170-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="d0170-155">Возвращает исходный код состояния клиенту.</span><span class="sxs-lookup"><span data-stu-id="d0170-155">Returns the original status code to the client.</span></span>
* <span data-ttu-id="d0170-156">Позволяет создать текст ответа путем повторного выполнения конвейера запросов с использованием другого пути.</span><span class="sxs-lookup"><span data-stu-id="d0170-156">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="d0170-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> обычно используется, если приложение:</span><span class="sxs-lookup"><span data-stu-id="d0170-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="d0170-158">Обрабатывает запрос без перенаправления к другой конечной точке.</span><span class="sxs-lookup"><span data-stu-id="d0170-158">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="d0170-159">Для веб-приложений в адресной строке браузера клиента отображается изначально запрошенная конечная точка.</span><span class="sxs-lookup"><span data-stu-id="d0170-159">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="d0170-160">Сохраняет и возвращает исходный код состояния с ответом.</span><span class="sxs-lookup"><span data-stu-id="d0170-160">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="d0170-161">Шаблоны могут содержать заполнитель (`{0}`) для кода состояния.</span><span class="sxs-lookup"><span data-stu-id="d0170-161">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="d0170-162">Шаблон должен начинаться с косой черты (`/`).</span><span class="sxs-lookup"><span data-stu-id="d0170-162">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="d0170-163">При использовании заполнителя убедитесь, что конечная точка (страница или контроллер) могут обрабатывать сегмент пути.</span><span class="sxs-lookup"><span data-stu-id="d0170-163">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="d0170-164">Например, страница Razor для ошибок должна принимать необязательное значение сегмента с директивой `@page`.</span><span class="sxs-lookup"><span data-stu-id="d0170-164">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="d0170-165">Можно отключить страницы кодов состояния для конкретных запросов в методе обработчика Razor Pages или в контроллере MVC.</span><span class="sxs-lookup"><span data-stu-id="d0170-165">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="d0170-166">Чтобы отключить страницы кодов состояния, попробуйте извлечь <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> из коллекции запроса [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) и отключить эту функцию, если она доступна.</span><span class="sxs-lookup"><span data-stu-id="d0170-166">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="d0170-167">Чтобы использовать перегрузку <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>, которая указывает на конечную точку в приложении, создайте представление MVC или страницу Razor для конечной точки.</span><span class="sxs-lookup"><span data-stu-id="d0170-167">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="d0170-168">Например, шаблон приложения Razor Pages позволяет создать следующую страницу и класс модели страницы:</span><span class="sxs-lookup"><span data-stu-id="d0170-168">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="d0170-169">*Error.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="d0170-169">*Error.cshtml*:</span></span>

::: moniker range=">= aspnetcore-2.2"

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
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

<span data-ttu-id="d0170-170">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d0170-170">*Error.cshtml.cs*:</span></span>

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

<span data-ttu-id="d0170-171">*Error.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="d0170-171">*Error.cshtml.cs*:</span></span>

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

::: moniker-end

## <a name="exception-handling-code"></a><span data-ttu-id="d0170-172">Код обработки исключений</span><span class="sxs-lookup"><span data-stu-id="d0170-172">Exception-handling code</span></span>

<span data-ttu-id="d0170-173">Код на страницах обработки исключений может создавать исключения.</span><span class="sxs-lookup"><span data-stu-id="d0170-173">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="d0170-174">Часто желательно, чтобы страницы ошибок в рабочей среде содержали только статическое содержимое.</span><span class="sxs-lookup"><span data-stu-id="d0170-174">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="d0170-175">Также обратите внимание, что после отправки заголовков для ответа:</span><span class="sxs-lookup"><span data-stu-id="d0170-175">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="d0170-176">Приложение не может изменить код состояния ответа.</span><span class="sxs-lookup"><span data-stu-id="d0170-176">The app can't change the response's status code.</span></span>
* <span data-ttu-id="d0170-177">Нельзя запустить страницу или обработчик исключений.</span><span class="sxs-lookup"><span data-stu-id="d0170-177">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="d0170-178">Необходимо завершить ответ или прервать подключение.</span><span class="sxs-lookup"><span data-stu-id="d0170-178">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="d0170-179">Обработка исключений на сервере</span><span class="sxs-lookup"><span data-stu-id="d0170-179">Server exception handling</span></span>

<span data-ttu-id="d0170-180">Помимо логики обработки исключений в приложении, [реализация сервера](xref:fundamentals/servers/index) может выполнять ряд операций по обработке исключений.</span><span class="sxs-lookup"><span data-stu-id="d0170-180">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="d0170-181">Если сервер перехватывает исключение перед отправкой заголовков ответа, он отсылает ответ *500 Internal Server Error* (внутренняя ошибка сервера) без текста ответа.</span><span class="sxs-lookup"><span data-stu-id="d0170-181">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="d0170-182">Если сервер перехватывает исключение после отправки заголовков ответа, он закрывает соединение.</span><span class="sxs-lookup"><span data-stu-id="d0170-182">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="d0170-183">Запросы, не обработанные приложением, обрабатываются сервером.</span><span class="sxs-lookup"><span data-stu-id="d0170-183">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="d0170-184">Все возникшие исключения обрабатываются с помощью механизма обработки исключений на сервере.</span><span class="sxs-lookup"><span data-stu-id="d0170-184">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="d0170-185">Настроенные пользовательские страницы ошибок, промежуточный слой обработки исключений и фильтры не влияют на этот процесс.</span><span class="sxs-lookup"><span data-stu-id="d0170-185">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="d0170-186">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="d0170-186">Startup exception handling</span></span>

<span data-ttu-id="d0170-187">Исключения, возникающие во время запуска приложения, могут обрабатываться только на уровне размещения.</span><span class="sxs-lookup"><span data-stu-id="d0170-187">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="d0170-188">При помощи [веб-узла](xref:fundamentals/host/web-host) вы можете [настроить реакцию узла на ошибки при запуске](xref:fundamentals/host/web-host#detailed-errors) с помощью ключей `captureStartupErrors` и `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="d0170-188">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="d0170-189">Узел может отображать страницу со сведениями о перехваченной ошибке при запуске, только если ошибка произошла после привязки адреса и порта узла.</span><span class="sxs-lookup"><span data-stu-id="d0170-189">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="d0170-190">При сбое привязки по какой-либо причине:</span><span class="sxs-lookup"><span data-stu-id="d0170-190">If any binding fails for any reason:</span></span>

* <span data-ttu-id="d0170-191">На уровне размещения в журнале фиксируется критическое исключение.</span><span class="sxs-lookup"><span data-stu-id="d0170-191">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="d0170-192">Выполняется аварийное завершение процесса dotnet.</span><span class="sxs-lookup"><span data-stu-id="d0170-192">The dotnet process crashes.</span></span>
* <span data-ttu-id="d0170-193">Если приложение запущено на сервере [Kestrel](xref:fundamentals/servers/kestrel), страница со сведениями об ошибке не выводится.</span><span class="sxs-lookup"><span data-stu-id="d0170-193">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="d0170-194">При работе в службе [IIS](/iis) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module) возвращает ошибку *502.5 Process Failure* (ошибка процесса), если процесс невозможно запустить.</span><span class="sxs-lookup"><span data-stu-id="d0170-194">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="d0170-195">Для получения дополнительной информации см. <xref:host-and-deploy/iis/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d0170-195">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="d0170-196">Сведения об устранении неполадок при запуске в службе приложений Azure см. в статье <xref:host-and-deploy/azure-apps/troubleshoot>.</span><span class="sxs-lookup"><span data-stu-id="d0170-196">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="d0170-197">Обработка ошибок в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="d0170-197">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="d0170-198">В приложениях [MVC](xref:mvc/overview) есть дополнительные возможности обработки ошибок, например настройка фильтров исключений и проведение проверки модели.</span><span class="sxs-lookup"><span data-stu-id="d0170-198">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="d0170-199">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="d0170-199">Exception filters</span></span>

<span data-ttu-id="d0170-200">Фильтры исключений можно настраивать глобально либо для отдельных контроллеров, либо для действий в приложении MVC.</span><span class="sxs-lookup"><span data-stu-id="d0170-200">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="d0170-201">Эти фильтры обрабатывают все необработанные исключения, которые возникают во время выполнения действия контроллера или другого фильтра.</span><span class="sxs-lookup"><span data-stu-id="d0170-201">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="d0170-202">Иным образом они не вызываются.</span><span class="sxs-lookup"><span data-stu-id="d0170-202">These filters aren't called otherwise.</span></span> <span data-ttu-id="d0170-203">Дополнительные сведения см. в статье <xref:mvc/controllers/filters>.</span><span class="sxs-lookup"><span data-stu-id="d0170-203">To learn more, see <xref:mvc/controllers/filters>.</span></span>

> [!TIP]
> <span data-ttu-id="d0170-204">Фильтры исключений полезны при перехвате исключений, которые происходят в действиях MVC. Но эти фильтры не так гибки, как ПО промежуточного слоя для обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="d0170-204">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="d0170-205">Мы рекомендуем использовать ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="d0170-205">We recommend the use of middleware.</span></span> <span data-ttu-id="d0170-206">Используйте фильтры, только если ошибки нужно обрабатывать *по-разному* (в зависимости от выбранного действия MVC).</span><span class="sxs-lookup"><span data-stu-id="d0170-206">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="d0170-207">Обработка ошибок состояния модели</span><span class="sxs-lookup"><span data-stu-id="d0170-207">Handle model state errors</span></span>

<span data-ttu-id="d0170-208">Перед вызовом каждого действия контроллера [модель проверяется](xref:mvc/models/validation). За проверку свойства [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) и соответствующую реакцию отвечает метод действия.</span><span class="sxs-lookup"><span data-stu-id="d0170-208">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="d0170-209">В некоторых приложениях соблюдается стандартное соглашение об обработке ошибок [проверки модели](xref:mvc/models/validation). Подходящим механизмом для реализации такой политики может быть [фильтр](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="d0170-209">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="d0170-210">Следует проверить, как выполняются действия при недопустимых состояниях модели.</span><span class="sxs-lookup"><span data-stu-id="d0170-210">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="d0170-211">Для получения дополнительной информации см. <xref:mvc/controllers/testing>.</span><span class="sxs-lookup"><span data-stu-id="d0170-211">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d0170-212">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d0170-212">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
