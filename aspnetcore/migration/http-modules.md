---
title: Перенос обработчики и модули HTTP на по промежуточного слоя ASP.NET Core
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: 516230a66ee3edba986c91d79684256aa8e4c994
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087005"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="bed1f-102">Перенос обработчики и модули HTTP на по промежуточного слоя ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bed1f-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="bed1f-103">По [Мэтт Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="bed1f-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="bed1f-104">В этой статье показано, как перенос существующего ASP.NET [HTTP-модулей и обработчиков в system.webserver](/iis/configuration/system.webserver/) на ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="bed1f-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="bed1f-105">Модули и обработчики revisited</span><span class="sxs-lookup"><span data-stu-id="bed1f-105">Modules and handlers revisited</span></span>

<span data-ttu-id="bed1f-106">Прежде чем переходить к по промежуточного слоя ASP.NET Core, сначала освежим в работе модулей и обработчиков HTTP:</span><span class="sxs-lookup"><span data-stu-id="bed1f-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Обработчик модулей](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="bed1f-108">**Существуют следующие обработчики**</span><span class="sxs-lookup"><span data-stu-id="bed1f-108">**Handlers are:**</span></span>

* <span data-ttu-id="bed1f-109">Классы, реализующие [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="bed1f-109">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

* <span data-ttu-id="bed1f-110">Позволяет обрабатывать запросы с помощью указанного имени файла или расширение, например *отчетов*</span><span class="sxs-lookup"><span data-stu-id="bed1f-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

* <span data-ttu-id="bed1f-111">[Настроить](/iis/configuration/system.webserver/handlers/) в *Web.config*</span><span class="sxs-lookup"><span data-stu-id="bed1f-111">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="bed1f-112">**Модули являются:**</span><span class="sxs-lookup"><span data-stu-id="bed1f-112">**Modules are:**</span></span>

* <span data-ttu-id="bed1f-113">Классы, реализующие [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="bed1f-113">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

* <span data-ttu-id="bed1f-114">Вызывается для каждого запроса</span><span class="sxs-lookup"><span data-stu-id="bed1f-114">Invoked for every request</span></span>

* <span data-ttu-id="bed1f-115">Возможность краткой записи (прекратить дальнейшую обработку запроса)</span><span class="sxs-lookup"><span data-stu-id="bed1f-115">Able to short-circuit (stop further processing of a request)</span></span>

* <span data-ttu-id="bed1f-116">Возможность добавить в HTTP-ответа, или создать свои собственные</span><span class="sxs-lookup"><span data-stu-id="bed1f-116">Able to add to the HTTP response, or create their own</span></span>

* <span data-ttu-id="bed1f-117">[Настроить](/iis/configuration/system.webserver/modules/) в *Web.config*</span><span class="sxs-lookup"><span data-stu-id="bed1f-117">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="bed1f-118">**Порядок, в котором модули обрабатывают входящие запросы определяется:**</span><span class="sxs-lookup"><span data-stu-id="bed1f-118">**The order in which modules process incoming requests is determined by:**</span></span>

1. <span data-ttu-id="bed1f-119">[Жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx), который является серии события, инициируемые ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)и т. д. Каждый модуль, можно создать обработчик для одного или нескольких событий.</span><span class="sxs-lookup"><span data-stu-id="bed1f-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

2. <span data-ttu-id="bed1f-120">Для того же события, порядок, в котором они настроены в *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="bed1f-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="bed1f-121">В дополнение к модулям, можно добавить обработчики событий жизненного цикла вашего *Global.asax.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="bed1f-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="bed1f-122">Эти обработчики запустите после обработчиков в настроенные модули.</span><span class="sxs-lookup"><span data-stu-id="bed1f-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="bed1f-123">Обработчики и модули на по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="bed1f-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="bed1f-124">**По промежуточного слоя, проще, чем модулей и обработчиков HTTP:**</span><span class="sxs-lookup"><span data-stu-id="bed1f-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

* <span data-ttu-id="bed1f-125">Модули, обработчиков *Global.asax.cs*, *Web.config* (за исключением конфигурации IIS) и жизненного цикла приложения будут удалены</span><span class="sxs-lookup"><span data-stu-id="bed1f-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

* <span data-ttu-id="bed1f-126">Роли модули и обработчики были выполнены на по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="bed1f-126">The roles of both modules and handlers have been taken over by middleware</span></span>

* <span data-ttu-id="bed1f-127">По промежуточного слоя настраиваются с помощью кода, а не в *Web.config*</span><span class="sxs-lookup"><span data-stu-id="bed1f-127">Middleware are configured using code rather than in *Web.config*</span></span>

* <span data-ttu-id="bed1f-128">[Ветвление конвейера](xref:fundamentals/middleware/index#use-run-and-map) позволяет отправлять запросы к по промежуточного слоя, основываясь на не только URL, но и от заголовков запроса, строки запроса, и т.д.</span><span class="sxs-lookup"><span data-stu-id="bed1f-128">[Pipeline branching](xref:fundamentals/middleware/index#use-run-and-map) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="bed1f-129">**По промежуточного слоя очень похожи на модули:**</span><span class="sxs-lookup"><span data-stu-id="bed1f-129">**Middleware are very similar to modules:**</span></span>

* <span data-ttu-id="bed1f-130">Вызывается в принципе, для каждого запроса</span><span class="sxs-lookup"><span data-stu-id="bed1f-130">Invoked in principle for every request</span></span>

* <span data-ttu-id="bed1f-131">Возможность краткой записи запроса, по [неправильно передает запрос следующему компоненту промежуточного слоя](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="bed1f-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

* <span data-ttu-id="bed1f-132">Возможность создавать свои собственные HTTP-ответа</span><span class="sxs-lookup"><span data-stu-id="bed1f-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="bed1f-133">**По промежуточного слоя и модули обрабатываются в другом порядке.**</span><span class="sxs-lookup"><span data-stu-id="bed1f-133">**Middleware and modules are processed in a different order:**</span></span>

* <span data-ttu-id="bed1f-134">Порядок по промежуточного слоя основан на порядке, в котором их вставки в конвейер запросов, хотя порядок модулей главным образом основан на [жизненного цикла приложения](https://msdn.microsoft.com/library/ms227673.aspx) события</span><span class="sxs-lookup"><span data-stu-id="bed1f-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

* <span data-ttu-id="bed1f-135">Порядок по промежуточного слоя для ответов обратна из того, что для запросов, хотя порядок модулей одинаков для запросов и ответов</span><span class="sxs-lookup"><span data-stu-id="bed1f-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

* <span data-ttu-id="bed1f-136">См. в разделе [Создание конвейера по промежуточного слоя с помощью IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="bed1f-136">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![ПО промежуточного слоя](http-modules/_static/middleware.png)

<span data-ttu-id="bed1f-138">Обратите внимание на то, как в приведенном выше рисунке, по промежуточного слоя проверки подлинности сокращено запроса.</span><span class="sxs-lookup"><span data-stu-id="bed1f-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="bed1f-139">Перенос кода модуля на по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="bed1f-139">Migrating module code to middleware</span></span>

<span data-ttu-id="bed1f-140">Существующий модуль HTTP будет выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bed1f-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="bed1f-141">Как показано в [по промежуточного слоя](xref:fundamentals/middleware/index) страницы, по промежуточного слоя ASP.NET Core — это класс, который предоставляет `Invoke` метод ведения `HttpContext` и возвращая `Task`.</span><span class="sxs-lookup"><span data-stu-id="bed1f-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="bed1f-142">Новый по промежуточного слоя будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bed1f-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="bed1f-143">Предыдущий шаблон по промежуточного слоя, сделанный в из раздела [написание по промежуточного слоя](xref:fundamentals/middleware/write).</span><span class="sxs-lookup"><span data-stu-id="bed1f-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/write).</span></span>

<span data-ttu-id="bed1f-144">*MyMiddlewareExtensions* вспомогательный класс упрощает для настройки по промежуточного слоя в вашей `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="bed1f-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="bed1f-145">`UseMyMiddleware` Метод добавляет класс по промежуточного слоя в конвейер запросов.</span><span class="sxs-lookup"><span data-stu-id="bed1f-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="bed1f-146">Службы, необходимые для по промежуточного слоя получить внедрен в конструктор по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="bed1f-147">Модуль может вызвать завершение запроса, например, если пользователь не имеет разрешения:</span><span class="sxs-lookup"><span data-stu-id="bed1f-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="bed1f-148">По промежуточного слоя обрабатывает это, не вызвав `Invoke` на следующее по промежуточного слоя в конвейере.</span><span class="sxs-lookup"><span data-stu-id="bed1f-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="bed1f-149">Имейте в виду, это не прервать полностью запрос, так как предыдущий по промежуточного слоя по-прежнему вызываться, когда ответ проходит обратно через конвейер.</span><span class="sxs-lookup"><span data-stu-id="bed1f-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="bed1f-150">При переносе функциональные возможности вашего модуля нового по промежуточного слоя, вы обнаружите, что ваш код не компилироваться, поскольку `HttpContext` класс существенно изменились в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bed1f-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="bed1f-151">[Впоследствии](#migrating-to-the-new-httpcontext), вы увидите, как перенести в новый HttpContext ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bed1f-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="bed1f-152">Миграция вставки модуля в конвейер запросов</span><span class="sxs-lookup"><span data-stu-id="bed1f-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="bed1f-153">Модули HTTP обычно добавляются в конвейер запросов с помощью *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="bed1f-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="bed1f-154">Преобразование с [Добавление нового по промежуточного слоя](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) в конвейер запросов в вашей `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="bed1f-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="bed1f-155">Точному в конвейере, задав новый по промежуточного слоя зависит от события, оно будет обработано как модуль (`BeginRequest`, `EndRequest`т. д.) и порядок их расположения в списке модулей в *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="bed1f-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="bed1f-156">Как уже говорилось, не жизненного цикла приложения в ASP.NET Core и порядок, в котором ответы обрабатываются по промежуточного слоя отличается от порядка, используемых модулями.</span><span class="sxs-lookup"><span data-stu-id="bed1f-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="bed1f-157">Это может сделать ваше решение упорядочивания более сложной задачей.</span><span class="sxs-lookup"><span data-stu-id="bed1f-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="bed1f-158">Если упорядочение становится проблемой, можно разделить на несколько компонентов по промежуточного слоя, которые могут быть упорядочены независимо друг от друга вашего модуля.</span><span class="sxs-lookup"><span data-stu-id="bed1f-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="bed1f-159">Перенос кода обработчика для по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="bed1f-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="bed1f-160">Обработчик HTTP выглядит примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bed1f-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="bed1f-161">В проекте ASP.NET Core следует преобразовать это по промежуточного слоя, аналогичную следующей:</span><span class="sxs-lookup"><span data-stu-id="bed1f-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="bed1f-162">Это по промежуточного слоя очень похож на по промежуточного слоя, соответствующий модулей.</span><span class="sxs-lookup"><span data-stu-id="bed1f-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="bed1f-163">Это единственное реальное различие здесь отсутствует вызов к `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="bed1f-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="bed1f-164">Это имеет смысл, поскольку обработчик является в конце конвейера запросов, таким образом, не следующее по промежуточного слоя для вызова.</span><span class="sxs-lookup"><span data-stu-id="bed1f-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="bed1f-165">Миграция вставки обработчик в конвейер запросов</span><span class="sxs-lookup"><span data-stu-id="bed1f-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="bed1f-166">Настройка обработчика HTTP-данных выполняется в *Web.config* и выглядит примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bed1f-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="bed1f-167">Можно преобразовать это, добавив новый обработчик по промежуточного слоя в конвейер запросов в вашей `Startup` класс, аналогичную по промежуточного слоя, преобразованные из модулей.</span><span class="sxs-lookup"><span data-stu-id="bed1f-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="bed1f-168">Проблема этого подхода является то, что он будет отправлять все запросы по промежуточного слоя новый обработчик.</span><span class="sxs-lookup"><span data-stu-id="bed1f-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="bed1f-169">Тем не менее требуется только доставку по промежуточного слоя запросов с помощью заданного модуля.</span><span class="sxs-lookup"><span data-stu-id="bed1f-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="bed1f-170">Даст те же функциональные возможности, которые были с обработчиком HTTP.</span><span class="sxs-lookup"><span data-stu-id="bed1f-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="bed1f-171">Одним из решений является ветвление конвейера запросов с помощью данного расширения, с помощью `MapWhen` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="bed1f-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="bed1f-172">Для этого в том же `Configure` метод, где добавить другого по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="bed1f-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="bed1f-173">`MapWhen` принимает следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="bed1f-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="bed1f-174">Лямбда-выражение, которое принимает `HttpContext` и возвращает `true` Если запрос должен вышли из строя ветвь.</span><span class="sxs-lookup"><span data-stu-id="bed1f-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="bed1f-175">Это означает, что можно выполнять ветвление запросов не только на основании их расширения, но также в заголовки запросов, параметры строки запроса и т. д.</span><span class="sxs-lookup"><span data-stu-id="bed1f-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="bed1f-176">Лямбда-выражение, которое принимает `IApplicationBuilder` и добавляет по промежуточного слоя для ветви.</span><span class="sxs-lookup"><span data-stu-id="bed1f-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="bed1f-177">Это означает, что можно добавить дополнительные по промежуточного слоя в ветвь перед обработчик по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="bed1f-178">Промежуточного слоя, добавляемого в конвейер, прежде чем ветвь будет вызываться для всех запросов; ветвь не оказывает влияния на них.</span><span class="sxs-lookup"><span data-stu-id="bed1f-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="bed1f-179">Возможности по промежуточного слоя с помощью шаблона параметров загрузки</span><span class="sxs-lookup"><span data-stu-id="bed1f-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="bed1f-180">Некоторые модули и обработчики имеют параметры конфигурации, которые хранятся в *Web.config*. Тем не менее, в ASP.NET Core новая модель конфигурации используется вместо *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="bed1f-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="bed1f-181">Новый [система конфигурации](xref:fundamentals/configuration/index) предоставляет следующие параметры, чтобы решить эту проблему:</span><span class="sxs-lookup"><span data-stu-id="bed1f-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="bed1f-182">Напрямую внедрить параметры в по промежуточного слоя, как показано в [разделу](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="bed1f-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="bed1f-183">Используйте [шаблон параметров](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="bed1f-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="bed1f-184">Создание класса, содержащего параметры по промежуточного слоя, например:</span><span class="sxs-lookup"><span data-stu-id="bed1f-184">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="bed1f-185">Store со значениями</span><span class="sxs-lookup"><span data-stu-id="bed1f-185">Store the option values</span></span>

   <span data-ttu-id="bed1f-186">Система конфигурации позволяет хранить параметр в любом нужные значения.</span><span class="sxs-lookup"><span data-stu-id="bed1f-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="bed1f-187">Однако наиболее сайтов используйте *appsettings.json*, поэтому мы рассмотрим этот подход:</span><span class="sxs-lookup"><span data-stu-id="bed1f-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="bed1f-188">*MyMiddlewareOptionsSection* вот имя раздела.</span><span class="sxs-lookup"><span data-stu-id="bed1f-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="bed1f-189">Он не обязательно совпадает с именем класса параметров.</span><span class="sxs-lookup"><span data-stu-id="bed1f-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="bed1f-190">Связать со значениями с класс параметров</span><span class="sxs-lookup"><span data-stu-id="bed1f-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="bed1f-191">Шаблон параметров использует платформой внедрения зависимостей ASP.NET Core, чтобы связать тип параметров (таких как `MyMiddlewareOptions`) с `MyMiddlewareOptions` объекта, имеющего параметры.</span><span class="sxs-lookup"><span data-stu-id="bed1f-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="bed1f-192">Обновление вашей `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="bed1f-192">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="bed1f-193">Если вы используете *appsettings.json*, добавьте его в построитель конфигурации в `Startup` конструктор:</span><span class="sxs-lookup"><span data-stu-id="bed1f-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="bed1f-194">Настройка параметров службы:</span><span class="sxs-lookup"><span data-stu-id="bed1f-194">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="bed1f-195">Свяжите параметры с вашего класса параметров:</span><span class="sxs-lookup"><span data-stu-id="bed1f-195">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="bed1f-196">Вставить параметры в ваш конструктор по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="bed1f-197">Это похоже на добавление параметров в контроллер.</span><span class="sxs-lookup"><span data-stu-id="bed1f-197">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="bed1f-198">[UseMiddleware](#http-modules-usemiddleware) метод расширения, который добавляет по промежуточного слоя для `IApplicationBuilder` берет на себя внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="bed1f-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="bed1f-199">Это не ограничивается `IOptions` объектов.</span><span class="sxs-lookup"><span data-stu-id="bed1f-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="bed1f-200">Таким образом могут внедряться и любой другой объект, требуется по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="bed1f-201">Загрузка параметры по промежуточного слоя посредством прямого внедрения</span><span class="sxs-lookup"><span data-stu-id="bed1f-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="bed1f-202">Шаблон параметров имеет то преимущество, что он создает ослабить связь между значениями параметров и их пользователей.</span><span class="sxs-lookup"><span data-stu-id="bed1f-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="bed1f-203">После связывания класса параметров со значениями фактических параметров любого другого класса можно получить доступ к параметрам через платформой внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="bed1f-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="bed1f-204">Нет необходимости передавать значения параметров.</span><span class="sxs-lookup"><span data-stu-id="bed1f-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="bed1f-205">Реализация делится Однако если вы хотите использовать же по промежуточного слоя дважды с различными параметрами.</span><span class="sxs-lookup"><span data-stu-id="bed1f-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="bed1f-206">Например авторизации промежуточного слоя, используемое в других ветвях, позволяя разные роли.</span><span class="sxs-lookup"><span data-stu-id="bed1f-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="bed1f-207">Два объекта различные варианты нельзя будет связать с одной параметры класса.</span><span class="sxs-lookup"><span data-stu-id="bed1f-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="bed1f-208">Решением является получение параметрических объектов со значениями фактических параметров в вашей `Startup` класса и передать их напрямую к каждому экземпляру по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="bed1f-209">Добавьте второй ключ для *appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="bed1f-209">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="bed1f-210">Чтобы добавить второй набор параметров для *appsettings.json* файла следует использовать новый ключ для уникальной идентификации:</span><span class="sxs-lookup"><span data-stu-id="bed1f-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="bed1f-211">Извлечь значения параметров и передавать их по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="bed1f-212">`Use...` Метода расширения (который добавляет по промежуточного слоя в конвейере) является логичным местом для передачи значений параметра:</span><span class="sxs-lookup"><span data-stu-id="bed1f-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="bed1f-213">Включите по промежуточного слоя, чтобы воспользоваться параметра options.</span><span class="sxs-lookup"><span data-stu-id="bed1f-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="bed1f-214">Обеспечьте перегрузку `Use...` метода расширения (который принимает параметр options и передает его `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="bed1f-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="bed1f-215">Когда `UseMiddleware` вызывается с параметрами, он передает параметры в ваш конструктор по промежуточного слоя, когда он создает экземпляр объекта по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="bed1f-216">Обратите внимание на то, как это создает оболочку объект параметров в `OptionsWrapper` объекта.</span><span class="sxs-lookup"><span data-stu-id="bed1f-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="bed1f-217">Этот код реализует `IOptions`, как требуется для конструктора по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="bed1f-218">Переход на новый HttpContext</span><span class="sxs-lookup"><span data-stu-id="bed1f-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="bed1f-219">Вы уже видели, `Invoke` метод в по промежуточного слоя принимает параметр типа `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="bed1f-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="bed1f-220">`HttpContext` сильно изменился в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bed1f-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="bed1f-221">В этом разделе показано, как перевести наиболее часто используемые свойства [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) к новому `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="bed1f-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="bed1f-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="bed1f-222">HttpContext</span></span>

<span data-ttu-id="bed1f-223">**HttpContext.Items** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="bed1f-224">**Уникальный идентификатор запроса (эквивалент System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="bed1f-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="bed1f-225">Предоставляет уникальный идентификатор для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="bed1f-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="bed1f-226">Очень полезно включить в журналах.</span><span class="sxs-lookup"><span data-stu-id="bed1f-226">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="bed1f-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="bed1f-227">HttpContext.Request</span></span>

<span data-ttu-id="bed1f-228">**HttpContext.Request.HttpMethod** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="bed1f-229">**HttpContext.Request.QueryString** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="bed1f-230">**HttpContext.Request.Url** и **HttpContext.Request.RawUrl** перевода:</span><span class="sxs-lookup"><span data-stu-id="bed1f-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="bed1f-231">**HttpContext.Request.IsSecureConnection** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="bed1f-232">**HttpContext.Request.UserHostAddress** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="bed1f-233">**HttpContext.Request.Cookies** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="bed1f-234">**HttpContext.Request.RequestContext.RouteData** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="bed1f-235">**HttpContext.Request.Headers** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="bed1f-236">**HttpContext.Request.UserAgent** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="bed1f-237">**HttpContext.Request.UrlReferrer** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="bed1f-238">**HttpContext.Request.ContentType** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="bed1f-239">**HttpContext.Request.Form** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="bed1f-240">Чтение значений формы, только в том случае, если тип содержимого sub — *x-www-формы-urlencoded* или *данные формы*.</span><span class="sxs-lookup"><span data-stu-id="bed1f-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="bed1f-241">**HttpContext.Request.InputStream** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="bed1f-242">Используйте этот код только в обработчике тип по промежуточного слоя, в конце конвейера.</span><span class="sxs-lookup"><span data-stu-id="bed1f-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="bed1f-243">Вы можете прочитать необработанном тексте, как показано выше только один раз в запрос.</span><span class="sxs-lookup"><span data-stu-id="bed1f-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="bed1f-244">По промежуточного слоя, попытка чтения текста после первого чтения будет считывать пустым текстом.</span><span class="sxs-lookup"><span data-stu-id="bed1f-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="bed1f-245">Это не относится к чтению формы, как показано выше, так как это делается из буфера.</span><span class="sxs-lookup"><span data-stu-id="bed1f-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="bed1f-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="bed1f-246">HttpContext.Response</span></span>

<span data-ttu-id="bed1f-247">**HttpContext.Response.Status** и **HttpContext.Response.StatusDescription** перевода:</span><span class="sxs-lookup"><span data-stu-id="bed1f-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="bed1f-248">**HttpContext.Response.ContentEncoding** и **HttpContext.Response.ContentType** перевода:</span><span class="sxs-lookup"><span data-stu-id="bed1f-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="bed1f-249">**HttpContext.Response.ContentType** на свой собственный также преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="bed1f-250">**HttpContext.Response.Output** преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="bed1f-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="bed1f-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="bed1f-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="bed1f-252">Обслуживает файл рассматривается [здесь](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="bed1f-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="bed1f-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="bed1f-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="bed1f-254">Отправляя заголовки ответа усложняется тем фактом, что если вы их после записи данных в текст ответа, они не отправляются.</span><span class="sxs-lookup"><span data-stu-id="bed1f-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="bed1f-255">Решение заключается в том, чтобы задать метод обратного вызова, который будет вызываться перед записью на ответ начинается справа.</span><span class="sxs-lookup"><span data-stu-id="bed1f-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="bed1f-256">Лучше всего это делается в начале `Invoke` метод в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="bed1f-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="bed1f-257">Это этого метода обратного вызова, которая задает вашей заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="bed1f-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="bed1f-258">Следующий код задает метод обратного вызова, вызванный `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="bed1f-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="bed1f-259">`SetHeaders` Метод обратного вызова будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bed1f-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="bed1f-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="bed1f-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="bed1f-261">Файлы cookie передаются обозревателю в *Set-Cookie* заголовок ответа.</span><span class="sxs-lookup"><span data-stu-id="bed1f-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="bed1f-262">В результате отправки файлов cookie требуется разрешение тому же обратному вызову, что и для отправки заголовков ответа:</span><span class="sxs-lookup"><span data-stu-id="bed1f-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="bed1f-263">`SetCookies` Метод обратного вызова будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bed1f-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="bed1f-264">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bed1f-264">Additional resources</span></span>

* [<span data-ttu-id="bed1f-265">Обработчики HTTP-данных и общие сведения о модули HTTP</span><span class="sxs-lookup"><span data-stu-id="bed1f-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="bed1f-266">Конфигурация</span><span class="sxs-lookup"><span data-stu-id="bed1f-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="bed1f-267">Запуск приложения</span><span class="sxs-lookup"><span data-stu-id="bed1f-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="bed1f-268">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="bed1f-268">Middleware</span></span>](xref:fundamentals/middleware/index)
