---
title: Включение запросов о происхождении (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как CORS в качестве стандарта для предоставления или отклонения запросов независимо от источника в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: security/cors
ms.openlocfilehash: 6be8b4da1642a9eff021371c229a17071d6e9bfb
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978475"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="d4f33-103">Включение запросов о происхождении (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4f33-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="d4f33-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d4f33-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d4f33-105">В этой статье показано, как включить поддержку CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4f33-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="d4f33-106">Безопасность обозревателя предотвращает веб-странице запросов в другой домен, отличного от того, который обслуживал веб-страницы.</span><span class="sxs-lookup"><span data-stu-id="d4f33-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="d4f33-107">Это ограничение называется *политика одного источника*.</span><span class="sxs-lookup"><span data-stu-id="d4f33-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="d4f33-108">Политика одного источника предотвращает чтение конфиденциальных данных с другого сайта вредоносный сайт.</span><span class="sxs-lookup"><span data-stu-id="d4f33-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="d4f33-109">В некоторых случаях может потребоваться разрешить другие сайты выполнять запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="d4f33-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="d4f33-110">Дополнительные сведения см. в разделе [Mozilla CORS статье](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="d4f33-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="d4f33-111">[CROSS Origin общий доступ к ресурсам](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="d4f33-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="d4f33-112">Такое W3C, позволяющий серверу смягчить ограничения политики одного источника "стандартный".</span><span class="sxs-lookup"><span data-stu-id="d4f33-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="d4f33-113">— **Не** средство безопасности, а CORS снижает безопасность.</span><span class="sxs-lookup"><span data-stu-id="d4f33-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="d4f33-114">API не является более безопасным, позволяя CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="d4f33-115">Дополнительные сведения см. в разделе [работает как CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="d4f33-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="d4f33-116">Позволяет серверу явным образом разрешить некоторые запросы независимо от источника, а другие — отклонять.</span><span class="sxs-lookup"><span data-stu-id="d4f33-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="d4f33-117">Является более безопасным и более гибким, чем предыдущие технологии, такие как [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="d4f33-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="d4f33-118">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d4f33-118">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/2.2-stage-samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="d4f33-119">Того же происхождения</span><span class="sxs-lookup"><span data-stu-id="d4f33-119">Same origin</span></span>

<span data-ttu-id="d4f33-120">Два URL-адреса иметь того же происхождения, если они имеют одинаковые схемы, узлов и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="d4f33-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="d4f33-121">Эти два URL-адреса у того же происхождения:</span><span class="sxs-lookup"><span data-stu-id="d4f33-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="d4f33-122">Эти URL-адреса имеют различное происхождение, чем предыдущие два URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="d4f33-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="d4f33-123">`https://example.net` &ndash; Другой домен</span><span class="sxs-lookup"><span data-stu-id="d4f33-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="d4f33-124">`https://www.example.com/foo.html` &ndash; Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="d4f33-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="d4f33-125">`http://example.com/foo.html` &ndash; Другой схемы</span><span class="sxs-lookup"><span data-stu-id="d4f33-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="d4f33-126">`https://example.com:9000/foo.html` &ndash; Другой порт</span><span class="sxs-lookup"><span data-stu-id="d4f33-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="d4f33-127">Internet Explorer не считает порт, при сравнении источников.</span><span class="sxs-lookup"><span data-stu-id="d4f33-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="d4f33-128">С помощью именованной политики и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="d4f33-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="d4f33-129">По промежуточного слоя CORS обрабатывает запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="d4f33-130">Следующий код включает доступ CORS для всего приложения с помощью указанного источника:</span><span class="sxs-lookup"><span data-stu-id="d4f33-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="d4f33-131">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="d4f33-131">The preceding code:</span></span>

* <span data-ttu-id="d4f33-132">Задает имя политики "\_myAllowSpecificOrigins».</span><span class="sxs-lookup"><span data-stu-id="d4f33-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="d4f33-133">Имя политики является произвольным.</span><span class="sxs-lookup"><span data-stu-id="d4f33-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="d4f33-134">Вызовы <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> метод расширения, который позволяет ядер.</span><span class="sxs-lookup"><span data-stu-id="d4f33-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables cores.</span></span>
* <span data-ttu-id="d4f33-135">Вызовы <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> с [лямбда-выражение](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="d4f33-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="d4f33-136">Лямбда-выражение принимает <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> объекта.</span><span class="sxs-lookup"><span data-stu-id="d4f33-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="d4f33-137">[Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="d4f33-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="d4f33-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> Вызов метода добавляет CORS службы в контейнер службы приложения:</span><span class="sxs-lookup"><span data-stu-id="d4f33-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="d4f33-139">Дополнительные сведения см. в разделе [параметры политики CORS](#cpo) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="d4f33-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="d4f33-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> Метода можно объединять в цепочку методы, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="d4f33-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="d4f33-141">Следующий выделенный код применяет политики CORS ко всем конечным точкам приложений с помощью по промежуточного слоя CORS:</span><span class="sxs-lookup"><span data-stu-id="d4f33-141">The following highlighted code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors(); 

    app.UseHttpsRedirection();
    app.UseMvc();
}
```

<span data-ttu-id="d4f33-142">См. в разделе [включить CORS в Razor Pages, контроллеры и методы действий](#ecors) для применения политики CORS на уровне страницы или контроллеру или действию.</span><span class="sxs-lookup"><span data-stu-id="d4f33-142">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="d4f33-143">Примечание.</span><span class="sxs-lookup"><span data-stu-id="d4f33-143">Note:</span></span>

* <span data-ttu-id="d4f33-144">`UseCors` должен вызываться перед `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-144">`UseCors` must be called before `UseMvc`.</span></span>
* <span data-ttu-id="d4f33-145">URL-адрес должен **не** содержать косую черту (`/`).</span><span class="sxs-lookup"><span data-stu-id="d4f33-145">The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="d4f33-146">Если URL-адрес заканчивается `/`, сравнение возвращает `false` и возвращается без заголовка.</span><span class="sxs-lookup"><span data-stu-id="d4f33-146">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="d4f33-147">См. в разделе [CORS теста](#test) инструкции по тестированию приведенный выше код.</span><span class="sxs-lookup"><span data-stu-id="d4f33-147">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="d4f33-148">Включение CORS с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="d4f33-148">Enable CORS with attributes</span></span>

<span data-ttu-id="d4f33-149">[ &lbrack;EnableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) атрибута представляет собой альтернативу глобально применение CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-149">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="d4f33-150">`[EnableCors]` Атрибута включает доступ CORS для выбранных конечных точек, а не всех конечных точек.</span><span class="sxs-lookup"><span data-stu-id="d4f33-150">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="d4f33-151">Используйте `[EnableCors]` для задания политики по умолчанию и `[EnableCors("{Policy String}")]` задание политики.</span><span class="sxs-lookup"><span data-stu-id="d4f33-151">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="d4f33-152">`[EnableCors]` Атрибут может применяться для:</span><span class="sxs-lookup"><span data-stu-id="d4f33-152">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="d4f33-153">Страница Razor `PageModel`</span><span class="sxs-lookup"><span data-stu-id="d4f33-153">Razor Page `PageModel`</span></span>
* <span data-ttu-id="d4f33-154">Контроллер</span><span class="sxs-lookup"><span data-stu-id="d4f33-154">Controller</span></span>
* <span data-ttu-id="d4f33-155">Метод действия контроллера</span><span class="sxs-lookup"><span data-stu-id="d4f33-155">Controller action method</span></span>

<span data-ttu-id="d4f33-156">Разные политики можно применить к контроллеру или странице модели/действию с `[EnableCors]` атрибута.</span><span class="sxs-lookup"><span data-stu-id="d4f33-156">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="d4f33-157">Когда `[EnableCors]` атрибут применяется к методу контроллеров/страницы модель и действие и включен CORS в по промежуточного слоя, обе политики применяются.</span><span class="sxs-lookup"><span data-stu-id="d4f33-157">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="d4f33-158">Мы не рекомендуем объединение политик.</span><span class="sxs-lookup"><span data-stu-id="d4f33-158">We recommend against combining policies.</span></span> <span data-ttu-id="d4f33-159">Используйте `[EnableCors]` атрибута или по промежуточного слоя, не одновременно в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="d4f33-159">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="d4f33-160">В следующем коде применяется другую политику к каждому методу:</span><span class="sxs-lookup"><span data-stu-id="d4f33-160">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="d4f33-161">Следующий код создает политику CORS по умолчанию и политику с именем `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="d4f33-161">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="d4f33-162">Отключить CORS</span><span class="sxs-lookup"><span data-stu-id="d4f33-162">Disable CORS</span></span>

<span data-ttu-id="d4f33-163">[ &lbrack;DisableCors&rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) атрибут отключает CORS для контроллера/страницы модели/действие.</span><span class="sxs-lookup"><span data-stu-id="d4f33-163">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="d4f33-164">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="d4f33-164">CORS policy options</span></span>

<span data-ttu-id="d4f33-165">В этом разделе описываются различные параметры, которые могут устанавливаться в политике CORS:</span><span class="sxs-lookup"><span data-stu-id="d4f33-165">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="d4f33-166">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="d4f33-166">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="d4f33-167">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="d4f33-167">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="d4f33-168">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="d4f33-168">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="d4f33-169">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="d4f33-169">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="d4f33-170">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="d4f33-170">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="d4f33-171">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="d4f33-171">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

 <span data-ttu-id="d4f33-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> вызывается в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-172"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d4f33-173">Для некоторых параметров, может оказаться удобным для чтения [работает как CORS](#how-cors) разделе сначала.</span><span class="sxs-lookup"><span data-stu-id="d4f33-173">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="d4f33-174">Задайте разрешенные источники</span><span class="sxs-lookup"><span data-stu-id="d4f33-174">Set the allowed origins</span></span>

<span data-ttu-id="d4f33-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Разрешает запросы CORS из всех источников с любой схемой (`http` или `https`).</span><span class="sxs-lookup"><span data-stu-id="d4f33-175"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="d4f33-176">`AllowAnyOrigin` не защищен, поскольку *любой веб-сайт* могут выполнять запросы независимо от источника к приложению.</span><span class="sxs-lookup"><span data-stu-id="d4f33-176">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

  ::: moniker range=">= aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="d4f33-177">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="d4f33-177">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="d4f33-178">CORS, служба возвращает недопустимый ответ CORS приложения настраивается с помощью обоих методов.</span><span class="sxs-lookup"><span data-stu-id="d4f33-178">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

  ::: moniker-end

  ::: moniker range="< aspnetcore-2.2"

  > [!NOTE]
  > <span data-ttu-id="d4f33-179">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделки межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="d4f33-179">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="d4f33-180">Для безопасного приложения укажите точный список источников, если клиент должен авторизоваться сам доступ к ресурсам сервера.</span><span class="sxs-lookup"><span data-stu-id="d4f33-180">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

  ::: moniker-end

<!-- REVIEW required
I changed from
Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration. **This** setting affects preflight requests and the ...
to
**`AllowAnyOrigin`** affects preflight requests and the

to remove the ambiguous **This**. 
-->

  <span data-ttu-id="d4f33-181">`AllowAnyOrigin` влияет на запросы перед запуском выявила и `Access-Control-Allow-Origin` заголовка.</span><span class="sxs-lookup"><span data-stu-id="d4f33-181">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="d4f33-182">Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.</span><span class="sxs-lookup"><span data-stu-id="d4f33-182">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d4f33-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Наборы <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> свойство быть функцией, которая позволяет источников для сопоставления домена с подстановочным знаком настроенных при оценке, если разрешено источника политики.</span><span class="sxs-lookup"><span data-stu-id="d4f33-183"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

  [!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-104&highlight=4)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="d4f33-184">Задайте разрешенные методы HTTP</span><span class="sxs-lookup"><span data-stu-id="d4f33-184">Set the allowed HTTP methods</span></span>

<span data-ttu-id="d4f33-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="d4f33-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="d4f33-186">Разрешает любой метод HTTP:</span><span class="sxs-lookup"><span data-stu-id="d4f33-186">Allows any HTTP method:</span></span>
* <span data-ttu-id="d4f33-187">Влияет на запросы перед запуском выявила и `Access-Control-Allow-Methods` заголовка.</span><span class="sxs-lookup"><span data-stu-id="d4f33-187">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="d4f33-188">Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.</span><span class="sxs-lookup"><span data-stu-id="d4f33-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="d4f33-189">Задать заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="d4f33-189">Set the allowed request headers</span></span>

<span data-ttu-id="d4f33-190">Чтобы разрешить специальные заголовки, которые будут отправляться в запрос CORS с именем *создания заголовков запроса*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="d4f33-190">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="d4f33-191">Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="d4f33-191">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="d4f33-192">Этот параметр влияет на предварительные запросы и `Access-Control-Request-Headers` заголовка.</span><span class="sxs-lookup"><span data-stu-id="d4f33-192">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="d4f33-193">Дополнительные сведения см. в разделе [перед запуском выявила запросы](#preflight-requests) раздел.</span><span class="sxs-lookup"><span data-stu-id="d4f33-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d4f33-194">Политики соответствия по промежуточного слоя CORS специальные заголовки, заданные `WithHeaders` , возможна только при отправке заголовков `Access-Control-Request-Headers` точно соответствовать заголовки, перечисленным в `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-194">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="d4f33-195">Например рассмотрим приложение, настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d4f33-195">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="d4f33-196">По промежуточного слоя CORS отклоняет Предварительный запрос следующий заголовок запроса, так как `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) не отображается в `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="d4f33-196">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="d4f33-197">Возвращает приложение *200 ОК* ответа, но не отправляет обратно заголовки CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-197">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="d4f33-198">Таким образом браузер не делает запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-198">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d4f33-199">По промежуточного слоя CORS позволяет всегда четыре заголовки в `Access-Control-Request-Headers` отправляемых независимо от значения, заданные в CorsPolicy.Headers.</span><span class="sxs-lookup"><span data-stu-id="d4f33-199">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="d4f33-200">Этот список заголовков входят:</span><span class="sxs-lookup"><span data-stu-id="d4f33-200">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="d4f33-201">Например рассмотрим приложение, настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d4f33-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="d4f33-202">По промежуточного слоя CORS успешно отвечает на Предварительный запрос следующий заголовок запроса, так как `Content-Language` всегда имеет список разрешенных:</span><span class="sxs-lookup"><span data-stu-id="d4f33-202">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="d4f33-203">Задайте заголовки ответа, предоставляемого</span><span class="sxs-lookup"><span data-stu-id="d4f33-203">Set the exposed response headers</span></span>

<span data-ttu-id="d4f33-204">По умолчанию браузер не предоставляет все заголовки ответа для приложения.</span><span class="sxs-lookup"><span data-stu-id="d4f33-204">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="d4f33-205">Дополнительные сведения см. в разделе [W3C независимо от источника общий доступ к ресурсам (терминология): Заголовок ответа на простой](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="d4f33-205">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="d4f33-206">Заголовки ответа, которые доступны по умолчанию являются:</span><span class="sxs-lookup"><span data-stu-id="d4f33-206">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="d4f33-207">Спецификация CORS вызывает эти заголовки *заголовки ответа на простой*.</span><span class="sxs-lookup"><span data-stu-id="d4f33-207">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="d4f33-208">Чтобы сделать доступным для приложения другие заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="d4f33-208">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="d4f33-209">Учетные данные в запросов о происхождении</span><span class="sxs-lookup"><span data-stu-id="d4f33-209">Credentials in cross-origin requests</span></span>

<span data-ttu-id="d4f33-210">Учетные данные, требующие особых действий в запрос CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-210">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="d4f33-211">По умолчанию браузер не отправляет учетные данные с помощью запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-211">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="d4f33-212">Учетные данные содержат файлы cookie и схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4f33-212">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="d4f33-213">Для отправки учетных данных с помощью запроса независимо от источника, клиент должен указать `XMLHttpRequest.withCredentials` для `true`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-213">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="d4f33-214">С помощью `XMLHttpRequest` напрямую:</span><span class="sxs-lookup"><span data-stu-id="d4f33-214">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="d4f33-215">С помощью jQuery:</span><span class="sxs-lookup"><span data-stu-id="d4f33-215">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="d4f33-216">С помощью [выборки API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="d4f33-216">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="d4f33-217">Server должен разрешать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="d4f33-217">The server must allow the credentials.</span></span> <span data-ttu-id="d4f33-218">Чтобы разрешить учетные данные от источника, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="d4f33-218">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="d4f33-219">Ответ HTTP содержит `Access-Control-Allow-Credentials` заголовок, который указывает обозревателю, учетные данные для запроса независимо от источника, поддерживает ли сервер.</span><span class="sxs-lookup"><span data-stu-id="d4f33-219">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="d4f33-220">Если браузер отправляет учетные данные, но ответ не содержит допустимый `Access-Control-Allow-Credentials` заголовок, браузер не предоставляет приложению ответ, и независимо от источника запрос завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="d4f33-220">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="d4f33-221">Учетные данные от источника является угрозу безопасности.</span><span class="sxs-lookup"><span data-stu-id="d4f33-221">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="d4f33-222">Веб-сайт в другом домене можно отправить учетные данные выполнившего вход пользователя в приложение от имени пользователя без уведомления пользователя.</span><span class="sxs-lookup"><span data-stu-id="d4f33-222">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="d4f33-223">Спецификация CORS также указывает, что параметр источники, которые можно `"*"` (все источники) является недопустимым при `Access-Control-Allow-Credentials` заголовок отсутствует.</span><span class="sxs-lookup"><span data-stu-id="d4f33-223">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="d4f33-224">Предварительные запросы</span><span class="sxs-lookup"><span data-stu-id="d4f33-224">Preflight requests</span></span>

<span data-ttu-id="d4f33-225">Для некоторых запросов CORS браузер посылает дополнительный запрос перед внесением самого запроса.</span><span class="sxs-lookup"><span data-stu-id="d4f33-225">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="d4f33-226">Этот запрос называется *Предварительный запрос*.</span><span class="sxs-lookup"><span data-stu-id="d4f33-226">This request is called a *preflight request*.</span></span> <span data-ttu-id="d4f33-227">Браузер можно пропустить Предварительный запрос, если выполняются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="d4f33-227">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="d4f33-228">Метод запроса — GET, HEAD или POST.</span><span class="sxs-lookup"><span data-stu-id="d4f33-228">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="d4f33-229">Приложение не задает заголовки запроса, отличное от `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, или `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-229">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="d4f33-230">`Content-Type` Заголовка, если задано, имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="d4f33-230">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="d4f33-231">Набора правил на заголовки запроса для запроса клиента применяется к заголовки, которые приложение задает путем вызова `setRequestHeader` на `XMLHttpRequest` объекта.</span><span class="sxs-lookup"><span data-stu-id="d4f33-231">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="d4f33-232">Спецификация CORS вызывает эти заголовки *создания заголовков запроса*.</span><span class="sxs-lookup"><span data-stu-id="d4f33-232">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="d4f33-233">Правило не применяется к заголовкам, можно задать браузер, такие как `User-Agent`, `Host`, или `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-233">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="d4f33-234">Ниже приведен пример Предварительный запрос:</span><span class="sxs-lookup"><span data-stu-id="d4f33-234">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="d4f33-235">Возможность предварительного запроса используется метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-235">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="d4f33-236">Он включает два специальных заголовков:</span><span class="sxs-lookup"><span data-stu-id="d4f33-236">It includes two special headers:</span></span>

* <span data-ttu-id="d4f33-237">`Access-Control-Request-Method`: Метод HTTP, который будет использоваться для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="d4f33-237">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="d4f33-238">`Access-Control-Request-Headers`: Список заголовков запросов, которые приложение задает для самого запроса.</span><span class="sxs-lookup"><span data-stu-id="d4f33-238">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="d4f33-239">Как уже говорилось ранее, не включая заголовки, которые задает браузер, такие как `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-239">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="d4f33-240">Предварительный запрос CORS может включать `Access-Control-Request-Headers` заголовок, который указывает серверу, заголовки, которые отправляются в самом запросе.</span><span class="sxs-lookup"><span data-stu-id="d4f33-240">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="d4f33-241">Чтобы разрешить специальные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="d4f33-241">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="d4f33-242">Чтобы разрешить все создавать заголовки запроса, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="d4f33-242">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="d4f33-243">Браузеры не полностью согласованные, в том, как они заданы `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="d4f33-243">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="d4f33-244">Если задать заголовки на что-либо отличное от `"*"` (или используйте <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), должен включать по крайней мере `Accept`, `Content-Type`, и `Origin`, а также любые пользовательские заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="d4f33-244">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="d4f33-245">Ниже приведен пример ответа на Предварительный запрос, (при условии, что сервер разрешает запрос).</span><span class="sxs-lookup"><span data-stu-id="d4f33-245">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="d4f33-246">Ответ включает в себя `Access-Control-Allow-Methods` заголовка, в которой перечислены разрешенные методы и при необходимости `Access-Control-Allow-Headers` заголовок, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="d4f33-246">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="d4f33-247">Если Предварительный запрос завершается успешно, браузер отправляет фактический запрос.</span><span class="sxs-lookup"><span data-stu-id="d4f33-247">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="d4f33-248">Если Предварительный запрос отклонен, приложение возвращает *200 ОК* ответа, но не отправляет обратно заголовки CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-248">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="d4f33-249">Таким образом браузер не делает запрос независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-249">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="d4f33-250">Задайте срок действия предварительного</span><span class="sxs-lookup"><span data-stu-id="d4f33-250">Set the preflight expiration time</span></span>

<span data-ttu-id="d4f33-251">`Access-Control-Max-Age` Заголовок указывает, как долго можно кэшировать ответ на Предварительный запрос.</span><span class="sxs-lookup"><span data-stu-id="d4f33-251">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="d4f33-252">Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="d4f33-252">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="d4f33-253">Как работает CORS</span><span class="sxs-lookup"><span data-stu-id="d4f33-253">How CORS works</span></span>

<span data-ttu-id="d4f33-254">В этом разделе описывается, что происходит в [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) запрос на уровне сообщений HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4f33-254">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="d4f33-255">CORS — **не** средство безопасности.</span><span class="sxs-lookup"><span data-stu-id="d4f33-255">CORS is **not** a security feature.</span></span> <span data-ttu-id="d4f33-256">CORS — это стандартный W3C, позволяющий серверу смягчить ограничения политики одного источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-256">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="d4f33-257">Например, использовать вредоносного субъекта [сценариев предотвращения между сайтами (XSS)](xref:security/cross-site-scripting) на веб-узле и выполнения запросов между сайтами с сайтом включен механизм CORS для кражи информации.</span><span class="sxs-lookup"><span data-stu-id="d4f33-257">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="d4f33-258">API не является более безопасным, позволяя CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-258">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="d4f33-259">Именно на клиент (браузер) принудительно CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-259">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="d4f33-260">Сервер выполняет запрос и возвращает ответ, клиент, который возвращает ошибку и блоки ответ.</span><span class="sxs-lookup"><span data-stu-id="d4f33-260">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="d4f33-261">Например любой из следующих средств отображения ответ сервера:</span><span class="sxs-lookup"><span data-stu-id="d4f33-261">For example, any of the following tools will display the server response:</span></span>
     * [<span data-ttu-id="d4f33-262">Fiddler</span><span class="sxs-lookup"><span data-stu-id="d4f33-262">Fiddler</span></span>](https://www.telerik.com/fiddler)
     * [<span data-ttu-id="d4f33-263">Postman</span><span class="sxs-lookup"><span data-stu-id="d4f33-263">Postman</span></span>](https://www.getpostman.com/)
     * [<span data-ttu-id="d4f33-264">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="d4f33-264">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
     * <span data-ttu-id="d4f33-265">Веб-браузер, введя URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="d4f33-265">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="d4f33-266">Это способ для сервера разрешить браузеры выполнения независимо от источника [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) или [выборки API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) запроса, в противном случае может быть запрещено.</span><span class="sxs-lookup"><span data-stu-id="d4f33-266">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="d4f33-267">Запросов о происхождении обозревателей (CORS) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="d4f33-267">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="d4f33-268">Прежде чем CORS [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) было использовано, чтобы обойти это ограничение.</span><span class="sxs-lookup"><span data-stu-id="d4f33-268">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="d4f33-269">JSONP не использует XHR, он использует `<script>` тег для получения ответа.</span><span class="sxs-lookup"><span data-stu-id="d4f33-269">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="d4f33-270">Сценарии могут быть загружена независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-270">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="d4f33-271">[Спецификация CORS]() появился ряд новых заголовков HTTP, которые позволяют запросы независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-271">The [CORS specification]() introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="d4f33-272">Если браузер поддерживает CORS, он устанавливает эти заголовки для запросов о происхождении автоматически.</span><span class="sxs-lookup"><span data-stu-id="d4f33-272">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="d4f33-273">Пользовательский код JavaScript не обязательно для включения CORS.</span><span class="sxs-lookup"><span data-stu-id="d4f33-273">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="d4f33-274">Ниже приведен пример запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-274">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="d4f33-275">`Origin` Заголовок предоставляет домена сайта, который выполняет запрос:</span><span class="sxs-lookup"><span data-stu-id="d4f33-275">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="d4f33-276">Если сервер разрешает запрос, он задает `Access-Control-Allow-Origin` заголовок ответа.</span><span class="sxs-lookup"><span data-stu-id="d4f33-276">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="d4f33-277">Значение этого заголовка либо соответствует `Origin` заголовок из запроса или подстановочное значение `"*"`, это значит, что допускается любого источника:</span><span class="sxs-lookup"><span data-stu-id="d4f33-277">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="d4f33-278">Если ответ не включает `Access-Control-Allow-Origin` заголовок, происходит сбой запроса независимо от источника.</span><span class="sxs-lookup"><span data-stu-id="d4f33-278">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="d4f33-279">В частности браузер запрещает запрос.</span><span class="sxs-lookup"><span data-stu-id="d4f33-279">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="d4f33-280">Даже если сервер возвращает успешный ответ, браузер не предоставления ответа в клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="d4f33-280">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="d4f33-281">Тестирование CORS</span><span class="sxs-lookup"><span data-stu-id="d4f33-281">Test CORS</span></span>

<span data-ttu-id="d4f33-282">Тестирование CORS:</span><span class="sxs-lookup"><span data-stu-id="d4f33-282">To test CORS:</span></span>

1. <span data-ttu-id="d4f33-283">[Создание проекта API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="d4f33-283">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="d4f33-284">Кроме того, вы можете [скачать пример](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="d4f33-284">Alternatively, you can [download the sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="d4f33-285">Включите CORS, с помощью одного из методов в этом документе.</span><span class="sxs-lookup"><span data-stu-id="d4f33-285">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="d4f33-286">Пример:</span><span class="sxs-lookup"><span data-stu-id="d4f33-286">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]
  
  > [!WARNING]
  > <span data-ttu-id="d4f33-287">`WithOrigins("https://localhost:<port>");` следует использовать только для тестирования примера приложения, аналогичную [скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="d4f33-287">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="d4f33-288">Создайте проект веб-приложения (Razor Pages или MVC).</span><span class="sxs-lookup"><span data-stu-id="d4f33-288">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="d4f33-289">В примере используется Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d4f33-289">The sample uses Razor Pages.</span></span> <span data-ttu-id="d4f33-290">Можно создать веб-приложения в том же решении, что и проект API.</span><span class="sxs-lookup"><span data-stu-id="d4f33-290">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="d4f33-291">Добавьте следующий выделенный код, который *Index.cshtml* файла:</span><span class="sxs-lookup"><span data-stu-id="d4f33-291">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="d4f33-292">В приведенном выше коде замените `url: 'https://<web app>.azurewebsites.net/api/values/1',` с URL-адрес развернутого приложения.</span><span class="sxs-lookup"><span data-stu-id="d4f33-292">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="d4f33-293">Разверните проект API.</span><span class="sxs-lookup"><span data-stu-id="d4f33-293">Deploy the API project.</span></span> <span data-ttu-id="d4f33-294">Например [развертывание в Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="d4f33-294">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="d4f33-295">Запустите приложение Razor Pages или MVC с рабочего стола и щелкнуть **теста** кнопки.</span><span class="sxs-lookup"><span data-stu-id="d4f33-295">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="d4f33-296">Используйте инструменты F12 для просмотра сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="d4f33-296">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="d4f33-297">Удаление из начала координат localhost `WithOrigins` и развернуть приложение.</span><span class="sxs-lookup"><span data-stu-id="d4f33-297">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="d4f33-298">Кроме того запустите клиентское приложение с другим портом.</span><span class="sxs-lookup"><span data-stu-id="d4f33-298">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="d4f33-299">Например запуск из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4f33-299">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="d4f33-300">Тестирование с помощью клиентского приложения.</span><span class="sxs-lookup"><span data-stu-id="d4f33-300">Test with the client app.</span></span> <span data-ttu-id="d4f33-301">Сбои CORS сообщение об ошибке, но сообщение об ошибке недоступно в код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4f33-301">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="d4f33-302">Используйте вкладку консоли в инструментах F12, чтобы просмотреть ошибку.</span><span class="sxs-lookup"><span data-stu-id="d4f33-302">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="d4f33-303">В зависимости от браузера отобразится сообщение об ошибке (в консоли инструменты F12) следующего вида:</span><span class="sxs-lookup"><span data-stu-id="d4f33-303">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

  * <span data-ttu-id="d4f33-304">С помощью Microsoft Edge:</span><span class="sxs-lookup"><span data-stu-id="d4f33-304">Using Microsoft Edge:</span></span>

    <span data-ttu-id="d4f33-305">**SEC7120: [CORS] источник `https://localhost:44375` не обнаружил `https://localhost:44375` в заголовке ответа Access-Control-Allow-Origin для ресурса независимо от источника `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="d4f33-305">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

  * <span data-ttu-id="d4f33-306">С помощью Chrome:</span><span class="sxs-lookup"><span data-stu-id="d4f33-306">Using Chrome:</span></span>

    <span data-ttu-id="d4f33-307">**Доступ к XMLHttpRequest в `https://webapi.azurewebsites.net/api/values/1` от начала `https://localhost:44375` была заблокирована политикой CORS: Заголовок «Access-Control-Allow-Origin» отсутствует для запрашиваемого ресурса.**</span><span class="sxs-lookup"><span data-stu-id="d4f33-307">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4f33-308">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d4f33-308">Additional resources</span></span>

* [<span data-ttu-id="d4f33-309">Кросс-совместного использования ресурсов (CORS)</span><span class="sxs-lookup"><span data-stu-id="d4f33-309">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
