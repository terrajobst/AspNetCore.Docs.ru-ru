---
title: "Включение запросы независимо от источника (CORS)"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 05/17/2017
ms.topic: article
ms.assetid: f9d95e88-4d7e-4d0c-a8e1-47de1128d505
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cors
ms.openlocfilehash: 44936c6db8a003a61091ec9a7fd3be5786d89b5b
ms.sourcegitcommit: 8cafdd1dd409d5070d227100ba0e094c779ac47b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2017
---
# <a name="enabling-cross-origin-requests-cors"></a><span data-ttu-id="4aa23-103">Включение запросы независимо от источника (CORS)</span><span class="sxs-lookup"><span data-stu-id="4aa23-103">Enabling Cross-Origin Requests (CORS)</span></span>

<span data-ttu-id="4aa23-104">По [Mike Wasson](https://github.com/mikewasson), [Бойера Shayne](https://twitter.com/spboyer), и [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4aa23-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4aa23-105">Безопасность обозревателя предотвращает внесение запросы AJAX в другой домен на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="4aa23-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="4aa23-106">Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="4aa23-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="4aa23-107">Однако иногда может потребоваться разрешить другим узлам, которые делают запросы независимо от источника веб-API.</span><span class="sxs-lookup"><span data-stu-id="4aa23-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="4aa23-108">[Кросс-общий доступ к ресурсам источника](http://www.w3.org/TR/cors/) (CORS) — это стандарт W3C, которая позволяет серверу ослабить политика одного источника.</span><span class="sxs-lookup"><span data-stu-id="4aa23-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="4aa23-109">С помощью CORS, сервер можно явно разрешить некоторые запросы независимо от источника при отклонении другим пользователям.</span><span class="sxs-lookup"><span data-stu-id="4aa23-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="4aa23-110">CORS является более безопасным и более гибким, чем ранее методов, например [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="4aa23-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="4aa23-111">В этом разделе показано, как включить CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4aa23-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="4aa23-112">Что такое «того же происхождения»?</span><span class="sxs-lookup"><span data-stu-id="4aa23-112">What is "same origin"?</span></span>

<span data-ttu-id="4aa23-113">Два URL-адреса имеют того же источника, если они имеют одинаковые схемы, узлов и портов.</span><span class="sxs-lookup"><span data-stu-id="4aa23-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="4aa23-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="4aa23-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="4aa23-115">Эти два URL-адреса имеют того же источника:</span><span class="sxs-lookup"><span data-stu-id="4aa23-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="4aa23-116">Эти URL-адреса имеют различные источники, чем предыдущий два:</span><span class="sxs-lookup"><span data-stu-id="4aa23-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="4aa23-117">`http://example.net`-Другой домен</span><span class="sxs-lookup"><span data-stu-id="4aa23-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="4aa23-118">`http://www.example.com/foo.html`-Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="4aa23-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="4aa23-119">`https://example.com/foo.html`-Другой схемы</span><span class="sxs-lookup"><span data-stu-id="4aa23-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="4aa23-120">`http://example.com:9000/foo.html`-Другой порт</span><span class="sxs-lookup"><span data-stu-id="4aa23-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="4aa23-121">При сравнении источников Internet Explorer не учитывает порт.</span><span class="sxs-lookup"><span data-stu-id="4aa23-121">Internet Explorer does not consider the port when comparing origins.</span></span>

## <a name="setting-up-cors"></a><span data-ttu-id="4aa23-122">Настройка CORS</span><span class="sxs-lookup"><span data-stu-id="4aa23-122">Setting up CORS</span></span>

<span data-ttu-id="4aa23-123">Чтобы добавить CORS для вашего приложения `Microsoft.AspNetCore.Cors` пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="4aa23-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

<span data-ttu-id="4aa23-124">Добавление служб CORS в файле Startup.cs:</span><span class="sxs-lookup"><span data-stu-id="4aa23-124">Add the CORS services in Startup.cs:</span></span>

<span data-ttu-id="4aa23-125">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-125">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]</span></span>

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="4aa23-126">Включение CORS с по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="4aa23-126">Enabling CORS with middleware</span></span>

<span data-ttu-id="4aa23-127">Чтобы включить CORS для всего приложения добавьте по промежуточного слоя CORS в конвейер запроса с помощью `UseCors` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="4aa23-127">To enable CORS for your entire application add the CORS middleware to your request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="4aa23-128">Обратите внимание, что по промежуточного слоя CORS должно предшествовать определенные конечные точки в приложении, который требуется поддерживать запросы независимо от источника (например,).</span><span class="sxs-lookup"><span data-stu-id="4aa23-128">Note that the CORS middleware must precede any defined endpoints in your app that you want to support cross-origin requests (ex.</span></span> <span data-ttu-id="4aa23-129">Перед вызовом любого метода `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="4aa23-129">before any call to `UseMvc`).</span></span>

<span data-ttu-id="4aa23-130">Можно задать политику независимо от источника, при добавлении по промежуточного слоя CORS с помощью `CorsPolicyBuilder` класса.</span><span class="sxs-lookup"><span data-stu-id="4aa23-130">You can specify a cross-origin policy when adding the CORS middleware using the `CorsPolicyBuilder` class.</span></span> <span data-ttu-id="4aa23-131">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="4aa23-131">There are two ways to do this.</span></span> <span data-ttu-id="4aa23-132">Первый — вызов UseCors с лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="4aa23-132">The first is to call UseCors with a lambda:</span></span>

<span data-ttu-id="4aa23-133">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-133">[!code-csharp[Main](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]</span></span>

<span data-ttu-id="4aa23-134">**Примечание:** URL-адрес должен быть указан без косой чертой (`/`).</span><span class="sxs-lookup"><span data-stu-id="4aa23-134">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="4aa23-135">Если URL-адрес завершается с `/`, сравнение будет возвращать `false` и будет возвращаться без заголовка.</span><span class="sxs-lookup"><span data-stu-id="4aa23-135">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="4aa23-136">Лямбда-выражение принимает `CorsPolicyBuilder` объекта.</span><span class="sxs-lookup"><span data-stu-id="4aa23-136">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="4aa23-137">Вы найдете список [параметры конфигурации](#cors-policy-options) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="4aa23-137">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="4aa23-138">В этом примере политика разрешает запросы независимо от источника от `http://example.com` и другие источники.</span><span class="sxs-lookup"><span data-stu-id="4aa23-138">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="4aa23-139">Обратите внимание, что CorsPolicyBuilder fluent API, поэтому можно соединить в цепочку вызовы методов:</span><span class="sxs-lookup"><span data-stu-id="4aa23-139">Note that CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

<span data-ttu-id="4aa23-140">[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-140">[!code-csharp[Main](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]</span></span>

<span data-ttu-id="4aa23-141">Второй подход заключается в том, чтобы определить именованный политики CORS, а затем выберите политику по имени во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="4aa23-141">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

<span data-ttu-id="4aa23-142">[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-142">[!code-csharp[Main](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]</span></span>

<span data-ttu-id="4aa23-143">В этом примере добавляется политика CORS с именем «AllowSpecificOrigin».</span><span class="sxs-lookup"><span data-stu-id="4aa23-143">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="4aa23-144">Чтобы выбрать политику, передать имя для `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="4aa23-144">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="4aa23-145">Включение CORS в MVC</span><span class="sxs-lookup"><span data-stu-id="4aa23-145">Enabling CORS in MVC</span></span>

<span data-ttu-id="4aa23-146">Также можно использовать MVC для применения определенных CORS каждого действия каждого контроллера или глобально для всех контроллеров.</span><span class="sxs-lookup"><span data-stu-id="4aa23-146">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="4aa23-147">При использовании MVC для включения CORS используются те же службы CORS, но не по промежуточного слоя CORS.</span><span class="sxs-lookup"><span data-stu-id="4aa23-147">When using MVC to enable CORS the same CORS services are used, but the CORS middleware is not.</span></span>

### <a name="per-action"></a><span data-ttu-id="4aa23-148">Каждого действия</span><span class="sxs-lookup"><span data-stu-id="4aa23-148">Per action</span></span>

<span data-ttu-id="4aa23-149">Чтобы задать политику CORS для определенных действий добавьте `[EnableCors]` атрибут действия.</span><span class="sxs-lookup"><span data-stu-id="4aa23-149">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="4aa23-150">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="4aa23-150">Specify the policy name.</span></span>

<span data-ttu-id="4aa23-151">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-151">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]</span></span>

### <a name="per-controller"></a><span data-ttu-id="4aa23-152">На каждый контроллер</span><span class="sxs-lookup"><span data-stu-id="4aa23-152">Per controller</span></span>

<span data-ttu-id="4aa23-153">Чтобы задать политику CORS для определенного контроллера добавьте `[EnableCors]` атрибут в класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="4aa23-153">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="4aa23-154">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="4aa23-154">Specify the policy name.</span></span>

<span data-ttu-id="4aa23-155">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-155">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]</span></span>

### <a name="globally"></a><span data-ttu-id="4aa23-156">Глобально</span><span class="sxs-lookup"><span data-stu-id="4aa23-156">Globally</span></span>

<span data-ttu-id="4aa23-157">Можно включить CORS глобально для всех контроллеров, добавив `CorsAuthorizationFilterFactory` фильтр в глобальную коллекцию фильтров:</span><span class="sxs-lookup"><span data-stu-id="4aa23-157">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

<span data-ttu-id="4aa23-158">[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-158">[!code-csharp[Main](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]</span></span>

<span data-ttu-id="4aa23-159">Очередность выполнения:: действия контроллера, глобальные.</span><span class="sxs-lookup"><span data-stu-id="4aa23-159">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="4aa23-160">Политики на уровне действия имеют приоритет над политиками уровня контроллера, и уровня контроллера политики имеют приоритет над глобальные политики.</span><span class="sxs-lookup"><span data-stu-id="4aa23-160">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="4aa23-161">Отключить CORS</span><span class="sxs-lookup"><span data-stu-id="4aa23-161">Disable CORS</span></span>

<span data-ttu-id="4aa23-162">Чтобы отключить CORS для контроллера или действия, используйте `[DisableCors]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="4aa23-162">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

<span data-ttu-id="4aa23-163">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-163">[!code-csharp[Main](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]</span></span>

## <a name="cors-policy-options"></a><span data-ttu-id="4aa23-164">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="4aa23-164">CORS policy options</span></span>

<span data-ttu-id="4aa23-165">В этом разделе описываются различные параметры, которые можно задать в политику CORS.</span><span class="sxs-lookup"><span data-stu-id="4aa23-165">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="4aa23-166">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="4aa23-166">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="4aa23-167">Набор разрешенных методов HTTP</span><span class="sxs-lookup"><span data-stu-id="4aa23-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="4aa23-168">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="4aa23-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="4aa23-169">Задать заголовки ответа предоставляется</span><span class="sxs-lookup"><span data-stu-id="4aa23-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="4aa23-170">Учетные данные в запросы независимо от источника</span><span class="sxs-lookup"><span data-stu-id="4aa23-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="4aa23-171">Задайте предварительный истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="4aa23-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="4aa23-172">Для некоторых параметров может оказаться удобным для чтения [работает как CORS](#how-cors-works) первой.</span><span class="sxs-lookup"><span data-stu-id="4aa23-172">For some options it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="4aa23-173">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="4aa23-173">Set the allowed origins</span></span>

<span data-ttu-id="4aa23-174">Чтобы разрешить один или несколько конкретных источников:</span><span class="sxs-lookup"><span data-stu-id="4aa23-174">To allow one or more specific origins:</span></span>

<span data-ttu-id="4aa23-175">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-175">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=19-23)]</span></span>

<span data-ttu-id="4aa23-176">Чтобы разрешить все источники:</span><span class="sxs-lookup"><span data-stu-id="4aa23-176">To allow all origins:</span></span>

<span data-ttu-id="4aa23-177">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-177">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs??range=27-31)]</span></span>

<span data-ttu-id="4aa23-178">Внимательно рассмотрите перед предоставлением запросов из любого источника.</span><span class="sxs-lookup"><span data-stu-id="4aa23-178">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="4aa23-179">Он означает, что практически любой веб-сайта можно вносить вызовы AJAX к вашему API.</span><span class="sxs-lookup"><span data-stu-id="4aa23-179">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="4aa23-180">Набор разрешенных методов HTTP</span><span class="sxs-lookup"><span data-stu-id="4aa23-180">Set the allowed HTTP methods</span></span>

<span data-ttu-id="4aa23-181">Чтобы разрешить все методы HTTP:</span><span class="sxs-lookup"><span data-stu-id="4aa23-181">To allow all HTTP methods:</span></span>

<span data-ttu-id="4aa23-182">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-182">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=44-49)]</span></span>

<span data-ttu-id="4aa23-183">Это влияет на возможность предварительного запросов и -методы управления доступом — разрешить заголовок.</span><span class="sxs-lookup"><span data-stu-id="4aa23-183">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="4aa23-184">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="4aa23-184">Set the allowed request headers</span></span>

<span data-ttu-id="4aa23-185">Предварительный запрос CORS может включать заголовок Access-Control-Request-Headers список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»).</span><span class="sxs-lookup"><span data-stu-id="4aa23-185">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="4aa23-186">Белый список указанные заголовки:</span><span class="sxs-lookup"><span data-stu-id="4aa23-186">To whitelist specific headers:</span></span>

<span data-ttu-id="4aa23-187">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-187">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=53-58)]</span></span>

<span data-ttu-id="4aa23-188">Чтобы разрешить все создавать заголовки запроса:</span><span class="sxs-lookup"><span data-stu-id="4aa23-188">To allow all author request headers:</span></span>

<span data-ttu-id="4aa23-189">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-189">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=62-67)]</span></span>

<span data-ttu-id="4aa23-190">Браузеры не полностью соответствуют в установке Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="4aa23-190">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="4aa23-191">Если задать заголовки на что-либо отличное от «*», следует включать по крайней мере «принять,» «content-type» и «источник», а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="4aa23-191">If you set headers to anything other than "*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="4aa23-192">Задать заголовки ответа предоставляется</span><span class="sxs-lookup"><span data-stu-id="4aa23-192">Set the exposed response headers</span></span>

<span data-ttu-id="4aa23-193">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="4aa23-193">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="4aa23-194">(См. [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="4aa23-194">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="4aa23-195">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="4aa23-195">Cache-Control</span></span>

* <span data-ttu-id="4aa23-196">Content-Language</span><span class="sxs-lookup"><span data-stu-id="4aa23-196">Content-Language</span></span>

* <span data-ttu-id="4aa23-197">Тип содержимого</span><span class="sxs-lookup"><span data-stu-id="4aa23-197">Content-Type</span></span>

* <span data-ttu-id="4aa23-198">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="4aa23-198">Expires</span></span>

* <span data-ttu-id="4aa23-199">Дата последнего изменения</span><span class="sxs-lookup"><span data-stu-id="4aa23-199">Last-Modified</span></span>

* <span data-ttu-id="4aa23-200">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="4aa23-200">Pragma</span></span>

<span data-ttu-id="4aa23-201">Спецификация CORS вызывает эти *заголовки ответа на простой*.</span><span class="sxs-lookup"><span data-stu-id="4aa23-201">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="4aa23-202">Чтобы сделать другие заголовки, доступны для приложения.</span><span class="sxs-lookup"><span data-stu-id="4aa23-202">To make other headers available to the application:</span></span>

<span data-ttu-id="4aa23-203">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-203">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=71-76)]</span></span>

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="4aa23-204">Учетные данные в запросы независимо от источника</span><span class="sxs-lookup"><span data-stu-id="4aa23-204">Credentials in cross-origin requests</span></span>

<span data-ttu-id="4aa23-205">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="4aa23-205">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="4aa23-206">По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4aa23-206">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="4aa23-207">Учетные данные включают файлы cookie, а также схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="4aa23-207">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="4aa23-208">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен задать XMLHttpRequest.withCredentials значение true.</span><span class="sxs-lookup"><span data-stu-id="4aa23-208">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="4aa23-209">Непосредственно с помощью XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="4aa23-209">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="4aa23-210">В jQuery:</span><span class="sxs-lookup"><span data-stu-id="4aa23-210">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="4aa23-211">Кроме того сервер необходимо разрешить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="4aa23-211">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="4aa23-212">Чтобы разрешить учетные данные независимо от источника:</span><span class="sxs-lookup"><span data-stu-id="4aa23-212">To allow cross-origin credentials:</span></span>

<span data-ttu-id="4aa23-213">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-213">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=80-85)]</span></span>

<span data-ttu-id="4aa23-214">Теперь в HTTP-ответе будет включать доступ-элемент управления-Allow-Credentials заголовок, который предписывает браузеру, поддерживает ли сервер учетные данные для запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4aa23-214">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="4aa23-215">Если браузер отправляет учетные данные, но ответа не содержит допустимый заголовок доступа-элемент управления-Allow-Credentials, браузер не предоставляет приложению ответ и происходит сбой запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="4aa23-215">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="4aa23-216">Следует с осторожностью позволяя учетных данных независимо от источника, так как это означает, что веб-сайт в другом домене может отправлять учетные данные вошедшего в систему пользователя в приложение от имени пользователя без оповещения пользователя.</span><span class="sxs-lookup"><span data-stu-id="4aa23-216">Be very careful about allowing cross-origin credentials, because it means a website at another domain can send a logged-in user’s credentials to your app on the user’s behalf, without the user being aware.</span></span> <span data-ttu-id="4aa23-217">CORS spec также состояния этого параметра источники, которые можно «*» (все источники) является недопустимым, если присутствует заголовок доступа-элемент управления-Allow-Credentials.</span><span class="sxs-lookup"><span data-stu-id="4aa23-217">The CORS spec also states that setting origins to "*" (all origins) is invalid if the Access-Control-Allow-Credentials header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="4aa23-218">Задайте предварительный истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="4aa23-218">Set the preflight expiration time</span></span>

<span data-ttu-id="4aa23-219">Заголовок доступа-элемент управления-Max-Age указывает, как долго ответ на Предварительный запрос может быть кэширован.</span><span class="sxs-lookup"><span data-stu-id="4aa23-219">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="4aa23-220">Для установки этого заголовка:</span><span class="sxs-lookup"><span data-stu-id="4aa23-220">To set this header:</span></span>

<span data-ttu-id="4aa23-221">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]</span><span class="sxs-lookup"><span data-stu-id="4aa23-221">[!code-csharp[Main](cors/sample/CorsExample4/Startup.cs?range=89-94)]</span></span>

<a name=cors-how-cors-works></a>

## <a name="how-cors-works"></a><span data-ttu-id="4aa23-222">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="4aa23-222">How CORS works</span></span>

<span data-ttu-id="4aa23-223">В этом разделе описывается, что происходит в запрос CORS, на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="4aa23-223">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="4aa23-224">Важно понять, как работает CORS, чтобы правильно настроить политики CORS и устранения неполадок, если не все работает должным образом.</span><span class="sxs-lookup"><span data-stu-id="4aa23-224">It’s important to understand how CORS works, so that you can configure your CORS policy correctly, and troubleshoot if things don’t work as you expect.</span></span>

<span data-ttu-id="4aa23-225">Спецификация CORS представлены несколько заголовки HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4aa23-225">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="4aa23-226">Если браузер поддерживает CORS, он устанавливает эти заголовки автоматически для запросов независимо от источника. не требуется никаких действий в коде JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4aa23-226">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don’t need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="4aa23-227">Ниже приведен пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="4aa23-227">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="4aa23-228">Заголовок «Источник» предоставляет домена сайта, который был выполнен запрос:</span><span class="sxs-lookup"><span data-stu-id="4aa23-228">The "Origin" header gives the domain of the site that is making the request:</span></span>

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

<span data-ttu-id="4aa23-229">Если сервер разрешает запрос, он устанавливает заголовка Access-Control-Allow-Origin.</span><span class="sxs-lookup"><span data-stu-id="4aa23-229">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="4aa23-230">Значение этого заголовка соответствует заголовку источника либо является использование подстановочного знака «*», это значит, что разрешены любые источники.:</span><span class="sxs-lookup"><span data-stu-id="4aa23-230">The value of this header either matches the Origin header, or is the wildcard value "*", meaning that any origin is allowed.:</span></span>

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

<span data-ttu-id="4aa23-231">Если ответ содержит Access-Control-Allow-Origin заголовок, происходит сбой запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="4aa23-231">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="4aa23-232">В частности браузер блокирует запрос.</span><span class="sxs-lookup"><span data-stu-id="4aa23-232">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="4aa23-233">Даже если сервер возвращает успешный ответ, браузер не делает ответ доступной клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="4aa23-233">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="4aa23-234">Предварительные запросы</span><span class="sxs-lookup"><span data-stu-id="4aa23-234">Preflight Requests</span></span>

<span data-ttu-id="4aa23-235">Для некоторых запросов CORS браузер отправляет запрос на дополнительные, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса.</span><span class="sxs-lookup"><span data-stu-id="4aa23-235">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="4aa23-236">Браузер может пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="4aa23-236">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="4aa23-237">Метод запроса является GET, HEAD или POST, и</span><span class="sxs-lookup"><span data-stu-id="4aa23-237">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="4aa23-238">Приложение не устанавливает все заголовки запросов, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, и</span><span class="sxs-lookup"><span data-stu-id="4aa23-238">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="4aa23-239">Заголовок Content-Type (если задать) является одним из следующих:</span><span class="sxs-lookup"><span data-stu-id="4aa23-239">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="4aa23-240">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="4aa23-240">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="4aa23-241">данные multipart/формы</span><span class="sxs-lookup"><span data-stu-id="4aa23-241">multipart/form-data</span></span>

  * <span data-ttu-id="4aa23-242">text/plain.</span><span class="sxs-lookup"><span data-stu-id="4aa23-242">text/plain</span></span>

<span data-ttu-id="4aa23-243">Заголовки, которые приложение задает путем вызова setRequestHeader на объект XMLHttpRequest применяется правило о заголовках запроса.</span><span class="sxs-lookup"><span data-stu-id="4aa23-243">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="4aa23-244">(Спецификации CORS вызывает эти «автор запроса заголовки»). Правило не применяется к заголовки, которые можно установить браузер, например User-Agent, узлу или Content-Length.</span><span class="sxs-lookup"><span data-stu-id="4aa23-244">(The CORS specification calls these "author request headers".) The rule does not apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="4aa23-245">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="4aa23-245">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="4aa23-246">Предварительный запрос с помощью метода HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="4aa23-246">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="4aa23-247">Он включает два особых заголовков:</span><span class="sxs-lookup"><span data-stu-id="4aa23-247">It includes two special headers:</span></span>

* <span data-ttu-id="4aa23-248">Access-Control-Request-Method: HTTP метод, который будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="4aa23-248">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="4aa23-249">Access-Control-Request-Headers: Список заголовков запросов, которые установлены приложения, для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="4aa23-249">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="4aa23-250">(Опять же, это не включает заголовки, которые задает браузера.)</span><span class="sxs-lookup"><span data-stu-id="4aa23-250">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="4aa23-251">Ниже приведен пример ответа, при условии, что сервер разрешает запрос:</span><span class="sxs-lookup"><span data-stu-id="4aa23-251">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="4aa23-252">Ответ включает и-методы управления доступом — разрешить заголовок, который содержит список допустимых методов и при необходимости заголовок Access-Control-разрешить-Headers, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="4aa23-252">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="4aa23-253">Если Предварительный запрос завершается успешно, браузер отправляет сам запрос, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="4aa23-253">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
