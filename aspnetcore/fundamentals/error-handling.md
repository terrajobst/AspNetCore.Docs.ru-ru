---
title: "Обработка ошибок в ASP.NET Core"
author: ardalis
description: "Узнайте, как обрабатывать ошибки в приложениях ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 019e31fa749a950db48575e1f4e8d4d26d1cde75
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="0bb78-103">Введение в обработку ошибок в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0bb78-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="0bb78-104">Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Том Дакстра](https://github.com/tdykstra/) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="0bb78-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="0bb78-105">В этой статье рассматриваются распространенные appoaches к обработке ошибок в приложениях ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0bb78-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="0bb78-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0bb78-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="0bb78-107">Странице исключение разработчика</span><span class="sxs-lookup"><span data-stu-id="0bb78-107">The developer exception page</span></span>

<span data-ttu-id="0bb78-108">Чтобы настроить приложение для отображения страницы, которая отображает подробные сведения об исключениях, установите `Microsoft.AspNetCore.Diagnostics` NuGet пакета и добавление строки в [настройки метода в класс Startup](startup.md):</span><span class="sxs-lookup"><span data-stu-id="0bb78-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="0bb78-109">Поместите `UseDeveloperExceptionPage` перед любое по промежуточного слоя, чтобы перехватывать исключения, такие как `app.UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="0bb78-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="0bb78-110">Включить странице исключение разработчика **только тогда, когда приложение работает в среде разработки**.</span><span class="sxs-lookup"><span data-stu-id="0bb78-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="0bb78-111">Сведения об исключении в подробные общего просмотра при запуске приложения в рабочей среде не требуется.</span><span class="sxs-lookup"><span data-stu-id="0bb78-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="0bb78-112">[Дополнительные сведения о настройке среды](environments.md).</span><span class="sxs-lookup"><span data-stu-id="0bb78-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="0bb78-113">Для просмотра страницы исключение разработчика, запустить образец приложения со средой, равным `Development`и добавьте `?throw=true` базовый URL-адрес приложения.</span><span class="sxs-lookup"><span data-stu-id="0bb78-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="0bb78-114">Страница содержит несколько вкладок с информацией об исключении и запрос.</span><span class="sxs-lookup"><span data-stu-id="0bb78-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="0bb78-115">Первая вкладка включает трассировку стека.</span><span class="sxs-lookup"><span data-stu-id="0bb78-115">The first tab includes a stack trace.</span></span> 

![Трассировка стека](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="0bb78-117">Следующая вкладка отображается параметры строки запроса, если они имеются.</span><span class="sxs-lookup"><span data-stu-id="0bb78-117">The next tab shows the query string parameters, if any.</span></span>

![Параметры строки запроса](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="0bb78-119">Этот запрос не все файлы cookie, но в противном случае они появятся на **файлы cookie** вкладки. Вы увидите заголовки, которые были переданы в последнюю позицию.</span><span class="sxs-lookup"><span data-stu-id="0bb78-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![Заголовки](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="0bb78-121">Настройка пользовательского исключения, обработка страницы</span><span class="sxs-lookup"><span data-stu-id="0bb78-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="0bb78-122">Рекомендуется настроить страницу обработчика исключений для использования, когда приложение не выполняется `Development` среде.</span><span class="sxs-lookup"><span data-stu-id="0bb78-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="0bb78-123">В приложении MVC не декорировать явным образом метод действия обработчика ошибок с атрибутами метода HTTP, такие как `HttpGet`.</span><span class="sxs-lookup"><span data-stu-id="0bb78-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="0bb78-124">С помощью явного команд может помешать достижение метод некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="0bb78-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="0bb78-125">Настройка состояния кодовые страницы</span><span class="sxs-lookup"><span data-stu-id="0bb78-125">Configuring status code pages</span></span>

<span data-ttu-id="0bb78-126">По умолчанию приложения не предоставляют состоянии кодовую страницу для коды состояния HTTP, например 500 (Внутренняя ошибка сервера) или 404 (не найдено).</span><span class="sxs-lookup"><span data-stu-id="0bb78-126">By default, your app won't provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="0bb78-127">Можно настроить `StatusCodePagesMiddleware` , добавив строку в `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="0bb78-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="0bb78-128">По умолчанию это по промежуточного слоя добавляет простой и текстовыми обработчики стандартные коды состояния, такие как 404:</span><span class="sxs-lookup"><span data-stu-id="0bb78-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 страницы](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="0bb78-130">По промежуточного слоя поддерживает несколько разных метода расширения.</span><span class="sxs-lookup"><span data-stu-id="0bb78-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="0bb78-131">Одна принимает лямбда-выражение, другой принимает строку содержимого, тип и формат.</span><span class="sxs-lookup"><span data-stu-id="0bb78-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="0bb78-132">Существуют методы расширения для перенаправления.</span><span class="sxs-lookup"><span data-stu-id="0bb78-132">There are also redirect extension methods.</span></span> <span data-ttu-id="0bb78-133">Один отправляет клиенту код 302 состояния и один возвращается исходный код состояния клиента, а также выполняет действия обработчика для URL-адрес перенаправления.</span><span class="sxs-lookup"><span data-stu-id="0bb78-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="0bb78-134">Если вам нужно отключить состояние кодовых страниц для определенных запросов, это можно сделать:</span><span class="sxs-lookup"><span data-stu-id="0bb78-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="0bb78-135">Код обработки исключений</span><span class="sxs-lookup"><span data-stu-id="0bb78-135">Exception-handling code</span></span>

<span data-ttu-id="0bb78-136">Код страницы обработки исключения может вызывать исключения.</span><span class="sxs-lookup"><span data-stu-id="0bb78-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="0bb78-137">Чаще всего рекомендуется для производственных страницы ошибок для состоять исключительно статического содержимого.</span><span class="sxs-lookup"><span data-stu-id="0bb78-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="0bb78-138">Кроме того помните, что после отправки заголовков ответа невозможно изменить код состояния ответа, а также любые исключения страницами или обработчиками запускается.</span><span class="sxs-lookup"><span data-stu-id="0bb78-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="0bb78-139">Ответ должен быть завершен либо подключение прервано.</span><span class="sxs-lookup"><span data-stu-id="0bb78-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="0bb78-140">Обработка исключений с сервера</span><span class="sxs-lookup"><span data-stu-id="0bb78-140">Server exception handling</span></span>

<span data-ttu-id="0bb78-141">Помимо логики приложения, обрабатывающего [сервера](servers/index.md) размещение приложение выполняет некоторые обработки исключений.</span><span class="sxs-lookup"><span data-stu-id="0bb78-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="0bb78-142">Если сервер перехватывает исключение, перед отправкой заголовки, сервер отправляет ответ 500 Внутренняя ошибка сервера без тела.</span><span class="sxs-lookup"><span data-stu-id="0bb78-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="0bb78-143">Если сервер перехватывает исключение после отправки заголовков, сервер закрывает соединение.</span><span class="sxs-lookup"><span data-stu-id="0bb78-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="0bb78-144">Запросы, не обрабатываются приложением, обрабатываются сервером.</span><span class="sxs-lookup"><span data-stu-id="0bb78-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="0bb78-145">Любое исключение, возникающее обрабатывается исключение сервера обработки.</span><span class="sxs-lookup"><span data-stu-id="0bb78-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="0bb78-146">Любой настроен настраиваемые страницы ошибок или по промежуточного слоя обработки исключений и фильтры не влияют на это поведение.</span><span class="sxs-lookup"><span data-stu-id="0bb78-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="0bb78-147">Обработка исключений при запуске</span><span class="sxs-lookup"><span data-stu-id="0bb78-147">Startup exception handling</span></span>

<span data-ttu-id="0bb78-148">Уровень размещения может обрабатывать исключения, происходящие во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="0bb78-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="0bb78-149">Вы можете [настройки узла при возникновении ошибок во время запуска](hosting.md#detailed-errors) с помощью `captureStartupErrors` и `detailedErrors` ключа.</span><span class="sxs-lookup"><span data-stu-id="0bb78-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="0bb78-150">Размещение можно показывать только страницу ошибки для ошибки записанного запуска, если эта ошибка возникает после привязки адреса узла и порта.</span><span class="sxs-lookup"><span data-stu-id="0bb78-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="0bb78-151">Если любая привязка какой-либо причине произошел сбой, уровня размещения входит критические исключения, сбои процесса dotnet, и страница ошибки, не отображается.</span><span class="sxs-lookup"><span data-stu-id="0bb78-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="0bb78-152">Обработка ошибок в ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0bb78-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="0bb78-153">[MVC](../mvc/index.md) приложения имеют некоторые дополнительные параметры для обработки ошибок, такие как Настройка фильтров исключений и проверки модели.</span><span class="sxs-lookup"><span data-stu-id="0bb78-153">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="0bb78-154">Фильтры исключений</span><span class="sxs-lookup"><span data-stu-id="0bb78-154">Exception Filters</span></span>

<span data-ttu-id="0bb78-155">Фильтры исключений можно задать глобально или на основе-контроллера или каждого действия в приложении MVC.</span><span class="sxs-lookup"><span data-stu-id="0bb78-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="0bb78-156">Эти фильтры обрабатывать любые необработанное исключение, возникающее во время выполнения действия контроллера или другой фильтр и не вызываются в противном случае.</span><span class="sxs-lookup"><span data-stu-id="0bb78-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="0bb78-157">Дополнительные сведения о фильтры исключений в [фильтры](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="0bb78-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="0bb78-158">Фильтры исключений подходят для перехвата исключений, возникающих в рамках действия MVC, но они не обладает такой гибкостью, как по промежуточного слоя обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="0bb78-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="0bb78-159">Предпочтение по промежуточного слоя в общем случае и использовать фильтры, только когда нужен для обработки ошибок *по-разному* основании действие MVC, которое было выбрано.</span><span class="sxs-lookup"><span data-stu-id="0bb78-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="0bb78-160">Состояние модели обработки ошибок</span><span class="sxs-lookup"><span data-stu-id="0bb78-160">Handling Model State Errors</span></span>

<span data-ttu-id="0bb78-161">[Проверка модели](../mvc/models/validation.md) происходит перед каждой вызываемого действия контроллера, и ответственность метода действия для проверки `ModelState.IsValid` и реагировать соответствующим образом.</span><span class="sxs-lookup"><span data-stu-id="0bb78-161">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it's the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="0bb78-162">Некоторые приложения будет реализовывать стандартного соглашения для обработки ошибок проверки модели, в этом случае [фильтра](../mvc/controllers/filters.md) может быть необходимо реализовать такую политику.</span><span class="sxs-lookup"><span data-stu-id="0bb78-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="0bb78-163">Следует проверить поведение свои действия с состояниями Недопустимая модель.</span><span class="sxs-lookup"><span data-stu-id="0bb78-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="0bb78-164">Дополнительные сведения см. в разделе [тестирование логики контроллера](../mvc/controllers/testing.md).</span><span class="sxs-lookup"><span data-stu-id="0bb78-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



