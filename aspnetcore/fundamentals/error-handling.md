---
title: "Обработка ошибок в ASP.NET Core"
author: ardalis
description: "Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="84a96-103">Общие сведения об обработке ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84a96-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="84a96-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra/) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="84a96-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="84a96-105">В этой статье рассматриваются основные методы обработки ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84a96-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="84a96-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="84a96-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="84a96-107">Страница со сведениями об исключении для разработчика</span><span class="sxs-lookup"><span data-stu-id="84a96-107">The developer exception page</span></span>

<span data-ttu-id="84a96-108">Чтобы настроить вывод в приложении страницы с подробными сведениями об исключениях, установите пакет NuGet `Microsoft.AspNetCore.Diagnostics` и добавьте строку в [метод Configure в классе Startup](startup.md):</span><span class="sxs-lookup"><span data-stu-id="84a96-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="84a96-109">Поместите метод `UseDeveloperExceptionPage` перед каждым промежуточным слоем, в котором нужно перехватывать исключения, например `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="84a96-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="84a96-110">Включать страницу со сведениями об исключении для разработчика следует **только тогда, когда приложение выполняется в среде разработки**.</span><span class="sxs-lookup"><span data-stu-id="84a96-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="84a96-111">Подробные сведения об исключениях не должны быть общедоступными при выполнении приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="84a96-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="84a96-112">[Дополнительные сведения о настройке сред](environments.md).</span><span class="sxs-lookup"><span data-stu-id="84a96-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="84a96-113">Чтобы увидеть страницу со сведениями об исключении для разработчика, запустите образец приложения, указав среду `Development`, и добавьте элемент `?throw=true` к базовому URL-адресу приложения.</span><span class="sxs-lookup"><span data-stu-id="84a96-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="84a96-114">Страница содержит несколько вкладок со сведениями об исключении и запросе.</span><span class="sxs-lookup"><span data-stu-id="84a96-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="84a96-115">На первой вкладке приводится трассировка стека.</span><span class="sxs-lookup"><span data-stu-id="84a96-115">The first tab includes a stack trace.</span></span> 

![Трассировка стека](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="84a96-117">На следующей вкладке представлены параметры строки запроса, если они имеются.</span><span class="sxs-lookup"><span data-stu-id="84a96-117">The next tab shows the query string parameters, if any.</span></span>

![Параметры строки запроса](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="84a96-119">У этого запроса не было файлов cookie, но если бы они были, они бы отображались на вкладке **Cookies** (Файлы cookie). На последней вкладке можно просмотреть переданные заголовки.</span><span class="sxs-lookup"><span data-stu-id="84a96-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Заголовки](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="84a96-121">Настройка пользовательской страницы обработки исключений</span><span class="sxs-lookup"><span data-stu-id="84a96-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="84a96-122">Если приложение выполняется не в среде `Development`, желательно настроить страницу обработки исключений.</span><span class="sxs-lookup"><span data-stu-id="84a96-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="84a96-123">В приложении MVC не следует явным образом декорировать метод действия обработки ошибок атрибутами метода HTTP, например `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="84a96-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="84a96-124">Из-за использования явных команд некоторые запросы могут не передаваться в метод.</span><span class="sxs-lookup"><span data-stu-id="84a96-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="84a96-125">Настройка страниц с кодами состояния</span><span class="sxs-lookup"><span data-stu-id="84a96-125">Configuring status code pages</span></span>

<span data-ttu-id="84a96-126">По умолчанию приложение не предоставляет специальных страниц для кодов состояния HTTP, таких как код 500 (внутренняя ошибка сервера) или 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="84a96-126">By default, your app won't provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="84a96-127">Вы можете настроить `StatusCodePagesMiddleware`, добавив строку в метод `Configure`:</span><span class="sxs-lookup"><span data-stu-id="84a96-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="84a96-128">По умолчанию этот промежуточный слой добавляет простые текстовые обработчики для распространенных кодов состояния, например 404:</span><span class="sxs-lookup"><span data-stu-id="84a96-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![Страница 404](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="84a96-130">Промежуточный слой поддерживает несколько разных методов расширения.</span><span class="sxs-lookup"><span data-stu-id="84a96-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="84a96-131">Один из них принимает лямбда-выражение, а другой — тип содержимого и строку формата.</span><span class="sxs-lookup"><span data-stu-id="84a96-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="84a96-132">Имеются также методы расширения для перенаправления.</span><span class="sxs-lookup"><span data-stu-id="84a96-132">There are also redirect extension methods.</span></span> <span data-ttu-id="84a96-133">Один из них отправляет клиенту код состояния 302, а другой возвращает исходный код состояния клиенту, а также выполняет обработчик для URL-адреса перенаправления.</span><span class="sxs-lookup"><span data-stu-id="84a96-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="84a96-134">Если нужно отключить страницы с кодами состояния для некоторых запросов, это можно сделать так:</span><span class="sxs-lookup"><span data-stu-id="84a96-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="84a96-135">Код обработки исключений</span><span class="sxs-lookup"><span data-stu-id="84a96-135">Exception-handling code</span></span>

<span data-ttu-id="84a96-136">Код на страницах обработки исключений может создавать исключения.</span><span class="sxs-lookup"><span data-stu-id="84a96-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="84a96-137">Часто желательно, чтобы страницы ошибок в рабочей среде содержали только статическое содержимое.</span><span class="sxs-lookup"><span data-stu-id="84a96-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="84a96-138">Кроме того, имейте в виду, что после отправки заголовков для ответа нельзя изменить код состояния ответа, а также невозможно выполнение страниц или обработчиков исключений.</span><span class="sxs-lookup"><span data-stu-id="84a96-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="84a96-139">Необходимо завершить ответ или прервать подключение.</span><span class="sxs-lookup"><span data-stu-id="84a96-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="84a96-140">Обработка исключений на сервере</span><span class="sxs-lookup"><span data-stu-id="84a96-140">Server exception handling</span></span>

<span data-ttu-id="84a96-141">Помимо логики обработки исключений в приложении, [сервер](servers/index.md), на котором размещается приложение, также выполняет ряд операций по обработке исключений.</span><span class="sxs-lookup"><span data-stu-id="84a96-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="84a96-142">Если сервер перехватывает исключение перед отправкой заголовков, он отсылает ответ 500 "Внутренняя ошибка сервера" без текста.</span><span class="sxs-lookup"><span data-stu-id="84a96-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="84a96-143">Если сервер перехватывает исключение после отправки заголовков, он закрывает соединение.</span><span class="sxs-lookup"><span data-stu-id="84a96-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="84a96-144">Запросы, не обработанные приложением, обрабатываются сервером.</span><span class="sxs-lookup"><span data-stu-id="84a96-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="84a96-145">Все возникшие исключения обрабатываются с помощью механизма обработки исключений на сервере.</span><span class="sxs-lookup"><span data-stu-id="84a96-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="84a96-146">Настроенные пользовательские страницы ошибок, промежуточный слой обработки исключений и фильтры не влияют на этот процесс.</span><span class="sxs-lookup"><span data-stu-id="84a96-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="84a96-147">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="84a96-147">Startup exception handling</span></span>

<span data-ttu-id="84a96-148">Исключения, возникающие во время запуска приложения, могут обрабатываться только на уровне размещения.</span><span class="sxs-lookup"><span data-stu-id="84a96-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="84a96-149">Вы можете [настроить реакцию узла на ошибки при запуске](hosting.md#detailed-errors) с помощью метода `captureStartupErrors` и ключа `detailedErrors`.</span><span class="sxs-lookup"><span data-stu-id="84a96-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="84a96-150">Узел может отображать страницу со сведениями о перехваченной ошибке при запуске, только если ошибка произошла после привязки адреса и порта узла.</span><span class="sxs-lookup"><span data-stu-id="84a96-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="84a96-151">Если выполнить привязку по какой-либо причине не удается, уровень размещения записывает в журнал критическое исключение, происходит аварийное завершение процесса dotnet и страница со сведениями об ошибке не выводится.</span><span class="sxs-lookup"><span data-stu-id="84a96-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="84a96-152">Обработка ошибок ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="84a96-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="84a96-153">В приложениях [MVC](xref:mvc/overview) есть дополнительные возможности обработки ошибок, например настройка фильтров исключений и проведение проверки модели.</span><span class="sxs-lookup"><span data-stu-id="84a96-153">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="84a96-154">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="84a96-154">Exception Filters</span></span>

<span data-ttu-id="84a96-155">Фильтры исключений можно настраивать глобально либо для отдельных контроллеров, либо для действий в приложении MVC.</span><span class="sxs-lookup"><span data-stu-id="84a96-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="84a96-156">Эти фильтры обрабатывают все необработанные исключения, которые возникают во время выполнения действия контроллера или другого фильтра. Иным образом они не вызываются.</span><span class="sxs-lookup"><span data-stu-id="84a96-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="84a96-157">Дополнительные сведения о фильтрах исключений см. в статье [Фильтры](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="84a96-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="84a96-158">Фильтры исключений хорошо подходят для перехвата исключений, которые происходят в действиях MVC, однако они не так гибки, как ПО промежуточного слоя для обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="84a96-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="84a96-159">ПО промежуточного слоя предпочтительнее для общих случаев. Фильтры следует использовать только тогда, когда обработка ошибок должна осуществляться *по-разному* в зависимости от выбранного действия MVC.</span><span class="sxs-lookup"><span data-stu-id="84a96-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="84a96-160">Обработка ошибок состояния модели</span><span class="sxs-lookup"><span data-stu-id="84a96-160">Handling Model State Errors</span></span>

<span data-ttu-id="84a96-161">[Проверка модели](../mvc/models/validation.md) проводится перед вызовом каждого действия контроллера. Метод действия отвечает за проверку свойства `ModelState.IsValid` и соответствующую реакцию.</span><span class="sxs-lookup"><span data-stu-id="84a96-161">[Model validation](../mvc/models/validation.md) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="84a96-162">В некоторых приложениях соблюдается стандартное соглашение об обработке ошибок проверки модели. В этом случае подходящим местом для реализации такой политики может быть [фильтр](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="84a96-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="84a96-163">Следует проверить, как выполняются действия при недопустимых состояниях модели.</span><span class="sxs-lookup"><span data-stu-id="84a96-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="84a96-164">Дополнительные сведения см. в статье [Тестирование логики контроллеров](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="84a96-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



