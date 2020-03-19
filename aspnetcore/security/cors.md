---
title: Включение запросов между источниками (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать CORS в качестве стандарта для разрешения или отклонения запросов между источниками в ASP.NET Core приложении.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511578"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="888e0-103">Включение запросов между источниками (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="888e0-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="888e0-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="888e0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="888e0-105">В этой статье показано, как включить CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="888e0-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="888e0-106">Безопасность в браузере предотвращает выполнение веб-страницей запросов к домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="888e0-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="888e0-107">Это ограничение называется *политикой того же происхождения*.</span><span class="sxs-lookup"><span data-stu-id="888e0-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="888e0-108">Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="888e0-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="888e0-109">Иногда может потребоваться разрешить другим сайтам выполнять запросы между источниками в приложении.</span><span class="sxs-lookup"><span data-stu-id="888e0-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="888e0-110">Дополнительные сведения см. в [статье Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="888e0-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="888e0-111">[Общий доступ к ресурсам в разных источниках](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="888e0-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="888e0-112">— Это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="888e0-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="888e0-113">**Не** является функцией безопасности, CORS ослабляет безопасность.</span><span class="sxs-lookup"><span data-stu-id="888e0-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="888e0-114">Интерфейс API не обеспечивает безопасную поддержку CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="888e0-115">Дополнительные сведения см. в разделе [как работает CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="888e0-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="888e0-116">Позволяет серверу явно разрешать некоторые запросы между источниками и отклонять другие.</span><span class="sxs-lookup"><span data-stu-id="888e0-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="888e0-117">Является более безопасным и более гибким, чем более ранние методики, например [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="888e0-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="888e0-118">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="888e0-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="888e0-119">Тот же источник</span><span class="sxs-lookup"><span data-stu-id="888e0-119">Same origin</span></span>

<span data-ttu-id="888e0-120">Два URL-адреса имеют одинаковый источник, если они имеют идентичные схемы, узлы и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="888e0-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="888e0-121">Эти два URL-адреса имеют один и тот же источник:</span><span class="sxs-lookup"><span data-stu-id="888e0-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="888e0-122">Эти URL-адреса имеют разные источники, чем предыдущие два URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="888e0-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="888e0-123">`https://example.net` &ndash; другой домен</span><span class="sxs-lookup"><span data-stu-id="888e0-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="888e0-124">`https://www.example.com/foo.html` &ndash; разных поддоменов</span><span class="sxs-lookup"><span data-stu-id="888e0-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="888e0-125">`http://example.com/foo.html` &ndash; другую схему</span><span class="sxs-lookup"><span data-stu-id="888e0-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="888e0-126">`https://example.com:9000/foo.html` &ndash; разных портов</span><span class="sxs-lookup"><span data-stu-id="888e0-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="888e0-127">При сравнении источников Internet Explorer не учитывает порт.</span><span class="sxs-lookup"><span data-stu-id="888e0-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="888e0-128">CORS с именованной политикой и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="888e0-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="888e0-129">По промежуточного слоя CORS обрабатывает запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="888e0-130">Следующий код включает CORS для всего приложения с указанным источником:</span><span class="sxs-lookup"><span data-stu-id="888e0-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="888e0-131">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="888e0-131">The preceding code:</span></span>

* <span data-ttu-id="888e0-132">Задает имя политики "\_МялловспеЦификоригинс".</span><span class="sxs-lookup"><span data-stu-id="888e0-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="888e0-133">Имя политики является произвольным.</span><span class="sxs-lookup"><span data-stu-id="888e0-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="888e0-134">Вызывает метод расширения <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, который включает CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="888e0-135">Вызывает <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> с [лямбда-выражением](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="888e0-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="888e0-136">Лямбда-выражение принимает объект <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="888e0-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="888e0-137">[Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="888e0-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="888e0-138">Вызов метода <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> добавляет службы CORS в контейнер службы приложения:</span><span class="sxs-lookup"><span data-stu-id="888e0-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="888e0-139">Дополнительные сведения см. в разделе [Параметры политики CORS](#cpo) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="888e0-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="888e0-140">Метод <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> может привести к цепочке методов, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="888e0-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="888e0-141">Примечание. URL-адрес **не** должен содержать косую черту (`/`).</span><span class="sxs-lookup"><span data-stu-id="888e0-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="888e0-142">Если URL-адрес завершается с `/`, то сравнение возвращает `false` и заголовок не возвращается.</span><span class="sxs-lookup"><span data-stu-id="888e0-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="888e0-143">Применение политик CORS ко всем конечным точкам</span><span class="sxs-lookup"><span data-stu-id="888e0-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="888e0-144">Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:</span><span class="sxs-lookup"><span data-stu-id="888e0-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> <span data-ttu-id="888e0-145">При маршрутизации конечных точек по промежуточного слоя CORS должно быть настроено для выполнения между вызовами `UseRouting` и `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="888e0-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="888e0-146">Неправильная конфигурация приведет к тому, что по промежуточного слоя будет работать правильно.</span><span class="sxs-lookup"><span data-stu-id="888e0-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

<span data-ttu-id="888e0-147">См. раздел [Включение CORS в Razor Pages, контроллерах и методах действий](#ecors) для применения политики CORS на уровне страницы, контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="888e0-147">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="888e0-148">Инструкции по тестированию приведенного выше кода см. в разделе [тестирование CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="888e0-148">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="888e0-149">Включение CORS с маршрутизацией конечных точек</span><span class="sxs-lookup"><span data-stu-id="888e0-149">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="888e0-150">С маршрутизацией конечных точек CORS можно включить для каждой конечной точки с помощью `RequireCors` набора методов расширения.</span><span class="sxs-lookup"><span data-stu-id="888e0-150">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="888e0-151">Кроме того, CORS можно также включить для всех контроллеров:</span><span class="sxs-lookup"><span data-stu-id="888e0-151">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="888e0-152">Включение CORS с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="888e0-152">Enable CORS with attributes</span></span>

<span data-ttu-id="888e0-153">Атрибут [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) предоставляет альтернативу глобальному применению CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-153">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="888e0-154">Атрибут `[EnableCors]` включает CORS для выбранных конечных точек, а не все конечные точки.</span><span class="sxs-lookup"><span data-stu-id="888e0-154">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="888e0-155">Используйте `[EnableCors]`, чтобы указать политику по умолчанию и `[EnableCors("{Policy String}")]`, чтобы указать политику.</span><span class="sxs-lookup"><span data-stu-id="888e0-155">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="888e0-156">Атрибут `[EnableCors]` может быть применен к:</span><span class="sxs-lookup"><span data-stu-id="888e0-156">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="888e0-157">`PageModel` страницы Razor</span><span class="sxs-lookup"><span data-stu-id="888e0-157">Razor Page `PageModel`</span></span>
* <span data-ttu-id="888e0-158">Контроллер</span><span class="sxs-lookup"><span data-stu-id="888e0-158">Controller</span></span>
* <span data-ttu-id="888e0-159">Метод действия контроллера</span><span class="sxs-lookup"><span data-stu-id="888e0-159">Controller action method</span></span>

<span data-ttu-id="888e0-160">К контроллеру, странице-модели или действию можно применить различные политики с помощью атрибута `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="888e0-160">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="888e0-161">При применении атрибута `[EnableCors]` к контроллеру, модели страницы или метода действия и включению CORS по промежуточного слоя применяются обе политики.</span><span class="sxs-lookup"><span data-stu-id="888e0-161">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="888e0-162">Мы советуем использовать сочетание политик.</span><span class="sxs-lookup"><span data-stu-id="888e0-162">We recommend against combining policies.</span></span> <span data-ttu-id="888e0-163">Используйте атрибут `[EnableCors]` или по промежуточного слоя, а не оба в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="888e0-163">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="888e0-164">Следующий код применяет к каждому методу другую политику:</span><span class="sxs-lookup"><span data-stu-id="888e0-164">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="888e0-165">Следующий код создает политику CORS по умолчанию и политику с именем `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="888e0-165">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="888e0-166">Отключение CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-166">Disable CORS</span></span>

<span data-ttu-id="888e0-167">Атрибут [&lbrack;дисаблекорс&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) отключает CORS для контроллера, модели страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="888e0-167">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="888e0-168">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-168">CORS policy options</span></span>

<span data-ttu-id="888e0-169">В этом разделе описаны различные параметры, которые можно задать в политике CORS:</span><span class="sxs-lookup"><span data-stu-id="888e0-169">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="888e0-170">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="888e0-170">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="888e0-171">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="888e0-171">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="888e0-172">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="888e0-172">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="888e0-173">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="888e0-173">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="888e0-174">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="888e0-174">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="888e0-175">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="888e0-175">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="888e0-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> вызывается в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="888e0-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="888e0-177">Для некоторых параметров может оказаться полезным сначала ознакомиться с разделом " [работа CORS](#how-cors) ".</span><span class="sxs-lookup"><span data-stu-id="888e0-177">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="888e0-178">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="888e0-178">Set the allowed origins</span></span>

<span data-ttu-id="888e0-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; разрешает запросы CORS из всех источников с любой схемой (`http` или `https`).</span><span class="sxs-lookup"><span data-stu-id="888e0-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="888e0-180">`AllowAnyOrigin` небезопасен, так как *любой веб-сайт* может выполнять запросы между источниками в приложение.</span><span class="sxs-lookup"><span data-stu-id="888e0-180">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="888e0-181">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="888e0-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="888e0-182">Служба CORS возвращает недопустимый ответ CORS, если приложение настроено с обоими методами.</span><span class="sxs-lookup"><span data-stu-id="888e0-182">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="888e0-183">`AllowAnyOrigin` влияет на предпечатные запросы и заголовок `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="888e0-183">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="888e0-184">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="888e0-184">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="888e0-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; задает свойство <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> политики как функцию, которая позволяет источникам соответствовать настроенному домену с подстановочными знаками при оценке того, разрешен ли источник.</span><span class="sxs-lookup"><span data-stu-id="888e0-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="888e0-186">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="888e0-186">Set the allowed HTTP methods</span></span>

<span data-ttu-id="888e0-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="888e0-188">Разрешает любой метод HTTP:</span><span class="sxs-lookup"><span data-stu-id="888e0-188">Allows any HTTP method:</span></span>
* <span data-ttu-id="888e0-189">Влияет на предпечатные запросы и заголовок `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="888e0-189">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="888e0-190">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="888e0-190">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="888e0-191">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="888e0-191">Set the allowed request headers</span></span>

<span data-ttu-id="888e0-192">Чтобы разрешить отправку конкретных заголовков в запросе CORS, называемом *заголовком запроса на создание*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="888e0-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="888e0-193">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="888e0-194">Этот параметр влияет на предпечатные запросы и заголовок `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="888e0-194">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="888e0-195">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="888e0-195">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>


<span data-ttu-id="888e0-196">Политика по промежуточного слоя CORS соответствует определенным заголовкам, указанным в `WithHeaders` только в том случае, если заголовки, отправленные в `Access-Control-Request-Headers`, точно соответствуют заголовкам, указанным в `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="888e0-196">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="888e0-197">Например, рассмотрим приложение, настроенное следующим образом:</span><span class="sxs-lookup"><span data-stu-id="888e0-197">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="888e0-198">По промежуточного слоя CORS отклоняет Предпечатный запрос со следующим заголовком запроса, так как `Content-Language` ([хеадернамес. контентлангуаже](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) отсутствует в `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="888e0-198">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="888e0-199">Приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно.</span><span class="sxs-lookup"><span data-stu-id="888e0-199">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="888e0-200">Поэтому браузер не пытается выполнить запрос между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-200">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="888e0-201">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="888e0-201">Set the exposed response headers</span></span>

<span data-ttu-id="888e0-202">По умолчанию браузер не предоставляет все заголовки ответа приложению.</span><span class="sxs-lookup"><span data-stu-id="888e0-202">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="888e0-203">Дополнительные сведения см. в разделе [общий доступ к ресурсам между источниками W3C (терминология): простой заголовок ответа](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="888e0-203">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="888e0-204">По умолчанию доступны заголовки ответов:</span><span class="sxs-lookup"><span data-stu-id="888e0-204">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="888e0-205">Спецификация CORS вызывает эти заголовки *простых заголовков ответа*.</span><span class="sxs-lookup"><span data-stu-id="888e0-205">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="888e0-206">Чтобы сделать другие заголовки доступными для приложения, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-206">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="888e0-207">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="888e0-207">Credentials in cross-origin requests</span></span>

<span data-ttu-id="888e0-208">Учетные данные требует специальной обработки в запросе CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-208">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="888e0-209">По умолчанию браузер не отправляет учетные данные с запросом между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-209">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="888e0-210">Учетные данные включают файлы cookie и схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="888e0-210">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="888e0-211">Чтобы отправить учетные данные с запросом между источниками, клиент должен задать для `XMLHttpRequest.withCredentials` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="888e0-211">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="888e0-212">Использование `XMLHttpRequest` напрямую:</span><span class="sxs-lookup"><span data-stu-id="888e0-212">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="888e0-213">Использование jQuery:</span><span class="sxs-lookup"><span data-stu-id="888e0-213">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="888e0-214">Использование [API выборки](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="888e0-214">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="888e0-215">Сервер должен разрешать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="888e0-215">The server must allow the credentials.</span></span> <span data-ttu-id="888e0-216">Чтобы разрешить учетные данные для разных источников, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-216">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="888e0-217">HTTP-ответ содержит заголовок `Access-Control-Allow-Credentials`, сообщающий браузеру, что сервер разрешает учетные данные для запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-217">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="888e0-218">Если браузер отправляет учетные данные, но ответ не включает допустимый заголовок `Access-Control-Allow-Credentials`, браузер не предоставляет ответ на приложение, и запрос на перекрестное происхождение завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="888e0-218">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="888e0-219">Разрешение учетных данных между источниками является угрозой безопасности.</span><span class="sxs-lookup"><span data-stu-id="888e0-219">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="888e0-220">Веб-сайт в другом домене может отправить учетные данные пользователя, выполнившего вход, в приложение от имени пользователя без ведома пользователя.</span><span class="sxs-lookup"><span data-stu-id="888e0-220">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="888e0-221">В спецификации CORS также указано, что при наличии заголовка `Access-Control-Allow-Credentials` недопустимыми являются исходные значения `"*"` (все источники).</span><span class="sxs-lookup"><span data-stu-id="888e0-221">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="888e0-222">Предпечатные запросы</span><span class="sxs-lookup"><span data-stu-id="888e0-222">Preflight requests</span></span>

<span data-ttu-id="888e0-223">Для некоторых запросов CORS браузер отправляет дополнительный запрос перед выполнением фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-223">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="888e0-224">Этот запрос называется *предпечатным запросом*.</span><span class="sxs-lookup"><span data-stu-id="888e0-224">This request is called a *preflight request*.</span></span> <span data-ttu-id="888e0-225">Браузер может пропустить Предпечатный запрос, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="888e0-225">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="888e0-226">Метод запроса — GET, HEAD или POST.</span><span class="sxs-lookup"><span data-stu-id="888e0-226">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="888e0-227">Приложение не устанавливает заголовки запросов, кроме `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`или `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="888e0-227">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="888e0-228">Заголовок `Content-Type`, если он задан, имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="888e0-228">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="888e0-229">Правило для заголовков запросов, заданных для запроса клиента, применяется к заголовкам, которые устанавливаются приложением путем вызова `setRequestHeader` для объекта `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="888e0-229">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="888e0-230">Спецификация CORS вызывает заголовки *запроса автора*заголовков.</span><span class="sxs-lookup"><span data-stu-id="888e0-230">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="888e0-231">Правило не применяется к заголовкам, которые могут быть заданы браузером, например `User-Agent`, `Host`или `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="888e0-231">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="888e0-232">Ниже приведен пример предпечатного запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-232">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="888e0-233">Запрос перед рейсом использует метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="888e0-233">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="888e0-234">Он включает два специальных заголовка:</span><span class="sxs-lookup"><span data-stu-id="888e0-234">It includes two special headers:</span></span>

* <span data-ttu-id="888e0-235">`Access-Control-Request-Method`: метод HTTP, который будет использоваться для фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-235">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="888e0-236">`Access-Control-Request-Headers`: список заголовков запросов, которые приложение задает для фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-236">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="888e0-237">Как упоминалось ранее, в него не входят заголовки, заданных браузером, например `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="888e0-237">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="888e0-238">Предпечатный запрос CORS может включать заголовок `Access-Control-Request-Headers`, указывающий серверу заголовки, отправляемые с фактическим запросом.</span><span class="sxs-lookup"><span data-stu-id="888e0-238">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="888e0-239">Чтобы разрешить определенные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-239">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="888e0-240">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-240">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="888e0-241">Обозреватели не полностью согласуются с тем, как они устанавливают `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="888e0-241">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="888e0-242">Если для заголовков заданы любые значения, кроме `"*"` (или использования <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), следует включить по крайней мере `Accept`, `Content-Type`и `Origin`, а также любые настраиваемые заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="888e0-242">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="888e0-243">Ниже приведен пример ответа на Предпечатный запрос (при условии, что сервер разрешает запрос):</span><span class="sxs-lookup"><span data-stu-id="888e0-243">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="888e0-244">Ответ содержит заголовок `Access-Control-Allow-Methods`, в котором перечислены допустимые методы и (необязательно) заголовок `Access-Control-Allow-Headers`, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="888e0-244">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="888e0-245">Если Предпечатный запрос выполнен, браузер отправляет фактический запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-245">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="888e0-246">Если Предпечатный запрос отклонен, приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно.</span><span class="sxs-lookup"><span data-stu-id="888e0-246">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="888e0-247">Поэтому браузер не пытается выполнить запрос между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-247">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="888e0-248">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="888e0-248">Set the preflight expiration time</span></span>

<span data-ttu-id="888e0-249">Заголовок `Access-Control-Max-Age` указывает, как долго может кэшироваться ответ на предварительный запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-249">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="888e0-250">Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-250">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="888e0-251">Принцип работы CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-251">How CORS works</span></span>

<span data-ttu-id="888e0-252">В этом разделе описывается, что происходит в запросе [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) на уровне HTTP-сообщений.</span><span class="sxs-lookup"><span data-stu-id="888e0-252">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="888e0-253">CORS **не** является функцией безопасности.</span><span class="sxs-lookup"><span data-stu-id="888e0-253">CORS is **not** a security feature.</span></span> <span data-ttu-id="888e0-254">CORS — это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="888e0-254">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="888e0-255">Например, вредоносный субъект может использовать для веб [-узла предотвращение межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) и выполнить межсайтовый запрос к сайту с поддержкой CORS, чтобы украсть информацию.</span><span class="sxs-lookup"><span data-stu-id="888e0-255">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="888e0-256">Интерфейс API не обеспечивает безопасную работу, разрешая CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-256">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="888e0-257">Для применения CORS требуется клиент (браузер).</span><span class="sxs-lookup"><span data-stu-id="888e0-257">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="888e0-258">Сервер выполняет запрос и возвращает ответ. это клиент, который возвращает сообщение об ошибке и блокирует ответ.</span><span class="sxs-lookup"><span data-stu-id="888e0-258">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="888e0-259">Например, в любом из следующих средств будет отображаться ответ сервера:</span><span class="sxs-lookup"><span data-stu-id="888e0-259">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="888e0-260">Fiddler</span><span class="sxs-lookup"><span data-stu-id="888e0-260">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="888e0-261">Postman</span><span class="sxs-lookup"><span data-stu-id="888e0-261">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="888e0-262">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="888e0-262">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="888e0-263">Веб-браузер, введя URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="888e0-263">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="888e0-264">Сервер может позволить обозревателям выполнять запросы API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) или [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) , которые в противном случае будут запрещены.</span><span class="sxs-lookup"><span data-stu-id="888e0-264">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="888e0-265">Браузеры (без CORS) не могут выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-265">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="888e0-266">Перед CORS использовалась технология [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) для обхода этого ограничения.</span><span class="sxs-lookup"><span data-stu-id="888e0-266">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="888e0-267">JSONP не использует XHR, он использует тег `<script>` для получения ответа.</span><span class="sxs-lookup"><span data-stu-id="888e0-267">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="888e0-268">Скрипты могут загружаться в разных источниках.</span><span class="sxs-lookup"><span data-stu-id="888e0-268">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="888e0-269">В [спецификации CORS](https://www.w3.org/TR/cors/) появились несколько новых HTTP-заголовков, которые позволяют выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-269">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="888e0-270">Если браузер поддерживает CORS, эти заголовки автоматически устанавливаются для запросов между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-270">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="888e0-271">Для включения CORS не требуется пользовательский код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="888e0-271">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="888e0-272">Ниже приведен пример запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-272">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="888e0-273">Заголовок `Origin` предоставляет домен сайта, выполняющего запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-273">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="888e0-274">Заголовок `Origin` является обязательным и должен отличаться от узла.</span><span class="sxs-lookup"><span data-stu-id="888e0-274">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="888e0-275">Если сервер разрешает запрос, он задает заголовок `Access-Control-Allow-Origin` в ответе.</span><span class="sxs-lookup"><span data-stu-id="888e0-275">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="888e0-276">Значение этого заголовка либо соответствует заголовку `Origin` из запроса, либо является подстановочным значением `"*"`, что означает, что любой источник разрешен:</span><span class="sxs-lookup"><span data-stu-id="888e0-276">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="888e0-277">Если ответ не содержит заголовок `Access-Control-Allow-Origin`, запрос на перекрестное происхождение завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="888e0-277">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="888e0-278">В частности, браузер не разрешает запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-278">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="888e0-279">Даже если сервер возвращает успешный ответ, браузер не делает ответ доступным клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="888e0-279">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="888e0-280">Тестирование CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-280">Test CORS</span></span>

<span data-ttu-id="888e0-281">Тестирование CORS:</span><span class="sxs-lookup"><span data-stu-id="888e0-281">To test CORS:</span></span>

1. <span data-ttu-id="888e0-282">[Создайте проект API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="888e0-282">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="888e0-283">Кроме того, можно [загрузить пример](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="888e0-283">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="888e0-284">Включите CORS с помощью одного из подходов, описанных в этом документе.</span><span class="sxs-lookup"><span data-stu-id="888e0-284">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="888e0-285">Пример:</span><span class="sxs-lookup"><span data-stu-id="888e0-285">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="888e0-286">`WithOrigins("https://localhost:<port>");` следует использовать только для тестирования примера приложения, подобного [образцу кода для загрузки](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="888e0-286">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="888e0-287">Создайте проект веб-приложения (Razor Pages или MVC).</span><span class="sxs-lookup"><span data-stu-id="888e0-287">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="888e0-288">В примере используется Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="888e0-288">The sample uses Razor Pages.</span></span> <span data-ttu-id="888e0-289">Вы можете создать веб-приложение в том же решении, что и проект API.</span><span class="sxs-lookup"><span data-stu-id="888e0-289">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="888e0-290">Добавьте выделенный ниже код в файл *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="888e0-290">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="888e0-291">В приведенном выше коде замените `url: 'https://<web app>.azurewebsites.net/api/values/1',` URL-адресом развернутого приложения.</span><span class="sxs-lookup"><span data-stu-id="888e0-291">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="888e0-292">Разверните проект API.</span><span class="sxs-lookup"><span data-stu-id="888e0-292">Deploy the API project.</span></span> <span data-ttu-id="888e0-293">Например, выполните [развертывание в Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="888e0-293">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="888e0-294">Запустите приложение Razor Pages или MVC на рабочем столе и нажмите кнопку **Test (тест** ).</span><span class="sxs-lookup"><span data-stu-id="888e0-294">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="888e0-295">Используйте средства F12 для просмотра сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="888e0-295">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="888e0-296">Удалите источник localhost из `WithOrigins` и разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="888e0-296">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="888e0-297">Кроме того, можно запустить клиентское приложение с другим портом.</span><span class="sxs-lookup"><span data-stu-id="888e0-297">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="888e0-298">Например, запустите из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="888e0-298">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="888e0-299">Протестируйте клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="888e0-299">Test with the client app.</span></span> <span data-ttu-id="888e0-300">Ошибки CORS возвращают ошибку, но сообщение об ошибке недоступно для JavaScript.</span><span class="sxs-lookup"><span data-stu-id="888e0-300">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="888e0-301">Чтобы просмотреть ошибку, используйте вкладку консоль в средствах F12.</span><span class="sxs-lookup"><span data-stu-id="888e0-301">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="888e0-302">В зависимости от браузера вы получаете ошибку (в консоли средств F12), как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="888e0-302">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="888e0-303">Использование Microsoft ребр:</span><span class="sxs-lookup"><span data-stu-id="888e0-303">Using Microsoft Edge:</span></span>

     <span data-ttu-id="888e0-304">**SEC7120: [CORS] `https://localhost:44375` источника не удалось найти `https://localhost:44375` в заголовке ответа Access-Control-Allow-Origin для ресурса между источниками в `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="888e0-304">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="888e0-305">Использование Chrome:</span><span class="sxs-lookup"><span data-stu-id="888e0-305">Using Chrome:</span></span>

     <span data-ttu-id="888e0-306">**Доступ к XMLHttpRequest на `https://webapi.azurewebsites.net/api/values/1` из исходной `https://localhost:44375` заблокирован политикой CORS: в запрошенном ресурсе отсутствует заголовок "Access-Control-Allow-Origin".**</span><span class="sxs-lookup"><span data-stu-id="888e0-306">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="888e0-307">Конечные точки с поддержкой CORS можно тестировать с помощью средства, такого как [Fiddler](https://www.telerik.com/fiddler) или [POST](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="888e0-307">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="888e0-308">При использовании средства источник запроса, указанный в заголовке `Origin`, должен отличаться от узла, получающего запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-308">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="888e0-309">Если запрос не имеет *источника* на основе значения заголовка `Origin`:</span><span class="sxs-lookup"><span data-stu-id="888e0-309">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="888e0-310">Для обработки запроса по промежуточного слоя CORS не требуется.</span><span class="sxs-lookup"><span data-stu-id="888e0-310">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="888e0-311">Заголовки CORS не возвращаются в ответе.</span><span class="sxs-lookup"><span data-stu-id="888e0-311">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="888e0-312">CORS в IIS</span><span class="sxs-lookup"><span data-stu-id="888e0-312">CORS in IIS</span></span>

<span data-ttu-id="888e0-313">При развертывании в IIS CORS необходимо запустить перед проверкой подлинности Windows, если сервер не настроен на разрешение анонимного доступа.</span><span class="sxs-lookup"><span data-stu-id="888e0-313">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="888e0-314">Для поддержки этого сценария необходимо установить и настроить [модуль IIS CORS](https://www.iis.net/downloads/microsoft/iis-cors-module) для приложения.</span><span class="sxs-lookup"><span data-stu-id="888e0-314">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="888e0-315">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="888e0-315">Additional resources</span></span>

* [<span data-ttu-id="888e0-316">Общий доступ к ресурсам между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="888e0-316">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="888e0-317">Приступая к работе с модулем IIS CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-317">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="888e0-318">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="888e0-318">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="888e0-319">В этой статье показано, как включить CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="888e0-319">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="888e0-320">Безопасность в браузере предотвращает выполнение веб-страницей запросов к домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="888e0-320">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="888e0-321">Это ограничение называется *политикой того же происхождения*.</span><span class="sxs-lookup"><span data-stu-id="888e0-321">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="888e0-322">Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="888e0-322">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="888e0-323">Иногда может потребоваться разрешить другим сайтам выполнять запросы между источниками в приложении.</span><span class="sxs-lookup"><span data-stu-id="888e0-323">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="888e0-324">Дополнительные сведения см. в [статье Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="888e0-324">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="888e0-325">[Общий доступ к ресурсам в разных источниках](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="888e0-325">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="888e0-326">— Это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="888e0-326">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="888e0-327">**Не** является функцией безопасности, CORS ослабляет безопасность.</span><span class="sxs-lookup"><span data-stu-id="888e0-327">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="888e0-328">Интерфейс API не обеспечивает безопасную поддержку CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-328">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="888e0-329">Дополнительные сведения см. в разделе [как работает CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="888e0-329">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="888e0-330">Позволяет серверу явно разрешать некоторые запросы между источниками и отклонять другие.</span><span class="sxs-lookup"><span data-stu-id="888e0-330">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="888e0-331">Является более безопасным и более гибким, чем более ранние методики, например [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="888e0-331">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="888e0-332">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="888e0-332">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="888e0-333">Тот же источник</span><span class="sxs-lookup"><span data-stu-id="888e0-333">Same origin</span></span>

<span data-ttu-id="888e0-334">Два URL-адреса имеют одинаковый источник, если они имеют идентичные схемы, узлы и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="888e0-334">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="888e0-335">Эти два URL-адреса имеют один и тот же источник:</span><span class="sxs-lookup"><span data-stu-id="888e0-335">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="888e0-336">Эти URL-адреса имеют разные источники, чем предыдущие два URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="888e0-336">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="888e0-337">`https://example.net` &ndash; другой домен</span><span class="sxs-lookup"><span data-stu-id="888e0-337">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="888e0-338">`https://www.example.com/foo.html` &ndash; разных поддоменов</span><span class="sxs-lookup"><span data-stu-id="888e0-338">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="888e0-339">`http://example.com/foo.html` &ndash; другую схему</span><span class="sxs-lookup"><span data-stu-id="888e0-339">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="888e0-340">`https://example.com:9000/foo.html` &ndash; разных портов</span><span class="sxs-lookup"><span data-stu-id="888e0-340">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="888e0-341">При сравнении источников Internet Explorer не учитывает порт.</span><span class="sxs-lookup"><span data-stu-id="888e0-341">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="888e0-342">CORS с именованной политикой и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="888e0-342">CORS with named policy and middleware</span></span>

<span data-ttu-id="888e0-343">По промежуточного слоя CORS обрабатывает запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-343">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="888e0-344">Следующий код включает CORS для всего приложения с указанным источником:</span><span class="sxs-lookup"><span data-stu-id="888e0-344">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="888e0-345">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="888e0-345">The preceding code:</span></span>

* <span data-ttu-id="888e0-346">Задает имя политики "\_МялловспеЦификоригинс".</span><span class="sxs-lookup"><span data-stu-id="888e0-346">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="888e0-347">Имя политики является произвольным.</span><span class="sxs-lookup"><span data-stu-id="888e0-347">The policy name is arbitrary.</span></span>
* <span data-ttu-id="888e0-348">Вызывает метод расширения <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, который включает CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-348">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="888e0-349">Вызывает <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> с [лямбда-выражением](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="888e0-349">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="888e0-350">Лямбда-выражение принимает объект <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="888e0-350">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="888e0-351">[Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="888e0-351">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="888e0-352">Вызов метода <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> добавляет службы CORS в контейнер службы приложения:</span><span class="sxs-lookup"><span data-stu-id="888e0-352">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="888e0-353">Дополнительные сведения см. в разделе [Параметры политики CORS](#cpo) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="888e0-353">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="888e0-354">Метод <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> может привести к цепочке методов, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="888e0-354">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="888e0-355">Примечание. URL-адрес **не** должен содержать косую черту (`/`).</span><span class="sxs-lookup"><span data-stu-id="888e0-355">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="888e0-356">Если URL-адрес завершается с `/`, то сравнение возвращает `false` и заголовок не возвращается.</span><span class="sxs-lookup"><span data-stu-id="888e0-356">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="888e0-357">Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:</span><span class="sxs-lookup"><span data-stu-id="888e0-357">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="888e0-358">Примечание. перед `UseMvc`необходимо вызвать `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="888e0-358">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="888e0-359">См. раздел [Включение CORS в Razor Pages, контроллерах и методах действий](#ecors) для применения политики CORS на уровне страницы, контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="888e0-359">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="888e0-360">Инструкции по тестированию приведенного выше кода см. в разделе [тестирование CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="888e0-360">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="888e0-361">Включение CORS с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="888e0-361">Enable CORS with attributes</span></span>

<span data-ttu-id="888e0-362">Атрибут [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) предоставляет альтернативу глобальному применению CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-362">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="888e0-363">Атрибут `[EnableCors]` включает CORS для выбранных конечных точек, а не все конечные точки.</span><span class="sxs-lookup"><span data-stu-id="888e0-363">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="888e0-364">Используйте `[EnableCors]`, чтобы указать политику по умолчанию и `[EnableCors("{Policy String}")]`, чтобы указать политику.</span><span class="sxs-lookup"><span data-stu-id="888e0-364">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="888e0-365">Атрибут `[EnableCors]` может быть применен к:</span><span class="sxs-lookup"><span data-stu-id="888e0-365">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="888e0-366">`PageModel` страницы Razor</span><span class="sxs-lookup"><span data-stu-id="888e0-366">Razor Page `PageModel`</span></span>
* <span data-ttu-id="888e0-367">Контроллер</span><span class="sxs-lookup"><span data-stu-id="888e0-367">Controller</span></span>
* <span data-ttu-id="888e0-368">Метод действия контроллера</span><span class="sxs-lookup"><span data-stu-id="888e0-368">Controller action method</span></span>

<span data-ttu-id="888e0-369">К контроллеру, странице-модели или действию можно применить различные политики с помощью атрибута `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="888e0-369">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="888e0-370">При применении атрибута `[EnableCors]` к контроллеру, модели страницы или метода действия и включению CORS по промежуточного слоя применяются обе политики.</span><span class="sxs-lookup"><span data-stu-id="888e0-370">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="888e0-371">Мы советуем использовать сочетание политик.</span><span class="sxs-lookup"><span data-stu-id="888e0-371">We recommend against combining policies.</span></span> <span data-ttu-id="888e0-372">Используйте атрибут `[EnableCors]` или по промежуточного слоя, а не оба в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="888e0-372">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="888e0-373">Следующий код применяет к каждому методу другую политику:</span><span class="sxs-lookup"><span data-stu-id="888e0-373">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="888e0-374">Следующий код создает политику CORS по умолчанию и политику с именем `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="888e0-374">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="888e0-375">Отключение CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-375">Disable CORS</span></span>

<span data-ttu-id="888e0-376">Атрибут [&lbrack;дисаблекорс&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) отключает CORS для контроллера, модели страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="888e0-376">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="888e0-377">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-377">CORS policy options</span></span>

<span data-ttu-id="888e0-378">В этом разделе описаны различные параметры, которые можно задать в политике CORS:</span><span class="sxs-lookup"><span data-stu-id="888e0-378">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="888e0-379">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="888e0-379">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="888e0-380">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="888e0-380">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="888e0-381">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="888e0-381">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="888e0-382">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="888e0-382">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="888e0-383">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="888e0-383">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="888e0-384">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="888e0-384">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="888e0-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> вызывается в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="888e0-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="888e0-386">Для некоторых параметров может оказаться полезным сначала ознакомиться с разделом " [работа CORS](#how-cors) ".</span><span class="sxs-lookup"><span data-stu-id="888e0-386">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="888e0-387">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="888e0-387">Set the allowed origins</span></span>

<span data-ttu-id="888e0-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; разрешает запросы CORS из всех источников с любой схемой (`http` или `https`).</span><span class="sxs-lookup"><span data-stu-id="888e0-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="888e0-389">`AllowAnyOrigin` небезопасен, так как *любой веб-сайт* может выполнять запросы между источниками в приложение.</span><span class="sxs-lookup"><span data-stu-id="888e0-389">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="888e0-390">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="888e0-390">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="888e0-391">Для безопасного приложения укажите точный список источников, если клиент должен авторизоваться для доступа к ресурсам сервера.</span><span class="sxs-lookup"><span data-stu-id="888e0-391">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="888e0-392">`AllowAnyOrigin` влияет на предпечатные запросы и заголовок `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="888e0-392">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="888e0-393">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="888e0-393">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="888e0-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; задает свойство <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> политики как функцию, которая позволяет источникам соответствовать настроенному домену с подстановочными знаками при оценке того, разрешен ли источник.</span><span class="sxs-lookup"><span data-stu-id="888e0-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="888e0-395">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="888e0-395">Set the allowed HTTP methods</span></span>

<span data-ttu-id="888e0-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="888e0-397">Разрешает любой метод HTTP:</span><span class="sxs-lookup"><span data-stu-id="888e0-397">Allows any HTTP method:</span></span>
* <span data-ttu-id="888e0-398">Влияет на предпечатные запросы и заголовок `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="888e0-398">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="888e0-399">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="888e0-399">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="888e0-400">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="888e0-400">Set the allowed request headers</span></span>

<span data-ttu-id="888e0-401">Чтобы разрешить отправку конкретных заголовков в запросе CORS, называемом *заголовком запроса на создание*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="888e0-401">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="888e0-402">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-402">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="888e0-403">Этот параметр влияет на предпечатные запросы и заголовок `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="888e0-403">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="888e0-404">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="888e0-404">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="888e0-405">По промежуточного слоя CORS всегда позволяет отправлять четыре заголовка в `Access-Control-Request-Headers` независимо от значений, настроенных в Корсполици. Headers.</span><span class="sxs-lookup"><span data-stu-id="888e0-405">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="888e0-406">Этот список заголовков включает:</span><span class="sxs-lookup"><span data-stu-id="888e0-406">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="888e0-407">Например, рассмотрим приложение, настроенное следующим образом:</span><span class="sxs-lookup"><span data-stu-id="888e0-407">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="888e0-408">По промежуточного слоя CORS успешно отправляется на Предпечатный запрос со следующим заголовком запроса, поскольку `Content-Language` всегда список разрешений:</span><span class="sxs-lookup"><span data-stu-id="888e0-408">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="888e0-409">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="888e0-409">Set the exposed response headers</span></span>

<span data-ttu-id="888e0-410">По умолчанию браузер не предоставляет все заголовки ответа приложению.</span><span class="sxs-lookup"><span data-stu-id="888e0-410">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="888e0-411">Дополнительные сведения см. в разделе [общий доступ к ресурсам между источниками W3C (терминология): простой заголовок ответа](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="888e0-411">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="888e0-412">По умолчанию доступны заголовки ответов:</span><span class="sxs-lookup"><span data-stu-id="888e0-412">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="888e0-413">Спецификация CORS вызывает эти заголовки *простых заголовков ответа*.</span><span class="sxs-lookup"><span data-stu-id="888e0-413">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="888e0-414">Чтобы сделать другие заголовки доступными для приложения, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-414">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="888e0-415">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="888e0-415">Credentials in cross-origin requests</span></span>

<span data-ttu-id="888e0-416">Учетные данные требует специальной обработки в запросе CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-416">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="888e0-417">По умолчанию браузер не отправляет учетные данные с запросом между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-417">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="888e0-418">Учетные данные включают файлы cookie и схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="888e0-418">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="888e0-419">Чтобы отправить учетные данные с запросом между источниками, клиент должен задать для `XMLHttpRequest.withCredentials` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="888e0-419">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="888e0-420">Использование `XMLHttpRequest` напрямую:</span><span class="sxs-lookup"><span data-stu-id="888e0-420">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="888e0-421">Использование jQuery:</span><span class="sxs-lookup"><span data-stu-id="888e0-421">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="888e0-422">Использование [API выборки](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="888e0-422">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="888e0-423">Сервер должен разрешать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="888e0-423">The server must allow the credentials.</span></span> <span data-ttu-id="888e0-424">Чтобы разрешить учетные данные для разных источников, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-424">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="888e0-425">HTTP-ответ содержит заголовок `Access-Control-Allow-Credentials`, сообщающий браузеру, что сервер разрешает учетные данные для запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-425">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="888e0-426">Если браузер отправляет учетные данные, но ответ не включает допустимый заголовок `Access-Control-Allow-Credentials`, браузер не предоставляет ответ на приложение, и запрос на перекрестное происхождение завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="888e0-426">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="888e0-427">Разрешение учетных данных между источниками является угрозой безопасности.</span><span class="sxs-lookup"><span data-stu-id="888e0-427">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="888e0-428">Веб-сайт в другом домене может отправить учетные данные пользователя, выполнившего вход, в приложение от имени пользователя без ведома пользователя.</span><span class="sxs-lookup"><span data-stu-id="888e0-428">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="888e0-429">В спецификации CORS также указано, что при наличии заголовка `Access-Control-Allow-Credentials` недопустимыми являются исходные значения `"*"` (все источники).</span><span class="sxs-lookup"><span data-stu-id="888e0-429">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="888e0-430">Предпечатные запросы</span><span class="sxs-lookup"><span data-stu-id="888e0-430">Preflight requests</span></span>

<span data-ttu-id="888e0-431">Для некоторых запросов CORS браузер отправляет дополнительный запрос перед выполнением фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-431">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="888e0-432">Этот запрос называется *предпечатным запросом*.</span><span class="sxs-lookup"><span data-stu-id="888e0-432">This request is called a *preflight request*.</span></span> <span data-ttu-id="888e0-433">Браузер может пропустить Предпечатный запрос, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="888e0-433">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="888e0-434">Метод запроса — GET, HEAD или POST.</span><span class="sxs-lookup"><span data-stu-id="888e0-434">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="888e0-435">Приложение не устанавливает заголовки запросов, кроме `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`или `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="888e0-435">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="888e0-436">Заголовок `Content-Type`, если он задан, имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="888e0-436">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="888e0-437">Правило для заголовков запросов, заданных для запроса клиента, применяется к заголовкам, которые устанавливаются приложением путем вызова `setRequestHeader` для объекта `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="888e0-437">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="888e0-438">Спецификация CORS вызывает заголовки *запроса автора*заголовков.</span><span class="sxs-lookup"><span data-stu-id="888e0-438">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="888e0-439">Правило не применяется к заголовкам, которые могут быть заданы браузером, например `User-Agent`, `Host`или `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="888e0-439">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="888e0-440">Ниже приведен пример предпечатного запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-440">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="888e0-441">Запрос перед рейсом использует метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="888e0-441">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="888e0-442">Он включает два специальных заголовка:</span><span class="sxs-lookup"><span data-stu-id="888e0-442">It includes two special headers:</span></span>

* <span data-ttu-id="888e0-443">`Access-Control-Request-Method`: метод HTTP, который будет использоваться для фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-443">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="888e0-444">`Access-Control-Request-Headers`: список заголовков запросов, которые приложение задает для фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="888e0-444">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="888e0-445">Как упоминалось ранее, в него не входят заголовки, заданных браузером, например `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="888e0-445">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="888e0-446">Предпечатный запрос CORS может включать заголовок `Access-Control-Request-Headers`, указывающий серверу заголовки, отправляемые с фактическим запросом.</span><span class="sxs-lookup"><span data-stu-id="888e0-446">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="888e0-447">Чтобы разрешить определенные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-447">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="888e0-448">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-448">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="888e0-449">Обозреватели не полностью согласуются с тем, как они устанавливают `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="888e0-449">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="888e0-450">Если для заголовков заданы любые значения, кроме `"*"` (или использования <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), следует включить по крайней мере `Accept`, `Content-Type`и `Origin`, а также любые настраиваемые заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="888e0-450">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="888e0-451">Ниже приведен пример ответа на Предпечатный запрос (при условии, что сервер разрешает запрос):</span><span class="sxs-lookup"><span data-stu-id="888e0-451">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="888e0-452">Ответ содержит заголовок `Access-Control-Allow-Methods`, в котором перечислены допустимые методы и (необязательно) заголовок `Access-Control-Allow-Headers`, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="888e0-452">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="888e0-453">Если Предпечатный запрос выполнен, браузер отправляет фактический запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-453">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="888e0-454">Если Предпечатный запрос отклонен, приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно.</span><span class="sxs-lookup"><span data-stu-id="888e0-454">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="888e0-455">Поэтому браузер не пытается выполнить запрос между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-455">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="888e0-456">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="888e0-456">Set the preflight expiration time</span></span>

<span data-ttu-id="888e0-457">Заголовок `Access-Control-Max-Age` указывает, как долго может кэшироваться ответ на предварительный запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-457">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="888e0-458">Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="888e0-458">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="888e0-459">Принцип работы CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-459">How CORS works</span></span>

<span data-ttu-id="888e0-460">В этом разделе описывается, что происходит в запросе [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) на уровне HTTP-сообщений.</span><span class="sxs-lookup"><span data-stu-id="888e0-460">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="888e0-461">CORS **не** является функцией безопасности.</span><span class="sxs-lookup"><span data-stu-id="888e0-461">CORS is **not** a security feature.</span></span> <span data-ttu-id="888e0-462">CORS — это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="888e0-462">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="888e0-463">Например, вредоносный субъект может использовать для веб [-узла предотвращение межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) и выполнить межсайтовый запрос к сайту с поддержкой CORS, чтобы украсть информацию.</span><span class="sxs-lookup"><span data-stu-id="888e0-463">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="888e0-464">Интерфейс API не обеспечивает безопасную работу, разрешая CORS.</span><span class="sxs-lookup"><span data-stu-id="888e0-464">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="888e0-465">Для применения CORS требуется клиент (браузер).</span><span class="sxs-lookup"><span data-stu-id="888e0-465">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="888e0-466">Сервер выполняет запрос и возвращает ответ. это клиент, который возвращает сообщение об ошибке и блокирует ответ.</span><span class="sxs-lookup"><span data-stu-id="888e0-466">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="888e0-467">Например, в любом из следующих средств будет отображаться ответ сервера:</span><span class="sxs-lookup"><span data-stu-id="888e0-467">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="888e0-468">Fiddler</span><span class="sxs-lookup"><span data-stu-id="888e0-468">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="888e0-469">Postman</span><span class="sxs-lookup"><span data-stu-id="888e0-469">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="888e0-470">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="888e0-470">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="888e0-471">Веб-браузер, введя URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="888e0-471">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="888e0-472">Сервер может позволить обозревателям выполнять запросы API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) или [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) , которые в противном случае будут запрещены.</span><span class="sxs-lookup"><span data-stu-id="888e0-472">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="888e0-473">Браузеры (без CORS) не могут выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-473">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="888e0-474">Перед CORS использовалась технология [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) для обхода этого ограничения.</span><span class="sxs-lookup"><span data-stu-id="888e0-474">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="888e0-475">JSONP не использует XHR, он использует тег `<script>` для получения ответа.</span><span class="sxs-lookup"><span data-stu-id="888e0-475">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="888e0-476">Скрипты могут загружаться в разных источниках.</span><span class="sxs-lookup"><span data-stu-id="888e0-476">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="888e0-477">В [спецификации CORS](https://www.w3.org/TR/cors/) появились несколько новых HTTP-заголовков, которые позволяют выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-477">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="888e0-478">Если браузер поддерживает CORS, эти заголовки автоматически устанавливаются для запросов между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-478">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="888e0-479">Для включения CORS не требуется пользовательский код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="888e0-479">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="888e0-480">Ниже приведен пример запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="888e0-480">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="888e0-481">Заголовок `Origin` предоставляет домен сайта, выполняющего запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-481">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="888e0-482">Заголовок `Origin` является обязательным и должен отличаться от узла.</span><span class="sxs-lookup"><span data-stu-id="888e0-482">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="888e0-483">Если сервер разрешает запрос, он задает заголовок `Access-Control-Allow-Origin` в ответе.</span><span class="sxs-lookup"><span data-stu-id="888e0-483">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="888e0-484">Значение этого заголовка либо соответствует заголовку `Origin` из запроса, либо является подстановочным значением `"*"`, что означает, что любой источник разрешен:</span><span class="sxs-lookup"><span data-stu-id="888e0-484">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="888e0-485">Если ответ не содержит заголовок `Access-Control-Allow-Origin`, запрос на перекрестное происхождение завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="888e0-485">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="888e0-486">В частности, браузер не разрешает запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-486">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="888e0-487">Даже если сервер возвращает успешный ответ, браузер не делает ответ доступным клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="888e0-487">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="888e0-488">Тестирование CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-488">Test CORS</span></span>

<span data-ttu-id="888e0-489">Тестирование CORS:</span><span class="sxs-lookup"><span data-stu-id="888e0-489">To test CORS:</span></span>

1. <span data-ttu-id="888e0-490">[Создайте проект API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="888e0-490">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="888e0-491">Кроме того, можно [загрузить пример](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="888e0-491">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="888e0-492">Включите CORS с помощью одного из подходов, описанных в этом документе.</span><span class="sxs-lookup"><span data-stu-id="888e0-492">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="888e0-493">Пример:</span><span class="sxs-lookup"><span data-stu-id="888e0-493">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="888e0-494">`WithOrigins("https://localhost:<port>");` следует использовать только для тестирования примера приложения, подобного [образцу кода для загрузки](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="888e0-494">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="888e0-495">Создайте проект веб-приложения (Razor Pages или MVC).</span><span class="sxs-lookup"><span data-stu-id="888e0-495">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="888e0-496">В примере используется Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="888e0-496">The sample uses Razor Pages.</span></span> <span data-ttu-id="888e0-497">Вы можете создать веб-приложение в том же решении, что и проект API.</span><span class="sxs-lookup"><span data-stu-id="888e0-497">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="888e0-498">Добавьте выделенный ниже код в файл *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="888e0-498">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="888e0-499">В приведенном выше коде замените `url: 'https://<web app>.azurewebsites.net/api/values/1',` URL-адресом развернутого приложения.</span><span class="sxs-lookup"><span data-stu-id="888e0-499">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="888e0-500">Разверните проект API.</span><span class="sxs-lookup"><span data-stu-id="888e0-500">Deploy the API project.</span></span> <span data-ttu-id="888e0-501">Например, выполните [развертывание в Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="888e0-501">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="888e0-502">Запустите приложение Razor Pages или MVC на рабочем столе и нажмите кнопку **Test (тест** ).</span><span class="sxs-lookup"><span data-stu-id="888e0-502">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="888e0-503">Используйте средства F12 для просмотра сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="888e0-503">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="888e0-504">Удалите источник localhost из `WithOrigins` и разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="888e0-504">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="888e0-505">Кроме того, можно запустить клиентское приложение с другим портом.</span><span class="sxs-lookup"><span data-stu-id="888e0-505">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="888e0-506">Например, запустите из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="888e0-506">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="888e0-507">Протестируйте клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="888e0-507">Test with the client app.</span></span> <span data-ttu-id="888e0-508">Ошибки CORS возвращают ошибку, но сообщение об ошибке недоступно для JavaScript.</span><span class="sxs-lookup"><span data-stu-id="888e0-508">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="888e0-509">Чтобы просмотреть ошибку, используйте вкладку консоль в средствах F12.</span><span class="sxs-lookup"><span data-stu-id="888e0-509">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="888e0-510">В зависимости от браузера вы получаете ошибку (в консоли средств F12), как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="888e0-510">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="888e0-511">Использование Microsoft ребр:</span><span class="sxs-lookup"><span data-stu-id="888e0-511">Using Microsoft Edge:</span></span>

     <span data-ttu-id="888e0-512">**SEC7120: [CORS] `https://localhost:44375` источника не удалось найти `https://localhost:44375` в заголовке ответа Access-Control-Allow-Origin для ресурса между источниками в `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="888e0-512">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="888e0-513">Использование Chrome:</span><span class="sxs-lookup"><span data-stu-id="888e0-513">Using Chrome:</span></span>

     <span data-ttu-id="888e0-514">**Доступ к XMLHttpRequest на `https://webapi.azurewebsites.net/api/values/1` из исходной `https://localhost:44375` заблокирован политикой CORS: в запрошенном ресурсе отсутствует заголовок "Access-Control-Allow-Origin".**</span><span class="sxs-lookup"><span data-stu-id="888e0-514">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="888e0-515">Конечные точки с поддержкой CORS можно тестировать с помощью средства, такого как [Fiddler](https://www.telerik.com/fiddler) или [POST](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="888e0-515">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="888e0-516">При использовании средства источник запроса, указанный в заголовке `Origin`, должен отличаться от узла, получающего запрос.</span><span class="sxs-lookup"><span data-stu-id="888e0-516">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="888e0-517">Если запрос не имеет *источника* на основе значения заголовка `Origin`:</span><span class="sxs-lookup"><span data-stu-id="888e0-517">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="888e0-518">Для обработки запроса по промежуточного слоя CORS не требуется.</span><span class="sxs-lookup"><span data-stu-id="888e0-518">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="888e0-519">Заголовки CORS не возвращаются в ответе.</span><span class="sxs-lookup"><span data-stu-id="888e0-519">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="888e0-520">CORS в IIS</span><span class="sxs-lookup"><span data-stu-id="888e0-520">CORS in IIS</span></span>

<span data-ttu-id="888e0-521">При развертывании в IIS CORS необходимо запустить перед проверкой подлинности Windows, если сервер не настроен на разрешение анонимного доступа.</span><span class="sxs-lookup"><span data-stu-id="888e0-521">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="888e0-522">Для поддержки этого сценария необходимо установить и настроить [модуль IIS CORS](https://www.iis.net/downloads/microsoft/iis-cors-module) для приложения.</span><span class="sxs-lookup"><span data-stu-id="888e0-522">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="888e0-523">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="888e0-523">Additional resources</span></span>

* [<span data-ttu-id="888e0-524">Общий доступ к ресурсам между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="888e0-524">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="888e0-525">Приступая к работе с модулем IIS CORS</span><span class="sxs-lookup"><span data-stu-id="888e0-525">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end