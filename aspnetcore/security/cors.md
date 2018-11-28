---
title: Включение запросов о происхождении (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как CORS в качестве стандарта для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/27/2018
uid: security/cors
ms.openlocfilehash: f0e01cfa618184d8a3b19c06212dc3914183a2e4
ms.sourcegitcommit: e7fafb153b9de7595c2558a0133f8d1c33a3bddb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52458547"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="08ee9-103">Включение запросов о происхождении (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08ee9-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="08ee9-104">По [Майк Уоссон](https://github.com/mikewasson), [Шейн Бойер](https://twitter.com/spboyer), и [том Дайкстра](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="08ee9-104">By [Mike Wasson](https://github.com/mikewasson), [Shayne Boyer](https://twitter.com/spboyer), and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="08ee9-105">Безопасность обозревателя предотвращает веб-странице запросов в другой домен, отличного от того, который обслуживал веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="08ee9-105">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="08ee9-106">Это ограничение называется *политика одного источника*.</span><span class="sxs-lookup"><span data-stu-id="08ee9-106">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="08ee9-107">Политика одного источника предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="08ee9-107">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="08ee9-108">В некоторых случаях может потребоваться разрешить другие сайты выполнять запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="08ee9-108">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span>

<span data-ttu-id="08ee9-109">[Кросс-Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) — это стандарт консорциума W3C, позволяющий серверу смягчить ограничения политики одного источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-109">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="08ee9-110">С помощью CORS сервер может явным образом разрешить некоторые запросы независимо от источника а другие — отклонять.</span><span class="sxs-lookup"><span data-stu-id="08ee9-110">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="08ee9-111">CORS — более безопасное и более гибким, чем предыдущие технологии, такие как [JSONP](https://wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="08ee9-111">CORS is safer and more flexible than earlier techniques, such as [JSONP](https://wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="08ee9-112">В этом разделе показано, как включить поддержку CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08ee9-112">This topic shows how to enable CORS in an ASP.NET Core app.</span></span>

## <a name="same-origin"></a><span data-ttu-id="08ee9-113">Того же происхождения</span><span class="sxs-lookup"><span data-stu-id="08ee9-113">Same origin</span></span>

<span data-ttu-id="08ee9-114">Два URL-адреса иметь того же происхождения, если они имеют одинаковые схемы, узлов и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="08ee9-114">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="08ee9-115">Эти два URL-адреса у того же происхождения:</span><span class="sxs-lookup"><span data-stu-id="08ee9-115">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="08ee9-116">Эти URL-адреса имеют различное происхождение, чем предыдущие два URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="08ee9-116">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="08ee9-117">`https://example.net` &ndash; Другой домен</span><span class="sxs-lookup"><span data-stu-id="08ee9-117">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="08ee9-118">`https://www.example.com/foo.html` &ndash; Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="08ee9-118">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="08ee9-119">`http://example.com/foo.html` &ndash; Другой схемы</span><span class="sxs-lookup"><span data-stu-id="08ee9-119">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="08ee9-120">`https://example.com:9000/foo.html` &ndash; Другой порт</span><span class="sxs-lookup"><span data-stu-id="08ee9-120">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

> [!NOTE]
> <span data-ttu-id="08ee9-121">Internet Explorer не считает порт, при сравнении источников.</span><span class="sxs-lookup"><span data-stu-id="08ee9-121">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="register-cors-services"></a><span data-ttu-id="08ee9-122">Регистрация службы CORS</span><span class="sxs-lookup"><span data-stu-id="08ee9-122">Register CORS services</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="08ee9-123">Справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.</span><span class="sxs-lookup"><span data-stu-id="08ee9-123">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="08ee9-124">Справочник по [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) или добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.</span><span class="sxs-lookup"><span data-stu-id="08ee9-124">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="08ee9-125">Добавьте ссылку на пакет [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) пакета.</span><span class="sxs-lookup"><span data-stu-id="08ee9-125">Add a package reference to the [Microsoft.AspNetCore.Cors](https://www.nuget.org/packages/Microsoft.AspNetCore.Cors/) package.</span></span>

::: moniker-end

<span data-ttu-id="08ee9-126">Вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> в `Startup.ConfigureServices` добавлять службы CORS в контейнере служб приложения:</span><span class="sxs-lookup"><span data-stu-id="08ee9-126">Call <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> in `Startup.ConfigureServices` to add CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/CorsExample1/Startup.cs?name=snippet_addcors&highlight=3)]

## <a name="enable-cors"></a><span data-ttu-id="08ee9-127">Включение CORS</span><span class="sxs-lookup"><span data-stu-id="08ee9-127">Enable CORS</span></span>

<span data-ttu-id="08ee9-128">После регистрации службы CORS, используйте один из следующих подходов описывается включение CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08ee9-128">After registering CORS services, use either of the following approaches to enable CORS in an ASP.NET Core app:</span></span>

* <span data-ttu-id="08ee9-129">[По промежуточного слоя CORS](#enable-cors-with-cors-middleware) &ndash; политики CORS применяются глобально для приложения с помощью по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="08ee9-129">[CORS Middleware](#enable-cors-with-cors-middleware) &ndash; Apply CORS policies globally to the app via middleware.</span></span>
* <span data-ttu-id="08ee9-130">[CORS в MVC](#enable-cors-in-mvc) &ndash; CORS для применения политик на действие или на контроллере.</span><span class="sxs-lookup"><span data-stu-id="08ee9-130">[CORS in MVC](#enable-cors-in-mvc) &ndash; Apply CORS policies per action or per controller.</span></span> <span data-ttu-id="08ee9-131">По промежуточного слоя CORS не используется.</span><span class="sxs-lookup"><span data-stu-id="08ee9-131">CORS Middleware isn't used.</span></span>

### <a name="enable-cors-with-cors-middleware"></a><span data-ttu-id="08ee9-132">Включение CORS с помощью по промежуточного слоя CORS</span><span class="sxs-lookup"><span data-stu-id="08ee9-132">Enable CORS with CORS Middleware</span></span>

<span data-ttu-id="08ee9-133">По промежуточного слоя CORS обрабатывает запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="08ee9-133">CORS Middleware handles cross-origin requests to the app.</span></span> <span data-ttu-id="08ee9-134">Чтобы включить по промежуточного слоя CORS в конвейер обработки запросов, вызовите <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> метод расширения в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-134">To enable CORS Middleware in the request processing pipeline, call the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method in `Startup.Configure`.</span></span>

<span data-ttu-id="08ee9-135">По промежуточного слоя CORS должен предшествовать любой определены конечные точки в приложении место для поддержки запросов о происхождении (например, перед вызовом `UseMvc` для по промежуточного слоя MVC и Razor Pages).</span><span class="sxs-lookup"><span data-stu-id="08ee9-135">CORS Middleware must precede any defined endpoints in your app where you want to support cross-origin requests (for example, before the call to `UseMvc` for MVC/Razor Pages Middleware).</span></span>

<span data-ttu-id="08ee9-136">Объект *политики независимо от источника* могут быть указаны при добавлении по промежуточного слоя CORS с помощью <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> класса.</span><span class="sxs-lookup"><span data-stu-id="08ee9-136">A *cross-origin policy* can be specified when adding the CORS Middleware using the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> class.</span></span> <span data-ttu-id="08ee9-137">Существует два подхода для определения политики CORS:</span><span class="sxs-lookup"><span data-stu-id="08ee9-137">There are two approaches for defining a CORS policy:</span></span>

* <span data-ttu-id="08ee9-138">Вызовите `UseCors` с лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="08ee9-138">Call `UseCors` with a lambda:</span></span>

  [!code-csharp[](cors/sample/CorsExample1/Startup.cs?highlight=11,12&range=22-38)]

  <span data-ttu-id="08ee9-139">Лямбда-выражение принимает <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> объекта.</span><span class="sxs-lookup"><span data-stu-id="08ee9-139">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="08ee9-140">[Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="08ee9-140">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this topic.</span></span> <span data-ttu-id="08ee9-141">В приведенном выше примере политика позволяет запросов о происхождении из `https://example.com` и другие источники.</span><span class="sxs-lookup"><span data-stu-id="08ee9-141">In the preceding example, the policy allows cross-origin requests from `https://example.com` and no other origins.</span></span>

  <span data-ttu-id="08ee9-142">URL-адрес должен быть указан без косой чертой (`/`).</span><span class="sxs-lookup"><span data-stu-id="08ee9-142">The URL must be specified without a trailing slash (`/`).</span></span> <span data-ttu-id="08ee9-143">Если URL-адрес заканчивается `/`, сравнение возвращает `false` и возвращается без заголовка.</span><span class="sxs-lookup"><span data-stu-id="08ee9-143">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

  <span data-ttu-id="08ee9-144">`CorsPolicyBuilder` имеет текучего API, поэтому можно объединять в цепочку вызовов методов:</span><span class="sxs-lookup"><span data-stu-id="08ee9-144">`CorsPolicyBuilder` has a fluent API, so you can chain method calls:</span></span>

  [!code-csharp[](cors/sample/CorsExample3/Startup.cs?highlight=2-3&range=29-32)]

* <span data-ttu-id="08ee9-145">Определите один или несколько именованных политик CORS и выберите политику по имени во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="08ee9-145">Define one or more named CORS policies and select the policy by name at runtime.</span></span> <span data-ttu-id="08ee9-146">В следующем примере добавляется определяемую пользователем политику CORS с именем *AllowSpecificOrigin*.</span><span class="sxs-lookup"><span data-stu-id="08ee9-146">The following example adds a user-defined CORS policy named *AllowSpecificOrigin*.</span></span> <span data-ttu-id="08ee9-147">Чтобы выбрать политику, передайте имя в `UseCors`:</span><span class="sxs-lookup"><span data-stu-id="08ee9-147">To select the policy, pass the name to `UseCors`:</span></span>

  [!code-csharp[](cors/sample/CorsExample2/Startup.cs?name=snippet_begin&highlight=5-6,21)]

### <a name="enable-cors-in-mvc"></a><span data-ttu-id="08ee9-148">Включение CORS в MVC</span><span class="sxs-lookup"><span data-stu-id="08ee9-148">Enable CORS in MVC</span></span>

<span data-ttu-id="08ee9-149">Также можно использовать MVC для применения определенных политик CORS на действие или на контроллере.</span><span class="sxs-lookup"><span data-stu-id="08ee9-149">You can alternatively use MVC to apply specific CORS policies per action or per controller.</span></span> <span data-ttu-id="08ee9-150">При использовании MVC для включения CORS, используются зарегистрированные службы CORS.</span><span class="sxs-lookup"><span data-stu-id="08ee9-150">When using MVC to enable CORS, the registered CORS services are used.</span></span> <span data-ttu-id="08ee9-151">По промежуточного слоя CORS не используется.</span><span class="sxs-lookup"><span data-stu-id="08ee9-151">The CORS Middleware isn't used.</span></span>

### <a name="per-action"></a><span data-ttu-id="08ee9-152">Каждого действия</span><span class="sxs-lookup"><span data-stu-id="08ee9-152">Per action</span></span>

<span data-ttu-id="08ee9-153">Чтобы указать политику CORS для определенного действия, добавьте [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибут к действию.</span><span class="sxs-lookup"><span data-stu-id="08ee9-153">To specify a CORS policy for a specific action, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the action.</span></span> <span data-ttu-id="08ee9-154">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="08ee9-154">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnAction&highlight=2)]

### <a name="per-controller"></a><span data-ttu-id="08ee9-155">Для контроллера</span><span class="sxs-lookup"><span data-stu-id="08ee9-155">Per controller</span></span>

<span data-ttu-id="08ee9-156">Чтобы указать политику CORS для определенного контроллера, добавьте [ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибут в класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="08ee9-156">To specify the CORS policy for a specific controller, add the [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute to the controller class.</span></span> <span data-ttu-id="08ee9-157">Укажите имя политики.</span><span class="sxs-lookup"><span data-stu-id="08ee9-157">Specify the policy name.</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=EnableOnController&highlight=2)]

<span data-ttu-id="08ee9-158">Ниже приведен порядок приоритета.</span><span class="sxs-lookup"><span data-stu-id="08ee9-158">The precedence order is:</span></span>

1. <span data-ttu-id="08ee9-159">action</span><span class="sxs-lookup"><span data-stu-id="08ee9-159">action</span></span>
1. <span data-ttu-id="08ee9-160">контроллер</span><span class="sxs-lookup"><span data-stu-id="08ee9-160">controller</span></span>

### <a name="disable-cors"></a><span data-ttu-id="08ee9-161">Отключить CORS</span><span class="sxs-lookup"><span data-stu-id="08ee9-161">Disable CORS</span></span>

<span data-ttu-id="08ee9-162">Чтобы отключить CORS для контроллера или действия, используйте [ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) атрибут:</span><span class="sxs-lookup"><span data-stu-id="08ee9-162">To disable CORS for a controller or action, use the [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute:</span></span>

[!code-csharp[](cors/sample/CorsMVC/Controllers/ValuesController.cs?name=DisableOnAction&highlight=2)]

## <a name="cors-policy-options"></a><span data-ttu-id="08ee9-163">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="08ee9-163">CORS policy options</span></span>

<span data-ttu-id="08ee9-164">В этом разделе описываются различные параметры, которые можно задать в политику CORS.</span><span class="sxs-lookup"><span data-stu-id="08ee9-164">This section describes the various options that you can set in a CORS policy.</span></span> <span data-ttu-id="08ee9-165"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> Метод вызывается в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-165">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> method is called in `Startup.ConfigureServices`.</span></span>

* [<span data-ttu-id="08ee9-166">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="08ee9-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="08ee9-167">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="08ee9-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="08ee9-168">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="08ee9-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="08ee9-169">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="08ee9-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="08ee9-170">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="08ee9-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="08ee9-171">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="08ee9-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="08ee9-172">Для некоторых параметров, может оказаться удобным для чтения [работает как CORS](#how-cors-works) разделе сначала.</span><span class="sxs-lookup"><span data-stu-id="08ee9-172">For some options, it may be helpful to read the [How CORS works](#how-cors-works) section first.</span></span>

### <a name="set-the-allowed-origins"></a><span data-ttu-id="08ee9-173">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="08ee9-173">Set the allowed origins</span></span>

<span data-ttu-id="08ee9-174">По промежуточного слоя CORS в ASP.NET Core MVC имеет несколько способов, чтобы указать разрешенные источники:</span><span class="sxs-lookup"><span data-stu-id="08ee9-174">The CORS middleware in ASP.NET Core MVC has a few ways to specify allowed origins:</span></span>

* <span data-ttu-id="08ee9-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Позволяет указать один или несколько URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="08ee9-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithOrigins*> &ndash; Allows specifying one or more URLs.</span></span> <span data-ttu-id="08ee9-176">URL-адрес может включать схему, имя узла и порт без включения данных пути.</span><span class="sxs-lookup"><span data-stu-id="08ee9-176">The URL may include the scheme, host name, and port without any path information.</span></span> <span data-ttu-id="08ee9-177">Например, `https://example.com`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-177">For example, `https://example.com`.</span></span> <span data-ttu-id="08ee9-178">URL-адрес должен быть указан без косой чертой (`/`).</span><span class="sxs-lookup"><span data-stu-id="08ee9-178">The URL must be specified without a trailing slash (`/`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=20-25&highlight=4-5)]

* <span data-ttu-id="08ee9-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Разрешает запросы CORS из всех источников с любой схемой (`http` или `https`).</span><span class="sxs-lookup"><span data-stu-id="08ee9-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=29-33&highlight=4)]

  <span data-ttu-id="08ee9-180">Тщательно обдумайте прежде чем разрешить запросы из любого источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-180">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="08ee9-181">Разрешение запросов из любого источника означает, что *любой веб-сайт* могут выполнять запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="08ee9-181">Allowing requests from any origin means that *any website* can make cross-origin requests to your app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="08ee9-182">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="08ee9-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="08ee9-183">CORS, служба возвращает недопустимый ответ CORS приложения настраивается с помощью обоих методов.</span><span class="sxs-lookup"><span data-stu-id="08ee9-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="08ee9-184">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="08ee9-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="08ee9-185">Можно указать точный список источников, если клиент должен авторизоваться сам доступ к ресурсам сервера.</span><span class="sxs-lookup"><span data-stu-id="08ee9-185">Consider specifying an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

  <span data-ttu-id="08ee9-186">Этот параметр влияет на предварительные запросы и `Access-Control-Allow-Origin` заголовка.</span><span class="sxs-lookup"><span data-stu-id="08ee9-186">This setting affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="08ee9-187">Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.</span><span class="sxs-lookup"><span data-stu-id="08ee9-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="08ee9-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Наборы <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> свойство быть функцией, которая позволяет источники, которые можно соответствовать домену настроенных с подстановочными знаками, при оценке, если разрешено источника политики.</span><span class="sxs-lookup"><span data-stu-id="08ee9-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcarded domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="08ee9-189">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="08ee9-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="08ee9-190">Чтобы разрешить все методы HTTP, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="08ee9-190">To allow all HTTP methods, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=46-51&highlight=5)]

<span data-ttu-id="08ee9-191">Этот параметр влияет на предварительные запросы и `Access-Control-Allow-Methods` заголовка.</span><span class="sxs-lookup"><span data-stu-id="08ee9-191">This setting affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="08ee9-192">Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.</span><span class="sxs-lookup"><span data-stu-id="08ee9-192">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="08ee9-193">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="08ee9-193">Set the allowed request headers</span></span>

<span data-ttu-id="08ee9-194">Чтобы разрешить специальные заголовки, которые будут отправляться в запрос CORS с именем *создания заголовков запроса*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="08ee9-194">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="08ee9-195">Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="08ee9-195">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="08ee9-196">Этот параметр влияет на предварительные запросы и `Access-Control-Request-Headers` заголовка.</span><span class="sxs-lookup"><span data-stu-id="08ee9-196">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="08ee9-197">Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.</span><span class="sxs-lookup"><span data-stu-id="08ee9-197">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="08ee9-198">Политики соответствия по промежуточного слоя CORS специальные заголовки, заданные `WithHeaders` , возможна только при отправке заголовков `Access-Control-Request-Headers` точно соответствовать заголовки, перечисленным в `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-198">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="08ee9-199">Например рассмотрим приложение, настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="08ee9-199">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="08ee9-200">По промежуточного слоя CORS отклоняет Предварительный запрос следующий заголовок запроса, так как `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) не отображается в `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="08ee9-200">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="08ee9-201">Возвращает приложение *200 ОК* ответа, но не отправляет обратно заголовки CORS.</span><span class="sxs-lookup"><span data-stu-id="08ee9-201">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="08ee9-202">Таким образом браузер не делает запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-202">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="08ee9-203">По промежуточного слоя CORS позволяет всегда четыре заголовки в `Access-Control-Request-Headers` отправляемых независимо от значения, заданные в CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="08ee9-203">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="08ee9-204">Этот список заголовков входят:</span><span class="sxs-lookup"><span data-stu-id="08ee9-204">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="08ee9-205">Например рассмотрим приложение, настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="08ee9-205">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="08ee9-206">По промежуточного слоя CORS успешно отвечает на Предварительный запрос следующий заголовок запроса, так как `Content-Language` всегда имеет список разрешенных:</span><span class="sxs-lookup"><span data-stu-id="08ee9-206">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="08ee9-207">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="08ee9-207">Set the exposed response headers</span></span>

<span data-ttu-id="08ee9-208">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="08ee9-208">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="08ee9-209">Дополнительные сведения см. в разделе [W3C независимо от источника общий доступ к ресурсам (терминология): простой заголовок ответа](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="08ee9-209">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="08ee9-210">Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="08ee9-210">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="08ee9-211">Спецификация CORS вызывает эти заголовки *заголовки ответа на простой*.</span><span class="sxs-lookup"><span data-stu-id="08ee9-211">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="08ee9-212">Чтобы сделать доступным для приложения другие заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="08ee9-212">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="08ee9-213">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="08ee9-213">Credentials in cross-origin requests</span></span>

<span data-ttu-id="08ee9-214">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="08ee9-214">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="08ee9-215">По умолчанию браузер не отправляет учетные данные с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-215">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="08ee9-216">Учетные данные содержат файлы cookie и схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="08ee9-216">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="08ee9-217">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать `XMLHttpRequest.withCredentials` для `true`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-217">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="08ee9-218">С помощью `XMLHttpRequest` напрямую:</span><span class="sxs-lookup"><span data-stu-id="08ee9-218">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="08ee9-219">В jQuery:</span><span class="sxs-lookup"><span data-stu-id="08ee9-219">In jQuery:</span></span>

```jQuery
$.ajax({
  type: 'get',
  url: 'https://www.example.com/home',
  xhrFields: {
    withCredentials: true
}
```

<span data-ttu-id="08ee9-220">Кроме того сервер необходимо разрешить учетные данные.</span><span class="sxs-lookup"><span data-stu-id="08ee9-220">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="08ee9-221">Чтобы разрешить учетные данные от источника, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="08ee9-221">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="08ee9-222">Ответ HTTP содержит `Access-Control-Allow-Credentials` заголовок, который указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.</span><span class="sxs-lookup"><span data-stu-id="08ee9-222">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="08ee9-223">Если браузер отправляет учетные данные, но ответ не содержит допустимый `Access-Control-Allow-Credentials` заголовок, браузер не предоставляет приложению ответ, и независимо от источника запрос завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="08ee9-223">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="08ee9-224">Будьте внимательны, если разрешить передачу учетных данных независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-224">Be careful when allowing cross-origin credentials.</span></span> <span data-ttu-id="08ee9-225">Веб-сайт в другом домене можно отправить учетные данные выполнившего вход пользователя в приложение от имени пользователя без уведомления пользователя.</span><span class="sxs-lookup"><span data-stu-id="08ee9-225">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span>

<span data-ttu-id="08ee9-226">Спецификация CORS также указывает, что параметр источники, которые можно `"*"` (все источники) является недопустимым при `Access-Control-Allow-Credentials` заголовок отсутствует.</span><span class="sxs-lookup"><span data-stu-id="08ee9-226">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="08ee9-227">Предварительные запросы</span><span class="sxs-lookup"><span data-stu-id="08ee9-227">Preflight requests</span></span>

<span data-ttu-id="08ee9-228">Для некоторых запросов CORS браузер посылает дополнительный запрос перед внесением самого запроса.</span><span class="sxs-lookup"><span data-stu-id="08ee9-228">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="08ee9-229">Этот запрос называется *Предварительный запрос*.</span><span class="sxs-lookup"><span data-stu-id="08ee9-229">This request is called a *preflight request*.</span></span> <span data-ttu-id="08ee9-230">Браузер можно пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="08ee9-230">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="08ee9-231">Метод запроса — GET, HEAD или POST.</span><span class="sxs-lookup"><span data-stu-id="08ee9-231">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="08ee9-232">Приложение не задает заголовки запроса, отличное от `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, или `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-232">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="08ee9-233">`Content-Type` Заголовка, если задано, имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="08ee9-233">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="08ee9-234">Набора правил на заголовки запроса для запроса клиента применяется к заголовки, которые приложение задает путем вызова `setRequestHeader` на `XMLHttpRequest` объекта.</span><span class="sxs-lookup"><span data-stu-id="08ee9-234">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="08ee9-235">Спецификация CORS вызывает эти заголовки *создания заголовков запроса*.</span><span class="sxs-lookup"><span data-stu-id="08ee9-235">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="08ee9-236">Правило не применяется к заголовкам, можно задать браузер, такие как `User-Agent`, `Host`, или `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-236">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="08ee9-237">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="08ee9-237">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="08ee9-238">Возможность предварительного запроса используется метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="08ee9-238">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="08ee9-239">Он включает два специальных заголовков:</span><span class="sxs-lookup"><span data-stu-id="08ee9-239">It includes two special headers:</span></span>

* <span data-ttu-id="08ee9-240">`Access-Control-Request-Method`: Метод HTTP, который будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="08ee9-240">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="08ee9-241">`Access-Control-Request-Headers`: Список заголовков запросов, которые приложение задает для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="08ee9-241">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="08ee9-242">Как уже говорилось ранее, не включая заголовки, которые задает браузер, такие как `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-242">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="08ee9-243">Предварительный запрос CORS может включать `Access-Control-Request-Headers` заголовок, который указывает серверу, заголовки, которые отправляются в самом запросе.</span><span class="sxs-lookup"><span data-stu-id="08ee9-243">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="08ee9-244">Чтобы разрешить специальные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="08ee9-244">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="08ee9-245">Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="08ee9-245">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="08ee9-246">Браузеры не полностью согласованные, в том, как они заданы `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="08ee9-246">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="08ee9-247">Если задать заголовки на что-либо отличное от `"*"` (или используйте <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), должен включать по крайней мере `Accept`, `Content-Type`, и `Origin`, а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="08ee9-247">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="08ee9-248">Ниже приведен пример ответа на Предварительный запрос, (при условии, что сервер разрешает запрос).</span><span class="sxs-lookup"><span data-stu-id="08ee9-248">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="08ee9-249">Ответ включает в себя `Access-Control-Allow-Methods` заголовка, в которой перечислены разрешенные методы и при необходимости `Access-Control-Allow-Headers` заголовок, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="08ee9-249">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="08ee9-250">Если Предварительный запрос завершается успешно, браузер отправляет фактический запрос.</span><span class="sxs-lookup"><span data-stu-id="08ee9-250">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="08ee9-251">Если Предварительный запрос отклонен, приложение возвращает *200 ОК* ответа, но не отправляет обратно заголовки CORS.</span><span class="sxs-lookup"><span data-stu-id="08ee9-251">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="08ee9-252">Таким образом браузер не делает запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-252">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="08ee9-253">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="08ee9-253">Set the preflight expiration time</span></span>

<span data-ttu-id="08ee9-254">`Access-Control-Max-Age` Заголовок указывает, как долго можно кэшировать ответ на Предварительный запрос.</span><span class="sxs-lookup"><span data-stu-id="08ee9-254">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="08ee9-255">Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="08ee9-255">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

## <a name="how-cors-works"></a><span data-ttu-id="08ee9-256">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="08ee9-256">How CORS works</span></span>

<span data-ttu-id="08ee9-257">В этом разделе описывается, что происходит в запрос CORS на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="08ee9-257">This section describes what happens in a CORS request at the level of the HTTP messages.</span></span> <span data-ttu-id="08ee9-258">Важно понять, как работает CORS, чтобы политика CORS можно настроить правильно и отладки при возникновении непредвиденному поведению.</span><span class="sxs-lookup"><span data-stu-id="08ee9-258">It's important to understand how CORS works so that the CORS policy can be configured correctly and debugged when unexpected behaviors occur.</span></span>

<span data-ttu-id="08ee9-259">Спецификация CORS представляет ряд новых заголовков HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-259">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="08ee9-260">Если браузер поддерживает CORS, он устанавливает эти заголовки для запросов о происхождении автоматически.</span><span class="sxs-lookup"><span data-stu-id="08ee9-260">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="08ee9-261">Пользовательский код JavaScript не обязательно для включения CORS.</span><span class="sxs-lookup"><span data-stu-id="08ee9-261">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="08ee9-262">Ниже приведен пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-262">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="08ee9-263">`Origin` Заголовок предоставляет домена сайта, который выполняет запрос:</span><span class="sxs-lookup"><span data-stu-id="08ee9-263">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="08ee9-264">Если сервер разрешает запрос, он задает `Access-Control-Allow-Origin` заголовок ответа.</span><span class="sxs-lookup"><span data-stu-id="08ee9-264">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="08ee9-265">Значение этого заголовка либо соответствует `Origin` заголовок из запроса или подстановочное значение `"*"`, это значит, что допускается любого источника:</span><span class="sxs-lookup"><span data-stu-id="08ee9-265">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="08ee9-266">Если ответ не включает `Access-Control-Allow-Origin` заголовок, происходит сбой запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="08ee9-266">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="08ee9-267">В частности браузер запрещает запрос.</span><span class="sxs-lookup"><span data-stu-id="08ee9-267">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="08ee9-268">Даже если сервер возвращает успешный ответ, браузер не предоставления ответа в клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="08ee9-268">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08ee9-269">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="08ee9-269">Additional resources</span></span>

* [<span data-ttu-id="08ee9-270">Кросс-совместного использования ресурсов (CORS)</span><span class="sxs-lookup"><span data-stu-id="08ee9-270">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
