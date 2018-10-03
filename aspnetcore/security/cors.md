---
title: Включение запросов о происхождении (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как CORS в качестве стандарта для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2018
uid: security/cors
ms.openlocfilehash: cfbf24edb1dae76f676d51738b0d57266688d53e
ms.sourcegitcommit: 317f9be24db600499e79d25872d743af74bd86c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045592"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="aef5f-103">Включение запросов о происхождении (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aef5f-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="aef5f-104">По [Майк Уоссон](https://github.com/mikewasson), [Шейн Бойер](https://twitter.com/spboyer), и [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="aef5f-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="aef5f-105">Безопасность обозревателя предотвращает веб-странице запросов в другой домен, отличного от того, который обслуживал веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="aef5f-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="aef5f-106">Это ограничение называется *политика одного источника*.</span><span class="sxs-lookup"><span data-stu-id="aef5f-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="aef5f-107">Политика одного источника предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="aef5f-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="aef5f-108">В некоторых случаях может потребоваться разрешить другие сайты выполнять запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="aef5f-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="aef5f-109">[Кросс-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) — это стандарт консорциума W3C, позволяющий серверу смягчить ограничения политики одного источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="aef5f-110">С помощью CORS сервер может явным образом разрешить некоторые запросы независимо от источника а другие — отклонять.</span><span class="sxs-lookup"><span data-stu-id="aef5f-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="aef5f-111">CORS — более безопасное и более гибким, чем предыдущие технологии, такие как [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="aef5f-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="aef5f-112">В этом разделе показано, как включить поддержку CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aef5f-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="aef5f-113">Того же происхождения</span><span class="sxs-lookup"><span data-stu-id="aef5f-113">Same origin</span></span>

<span data-ttu-id="aef5f-114">Два URL-адреса иметь того же происхождения, если они имеют одинаковые схемы, узлов и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="aef5f-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="aef5f-115">Эти два URL-адреса у того же происхождения:</span><span class="sxs-lookup"><span data-stu-id="aef5f-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="aef5f-116">Эти URL-адреса имеют различное происхождение, чем предыдущие два URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="aef5f-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="aef5f-117">`https://example.net` &ndash; Другой домен</span><span class="sxs-lookup"><span data-stu-id="aef5f-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="aef5f-118">`https://www.example.com/foo.html` &ndash; Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="aef5f-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="aef5f-119">`http://example.com/foo.html` &ndash; Другой схемы</span><span class="sxs-lookup"><span data-stu-id="aef5f-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="aef5f-120">`https://example.com:9000/foo.html` &ndash; Другой порт</span><span class="sxs-lookup"><span data-stu-id="aef5f-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="aef5f-121">Internet Explorer не считает порт, при сравнении источников.</span><span class="sxs-lookup"><span data-stu-id="aef5f-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="aef5f-122">Регистрация службы CORS</span><span class="sxs-lookup"><span data-stu-id="aef5f-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aef5f-123">Справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.</span><span class="sxs-lookup"><span data-stu-id="aef5f-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aef5f-124">Справочник по [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) или добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.</span><span class="sxs-lookup"><span data-stu-id="aef5f-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aef5f-125">Добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.</span><span class="sxs-lookup"><span data-stu-id="aef5f-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="aef5f-126">Вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> в `Startup.ConfigureServices` добавлять службы CORS в контейнере служб приложения:</span><span class="sxs-lookup"><span data-stu-id="aef5f-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="aef5f-127">Включение CORS</span><span class="sxs-lookup"><span data-stu-id="aef5f-127">Enable CORS</span></span>

<span data-ttu-id="aef5f-128">После регистрации службы CORS, используйте один из следующих подходов описывается включение CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aef5f-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="aef5f-129">[По промежуточного слоя CORS](#enable-cors-with-cors-middleware) &ndash; политики CORS применяются глобально для приложения с помощью по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="aef5f-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="aef5f-130">[CORS в MVC](#enable-cors-in-mvc) &ndash; CORS для применения политик на действие или на контроллере.</span><span class="sxs-lookup"><span data-stu-id="aef5f-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="aef5f-131">По промежуточного слоя CORS не используется.</span><span class="sxs-lookup"><span data-stu-id="aef5f-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="aef5f-132">Включение CORS с помощью по промежуточного слоя CORS</span><span class="sxs-lookup"><span data-stu-id="aef5f-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="aef5f-133">По промежуточного слоя CORS обрабатывает запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="aef5f-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="aef5f-134">Чтобы включить по промежуточного слоя CORS в конвейер обработки запросов, вызовите <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> метод расширения в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="aef5f-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="aef5f-135">По промежуточного слоя CORS должен предшествовать любой определены конечные точки в приложении место для поддержки запросов о происхождении (например, перед вызовом `UseMvc` для по промежуточного слоя MVC и Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="aef5f-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="aef5f-136">Объект *политики независимо от источника* могут быть указаны при добавлении по промежуточного слоя CORS с помощью <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> класса.</span><span class="sxs-lookup"><span data-stu-id="aef5f-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="aef5f-137">Существует два подхода для определения политики CORS:</span><span class="sxs-lookup"><span data-stu-id="aef5f-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="aef5f-138">Вызовите `UseCors` с лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="aef5f-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="aef5f-139">Лямбда-выражение принимает <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> объекта.</span><span class="sxs-lookup"><span data-stu-id="aef5f-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="aef5f-140">[Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="aef5f-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="aef5f-141">В приведенном выше примере политика позволяет запросов о происхождении из `https://example.com` и другие источники.</span><span class="sxs-lookup"><span data-stu-id="aef5f-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="aef5f-142">URL-адрес должен быть указан без косой чертой (`/`).</span><span class="sxs-lookup"><span data-stu-id="aef5f-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="aef5f-143">Если URL-адрес заканчивается `/`, сравнение возвращает `false` и возвращается без заголовка.</span><span class="sxs-lookup"><span data-stu-id="aef5f-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="aef5f-144">`CorsPolicyBuilder` имеет текучего API, поэтому можно объединять в цепочку вызовов методов:</span><span class="sxs-lookup"><span data-stu-id="aef5f-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="aef5f-145">Определите один или несколько именованных политик CORS и выберите политику по имени во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="aef5f-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="aef5f-146">В следующем примере добавляется определяемую пользователем политику CORS с именем *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="aef5f-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="aef5f-147">Чтобы выбрать политику, передайте имя в `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="aef5f-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="aef5f-148">Включение CORS в MVC</span><span class="sxs-lookup"><span data-stu-id="aef5f-148">Enable CORS in MVC</span></span>

<span data-ttu-id="aef5f-149">Также можно использовать MVC для применения определенных политик CORS на действие или на контроллере.</span><span class="sxs-lookup"><span data-stu-id="aef5f-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="aef5f-150">При использовании MVC для включения CORS, используются зарегистрированные службы CORS.</span><span class="sxs-lookup"><span data-stu-id="aef5f-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="aef5f-151">По промежуточного слоя CORS не используется.</span><span class="sxs-lookup"><span data-stu-id="aef5f-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="aef5f-152">Каждого действия</span><span class="sxs-lookup"><span data-stu-id="aef5f-152">Per action</span></span>

<span data-ttu-id="aef5f-153">Чтобы указать политику CORS для определенного действия, добавьте [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибут к действию.</span><span class="sxs-lookup"><span data-stu-id="aef5f-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="aef5f-154">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="aef5f-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="aef5f-155">Для контроллера</span><span class="sxs-lookup"><span data-stu-id="aef5f-155">Per controller</span></span>

<span data-ttu-id="aef5f-156">Чтобы указать политику CORS для определенного контроллера, добавьте [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибут в класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="aef5f-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="aef5f-157">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="aef5f-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="aef5f-158">Ниже приведен порядок приоритета.</span><span class="sxs-lookup"><span data-stu-id="aef5f-158">The precedence order is:</span></span>

1. <span data-ttu-id="aef5f-159">action</span><span class="sxs-lookup"><span data-stu-id="aef5f-159">action</span></span>
1. <span data-ttu-id="aef5f-160">контроллер</span><span class="sxs-lookup"><span data-stu-id="aef5f-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="aef5f-161">Отключить CORS</span><span class="sxs-lookup"><span data-stu-id="aef5f-161">Disable CORS</span></span>

<span data-ttu-id="aef5f-162">Чтобы отключить CORS для контроллера или действия, используйте [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) атрибут:</span><span class="sxs-lookup"><span data-stu-id="aef5f-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="aef5f-163">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="aef5f-163">CORS policy options</span></span>

<span data-ttu-id="aef5f-164">В этом разделе описываются различные параметры, которые можно задать в политику CORS.</span><span class="sxs-lookup"><span data-stu-id="aef5f-164">This section describes the various options that you can set in a CORS policy.</span></span>

* [<span data-ttu-id="aef5f-165">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="aef5f-165">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="aef5f-166">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="aef5f-166">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="aef5f-167">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="aef5f-167">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="aef5f-168">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="aef5f-168">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="aef5f-169">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="aef5f-169">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="aef5f-170">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="aef5f-170">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="aef5f-171">Для некоторых параметров, может оказаться удобным для чтения [работает как CORS](#how-cors-works) разделе сначала.</span><span class="sxs-lookup"><span data-stu-id="aef5f-171">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="aef5f-172">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="aef5f-172">Set the allowed origins</span></span>

<span data-ttu-id="aef5f-173">Чтобы разрешить один или несколько определенных источников, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-173">To allow one or more specific origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-24&highlight=4)]

<span data-ttu-id="aef5f-174">Чтобы разрешить любые источники, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-174">To allow all origins, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=28-32&highlight=4)]

<span data-ttu-id="aef5f-175">Тщательно обдумайте прежде чем разрешить запросы из любого источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-175">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="aef5f-176">Разрешение запросов из любого источника означает, что *любой веб-сайт* могут выполнять запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="aef5f-176">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

<span data-ttu-id="aef5f-177">Этот параметр влияет на [перед запуском выявила запросы и заголовка Access-Control-Allow-Origin](#preflight-requests) (описывается далее в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="aef5f-177">This setting affects [preflight requests and the Access-Control-Allow-Origin header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="aef5f-178">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="aef5f-178">Set the allowed HTTP methods</span></span>

<span data-ttu-id="aef5f-179">Чтобы разрешить все методы HTTP, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-179">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=45-50&highlight=5)]

<span data-ttu-id="aef5f-180">Этот параметр влияет на [перед запуском выявила запросы и заголовка Access-Control-Allow-Methods](#preflight-requests) (описывается далее в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="aef5f-180">This setting affects [preflight requests and the Access-Control-Allow-Methods header](#preflight-requests) (described later in this topic).</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="aef5f-181">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="aef5f-181">Set the allowed request headers</span></span>

<span data-ttu-id="aef5f-182">Чтобы разрешить специальные заголовки, которые будут отправляться в запрос CORS с именем *создания заголовков запроса*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="aef5f-182">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="aef5f-183">Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-183">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="aef5f-184">Этот параметр влияет на [перед запуском выявила запросы и заголовка Access-Control-Request-Headers](#preflight-requests) (описывается далее в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="aef5f-184">This setting affects [preflight requests and the Access-Control-Request-Headers header](#preflight-requests) (described later in this topic).</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="aef5f-185">Политики соответствия по промежуточного слоя CORS специальные заголовки, заданные `WithHeaders` , возможна только при отправке заголовков `Access-Control-Request-Headers` точно соответствовать заголовки, перечисленным в `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="aef5f-185">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="aef5f-186">Например рассмотрим приложение, настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="aef5f-186">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="aef5f-187">По промежуточного слоя CORS отклоняет Предварительный запрос следующий заголовок запроса, так как `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) не отображается в `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="aef5f-187">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="aef5f-188">Возвращает приложение *200 ОК* ответа, но не отправляет обратно заголовки CORS.</span><span class="sxs-lookup"><span data-stu-id="aef5f-188">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="aef5f-189">Таким образом браузер не делает запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-189">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="aef5f-190">По промежуточного слоя CORS позволяет всегда четыре заголовки в `Access-Control-Request-Headers` отправляемых независимо от значения, заданные в CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="aef5f-190">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="aef5f-191">Этот список заголовков входят:</span><span class="sxs-lookup"><span data-stu-id="aef5f-191">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="aef5f-192">Например рассмотрим приложение, настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="aef5f-192">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="aef5f-193">По промежуточного слоя CORS успешно отвечает на Предварительный запрос следующий заголовок запроса, так как `Content-Language` всегда имеет список разрешенных:</span><span class="sxs-lookup"><span data-stu-id="aef5f-193">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="aef5f-194">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="aef5f-194">Set the exposed response headers</span></span>

<span data-ttu-id="aef5f-195">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="aef5f-195">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="aef5f-196">Дополнительные сведения см. в разделе [W3C независимо от источника общий доступ к ресурсам (терминология): простой заголовок ответа](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="aef5f-196">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="aef5f-197">Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="aef5f-197">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="aef5f-198">Спецификация CORS вызывает эти заголовки *заголовки ответа на простой*.</span><span class="sxs-lookup"><span data-stu-id="aef5f-198">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="aef5f-199">Чтобы сделать доступным для приложения другие заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-199">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=72-77&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="aef5f-200">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="aef5f-200">Credentials in cross-origin requests</span></span>

<span data-ttu-id="aef5f-201">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="aef5f-201">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="aef5f-202">По умолчанию браузер не отправляет учетные данные с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-202">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="aef5f-203">Учетные данные содержат файлы cookie и схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="aef5f-203">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="aef5f-204">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать `XMLHttpRequest.withCredentials` для `true`.</span><span class="sxs-lookup"><span data-stu-id="aef5f-204">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="aef5f-205">С помощью `XMLHttpRequest` напрямую:</span><span class="sxs-lookup"><span data-stu-id="aef5f-205">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="aef5f-206">В jQuery:</span><span class="sxs-lookup"><span data-stu-id="aef5f-206">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="aef5f-207">Кроме того сервер необходимо разрешить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="aef5f-207">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="aef5f-208">Чтобы разрешить учетные данные от источника, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-208">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=81-86&highlight=5)]

<span data-ttu-id="aef5f-209">Ответ HTTP содержит `Access-Control-Allow-Credentials` заголовок, который указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.</span><span class="sxs-lookup"><span data-stu-id="aef5f-209">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="aef5f-210">Если браузер отправляет учетные данные, но ответ не содержит допустимый `Access-Control-Allow-Credentials` заголовок, браузер не предоставляет приложению ответ, и независимо от источника запрос завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="aef5f-210">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="aef5f-211">Будьте внимательны, если разрешить передачу учетных данных независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-211">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="aef5f-212">Веб-сайт в другом домене можно отправить учетные данные выполнившего вход пользователя в приложение от имени пользователя без уведомления пользователя.</span><span class="sxs-lookup"><span data-stu-id="aef5f-212">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="aef5f-213">Спецификация CORS также указывает, что параметр источники, которые можно `"*"` (все источники) является недопустимым при `Access-Control-Allow-Credentials` заголовок отсутствует.</span><span class="sxs-lookup"><span data-stu-id="aef5f-213">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="aef5f-214">Предварительные запросы</span><span class="sxs-lookup"><span data-stu-id="aef5f-214">Preflight requests</span></span>

<span data-ttu-id="aef5f-215">Для некоторых запросов CORS браузер посылает дополнительный запрос перед внесением самого запроса.</span><span class="sxs-lookup"><span data-stu-id="aef5f-215">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="aef5f-216">Этот запрос называется *Предварительный запрос*.</span><span class="sxs-lookup"><span data-stu-id="aef5f-216">This request is called a *preflight request*.</span></span> <span data-ttu-id="aef5f-217">Браузер можно пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="aef5f-217">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="aef5f-218">Метод запроса — GET, HEAD или POST.</span><span class="sxs-lookup"><span data-stu-id="aef5f-218">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="aef5f-219">Приложение не задает заголовки запроса, отличное от `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, или `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="aef5f-219">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="aef5f-220">`Content-Type` Заголовка, если задано, имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="aef5f-220">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="aef5f-221">Набора правил на заголовки запроса для запроса клиента применяется к заголовки, которые приложение задает путем вызова `setRequestHeader` на `XMLHttpRequest` объекта.</span><span class="sxs-lookup"><span data-stu-id="aef5f-221">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="aef5f-222">Спецификация CORS вызывает эти заголовки *создания заголовков запроса*.</span><span class="sxs-lookup"><span data-stu-id="aef5f-222">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="aef5f-223">Правило не применяется к заголовкам, можно задать браузер, такие как `User-Agent`, `Host`, или `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="aef5f-223">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="aef5f-224">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="aef5f-224">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="aef5f-225">Возможность предварительного запроса используется метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="aef5f-225">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="aef5f-226">Он включает два специальных заголовков:</span><span class="sxs-lookup"><span data-stu-id="aef5f-226">It includes two special headers:</span></span>

* <span data-ttu-id="aef5f-227">`Access-Control-Request-Method`: Метод HTTP, который будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="aef5f-227">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="aef5f-228">`Access-Control-Request-Headers`: Список заголовков запросов, которые приложение задает для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="aef5f-228">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="aef5f-229">Как уже говорилось ранее, не включая заголовки, которые задает браузер, такие как `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="aef5f-229">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="aef5f-230">Предварительный запрос CORS может включать `Access-Control-Request-Headers` заголовок, который указывает серверу, заголовки, которые отправляются в самом запросе.</span><span class="sxs-lookup"><span data-stu-id="aef5f-230">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="aef5f-231">Чтобы разрешить специальные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-231">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=54-59&highlight=5)]

<span data-ttu-id="aef5f-232">Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-232">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=63-68&highlight=5)]

<span data-ttu-id="aef5f-233">Браузеры не полностью согласованные, в том, как они заданы `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="aef5f-233">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="aef5f-234">Если задать заголовки на что-либо отличное от `"*"` (или используйте <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), должен включать по крайней мере `Accept`, `Content-Type`, и `Origin`, а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="aef5f-234">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="aef5f-235">Ниже приведен пример ответа на Предварительный запрос, (при условии, что сервер разрешает запрос).</span><span class="sxs-lookup"><span data-stu-id="aef5f-235">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="aef5f-236">Ответ включает в себя `Access-Control-Allow-Methods` заголовка, в которой перечислены разрешенные методы и при необходимости `Access-Control-Allow-Headers` заголовок, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="aef5f-236">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="aef5f-237">Если Предварительный запрос завершается успешно, браузер отправляет фактический запрос.</span><span class="sxs-lookup"><span data-stu-id="aef5f-237">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="aef5f-238">Если Предварительный запрос отклонен, приложение возвращает *200 ОК* ответа, но не отправляет обратно заголовки CORS.</span><span class="sxs-lookup"><span data-stu-id="aef5f-238">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="aef5f-239">Таким образом браузер не делает запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-239">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="aef5f-240">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="aef5f-240">Set the preflight expiration time</span></span>

<span data-ttu-id="aef5f-241">`Access-Control-Max-Age` Заголовок указывает, как долго можно кэшировать ответ на Предварительный запрос.</span><span class="sxs-lookup"><span data-stu-id="aef5f-241">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="aef5f-242">Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="aef5f-242">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=90-95&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="aef5f-243">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="aef5f-243">How CORS works</span></span>

<span data-ttu-id="aef5f-244">В этом разделе описывается, что происходит в запрос CORS на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="aef5f-244">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="aef5f-245">Важно понять, как работает CORS, чтобы политика CORS можно настроить правильно и отладки при возникновении непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="aef5f-245">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="aef5f-246">Спецификация CORS представляет ряд новых заголовков HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-246">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="aef5f-247">Если браузер поддерживает CORS, он устанавливает эти заголовки для запросов о происхождении автоматически.</span><span class="sxs-lookup"><span data-stu-id="aef5f-247">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="aef5f-248">Пользовательский код JavaScript не обязательно для включения CORS.</span><span class="sxs-lookup"><span data-stu-id="aef5f-248">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="aef5f-249">Ниже приведен пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-249">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="aef5f-250">`Origin` Заголовок предоставляет домена сайта, который выполняет запрос:</span><span class="sxs-lookup"><span data-stu-id="aef5f-250">The `Origin` header provides the domain of the site that's making the request:</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="aef5f-251">Если сервер разрешает запрос, он задает `Access-Control-Allow-Origin` заголовок ответа.</span><span class="sxs-lookup"><span data-stu-id="aef5f-251">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="aef5f-252">Значение этого заголовка либо соответствует `Origin` заголовок из запроса или подстановочное значение `"*"`, это значит, что допускается любого источника:</span><span class="sxs-lookup"><span data-stu-id="aef5f-252">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="aef5f-253">Если ответ не включает `Access-Control-Allow-Origin` заголовок, происходит сбой запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="aef5f-253">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="aef5f-254">В частности браузер запрещает запрос.</span><span class="sxs-lookup"><span data-stu-id="aef5f-254">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="aef5f-255">Даже если сервер возвращает успешный ответ, браузер не предоставления ответа в клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="aef5f-255">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aef5f-256">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="aef5f-256">Additional resources</span></span>

* [<span data-ttu-id="aef5f-257">Кросс-совместного использования ресурсов (CORS)</span><span class="sxs-lookup"><span data-stu-id="aef5f-257">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
