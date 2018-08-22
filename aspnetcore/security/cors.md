---
title: Включение запросов о происхождении (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как CORS в качестве стандарта для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core.
ms.author: riande
ms.date: 08/17/2018
uid: security/cors
ms.openlocfilehash: 0dbb7933c76bb0d1d0cab519ea08c6c8f0ebedfd
ms.sourcegitcommit: 64c2ca86fff445944b155635918126165ee0f8aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/18/2018
ms.locfileid: "41838936"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="07bd8-103">Включение запросов о происхождении (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07bd8-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="07bd8-104">По [Майк Уоссон](https://github.com/mikewasson), [Шейн Бойер](https://twitter.com/spboyer), и [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="07bd8-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="07bd8-105">Безопасность обозревателя запрещает отправку запросов AJAX в другой домен веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="07bd8-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="07bd8-106">Это ограничение называется *политика одного источника*и предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="07bd8-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="07bd8-107">Тем не менее иногда вам может потребоваться разрешить другие сайты, которые выполняют запросы независимо от источника к веб-API.</span><span class="sxs-lookup"><span data-stu-id="07bd8-107">However, sometimes you might want to let other sites make cross-origin requests to your web API.</span></span>

<span data-ttu-id="07bd8-108">[Кросс-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) — это стандарт консорциума W3C, позволяющий серверу смягчить ограничения политики одного источника.</span><span class="sxs-lookup"><span data-stu-id="07bd8-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="07bd8-109">С помощью CORS сервер может явным образом разрешить некоторые запросы независимо от источника а другие — отклонять.</span><span class="sxs-lookup"><span data-stu-id="07bd8-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="07bd8-110">CORS — более безопасное и более гибким, чем предыдущие технологии, такие как [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="07bd8-110">CORS is safer and more flexible than earlier techniques such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="07bd8-111">В этом разделе показано, как включить поддержку CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="07bd8-111">This topic shows how to enable CORS in an ASP.NET Core application.</span></span>

## <a name="what-is-same-origin"></a><span data-ttu-id="07bd8-112">Что такое «того же происхождения»?</span><span class="sxs-lookup"><span data-stu-id="07bd8-112">What is "same origin"?</span></span>

<span data-ttu-id="07bd8-113">Два URL-адреса иметь того же происхождения, если они имеют одинаковые схемы, узлов и порты.</span><span class="sxs-lookup"><span data-stu-id="07bd8-113">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="07bd8-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="07bd8-114">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="07bd8-115">Эти два URL-адреса у того же происхождения:</span><span class="sxs-lookup"><span data-stu-id="07bd8-115">These two URLs have the same origin:</span></span>

* `http://example.com/foo.html`

* `http://example.com/bar.html`

<span data-ttu-id="07bd8-116">Эти URL-адреса имеют различное происхождение по сравнению с предыдущим два:</span><span class="sxs-lookup"><span data-stu-id="07bd8-116">These URLs have different origins than the previous two:</span></span>

* <span data-ttu-id="07bd8-117">`http://example.net` -Другой домен</span><span class="sxs-lookup"><span data-stu-id="07bd8-117">`http://example.net` - Different domain</span></span>

* <span data-ttu-id="07bd8-118">`http://www.example.com/foo.html` -Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="07bd8-118">`http://www.example.com/foo.html` - Different subdomain</span></span>

* <span data-ttu-id="07bd8-119">`https://example.com/foo.html` -Другую схему</span><span class="sxs-lookup"><span data-stu-id="07bd8-119">`https://example.com/foo.html` - Different scheme</span></span>

* <span data-ttu-id="07bd8-120">`http://example.com:9000/foo.html` -Другой порт</span><span class="sxs-lookup"><span data-stu-id="07bd8-120">`http://example.com:9000/foo.html` - Different port</span></span>

> [!NOTE]
> <span data-ttu-id="07bd8-121">Internet Explorer не считает порт, при сравнении источников.</span><span class="sxs-lookup"><span data-stu-id="07bd8-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="enable-cors"></a><span data-ttu-id="07bd8-122">Включение CORS</span><span class="sxs-lookup"><span data-stu-id="07bd8-122">Enable CORS</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="07bd8-123">Чтобы настроить CORS для приложения добавьте `Microsoft.AspNetCore.Cors` пакета в проект.</span><span class="sxs-lookup"><span data-stu-id="07bd8-123">To set up CORS for your application add the `Microsoft.AspNetCore.Cors` package to your project.</span></span>

::: moniker-end

<span data-ttu-id="07bd8-124">Вызовите [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="07bd8-124">Call [AddCors](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors)]

## <a name="enabling-cors-with-middleware"></a><span data-ttu-id="07bd8-125">Включение CORS с по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="07bd8-125">Enabling CORS with middleware</span></span>

<span data-ttu-id="07bd8-126">Чтобы включить CORS, добавьте по промежуточного слоя CORS в конвейер запросов с помощью `UseCors` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="07bd8-126">To enable CORS, add the CORS middleware to the request pipeline using the `UseCors` extension method.</span></span> <span data-ttu-id="07bd8-127">По промежуточного слоя CORS должен предшествовать любой определены конечные точки в приложении место для поддержки запросов о происхождении (например, предшествующий вызову `UseMvc`).</span><span class="sxs-lookup"><span data-stu-id="07bd8-127">The CORS middleware must precede any defined endpoints in your app where you want to support cross-origin requests (For example, before any call to `UseMvc`).</span></span>

<span data-ttu-id="07bd8-128">Можно указать политику независимо от источника, при добавлении по промежуточного слоя CORS с помощью [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) класса.</span><span class="sxs-lookup"><span data-stu-id="07bd8-128">A cross-origin policy can be specified when adding the CORS middleware using the [CorsPolicyBuilder](/dotnet/api/microsoft.extensions.dependencyinjection.corsservicecollectionextensions.addcors) class.</span></span> <span data-ttu-id="07bd8-129">Это можно сделать двумя способами.</span><span class="sxs-lookup"><span data-stu-id="07bd8-129">There are two ways to do this.</span></span> <span data-ttu-id="07bd8-130">Первый способ — вызвать `UseCors` с лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="07bd8-130">The first is to call `UseCors` with a lambda:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

<span data-ttu-id="07bd8-131">**Примечание:** URL-адрес должен быть указан без косой чертой (`/`).</span><span class="sxs-lookup"><span data-stu-id="07bd8-131">**Note:** The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="07bd8-132">Если URL-адрес заканчивается `/`, сравнение вернет `false` и будет возвращаться без заголовка.</span><span class="sxs-lookup"><span data-stu-id="07bd8-132">If the URL terminates with `/`, the comparison will return `false` and no header will be returned.</span></span>

<span data-ttu-id="07bd8-133">Лямбда-выражение принимает `CorsPolicyBuilder` объекта.</span><span class="sxs-lookup"><span data-stu-id="07bd8-133">The lambda takes a `CorsPolicyBuilder` object.</span></span> <span data-ttu-id="07bd8-134">Вы найдете список [параметры конфигурации](#cors-policy-options) далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="07bd8-134">You'll find a list of the [configuration options](#cors-policy-options) later in this topic.</span></span> <span data-ttu-id="07bd8-135">В этом примере политика позволяет запросов о происхождении из `http://example.com` и другие источники.</span><span class="sxs-lookup"><span data-stu-id="07bd8-135">In this example, the policy allows cross-origin requests from `http://example.com` and no other origins.</span></span>

<span data-ttu-id="07bd8-136">CorsPolicyBuilder имеет текучего API, поэтому можно объединять в цепочку вызовов методов:</span><span class="sxs-lookup"><span data-stu-id="07bd8-136">CorsPolicyBuilder has a fluent API, so you can chain method calls:</span></span>

[!code-csharp[](../security/cors/sample/CorsExample3/Startup.cs?highlight=3&range=29-32)]

<span data-ttu-id="07bd8-137">Второй подход заключается в том, чтобы определить один или несколько именованных политик CORS, а затем выбрать политику по имени во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="07bd8-137">The second approach is to define one or more named CORS policies, and then select the policy by name at run time.</span></span>

[!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin)]

<span data-ttu-id="07bd8-138">Этот пример добавляет политику CORS, с именем «AllowSpecificOrigin».</span><span class="sxs-lookup"><span data-stu-id="07bd8-138">This example adds a CORS policy named "AllowSpecificOrigin".</span></span> <span data-ttu-id="07bd8-139">Чтобы выбрать политику, передайте имя в `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="07bd8-139">To select the policy, pass the name to `UseCors`.</span></span>

## <a name="enabling-cors-in-mvc"></a><span data-ttu-id="07bd8-140">Включение CORS в MVC</span><span class="sxs-lookup"><span data-stu-id="07bd8-140">Enabling CORS in MVC</span></span>

<span data-ttu-id="07bd8-141">Также можно использовать MVC для применения определенных CORS каждого действия, отдельного контроллера или глобально для всех контроллеров.</span><span class="sxs-lookup"><span data-stu-id="07bd8-141">You can alternatively use MVC to apply specific CORS per action, per controller, or globally for all controllers.</span></span> <span data-ttu-id="07bd8-142">При использовании MVC для включения CORS используются те же службы CORS, но не по промежуточного слоя CORS.</span><span class="sxs-lookup"><span data-stu-id="07bd8-142">When using MVC to enable CORS the same CORS services are used, but the CORS middleware isn't.</span></span>

### <a name="per-action"></a><span data-ttu-id="07bd8-143">Каждого действия</span><span class="sxs-lookup"><span data-stu-id="07bd8-143">Per action</span></span>

<span data-ttu-id="07bd8-144">Чтобы указать политику CORS для определенного действия добавьте `[EnableCors]` атрибут к действию.</span><span class="sxs-lookup"><span data-stu-id="07bd8-144">To specify a CORS policy for a specific action add the `[EnableCors]` attribute to the action.</span></span> <span data-ttu-id="07bd8-145">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="07bd8-145">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction)]

### <a name="per-controller"></a><span data-ttu-id="07bd8-146">Для контроллера</span><span class="sxs-lookup"><span data-stu-id="07bd8-146">Per controller</span></span>

<span data-ttu-id="07bd8-147">Чтобы указать политику CORS для определенного контроллера добавьте `[EnableCors]` атрибут в класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="07bd8-147">To specify the CORS policy for a specific controller add the `[EnableCors]` attribute to the controller class.</span></span> <span data-ttu-id="07bd8-148">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="07bd8-148">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController)]

### <a name="globally"></a><span data-ttu-id="07bd8-149">Глобально</span><span class="sxs-lookup"><span data-stu-id="07bd8-149">Globally</span></span>

<span data-ttu-id="07bd8-150">Вы можете включить CORS глобально для всех контроллеров, добавив `CorsAuthorizationFilterFactory` фильтр в глобальную коллекцию фильтров:</span><span class="sxs-lookup"><span data-stu-id="07bd8-150">You can enable CORS globally for all controllers by adding the `CorsAuthorizationFilterFactory` filter to the global filter collection:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Startup2.cs?name=snippet_configureservices)]

<span data-ttu-id="07bd8-151">Очередность: действия, контроллера, глобальные.</span><span class="sxs-lookup"><span data-stu-id="07bd8-151">The precedence order is: Action, controller, global.</span></span> <span data-ttu-id="07bd8-152">Политики на уровне действия имеют приоритет над политиками уровня контроллера, и политики на уровне контроллера имеют приоритет над глобальные политики.</span><span class="sxs-lookup"><span data-stu-id="07bd8-152">Action-level policies take precedence over controller-level policies, and controller-level policies take precedence over global policies.</span></span>

### <a name="disable-cors"></a><span data-ttu-id="07bd8-153">Отключить CORS</span><span class="sxs-lookup"><span data-stu-id="07bd8-153">Disable CORS</span></span>

<span data-ttu-id="07bd8-154">Чтобы отключить CORS для контроллера или действия, используйте `[DisableCors]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="07bd8-154">To disable CORS for a controller or action, use the `[DisableCors]` attribute.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction)]

## <a name="cors-policy-options"></a><span data-ttu-id="07bd8-155">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="07bd8-155">CORS policy options</span></span>

<span data-ttu-id="07bd8-156">В этом разделе описываются различные параметры, которые можно задать в политику CORS.</span><span class="sxs-lookup"><span data-stu-id="07bd8-156">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="07bd8-157">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="07bd8-157">Set the allowed origins</span></span>](#set-the-allowed-origins)

* [<span data-ttu-id="07bd8-158">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="07bd8-158">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)

* [<span data-ttu-id="07bd8-159">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="07bd8-159">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)

* [<span data-ttu-id="07bd8-160">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="07bd8-160">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)

* [<span data-ttu-id="07bd8-161">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="07bd8-161">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)

* [<span data-ttu-id="07bd8-162">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="07bd8-162">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="07bd8-163">Для некоторых параметров, может оказаться удобным для чтения [работает как CORS](#how-cors-works) первого.</span><span class="sxs-lookup"><span data-stu-id="07bd8-163">For some options, it may be helpful to read [How CORS works](#how-cors-works) first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="07bd8-164">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="07bd8-164">Set the allowed origins</span></span>

<span data-ttu-id="07bd8-165">Чтобы разрешить один или несколько определенных источников:</span><span class="sxs-lookup"><span data-stu-id="07bd8-165">To allow one or more specific origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=19-23)]

<span data-ttu-id="07bd8-166">Для разрешения всех источников:</span><span class="sxs-lookup"><span data-stu-id="07bd8-166">To allow all origins:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs??range=27-31)]

<span data-ttu-id="07bd8-167">Тщательно обдумайте прежде чем разрешить запросы из любого источника.</span><span class="sxs-lookup"><span data-stu-id="07bd8-167">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="07bd8-168">Это означает, что буквально любой веб-сайт можно вызовы AJAX к вашему API.</span><span class="sxs-lookup"><span data-stu-id="07bd8-168">It means that literally any website can make AJAX calls to your API.</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="07bd8-169">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="07bd8-169">Set the allowed HTTP methods</span></span>

<span data-ttu-id="07bd8-170">Чтобы разрешить все методы HTTP:</span><span class="sxs-lookup"><span data-stu-id="07bd8-170">To allow all HTTP methods:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=44-49)]

<span data-ttu-id="07bd8-171">Это влияет на предварительных запросов и заголовка Access-Control-Allow-Methods.</span><span class="sxs-lookup"><span data-stu-id="07bd8-171">This affects pre-flight requests and Access-Control-Allow-Methods header.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="07bd8-172">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="07bd8-172">Set the allowed request headers</span></span>

<span data-ttu-id="07bd8-173">Предварительный запрос CORS может включать заголовок Access-Control-Request-Headers, список заголовков HTTP, установленный приложением (так называемого «author заголовки запроса»).</span><span class="sxs-lookup"><span data-stu-id="07bd8-173">A CORS preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span>

<span data-ttu-id="07bd8-174">В список разрешений определенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="07bd8-174">To whitelist specific headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=53-58)]

<span data-ttu-id="07bd8-175">Чтобы разрешить все создавать заголовки запроса:</span><span class="sxs-lookup"><span data-stu-id="07bd8-175">To allow all author request headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=62-67)]

<span data-ttu-id="07bd8-176">Браузеры не полностью соответствуют в установке Access-Control-Request-Headers.</span><span class="sxs-lookup"><span data-stu-id="07bd8-176">Browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="07bd8-177">Если задать заголовки на что-либо отличное от «\*», следует включать по крайней мере «принять,» «content-type» и «началом координат», а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="07bd8-177">If you set headers to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="07bd8-178">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="07bd8-178">Set the exposed response headers</span></span>

<span data-ttu-id="07bd8-179">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="07bd8-179">By default, the browser doesn't expose all of the response headers to the application.</span></span> <span data-ttu-id="07bd8-180">(См. в разделе [ http://www.w3.org/TR/cors/#simple-response-header ](http://www.w3.org/TR/cors/#simple-response-header).) Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="07bd8-180">(See [http://www.w3.org/TR/cors/#simple-response-header](http://www.w3.org/TR/cors/#simple-response-header).) The response headers that are available by default are:</span></span>

* <span data-ttu-id="07bd8-181">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="07bd8-181">Cache-Control</span></span>

* <span data-ttu-id="07bd8-182">Content-Language</span><span class="sxs-lookup"><span data-stu-id="07bd8-182">Content-Language</span></span>

* <span data-ttu-id="07bd8-183">Content-Type</span><span class="sxs-lookup"><span data-stu-id="07bd8-183">Content-Type</span></span>

* <span data-ttu-id="07bd8-184">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="07bd8-184">Expires</span></span>

* <span data-ttu-id="07bd8-185">Дата последнего изменения</span><span class="sxs-lookup"><span data-stu-id="07bd8-185">Last-Modified</span></span>

* <span data-ttu-id="07bd8-186">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="07bd8-186">Pragma</span></span>

<span data-ttu-id="07bd8-187">Спецификация CORS вызывает эти *заголовки ответа на простой*.</span><span class="sxs-lookup"><span data-stu-id="07bd8-187">The CORS spec calls these *simple response headers*.</span></span> <span data-ttu-id="07bd8-188">Чтобы сделать доступными для приложения другие заголовки:</span><span class="sxs-lookup"><span data-stu-id="07bd8-188">To make other headers available to the application:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=71-76)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="07bd8-189">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="07bd8-189">Credentials in cross-origin requests</span></span>

<span data-ttu-id="07bd8-190">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="07bd8-190">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="07bd8-191">По умолчанию браузер не отправляет никаких учетных данных с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="07bd8-191">By default, the browser doesn't send any credentials with a cross-origin request.</span></span> <span data-ttu-id="07bd8-192">Учетные данные содержат файлы cookie, а также схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="07bd8-192">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="07bd8-193">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать XMLHttpRequest.withCredentials значение true.</span><span class="sxs-lookup"><span data-stu-id="07bd8-193">To send credentials with a cross-origin request, the client must set XMLHttpRequest.withCredentials to true.</span></span>

<span data-ttu-id="07bd8-194">Непосредственное использование XMLHttpRequest:</span><span class="sxs-lookup"><span data-stu-id="07bd8-194">Using XMLHttpRequest directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'http://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="07bd8-195">В jQuery:</span><span class="sxs-lookup"><span data-stu-id="07bd8-195">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'http://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="07bd8-196">Кроме того сервер необходимо разрешить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="07bd8-196">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="07bd8-197">Чтобы разрешить учетные данные от источника:</span><span class="sxs-lookup"><span data-stu-id="07bd8-197">To allow cross-origin credentials:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=80-85)]

<span data-ttu-id="07bd8-198">Теперь HTTP-ответа будет включать заголовок доступа-элемент управления-Allow-Credentials, который указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.</span><span class="sxs-lookup"><span data-stu-id="07bd8-198">Now the HTTP response will include an Access-Control-Allow-Credentials header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="07bd8-199">Если браузер отправляет учетные данные, но ответ не содержит допустимый заголовка Access-элемент управления-Allow-Credentials, браузер не возвращают ответ в приложение, и сбоя запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="07bd8-199">If the browser sends credentials, but the response doesn't include a valid Access-Control-Allow-Credentials header, the browser won't expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="07bd8-200">Будьте внимательны, если разрешить передачу учетных данных независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="07bd8-200">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="07bd8-201">Веб-сайт в другом домене может отправлять учетные данные вошедшего в систему пользователя приложения от имени пользователя без уведомления пользователя.</span><span class="sxs-lookup"><span data-stu-id="07bd8-201">A website at another domain can send a logged-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <span data-ttu-id="07bd8-202">Спецификация CORS также указывает, что параметр источники, которые можно `"*"` (все источники) является недопустимым при `Access-Control-Allow-Credentials` заголовок отсутствует.</span><span class="sxs-lookup"><span data-stu-id="07bd8-202">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="07bd8-203">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="07bd8-203">Set the preflight expiration time</span></span>

<span data-ttu-id="07bd8-204">Заголовок доступа-элемент управления-Max-Age указывает, как долго можно кэшировать ответ на Предварительный запрос.</span><span class="sxs-lookup"><span data-stu-id="07bd8-204">The Access-Control-Max-Age header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="07bd8-205">Чтобы задать этот заголовок:</span><span class="sxs-lookup"><span data-stu-id="07bd8-205">To set this header:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=89-94)]

<a name="cors-how-cors-works"></a>

## <a name="how-cors-works"></a><span data-ttu-id="07bd8-206">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="07bd8-206">How CORS works</span></span>

<span data-ttu-id="07bd8-207">В этом разделе описывается, что происходит в запрос CORS на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="07bd8-207">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="07bd8-208">Важно понять, как работает CORS, чтобы политика CORS можно настроить правильно и отладки при возникновении непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="07bd8-208">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="07bd8-209">Спецификация CORS представляет ряд новых заголовков HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="07bd8-209">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="07bd8-210">Если браузер поддерживает CORS, он устанавливает эти заголовки для запросов о происхождении автоматически.</span><span class="sxs-lookup"><span data-stu-id="07bd8-210">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="07bd8-211">Пользовательский код JavaScript не обязательно для включения CORS.</span><span class="sxs-lookup"><span data-stu-id="07bd8-211">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="07bd8-212">Вот пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="07bd8-212">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="07bd8-213">`Origin` Заголовок предоставляет домена сайта, который выполняет запрос:</span><span class="sxs-lookup"><span data-stu-id="07bd8-213">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="07bd8-214">Если сервер разрешает запрос, он задает заголовка Access-Control-Allow-Origin в ответе.</span><span class="sxs-lookup"><span data-stu-id="07bd8-214">If the server allows the request, it sets the Access-Control-Allow-Origin header in the response.</span></span> <span data-ttu-id="07bd8-215">Значение этого заголовка соответствует заголовку источника из запроса, либо значение подстановочный знак «\*», это значит, что допускается любого источника:</span><span class="sxs-lookup"><span data-stu-id="07bd8-215">The value of this header either matches the Origin header from the request, or is the wildcard value "\*", meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="07bd8-216">Если ответ не содержит заголовка Access-Control-Allow-Origin, сбоя запроса AJAX.</span><span class="sxs-lookup"><span data-stu-id="07bd8-216">If the response doesn't include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="07bd8-217">В частности браузер запрещает запрос.</span><span class="sxs-lookup"><span data-stu-id="07bd8-217">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="07bd8-218">Даже если сервер возвращает успешный ответ, браузер не предоставить ответ клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="07bd8-218">Even if the server returns a successful response, the browser doesn't make the response available to the client application.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="07bd8-219">Предварительные запросы</span><span class="sxs-lookup"><span data-stu-id="07bd8-219">Preflight Requests</span></span>

<span data-ttu-id="07bd8-220">Для некоторых запросов CORS браузер посылает дополнительный запрос, называется «Предварительный запрос,» перед отправкой самого запроса для ресурса.</span><span class="sxs-lookup"><span data-stu-id="07bd8-220">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span> <span data-ttu-id="07bd8-221">Браузер можно пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="07bd8-221">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="07bd8-222">Метод запроса — GET, HEAD или POST, и</span><span class="sxs-lookup"><span data-stu-id="07bd8-222">The request method is GET, HEAD, or POST, and</span></span>

* <span data-ttu-id="07bd8-223">Приложение не устанавливает все заголовки запроса, отличные от Accept, Accept-Language, Content-Language, Content-Type или последнего-событие-ID, и</span><span class="sxs-lookup"><span data-stu-id="07bd8-223">The application doesn't set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, and</span></span>

* <span data-ttu-id="07bd8-224">Заголовок Content-Type (если задать) является одним из следующих:</span><span class="sxs-lookup"><span data-stu-id="07bd8-224">The Content-Type header (if set) is one of the following:</span></span>

  * <span data-ttu-id="07bd8-225">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="07bd8-225">application/x-www-form-urlencoded</span></span>

  * <span data-ttu-id="07bd8-226">данные multipart/формы</span><span class="sxs-lookup"><span data-stu-id="07bd8-226">multipart/form-data</span></span>

  * <span data-ttu-id="07bd8-227">text/plain</span><span class="sxs-lookup"><span data-stu-id="07bd8-227">text/plain</span></span>

<span data-ttu-id="07bd8-228">Правило о заголовках запроса применяется к заголовки, которые приложение задает путем вызова setRequestHeader для объекта XMLHttpRequest.</span><span class="sxs-lookup"><span data-stu-id="07bd8-228">The rule about request headers applies to headers that the application sets by calling setRequestHeader on the XMLHttpRequest object.</span></span> <span data-ttu-id="07bd8-229">(Спецификации CORS вызывает эти «заголовки запроса автора»). Правило не применяется к заголовки, которые можно установить браузер, например User-Agent, узла или Content-Length.</span><span class="sxs-lookup"><span data-stu-id="07bd8-229">(The CORS specification calls these "author request headers".) The rule doesn't apply to headers the browser can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="07bd8-230">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="07bd8-230">Here is an example of a preflight request:</span></span>

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

<span data-ttu-id="07bd8-231">Возможность предварительного запроса используется метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="07bd8-231">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="07bd8-232">Он включает два специальных заголовков:</span><span class="sxs-lookup"><span data-stu-id="07bd8-232">It includes two special headers:</span></span>

* <span data-ttu-id="07bd8-233">Access-Control-Request-Method: Метод HTTP, будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="07bd8-233">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>

* <span data-ttu-id="07bd8-234">Access-Control-Request-Headers: Список заголовков запросов, устанавливающие приложение на самого запроса.</span><span class="sxs-lookup"><span data-stu-id="07bd8-234">Access-Control-Request-Headers: A list of request headers that the application set on the actual request.</span></span> <span data-ttu-id="07bd8-235">(Опять же, это не включает заголовки, которые задает браузера.)</span><span class="sxs-lookup"><span data-stu-id="07bd8-235">(Again, this doesn't include headers that the browser sets.)</span></span>

<span data-ttu-id="07bd8-236">Ниже приведен пример ответа, при условии, что сервер разрешает запрос.</span><span class="sxs-lookup"><span data-stu-id="07bd8-236">Here is an example response, assuming that the server allows the request:</span></span>

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

<span data-ttu-id="07bd8-237">Ответ содержит заголовок Access-Control-Allow-Methods, в которой перечислены разрешенные методы и при необходимости заголовок Access-Control-разрешить-Headers, в которой перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="07bd8-237">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="07bd8-238">Если Предварительный запрос завершается успешно, браузер отправляет фактический запрос, как описано выше.</span><span class="sxs-lookup"><span data-stu-id="07bd8-238">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>
