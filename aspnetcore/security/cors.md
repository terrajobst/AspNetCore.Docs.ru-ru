---
title: "Включение запросы независимо от источника (CORS)"
author: rick-anderson
description: "В этом документе представлены CORS как стандарт для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 05/17/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cors
ms.openlocfilehash: ee61798fc1bde89ca3712eae9b7c4413e58cf70d
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/02/2018
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="4c468-103">Включение запросы независимо от источника (CORS)</span><span class="sxs-lookup"><span data-stu-id="4c468-103">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="4c468-104">По [Mike Wasson](https://github.com/mikewasson), [Бойера Shayne](https://twitter.com/spboyer), и [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4c468-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4c468-105">Безопасность обозревателя предотвращает внесение запросы AJAX в другой домен на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="4c468-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="4c468-106">Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="4c468-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="4c468-107">Однако иногда может потребоваться разрешить другим узлам, которые делают запросы независимо от источника веб-API.</span><span class="sxs-lookup"><span data-stu-id="4c468-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="4c468-108">[Кросс-общий доступ к ресурсам источника](http://www.w3.org/TR/cors/) (CORS) — это стандарт W3C, которая позволяет серверу ослабить политика одного источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="4c468-109">С помощью CORS, сервер можно явно разрешить некоторые запросы независимо от источника при отклонении другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="4c468-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="4c468-110">CORS является более безопасным и более гибким, чем ранее методов, например [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="4c468-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="4c468-111">В этом разделе показано, как включить CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c468-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="4c468-112">Что такое «того же происхождения»?</span><span class="sxs-lookup"><span data-stu-id="4c468-112">What is "same origin"?</span></span>

<span data-ttu-id="4c468-113">Два URL-адреса имеют того же источника, если они имеют одинаковые схемы, узлов и портов.</span><span class="sxs-lookup"><span data-stu-id="4c468-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="4c468-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="4c468-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="4c468-115">Эти два URL-адреса имеют того же источника:</span><span class="sxs-lookup"><span data-stu-id="4c468-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="4c468-116">Эти URL-адреса имеют различные источники, чем предыдущий два:</span><span class="sxs-lookup"><span data-stu-id="4c468-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="4c468-117">`http://example.net` -Другой домен</span><span class="sxs-lookup"><span data-stu-id="4c468-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="4c468-118">`http://www.example.com/foo.html` -Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="4c468-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="4c468-119">`https://example.com/foo.html` -Другой схемы</span><span class="sxs-lookup"><span data-stu-id="4c468-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="4c468-120">`http://example.com:9000/foo.html` -Другой порт</span><span class="sxs-lookup"><span data-stu-id="4c468-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="4c468-121">Internet Explorer не рассматривает порт, при сравнении источников.</span><span class="sxs-lookup"><span data-stu-id="4c468-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="4c468-122">Настройка CORS</span><span class="sxs-lookup"><span data-stu-id="4c468-122">Setting up CORS</span></span>

<span data-ttu-id="4c468-123">Чтобы добавить CORS для вашего приложения `Microsoft.AspNetCore.Cors` пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="4c468-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="4c468-124">Добавление служб CORS в файле Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="4c468-124">Add the CORS services in Startup.cs:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="4c468-125">Включение CORS с по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="4c468-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="4c468-126">Чтобы включить CORS для всего приложения добавьте по промежуточного слоя CORS в конвейер запроса с помощью `UseCors` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="4c468-126">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="4c468-127">Обратите внимание, что по промежуточного слоя CORS должно предшествовать определенные конечные точки в приложении, который требуется поддерживать запросы независимо от источника (например,).</span><span class="sxs-lookup"><span data-stu-id="4c468-127">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="4c468-128">Перед вызовом любого метода `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="4c468-128">before any call to `UseMvc`).</span></span>

<span data-ttu-id="4c468-129">Можно задать политику независимо от источника, при добавлении по промежуточного слоя CORS с помощью `CorsPolicyBuilder` класса.</span><span class="sxs-lookup"><span data-stu-id="4c468-129">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="4c468-130">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="4c468-130">There are two ways to do this.</span></span> <span data-ttu-id="4c468-131">Первый — вызов UseCors с лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="4c468-131">The first is to call UseCors with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="4c468-132">**Примечание:** URL-адрес должен быть указан без косой чертой (`/`).</span><span class="sxs-lookup"><span data-stu-id="4c468-132">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="4c468-133">Если URL-адрес завершается с `/`, сравнение будет возвращать `false` и будет возвращаться без заголовка.</span><span class="sxs-lookup"><span data-stu-id="4c468-133">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="4c468-134">Лямбда-выражение принимает `CorsPolicyBuilder` объекта.</span><span class="sxs-lookup"><span data-stu-id="4c468-134">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="4c468-135">Вы найдете список [параметры конфигурации](#cors-policy-options) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="4c468-135">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="4c468-136">В этом примере политика разрешает запросы независимо от источника от `http://example.com` и другие источники.</span><span class="sxs-lookup"><span data-stu-id="4c468-136">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="4c468-137">Обратите внимание, что CorsPolicyBuilder fluent API, поэтому можно соединить в цепочку вызовы методов:</span><span class="sxs-lookup"><span data-stu-id="4c468-137">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="4c468-138">Второй подход заключается в том, чтобы определить именованный политики CORS, а затем выберите политику по имени во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="4c468-138">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="4c468-139">В этом примере добавляется политика CORS с именем «AllowSpecificOrigin».</span><span class="sxs-lookup"><span data-stu-id="4c468-139">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="4c468-140">Чтобы выбрать политику, передать имя для `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="4c468-140">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="4c468-141">Включение CORS в MVC</span><span class="sxs-lookup"><span data-stu-id="4c468-141">Enabling CORS in MVC</span></span>

<span data-ttu-id="4c468-142">Также можно использовать MVC для применения определенных CORS каждого действия каждого контроллера или глобально для всех контроллеров.</span><span class="sxs-lookup"><span data-stu-id="4c468-142">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="4c468-143">При использовании MVC для включения CORS используются те же службы CORS, но не по промежуточного слоя CORS.</span><span class="sxs-lookup"><span data-stu-id="4c468-143">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="4c468-144">Каждого действия</span><span class="sxs-lookup"><span data-stu-id="4c468-144">Per action</span></span>

<span data-ttu-id="4c468-145">Чтобы задать политику CORS для определенных действий добавьте `[EnableCors]` атрибут действия.</span><span class="sxs-lookup"><span data-stu-id="4c468-145">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="4c468-146">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="4c468-146">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="4c468-147">На каждый контроллер</span><span class="sxs-lookup"><span data-stu-id="4c468-147">Per controller</span></span>

<span data-ttu-id="4c468-148">Чтобы задать политику CORS для определенного контроллера добавьте `[EnableCors]` атрибут в класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="4c468-148">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="4c468-149">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="4c468-149">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="4c468-150">Глобально</span><span class="sxs-lookup"><span data-stu-id="4c468-150">Globally</span></span>

<span data-ttu-id="4c468-151">Можно включить CORS глобально для всех контроллеров, добавив `CorsAuthorizationFilterFactory` фильтр в глобальную коллекцию фильтров:</span><span class="sxs-lookup"><span data-stu-id="4c468-151">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="4c468-152">Очередность выполнения:: действия контроллера, глобальные.</span><span class="sxs-lookup"><span data-stu-id="4c468-152">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="4c468-153">Политики на уровне действия имеют приоритет над политиками уровня контроллера, и уровня контроллера политики имеют приоритет над глобальные политики.</span><span class="sxs-lookup"><span data-stu-id="4c468-153">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="4c468-154">Отключить CORS</span><span class="sxs-lookup"><span data-stu-id="4c468-154">Disable CORS</span></span>

<span data-ttu-id="4c468-155">Чтобы отключить CORS для контроллера или действия, используйте `[DisableCors]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="4c468-155">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="4c468-156">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="4c468-156">CORS policy options</span></span>

<span data-ttu-id="4c468-157">В этом разделе описываются различные параметры, которые можно задать в политику CORS.</span><span class="sxs-lookup"><span data-stu-id="4c468-157">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="4c468-158">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="4c468-158">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="4c468-159">Набор разрешенных методов HTTP</span><span class="sxs-lookup"><span data-stu-id="4c468-159">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="4c468-160">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="4c468-160">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="4c468-161">Задать заголовки ответа предоставляется</span><span class="sxs-lookup"><span data-stu-id="4c468-161">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="4c468-162">Учетные данные в запросы независимо от источника</span><span class="sxs-lookup"><span data-stu-id="4c468-162">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="4c468-163">Задайте предварительный истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="4c468-163">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="4c468-164">Для некоторых параметров может оказаться удобным для чтения [работает как CORS](#how-cors-works) первой.</span><span class="sxs-lookup"><span data-stu-id="4c468-164">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="4c468-165">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="4c468-165">Set the allowed origins</span></span>

<span data-ttu-id="4c468-166">Чтобы разрешить один или несколько конкретных источников:</span><span class="sxs-lookup"><span data-stu-id="4c468-166">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="4c468-167">Чтобы разрешить все источники:</span><span class="sxs-lookup"><span data-stu-id="4c468-167">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="4c468-168">Внимательно рассмотрите перед предоставлением запросов из любого источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-168">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="4c468-169">Он означает, что практически любой веб-сайта можно вносить вызовы AJAX к вашему API.</span><span class="sxs-lookup"><span data-stu-id="4c468-169">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="4c468-170">Набор разрешенных методов HTTP</span><span class="sxs-lookup"><span data-stu-id="4c468-170">Set the allowed HTTP methods</span></span>

<span data-ttu-id="4c468-171">Чтобы разрешить все методы HTTP:</span><span class="sxs-lookup"><span data-stu-id="4c468-171">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="4c468-172">Это влияет на возможность предварительного запросов и -методы управления доступом — разрешить заголовок.</span><span class="sxs-lookup"><span data-stu-id="4c468-172">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="4c468-173">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="4c468-173">Set the allowed request headers</span></span>

<span data-ttu-id="4c468-174">Предварительный запрос CORS может включать заголовок Access-Control-Request-Headers список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»).</span><span class="sxs-lookup"><span data-stu-id="4c468-174">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="4c468-175">Белый список указанные заголовки:</span><span class="sxs-lookup"><span data-stu-id="4c468-175">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="4c468-176">Чтобы разрешить все создавать заголовки запроса:</span><span class="sxs-lookup"><span data-stu-id="4c468-176">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="4c468-177">Браузеры не полностью соответствуют в установке Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="4c468-177">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="4c468-178">Если задать заголовки на что-либо отличное от «\*», следует включать по крайней мере «принять,» «content-type» и «источник», а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="4c468-178">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="4c468-179">Задать заголовки ответа предоставляется</span><span class="sxs-lookup"><span data-stu-id="4c468-179">Set the exposed response headers</span></span>

<span data-ttu-id="4c468-180">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="4c468-180">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="4c468-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="4c468-181">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="4c468-182">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="4c468-182">Cache-Control</span></span>

* <span data-ttu-id="4c468-183">Content-Language</span><span class="sxs-lookup"><span data-stu-id="4c468-183">Content-Language</span></span>

* <span data-ttu-id="4c468-184">Тип содержимого</span><span class="sxs-lookup"><span data-stu-id="4c468-184">Content-Type</span></span>

* <span data-ttu-id="4c468-185">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="4c468-185">Expires</span></span>

* <span data-ttu-id="4c468-186">Last-Modified</span><span class="sxs-lookup"><span data-stu-id="4c468-186">Last-Modified</span></span>

* <span data-ttu-id="4c468-187">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="4c468-187">Pragma</span></span>

<span data-ttu-id="4c468-188">Спецификация CORS вызывает эти *заголовки ответа на простой*.</span><span class="sxs-lookup"><span data-stu-id="4c468-188">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="4c468-189">Чтобы сделать другие заголовки, доступны для приложения.</span><span class="sxs-lookup"><span data-stu-id="4c468-189">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="4c468-190">Учетные данные в запросы независимо от источника</span><span class="sxs-lookup"><span data-stu-id="4c468-190">Credentials in cross-origin requests</span></span>

<span data-ttu-id="4c468-191">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="4c468-191">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="4c468-192">По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-192">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="4c468-193">Учетные данные включают файлы cookie, а также схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="4c468-193">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="4c468-194">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен задать XMLHttpRequest.withCredentials значение true.</span><span class="sxs-lookup"><span data-stu-id="4c468-194">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="4c468-195">Непосредственно с помощью XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="4c468-195">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="4c468-196">В jQuery:</span><span class="sxs-lookup"><span data-stu-id="4c468-196">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="4c468-197">Кроме того сервер необходимо разрешить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="4c468-197">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="4c468-198">Чтобы разрешить учетные данные независимо от источника:</span><span class="sxs-lookup"><span data-stu-id="4c468-198">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="4c468-199">Теперь в HTTP-ответе будет включать доступ-элемент управления-Allow-Credentials заголовок, который предписывает браузеру, поддерживает ли сервер учетные данные для запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-199">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="4c468-200">Если браузер отправляет учетные данные, но ответ не содержит допустимый заголовок доступа-элемент управления-Allow-Credentials, браузер не будет предоставлять приложению ответ и происходит сбой запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="4c468-200">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="4c468-201">Будьте внимательны при разрешении учетные данные независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-201">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="4c468-202">Веб-сайт в другом домене может отправлять учетные данные вошедшего в систему пользователя приложения от имени пользователя без ведома пользователя.</span><span class="sxs-lookup"><span data-stu-id="4c468-202">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="4c468-203">Спецификация CORS указывается этот параметр источники, которые можно «\*» (все источники) является недопустимым при `Access-Control-Allow-Credentials` заголовок присутствует.</span><span class="sxs-lookup"><span data-stu-id="4c468-203">The CORS specification also states that setting origins to "\*" (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="4c468-204">Задайте предварительный истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="4c468-204">Set the preflight expiration time</span></span>

<span data-ttu-id="4c468-205">Заголовок доступа-элемент управления-Max-Age указывает, как долго ответ на Предварительный запрос может быть кэширован.</span><span class="sxs-lookup"><span data-stu-id="4c468-205">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="4c468-206">Для установки этого заголовка:</span><span class="sxs-lookup"><span data-stu-id="4c468-206">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="4c468-207">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="4c468-207">How CORS works</span></span>

<span data-ttu-id="4c468-208">В этом разделе описывается, что происходит в запрос CORS на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="4c468-208">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="4c468-209">Важно понять, как работает CORS, чтобы правильно можно настроить политику CORS и troubleshooted при возникновении непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="4c468-209">It's important to understand how CORS works so that the CORS policy can be configured correctly and troubleshooted when unexpected behaviors occur.</span></span>

<span data-ttu-id="4c468-210">Спецификация CORS представлены несколько заголовки HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-210">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="4c468-211">Если браузер поддерживает CORS, он устанавливает эти заголовки автоматически для запросов, независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-211">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="4c468-212">Настраиваемый код JavaScript не требуется для включения CORS.</span><span class="sxs-lookup"><span data-stu-id="4c468-212">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="4c468-213">Ниже приведен пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4c468-213">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="4c468-214">`Origin` Заголовок предоставляет домена сайта, который был выполнен запрос:</span><span class="sxs-lookup"><span data-stu-id="4c468-214">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET http://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: http://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: http://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="4c468-215">Если сервер разрешает запрос, он устанавливает заголовка Access-Control-Allow-Origin в ответе.</span><span class="sxs-lookup"><span data-stu-id="4c468-215">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="4c468-216">Значение этого заголовка соответствует заголовку источника из запроса, либо значение подстановочный знак «\*», это значит, что разрешены любые источники:</span><span class="sxs-lookup"><span data-stu-id="4c468-216">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="4c468-217">Если ответ не содержит заголовка Access-Control-Allow-Origin, происходит сбой AJAX-запросом.</span><span class="sxs-lookup"><span data-stu-id="4c468-217">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="4c468-218">В частности браузер блокирует запрос.</span><span class="sxs-lookup"><span data-stu-id="4c468-218">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="4c468-219">Даже если сервер возвращает успешный ответ, браузер не освободить ответа в клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="4c468-219">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="4c468-220">Предварительные запросы</span><span class="sxs-lookup"><span data-stu-id="4c468-220">Preflight Requests</span></span>

<span data-ttu-id="4c468-221">Для некоторых запросов CORS браузер отправляет запрос на дополнительные, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса.</span><span class="sxs-lookup"><span data-stu-id="4c468-221">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="4c468-222">Браузер может пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="4c468-222">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="4c468-223">Метод запроса является GET, HEAD или POST, и</span><span class="sxs-lookup"><span data-stu-id="4c468-223">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="4c468-224">Приложение не устанавливает все заголовки запросов, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, и</span><span class="sxs-lookup"><span data-stu-id="4c468-224">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="4c468-225">Заголовок Content-Type (если задать) является одним из следующих:</span><span class="sxs-lookup"><span data-stu-id="4c468-225">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="4c468-226">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="4c468-226">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="4c468-227">данные multipart/формы</span><span class="sxs-lookup"><span data-stu-id="4c468-227">multipart/form-data</span></span>

  * <span data-ttu-id="4c468-228">text/plain.</span><span class="sxs-lookup"><span data-stu-id="4c468-228">text/plain</span></span>

<span data-ttu-id="4c468-229">Заголовки, которые приложение задает путем вызова setRequestHeader на объект XMLHttpRequest применяется правило о заголовках запроса.</span><span class="sxs-lookup"><span data-stu-id="4c468-229">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="4c468-230">(Спецификации CORS вызывает эти «автор запроса заголовки»). Правило не применяется к заголовки, которые можно установить браузер, например User-Agent, узлу или Content-Length.</span><span class="sxs-lookup"><span data-stu-id="4c468-230">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="4c468-231">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="4c468-231">Here is an example of a preflight request:</span></span>

```
OPTIONS http://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: http://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="4c468-232">Предварительный запрос с помощью метода HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="4c468-232">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="4c468-233">Он включает два особых заголовков:</span><span class="sxs-lookup"><span data-stu-id="4c468-233">It includes two special headers:</span></span>

* <span data-ttu-id="4c468-234">Access-Control-Request-Method: HTTP метод, который будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="4c468-234">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="4c468-235">Access-Control-Request-Headers: Список заголовков запросов, которые установлены приложения, для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="4c468-235">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="4c468-236">(Опять же, сюда не входят заголовки, которые задает браузера.)</span><span class="sxs-lookup"><span data-stu-id="4c468-236">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="4c468-237">Ниже приведен пример ответа, при условии, что сервер разрешает запрос:</span><span class="sxs-lookup"><span data-stu-id="4c468-237">Here is an example response, assuming that the server allows the request:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: http://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="4c468-238">Ответ включает и-методы управления доступом — разрешить заголовок, который содержит список допустимых методов и при необходимости заголовок Access-Control-разрешить-Headers, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="4c468-238">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="4c468-239">Если Предварительный запрос завершается успешно, браузер отправляет сам запрос, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="4c468-239">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
