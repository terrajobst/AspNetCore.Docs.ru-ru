---
title: "Миграция обработчики HTTP-данных и модули в по промежуточного слоя ASP.NET Core"
author: rick-anderson
description: 
ms.author: tdykstra
manager: wpickett
ms.date: 12/07/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/http-modules
ms.openlocfilehash: 44b2b38c284e678344432d4473162404b4bb75a5
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="70023-102">Миграция обработчики HTTP-данных и модули в по промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70023-102">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="70023-103">По [Мэтт Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="70023-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="70023-104">В этой статье показано, как выполнить миграцию существующих ASP.NET [HTTP-модули и обработчики в system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) для ASP.NET Core [по промежуточного слоя](../fundamentals/middleware.md).</span><span class="sxs-lookup"><span data-stu-id="70023-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="70023-105">Модули и обработчики продукта</span><span class="sxs-lookup"><span data-stu-id="70023-105">Modules and handlers revisited</span></span>

<span data-ttu-id="70023-106">Прежде чем переходить к по промежуточного слоя ASP.NET Core давайте сначала освежить работу модулей и обработчиков HTTP:</span><span class="sxs-lookup"><span data-stu-id="70023-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Обработчик модулей](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="70023-108">**Существуют следующие обработчики.**</span><span class="sxs-lookup"><span data-stu-id="70023-108">**Handlers are:**</span></span>

   * <span data-ttu-id="70023-109">Классы, реализующие [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="70023-109">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="70023-110">Используется для обработки запросов с заданным именем файла или расширения, такие как *.report*</span><span class="sxs-lookup"><span data-stu-id="70023-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="70023-111">[Настроить](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) в *Web.config*</span><span class="sxs-lookup"><span data-stu-id="70023-111">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="70023-112">**Модули являются:**</span><span class="sxs-lookup"><span data-stu-id="70023-112">**Modules are:**</span></span>

   * <span data-ttu-id="70023-113">Классы, реализующие [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="70023-113">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="70023-114">Вызывается для каждого запроса</span><span class="sxs-lookup"><span data-stu-id="70023-114">Invoked for every request</span></span>

   * <span data-ttu-id="70023-115">Возможность краткой записи (прекратить дальнейшую обработку запроса)</span><span class="sxs-lookup"><span data-stu-id="70023-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="70023-116">Возможность добавить в HTTP-ответ или создать свои собственные</span><span class="sxs-lookup"><span data-stu-id="70023-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="70023-117">[Настроить](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) в *Web.config*</span><span class="sxs-lookup"><span data-stu-id="70023-117">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="70023-118">**Порядок, в котором модули обработки входящих запросов определяется:**</span><span class="sxs-lookup"><span data-stu-id="70023-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="70023-119">[Жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx), который является ряда событий, произошедших в ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest)и т. д. Каждый модуль можно создать обработчик для одного или нескольких событий.</span><span class="sxs-lookup"><span data-stu-id="70023-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="70023-120">Для одного события в порядке, в котором они были заданы на *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="70023-120">For the same event, the order in which they are configured in *Web.config*.</span></span>

<span data-ttu-id="70023-121">В дополнение к модули, можно добавить обработчики событий жизненного цикла для вашего *Global.asax.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="70023-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="70023-122">Эти обработчики запускать после обработчики в настроенные модули.</span><span class="sxs-lookup"><span data-stu-id="70023-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="70023-123">Из обработчиков и модулей с по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="70023-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="70023-124">**По промежуточного слоя, проще, чем модулей и обработчиков HTTP:**</span><span class="sxs-lookup"><span data-stu-id="70023-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="70023-125">Модули, обработчиками, *Global.asax.cs*, *Web.config* (за исключением конфигурации IIS) и жизненного цикла приложения будут удалены</span><span class="sxs-lookup"><span data-stu-id="70023-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="70023-126">Роли модули и обработчики было выполнено по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="70023-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="70023-127">По промежуточного слоя, настроенные с помощью кода, а не в *Web.config*</span><span class="sxs-lookup"><span data-stu-id="70023-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="70023-128">[Ветвление конвейера](../fundamentals/middleware.md#middleware-run-map-use) позволяет отправлять запросы для определенного по промежуточного слоя, основанный на не только URL, но также и от заголовки запроса, строки запроса, и т. д.</span><span class="sxs-lookup"><span data-stu-id="70023-128">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="70023-129">**По промежуточного слоя очень похожи на модули:**</span><span class="sxs-lookup"><span data-stu-id="70023-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="70023-130">Вызывается в принципе для каждого запроса</span><span class="sxs-lookup"><span data-stu-id="70023-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="70023-131">Возможность краткой записи запроса, по [неправильно передает запрос на следующее по промежуточного слоя](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="70023-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="70023-132">Возможность создавать свои собственные HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="70023-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="70023-133">**По промежуточного слоя и модули, обрабатываются в другом порядке:**</span><span class="sxs-lookup"><span data-stu-id="70023-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="70023-134">Порядок по промежуточного слоя основан на порядке, в котором они вставляются в конвейер запросов, хотя порядок модулей главным образом основан на [жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx) события</span><span class="sxs-lookup"><span data-stu-id="70023-134">Order of middleware is based on the order in which they are inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="70023-135">Порядок по промежуточного слоя для ответов — обратное из того, что для запросов, а порядок модулей является одинаковым для запросов и ответов</span><span class="sxs-lookup"><span data-stu-id="70023-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="70023-136">В разделе [Создание конвейера по промежуточного слоя с IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="70023-136">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![ПО промежуточного слоя](http-modules/_static/middleware.png)

<span data-ttu-id="70023-138">Обратите внимание на то, как в приведенном выше рисунке промежуточного по проверки подлинности short-circuited запроса.</span><span class="sxs-lookup"><span data-stu-id="70023-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="70023-139">Перенос кода модуля в по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="70023-139">Migrating module code to middleware</span></span>

<span data-ttu-id="70023-140">Существующий модуль HTTP будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="70023-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="70023-141">Как показано в [по промежуточного слоя](../fundamentals/middleware.md) , по промежуточного слоя ASP.NET Core используется класс, предоставляющий `Invoke` метод ведения `HttpContext` и возвращая `Task`.</span><span class="sxs-lookup"><span data-stu-id="70023-141">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="70023-142">Новый по промежуточного слоя будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="70023-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="70023-143">По промежуточного слоя шаблон был взят из раздела [записи по промежуточного слоя](../fundamentals/middleware.md#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="70023-143">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="70023-144">*MyMiddlewareExtensions* вспомогательный класс для упрощения настройки по промежуточного слоя в вашей `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="70023-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="70023-145">`UseMyMiddleware` Метод добавляет по промежуточного слоя класса конвейера запросов.</span><span class="sxs-lookup"><span data-stu-id="70023-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="70023-146">Службы, необходимые по промежуточного слоя получить введенный в конструкторе по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="70023-147">Модуль может вызвать завершение запроса, например, если пользователь не авторизован:</span><span class="sxs-lookup"><span data-stu-id="70023-147">Your module might terminate a request, for example if the user is not authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="70023-148">Обрабатывает это по промежуточного слоя, не вызвав `Invoke` на следующее по промежуточного слоя в конвейере.</span><span class="sxs-lookup"><span data-stu-id="70023-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="70023-149">Имейте в виду, что это не завершает полностью запроса, из-за предыдущих middlewares по-прежнему вызываться, когда ответ проходит обратно через конвейер.</span><span class="sxs-lookup"><span data-stu-id="70023-149">Keep in mind that this does not fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="70023-150">При переносе функциональные возможности своего модуля новый по промежуточного слоя, может оказаться, что код не будет компилироваться из-за `HttpContext` класса в ASP.NET Core значительно изменились.</span><span class="sxs-lookup"><span data-stu-id="70023-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="70023-151">[Позже на](#migrating-to-the-new-httpcontext), будет показано, как выполнить миграцию на новый HttpContext ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70023-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="70023-152">Миграция вставки модуля в конвейер обработки запросов</span><span class="sxs-lookup"><span data-stu-id="70023-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="70023-153">HTTP-модули обычно добавляются конвейера запросов, используя *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="70023-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="70023-154">Преобразование с [Добавление нового по промежуточного слоя](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) в конвейер обработки запросов в вашей `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="70023-154">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="70023-155">Точному конвейера, где вставить новый по промежуточного слоя зависит от события, он обрабатывается как модуль (`BeginRequest`, `EndRequest`, т. д.) и порядок их расположения в список модулей в *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="70023-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="70023-156">Как уже указывалось, не жизненного цикла приложения в ASP.NET Core и порядок обработки ответов по промежуточного слоя отличается от порядка, используемого в модулях.</span><span class="sxs-lookup"><span data-stu-id="70023-156">As previously stated, there is no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="70023-157">Это может принять решение о упорядочивания более сложной задачей.</span><span class="sxs-lookup"><span data-stu-id="70023-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="70023-158">Если порядок становится проблемой, модуль может разбиваться несколько компонентов по промежуточного слоя, которые могут быть упорядочены независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="70023-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="70023-159">Перенос кода обработчика в по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="70023-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="70023-160">Обработчик HTTP-данных выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="70023-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="70023-161">В проекте ASP.NET Core следует преобразовать это по промежуточного слоя, аналогичный следующему:</span><span class="sxs-lookup"><span data-stu-id="70023-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="70023-162">Это по промежуточного слоя очень похожа на соответствующий модули по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="70023-163">Единственная разница заключается в следующем Вот вызов `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="70023-163">The only real difference is that here there is no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="70023-164">Это имеет смысл, поскольку обработчик в конце конвейера запросов, таким образом, не следующее по промежуточного слоя для вызова.</span><span class="sxs-lookup"><span data-stu-id="70023-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="70023-165">Миграция вставки обработчик в конвейер обработки запросов</span><span class="sxs-lookup"><span data-stu-id="70023-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="70023-166">Настройка обработчика HTTP-данных выполняется в *Web.config* и выглядит примерно так:</span><span class="sxs-lookup"><span data-stu-id="70023-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="70023-167">Можно было преобразовать путем добавления нового обработчика по промежуточного слоя в конвейере вашей `Startup` класса, аналогично по промежуточного слоя, преобразованные из модулей.</span><span class="sxs-lookup"><span data-stu-id="70023-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="70023-168">Проблема с такого подхода заключается в том, что он отправит все запросы по промежуточного слоя новый обработчик.</span><span class="sxs-lookup"><span data-stu-id="70023-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="70023-169">Тем не менее необходимо только запросы расширений для доступа по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="70023-170">В этом случае получат вы те же функциональные возможности, которые были с обработчиком HTTP.</span><span class="sxs-lookup"><span data-stu-id="70023-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="70023-171">Одно из решений — создать ветвь конвейера запросов с использованием данного расширения с помощью `MapWhen` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="70023-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="70023-172">Для этого в том же `Configure` метод, который добавлять другого по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="70023-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="70023-173">`MapWhen`принимает следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="70023-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="70023-174">Лямбда-выражения, принимающего `HttpContext` и возвращает `true` Если ветвь сбоя запроса.</span><span class="sxs-lookup"><span data-stu-id="70023-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="70023-175">Это означает, что можно создать ветвь запросов не только на основании их расширения, но и заголовки запроса, параметров строки запроса, и т. д.</span><span class="sxs-lookup"><span data-stu-id="70023-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="70023-176">Лямбда-выражения, принимающего `IApplicationBuilder` и добавляет по промежуточного слоя для ветви.</span><span class="sxs-lookup"><span data-stu-id="70023-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="70023-177">Это означает, что можно добавить дополнительные по промежуточного слоя в ветвь перед обработчика по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="70023-178">По промежуточного слоя, добавить в конвейер, прежде чем ветвь будет вызываться для всех запросов; ветвь не оказывает влияния на них.</span><span class="sxs-lookup"><span data-stu-id="70023-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="70023-179">Возможности по промежуточного слоя, с помощью шаблона параметров загрузки</span><span class="sxs-lookup"><span data-stu-id="70023-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="70023-180">Некоторые модули и обработчики имеют параметры конфигурации, которые хранятся в *Web.config*. Однако в ASP.NET Core новая модель конфигурации используются вместо *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="70023-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="70023-181">Новый [система конфигурации](xref:fundamentals/configuration/index) предоставляет следующие параметры, чтобы устранить эту проблему:</span><span class="sxs-lookup"><span data-stu-id="70023-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="70023-182">Напрямую внедрить параметры в по промежуточного слоя, как показано в [разделу](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="70023-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="70023-183">Используйте [параметры шаблона](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="70023-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="70023-184">Создание класса, содержащего параметры по промежуточного слоя, например:</span><span class="sxs-lookup"><span data-stu-id="70023-184">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="70023-185">Хранить значения параметров</span><span class="sxs-lookup"><span data-stu-id="70023-185">Store the option values</span></span>

    <span data-ttu-id="70023-186">Система конфигурации позволяет хранить значения параметров в любом месте требуется.</span><span class="sxs-lookup"><span data-stu-id="70023-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="70023-187">Однако наиболее сайтов используйте *appsettings.json*, поэтому мы будем такой подход:</span><span class="sxs-lookup"><span data-stu-id="70023-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="70023-188">*MyMiddlewareOptionsSection* вот имя раздела.</span><span class="sxs-lookup"><span data-stu-id="70023-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="70023-189">Он не должен совпадать с именем класса параметров.</span><span class="sxs-lookup"><span data-stu-id="70023-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="70023-190">Связать со значениями параметров в классе</span><span class="sxs-lookup"><span data-stu-id="70023-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="70023-191">Параметры шаблона используется платформа внедрения зависимостей ASP.NET Core сопоставление параметров типа (например, `MyMiddlewareOptions`) с `MyMiddlewareOptions` объекта, имеющего параметры.</span><span class="sxs-lookup"><span data-stu-id="70023-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="70023-192">Обновление вашего `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="70023-192">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="70023-193">Если вы используете *appsettings.json*, добавьте его в конструктор конфигурации в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="70023-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="70023-194">Настройка параметров службы:</span><span class="sxs-lookup"><span data-stu-id="70023-194">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="70023-195">Свяжите варианты с классе параметров:</span><span class="sxs-lookup"><span data-stu-id="70023-195">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="70023-196">Вставить параметры в ваш конструктор по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="70023-197">Это похоже на добавление параметров в контроллере.</span><span class="sxs-lookup"><span data-stu-id="70023-197">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="70023-198">[UseMiddleware](#http-modules-usemiddleware) метод расширения, который добавляет по промежуточного слоя для `IApplicationBuilder` берет на себя внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="70023-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="70023-199">Это не ограничивается `IOptions` объектов.</span><span class="sxs-lookup"><span data-stu-id="70023-199">This is not limited to `IOptions` objects.</span></span> <span data-ttu-id="70023-200">Любой другой объект, требуется по промежуточного слоя могут быть добавлены таким способом.</span><span class="sxs-lookup"><span data-stu-id="70023-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="70023-201">Загрузка параметры по промежуточного слоя посредством прямого внедрения</span><span class="sxs-lookup"><span data-stu-id="70023-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="70023-202">Шаблон параметров имеет то преимущество, что он создает свободные взаимозависимость между значениями параметров и их пользователями.</span><span class="sxs-lookup"><span data-stu-id="70023-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="70023-203">После класс параметров связан с фактические параметры значений и любого другого класса могут получить доступ к параметры на платформе внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="70023-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="70023-204">Нет необходимости передавать значения параметров.</span><span class="sxs-lookup"><span data-stu-id="70023-204">There is no need to pass around options values.</span></span>

<span data-ttu-id="70023-205">Это разбивает то, что если вы хотите использовать одинаковые по промежуточного слоя дважды с различными параметрами.</span><span class="sxs-lookup"><span data-stu-id="70023-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="70023-206">Например авторизации промежуточного слоя, используемое в различных ветвях, позволяя различным ролям.</span><span class="sxs-lookup"><span data-stu-id="70023-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="70023-207">Два различных параметров объекта невозможно связать с одной параметры класса.</span><span class="sxs-lookup"><span data-stu-id="70023-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="70023-208">Решением является получить объекты параметров со значениями фактические параметры в вашей `Startup` класса и передавать их непосредственно в каждый экземпляр по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="70023-209">Добавьте второй ключ *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="70023-209">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="70023-210">Чтобы добавить второй набор параметров для *appsettings.json* файла следует использовать новый ключ для его однозначной идентификации:</span><span class="sxs-lookup"><span data-stu-id="70023-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="70023-211">Получать значения параметров и их передачи в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="70023-212">`Use...` Метода расширения (которая добавляет по промежуточного слоя в конвейере) логично передать значения параметров:</span><span class="sxs-lookup"><span data-stu-id="70023-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="70023-213">Включение по промежуточного слоя, чтобы воспользоваться параметр options.</span><span class="sxs-lookup"><span data-stu-id="70023-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="70023-214">Предоставить перегрузку `Use...` метода расширения (которая принимает параметр параметры и передает их в `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="70023-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="70023-215">Когда `UseMiddleware` вызывается с параметрами, он передает параметры по промежуточного слоя конструктор при инициализации соответствующего объекта по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="70023-216">Обратите внимание на то, как это создает оболочку для объекта параметров в `OptionsWrapper` объекта.</span><span class="sxs-lookup"><span data-stu-id="70023-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="70023-217">Это реализуется `IOptions`, как требуется для конструктора по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="70023-218">Переход на новый интерфейс HttpContext</span><span class="sxs-lookup"><span data-stu-id="70023-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="70023-219">Вы уже видели раньше, `Invoke` метод в по промежуточного слоя принимает параметр типа `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="70023-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="70023-220">`HttpContext`значительно изменилась в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="70023-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="70023-221">В этом разделе показано, как преобразовать наиболее часто используемые свойства [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) к новому `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="70023-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="70023-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="70023-222">HttpContext</span></span>

<span data-ttu-id="70023-223">**HttpContext.Items** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="70023-224">**Уникальный идентификатор запроса (аналог не System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="70023-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="70023-225">Предоставляет уникальный идентификатор для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="70023-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="70023-226">Удобно использовать для включения в журналах.</span><span class="sxs-lookup"><span data-stu-id="70023-226">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="70023-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="70023-227">HttpContext.Request</span></span>

<span data-ttu-id="70023-228">**HttpContext.Request.HttpMethod** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="70023-229">**HttpContext.Request.QueryString** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="70023-230">**HttpContext.Request.Url** и **HttpContext.Request.RawUrl** перевести на:</span><span class="sxs-lookup"><span data-stu-id="70023-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="70023-231">**HttpContext.Request.IsSecureConnection** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="70023-232">**HttpContext.Request.UserHostAddress** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="70023-233">**HttpContext.Request.Cookies** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="70023-234">**HttpContext.Request.RequestContext.RouteData** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="70023-235">**HttpContext.Request.Headers** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="70023-236">**HttpContext.Request.UserAgent** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="70023-237">**HttpContext.Request.UrlReferrer** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="70023-238">**HttpContext.Request.ContentType** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="70023-239">**HttpContext.Request.Form** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="70023-240">Чтение значений формы, только если тип содержимого sub *x-www-формы-urlencoded* или *данные формы*.</span><span class="sxs-lookup"><span data-stu-id="70023-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="70023-241">**HttpContext.Request.InputStream** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="70023-242">Этот код можно используйте только в обработчике типа по промежуточного слоя, в конце конвейера.</span><span class="sxs-lookup"><span data-stu-id="70023-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="70023-243">Можно считывать необработанный текст, как показано выше только один раз для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="70023-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="70023-244">Попытка чтения после первого чтения тела запроса по промежуточного слоя будет считывать пустой текст.</span><span class="sxs-lookup"><span data-stu-id="70023-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="70023-245">Это не относится к чтению формы, как показано выше, из-за этого из буфера.</span><span class="sxs-lookup"><span data-stu-id="70023-245">This does not apply to reading a form as shown earlier, because that is done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="70023-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="70023-246">HttpContext.Response</span></span>

<span data-ttu-id="70023-247">**HttpContext.Response.Status** и **HttpContext.Response.StatusDescription** перевести на:</span><span class="sxs-lookup"><span data-stu-id="70023-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="70023-248">**HttpContext.Response.ContentEncoding** и **HttpContext.Response.ContentType** перевести на:</span><span class="sxs-lookup"><span data-stu-id="70023-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="70023-249">**HttpContext.Response.ContentType** на свой собственный также преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="70023-250">**HttpContext.Response.Output** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="70023-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="70023-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="70023-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="70023-252">Обслуживающий файл рассматривается [здесь](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="70023-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="70023-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="70023-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="70023-254">Отправка заголовки ответа, осложняется тем, что если задать ничего произошло после записи в текст ответа, они не отправляется.</span><span class="sxs-lookup"><span data-stu-id="70023-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="70023-255">Решение заключается в том, чтобы задать метод обратного вызова, который будет вызываться перед записью на ответ начинается справа.</span><span class="sxs-lookup"><span data-stu-id="70023-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="70023-256">Лучше всего для этого в начале `Invoke` метод в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="70023-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="70023-257">Это этот метод обратного вызова, который задает заголовки ответа, к.</span><span class="sxs-lookup"><span data-stu-id="70023-257">It is this callback method that sets your response headers.</span></span>

<span data-ttu-id="70023-258">Следующий код задает метод обратного вызова вызывается `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="70023-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="70023-259">`SetHeaders` Метод обратного вызова будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="70023-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="70023-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="70023-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="70023-261">Файлы cookie передаваться в браузере к *Set-Cookie* заголовок ответа.</span><span class="sxs-lookup"><span data-stu-id="70023-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="70023-262">В результате для отправки файлов cookie требуется тому же обратному вызову, что использовались для отправки заголовков ответа:</span><span class="sxs-lookup"><span data-stu-id="70023-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="70023-263">`SetCookies` Метод обратного вызова будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="70023-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="70023-264">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="70023-264">Additional Resources</span></span>

* [<span data-ttu-id="70023-265">Обработчики HTTP-данных и общие сведения о модули HTTP</span><span class="sxs-lookup"><span data-stu-id="70023-265">HTTP Handlers and HTTP Modules Overview</span></span>](https://docs.microsoft.com/iis/configuration/system.webserver/)

* [<span data-ttu-id="70023-266">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="70023-266">Configuration</span></span>](xref:fundamentals/configuration/index)

* [<span data-ttu-id="70023-267">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="70023-267">Application Startup</span></span>](../fundamentals/startup.md)

* [<span data-ttu-id="70023-268">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="70023-268">Middleware</span></span>](../fundamentals/middleware.md)
