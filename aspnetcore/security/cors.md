---
title: Включение запросов между источниками (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать CORS в качестве стандарта для разрешения или отклонения запросов между источниками в ASP.NET Core приложении.
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 57098be73164c71d1b0d1fe2f3aee7ec41a32346
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727314"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="d553a-103">Включение запросов между источниками (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d553a-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="d553a-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d553a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d553a-105">В этой статье показано, как включить CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d553a-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="d553a-106">Безопасность в браузере предотвращает выполнение веб-страницей запросов к домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="d553a-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="d553a-107">Это ограничение называется *политикой того же происхождения*.</span><span class="sxs-lookup"><span data-stu-id="d553a-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="d553a-108">Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="d553a-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="d553a-109">Иногда может потребоваться разрешить другим сайтам выполнять запросы между источниками в приложении.</span><span class="sxs-lookup"><span data-stu-id="d553a-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="d553a-110">Дополнительные сведения см. в [статье Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="d553a-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="d553a-111">[Общий доступ к ресурсам в разных источниках](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="d553a-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="d553a-112">— Это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="d553a-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="d553a-113">**Не** является функцией безопасности, CORS ослабляет безопасность.</span><span class="sxs-lookup"><span data-stu-id="d553a-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="d553a-114">Интерфейс API не обеспечивает безопасную поддержку CORS.</span><span class="sxs-lookup"><span data-stu-id="d553a-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="d553a-115">Дополнительные сведения см. в разделе [как работает CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="d553a-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="d553a-116">Позволяет серверу явно разрешать некоторые запросы между источниками и отклонять другие.</span><span class="sxs-lookup"><span data-stu-id="d553a-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="d553a-117">Является более безопасным и более гибким, чем более ранние методики, например [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="d553a-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="d553a-118">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d553a-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="d553a-119">Тот же источник</span><span class="sxs-lookup"><span data-stu-id="d553a-119">Same origin</span></span>

<span data-ttu-id="d553a-120">Два URL-адреса имеют одинаковый источник, если они имеют идентичные схемы, узлы и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="d553a-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="d553a-121">Эти два URL-адреса имеют один и тот же источник:</span><span class="sxs-lookup"><span data-stu-id="d553a-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="d553a-122">Эти URL-адреса имеют разные источники, чем предыдущие два URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="d553a-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="d553a-123">`https://example.net` &ndash; другой домен</span><span class="sxs-lookup"><span data-stu-id="d553a-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="d553a-124">`https://www.example.com/foo.html` &ndash; разных поддоменов</span><span class="sxs-lookup"><span data-stu-id="d553a-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="d553a-125">`http://example.com/foo.html` &ndash; другую схему</span><span class="sxs-lookup"><span data-stu-id="d553a-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="d553a-126">`https://example.com:9000/foo.html` &ndash; разных портов</span><span class="sxs-lookup"><span data-stu-id="d553a-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="d553a-127">При сравнении источников Internet Explorer не учитывает порт.</span><span class="sxs-lookup"><span data-stu-id="d553a-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="d553a-128">CORS с именованной политикой и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="d553a-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="d553a-129">По промежуточного слоя CORS обрабатывает запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="d553a-130">Следующий код включает CORS для всего приложения с указанным источником:</span><span class="sxs-lookup"><span data-stu-id="d553a-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="d553a-131">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="d553a-131">The preceding code:</span></span>

* <span data-ttu-id="d553a-132">Задает имя политики "\_МялловспеЦификоригинс".</span><span class="sxs-lookup"><span data-stu-id="d553a-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="d553a-133">Имя политики является произвольным.</span><span class="sxs-lookup"><span data-stu-id="d553a-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="d553a-134">Вызывает метод расширения <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>, который включает CORS.</span><span class="sxs-lookup"><span data-stu-id="d553a-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="d553a-135">Вызывает <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> с [лямбда-выражением](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="d553a-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="d553a-136">Лямбда-выражение принимает объект <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>.</span><span class="sxs-lookup"><span data-stu-id="d553a-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="d553a-137">[Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="d553a-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="d553a-138">Вызов метода <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> добавляет службы CORS в контейнер службы приложения:</span><span class="sxs-lookup"><span data-stu-id="d553a-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="d553a-139">Дополнительные сведения см. в разделе [Параметры политики CORS](#cpo) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="d553a-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="d553a-140">Метод <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> может привести к цепочке методов, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="d553a-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="d553a-141">Примечание. URL-адрес **не** должен содержать косую черту (`/`).</span><span class="sxs-lookup"><span data-stu-id="d553a-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="d553a-142">Если URL-адрес завершается с `/`, то сравнение возвращает `false` и заголовок не возвращается.</span><span class="sxs-lookup"><span data-stu-id="d553a-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="d553a-143">Применение политик CORS ко всем конечным точкам</span><span class="sxs-lookup"><span data-stu-id="d553a-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="d553a-144">Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:</span><span class="sxs-lookup"><span data-stu-id="d553a-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> <span data-ttu-id="d553a-145">При маршрутизации конечных точек по промежуточного слоя CORS должно быть настроено для выполнения между вызовами `UseRouting` и `UseEndpoints`.</span><span class="sxs-lookup"><span data-stu-id="d553a-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="d553a-146">Неправильная конфигурация приведет к тому, что по промежуточного слоя будет работать правильно.</span><span class="sxs-lookup"><span data-stu-id="d553a-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="d553a-147">Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:</span><span class="sxs-lookup"><span data-stu-id="d553a-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="d553a-148">Примечание. перед `UseMvc`необходимо вызвать `UseCors`.</span><span class="sxs-lookup"><span data-stu-id="d553a-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="d553a-149">См. раздел [Включение CORS в Razor Pages, контроллерах и методах действий](#ecors) для применения политики CORS на уровне страницы, контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="d553a-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="d553a-150">Инструкции по тестированию приведенного выше кода см. в разделе [тестирование CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="d553a-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="d553a-151">Включение CORS с маршрутизацией конечных точек</span><span class="sxs-lookup"><span data-stu-id="d553a-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="d553a-152">С маршрутизацией конечных точек CORS можно включить для каждой конечной точки с помощью `RequireCors` набора методов расширения.</span><span class="sxs-lookup"><span data-stu-id="d553a-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="d553a-153">Кроме того, CORS можно также включить для всех контроллеров:</span><span class="sxs-lookup"><span data-stu-id="d553a-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="d553a-154">Включение CORS с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="d553a-154">Enable CORS with attributes</span></span>

<span data-ttu-id="d553a-155">Атрибут [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) предоставляет альтернативу глобальному применению CORS.</span><span class="sxs-lookup"><span data-stu-id="d553a-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="d553a-156">Атрибут `[EnableCors]` включает CORS для выбранных конечных точек, а не все конечные точки.</span><span class="sxs-lookup"><span data-stu-id="d553a-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="d553a-157">Используйте `[EnableCors]`, чтобы указать политику по умолчанию и `[EnableCors("{Policy String}")]`, чтобы указать политику.</span><span class="sxs-lookup"><span data-stu-id="d553a-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="d553a-158">Атрибут `[EnableCors]` может быть применен к:</span><span class="sxs-lookup"><span data-stu-id="d553a-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="d553a-159">`PageModel` страницы Razor</span><span class="sxs-lookup"><span data-stu-id="d553a-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="d553a-160">Контроллер</span><span class="sxs-lookup"><span data-stu-id="d553a-160">Controller</span></span>
* <span data-ttu-id="d553a-161">Метод действия контроллера</span><span class="sxs-lookup"><span data-stu-id="d553a-161">Controller action method</span></span>

<span data-ttu-id="d553a-162">К контроллеру, странице-модели или действию можно применить различные политики с помощью атрибута `[EnableCors]`.</span><span class="sxs-lookup"><span data-stu-id="d553a-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="d553a-163">При применении атрибута `[EnableCors]` к контроллеру, модели страницы или метода действия и включению CORS по промежуточного слоя применяются обе политики.</span><span class="sxs-lookup"><span data-stu-id="d553a-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="d553a-164">Мы советуем использовать сочетание политик.</span><span class="sxs-lookup"><span data-stu-id="d553a-164">We recommend against combining policies.</span></span> <span data-ttu-id="d553a-165">Используйте атрибут `[EnableCors]` или по промежуточного слоя, а не оба в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="d553a-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="d553a-166">Следующий код применяет к каждому методу другую политику:</span><span class="sxs-lookup"><span data-stu-id="d553a-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="d553a-167">Следующий код создает политику CORS по умолчанию и политику с именем `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="d553a-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="d553a-168">Отключение CORS</span><span class="sxs-lookup"><span data-stu-id="d553a-168">Disable CORS</span></span>

<span data-ttu-id="d553a-169">Атрибут [&lbrack;дисаблекорс&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) отключает CORS для контроллера, модели страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="d553a-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="d553a-170">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="d553a-170">CORS policy options</span></span>

<span data-ttu-id="d553a-171">В этом разделе описаны различные параметры, которые можно задать в политике CORS:</span><span class="sxs-lookup"><span data-stu-id="d553a-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="d553a-172">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="d553a-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="d553a-173">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="d553a-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="d553a-174">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="d553a-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="d553a-175">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="d553a-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="d553a-176">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="d553a-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="d553a-177">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="d553a-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="d553a-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> вызывается в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d553a-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d553a-179">Для некоторых параметров может оказаться полезным сначала ознакомиться с разделом " [работа CORS](#how-cors) ".</span><span class="sxs-lookup"><span data-stu-id="d553a-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="d553a-180">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="d553a-180">Set the allowed origins</span></span>

<span data-ttu-id="d553a-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; разрешает запросы CORS из всех источников с любой схемой (`http` или `https`).</span><span class="sxs-lookup"><span data-stu-id="d553a-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="d553a-182">`AllowAnyOrigin` небезопасен, так как *любой веб-сайт* может выполнять запросы между источниками в приложение.</span><span class="sxs-lookup"><span data-stu-id="d553a-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="d553a-183">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="d553a-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="d553a-184">Служба CORS возвращает недопустимый ответ CORS, если приложение настроено с обоими методами.</span><span class="sxs-lookup"><span data-stu-id="d553a-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="d553a-185">Указание `AllowAnyOrigin` и `AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="d553a-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="d553a-186">Для безопасного приложения укажите точный список источников, если клиент должен авторизоваться для доступа к ресурсам сервера.</span><span class="sxs-lookup"><span data-stu-id="d553a-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="d553a-187">`AllowAnyOrigin` влияет на предпечатные запросы и заголовок `Access-Control-Allow-Origin`.</span><span class="sxs-lookup"><span data-stu-id="d553a-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="d553a-188">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="d553a-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="d553a-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; задает свойство <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> политики как функцию, которая позволяет источникам соответствовать настроенному домену с подстановочными знаками при оценке того, разрешен ли источник.</span><span class="sxs-lookup"><span data-stu-id="d553a-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="d553a-190">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="d553a-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="d553a-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>.</span><span class="sxs-lookup"><span data-stu-id="d553a-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="d553a-192">Разрешает любой метод HTTP:</span><span class="sxs-lookup"><span data-stu-id="d553a-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="d553a-193">Влияет на предпечатные запросы и заголовок `Access-Control-Allow-Methods`.</span><span class="sxs-lookup"><span data-stu-id="d553a-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="d553a-194">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="d553a-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="d553a-195">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="d553a-195">Set the allowed request headers</span></span>

<span data-ttu-id="d553a-196">Чтобы разрешить отправку конкретных заголовков в запросе CORS, называемом *заголовком запроса на создание*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="d553a-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="d553a-197">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="d553a-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="d553a-198">Этот параметр влияет на предпечатные запросы и заголовок `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="d553a-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="d553a-199">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="d553a-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d553a-200">Политика по промежуточного слоя CORS соответствует определенным заголовкам, указанным в `WithHeaders` только в том случае, если заголовки, отправленные в `Access-Control-Request-Headers`, точно соответствуют заголовкам, указанным в `WithHeaders`.</span><span class="sxs-lookup"><span data-stu-id="d553a-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="d553a-201">Например, рассмотрим приложение, настроенное следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d553a-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="d553a-202">По промежуточного слоя CORS отклоняет Предпечатный запрос со следующим заголовком запроса, так как `Content-Language` ([хеадернамес. контентлангуаже](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) отсутствует в `WithHeaders`:</span><span class="sxs-lookup"><span data-stu-id="d553a-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="d553a-203">Приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно.</span><span class="sxs-lookup"><span data-stu-id="d553a-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="d553a-204">Поэтому браузер не пытается выполнить запрос между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d553a-205">По промежуточного слоя CORS всегда позволяет отправлять четыре заголовка в `Access-Control-Request-Headers` независимо от значений, настроенных в Корсполици. Headers.</span><span class="sxs-lookup"><span data-stu-id="d553a-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="d553a-206">Этот список заголовков включает:</span><span class="sxs-lookup"><span data-stu-id="d553a-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="d553a-207">Например, рассмотрим приложение, настроенное следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d553a-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="d553a-208">По промежуточного слоя CORS успешно отправляется на Предпечатный запрос со следующим заголовком запроса, поскольку `Content-Language` всегда список разрешений:</span><span class="sxs-lookup"><span data-stu-id="d553a-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="d553a-209">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="d553a-209">Set the exposed response headers</span></span>

<span data-ttu-id="d553a-210">По умолчанию браузер не предоставляет все заголовки ответа приложению.</span><span class="sxs-lookup"><span data-stu-id="d553a-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="d553a-211">Дополнительные сведения см. в разделе [общий доступ к ресурсам между источниками W3C (терминология): простой заголовок ответа](https://www.w3.org/TR/cors/#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="d553a-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="d553a-212">По умолчанию доступны заголовки ответов:</span><span class="sxs-lookup"><span data-stu-id="d553a-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="d553a-213">Спецификация CORS вызывает эти заголовки *простых заголовков ответа*.</span><span class="sxs-lookup"><span data-stu-id="d553a-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="d553a-214">Чтобы сделать другие заголовки доступными для приложения, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="d553a-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="d553a-215">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="d553a-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="d553a-216">Учетные данные требует специальной обработки в запросе CORS.</span><span class="sxs-lookup"><span data-stu-id="d553a-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="d553a-217">По умолчанию браузер не отправляет учетные данные с запросом между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="d553a-218">Учетные данные включают файлы cookie и схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="d553a-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="d553a-219">Чтобы отправить учетные данные с запросом между источниками, клиент должен задать для `XMLHttpRequest.withCredentials` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="d553a-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="d553a-220">Использование `XMLHttpRequest` напрямую:</span><span class="sxs-lookup"><span data-stu-id="d553a-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="d553a-221">Использование jQuery:</span><span class="sxs-lookup"><span data-stu-id="d553a-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="d553a-222">Использование [API выборки](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="d553a-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="d553a-223">Сервер должен разрешать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="d553a-223">The server must allow the credentials.</span></span> <span data-ttu-id="d553a-224">Чтобы разрешить учетные данные для разных источников, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="d553a-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="d553a-225">HTTP-ответ содержит заголовок `Access-Control-Allow-Credentials`, сообщающий браузеру, что сервер разрешает учетные данные для запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="d553a-226">Если браузер отправляет учетные данные, но ответ не включает допустимый заголовок `Access-Control-Allow-Credentials`, браузер не предоставляет ответ на приложение, и запрос на перекрестное происхождение завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="d553a-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="d553a-227">Разрешение учетных данных между источниками является угрозой безопасности.</span><span class="sxs-lookup"><span data-stu-id="d553a-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="d553a-228">Веб-сайт в другом домене может отправить учетные данные пользователя, выполнившего вход, в приложение от имени пользователя без ведома пользователя.</span><span class="sxs-lookup"><span data-stu-id="d553a-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="d553a-229">В спецификации CORS также указано, что при наличии заголовка `Access-Control-Allow-Credentials` недопустимыми являются исходные значения `"*"` (все источники).</span><span class="sxs-lookup"><span data-stu-id="d553a-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="d553a-230">Предпечатные запросы</span><span class="sxs-lookup"><span data-stu-id="d553a-230">Preflight requests</span></span>

<span data-ttu-id="d553a-231">Для некоторых запросов CORS браузер отправляет дополнительный запрос перед выполнением фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="d553a-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="d553a-232">Этот запрос называется *предпечатным запросом*.</span><span class="sxs-lookup"><span data-stu-id="d553a-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="d553a-233">Браузер может пропустить Предпечатный запрос, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="d553a-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="d553a-234">Метод запроса — GET, HEAD или POST.</span><span class="sxs-lookup"><span data-stu-id="d553a-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="d553a-235">Приложение не устанавливает заголовки запросов, кроме `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`или `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="d553a-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="d553a-236">Заголовок `Content-Type`, если он задан, имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="d553a-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="d553a-237">Правило для заголовков запросов, заданных для запроса клиента, применяется к заголовкам, которые устанавливаются приложением путем вызова `setRequestHeader` для объекта `XMLHttpRequest`.</span><span class="sxs-lookup"><span data-stu-id="d553a-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="d553a-238">Спецификация CORS вызывает заголовки *запроса автора*заголовков.</span><span class="sxs-lookup"><span data-stu-id="d553a-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="d553a-239">Правило не применяется к заголовкам, которые могут быть заданы браузером, например `User-Agent`, `Host`или `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="d553a-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="d553a-240">Ниже приведен пример предпечатного запроса.</span><span class="sxs-lookup"><span data-stu-id="d553a-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="d553a-241">Запрос перед рейсом использует метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="d553a-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="d553a-242">Он включает два специальных заголовка:</span><span class="sxs-lookup"><span data-stu-id="d553a-242">It includes two special headers:</span></span>

* <span data-ttu-id="d553a-243">`Access-Control-Request-Method`: метод HTTP, который будет использоваться для фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="d553a-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="d553a-244">`Access-Control-Request-Headers`: список заголовков запросов, которые приложение задает для фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="d553a-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="d553a-245">Как упоминалось ранее, в него не входят заголовки, заданных браузером, например `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="d553a-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="d553a-246">Предпечатный запрос CORS может включать заголовок `Access-Control-Request-Headers`, указывающий серверу заголовки, отправляемые с фактическим запросом.</span><span class="sxs-lookup"><span data-stu-id="d553a-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="d553a-247">Чтобы разрешить определенные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="d553a-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="d553a-248">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="d553a-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="d553a-249">Обозреватели не полностью согласуются с тем, как они устанавливают `Access-Control-Request-Headers`.</span><span class="sxs-lookup"><span data-stu-id="d553a-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="d553a-250">Если для заголовков заданы любые значения, кроме `"*"` (или использования <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), следует включить по крайней мере `Accept`, `Content-Type`и `Origin`, а также любые настраиваемые заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="d553a-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="d553a-251">Ниже приведен пример ответа на Предпечатный запрос (при условии, что сервер разрешает запрос):</span><span class="sxs-lookup"><span data-stu-id="d553a-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="d553a-252">Ответ содержит заголовок `Access-Control-Allow-Methods`, в котором перечислены допустимые методы и (необязательно) заголовок `Access-Control-Allow-Headers`, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="d553a-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="d553a-253">Если Предпечатный запрос выполнен, браузер отправляет фактический запрос.</span><span class="sxs-lookup"><span data-stu-id="d553a-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="d553a-254">Если Предпечатный запрос отклонен, приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно.</span><span class="sxs-lookup"><span data-stu-id="d553a-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="d553a-255">Поэтому браузер не пытается выполнить запрос между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="d553a-256">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="d553a-256">Set the preflight expiration time</span></span>

<span data-ttu-id="d553a-257">Заголовок `Access-Control-Max-Age` указывает, как долго может кэшироваться ответ на предварительный запрос.</span><span class="sxs-lookup"><span data-stu-id="d553a-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="d553a-258">Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="d553a-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="d553a-259">Принцип работы CORS</span><span class="sxs-lookup"><span data-stu-id="d553a-259">How CORS works</span></span>

<span data-ttu-id="d553a-260">В этом разделе описывается, что происходит в запросе [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) на уровне HTTP-сообщений.</span><span class="sxs-lookup"><span data-stu-id="d553a-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="d553a-261">CORS **не** является функцией безопасности.</span><span class="sxs-lookup"><span data-stu-id="d553a-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="d553a-262">CORS — это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="d553a-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="d553a-263">Например, вредоносный субъект может использовать для веб [-узла предотвращение межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) и выполнить межсайтовый запрос к сайту с поддержкой CORS, чтобы украсть информацию.</span><span class="sxs-lookup"><span data-stu-id="d553a-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="d553a-264">Интерфейс API не обеспечивает безопасную работу, разрешая CORS.</span><span class="sxs-lookup"><span data-stu-id="d553a-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="d553a-265">Для применения CORS требуется клиент (браузер).</span><span class="sxs-lookup"><span data-stu-id="d553a-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="d553a-266">Сервер выполняет запрос и возвращает ответ. это клиент, который возвращает сообщение об ошибке и блокирует ответ.</span><span class="sxs-lookup"><span data-stu-id="d553a-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="d553a-267">Например, в любом из следующих средств будет отображаться ответ сервера:</span><span class="sxs-lookup"><span data-stu-id="d553a-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="d553a-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="d553a-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="d553a-269">Postman</span><span class="sxs-lookup"><span data-stu-id="d553a-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="d553a-270">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="d553a-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="d553a-271">Веб-браузер, введя URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="d553a-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="d553a-272">Сервер может позволить обозревателям выполнять запросы API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) или [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) , которые в противном случае будут запрещены.</span><span class="sxs-lookup"><span data-stu-id="d553a-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="d553a-273">Браузеры (без CORS) не могут выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="d553a-274">Перед CORS использовалась технология [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) для обхода этого ограничения.</span><span class="sxs-lookup"><span data-stu-id="d553a-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="d553a-275">JSONP не использует XHR, он использует тег `<script>` для получения ответа.</span><span class="sxs-lookup"><span data-stu-id="d553a-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="d553a-276">Скрипты могут загружаться в разных источниках.</span><span class="sxs-lookup"><span data-stu-id="d553a-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="d553a-277">В [спецификации CORS](https://www.w3.org/TR/cors/) появились несколько новых HTTP-заголовков, которые позволяют выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="d553a-278">Если браузер поддерживает CORS, эти заголовки автоматически устанавливаются для запросов между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="d553a-279">Для включения CORS не требуется пользовательский код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d553a-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="d553a-280">Ниже приведен пример запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="d553a-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="d553a-281">Заголовок `Origin` предоставляет домен сайта, выполняющего запрос.</span><span class="sxs-lookup"><span data-stu-id="d553a-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="d553a-282">Заголовок `Origin` является обязательным и должен отличаться от узла.</span><span class="sxs-lookup"><span data-stu-id="d553a-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="d553a-283">Если сервер разрешает запрос, он задает заголовок `Access-Control-Allow-Origin` в ответе.</span><span class="sxs-lookup"><span data-stu-id="d553a-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="d553a-284">Значение этого заголовка либо соответствует заголовку `Origin` из запроса, либо является подстановочным значением `"*"`, что означает, что любой источник разрешен:</span><span class="sxs-lookup"><span data-stu-id="d553a-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="d553a-285">Если ответ не содержит заголовок `Access-Control-Allow-Origin`, запрос на перекрестное происхождение завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="d553a-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="d553a-286">В частности, браузер не разрешает запрос.</span><span class="sxs-lookup"><span data-stu-id="d553a-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="d553a-287">Даже если сервер возвращает успешный ответ, браузер не делает ответ доступным клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="d553a-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="d553a-288">Тестирование CORS</span><span class="sxs-lookup"><span data-stu-id="d553a-288">Test CORS</span></span>

<span data-ttu-id="d553a-289">Тестирование CORS:</span><span class="sxs-lookup"><span data-stu-id="d553a-289">To test CORS:</span></span>

1. <span data-ttu-id="d553a-290">[Создайте проект API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="d553a-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="d553a-291">Кроме того, можно [загрузить пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="d553a-291">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="d553a-292">Включите CORS с помощью одного из подходов, описанных в этом документе.</span><span class="sxs-lookup"><span data-stu-id="d553a-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="d553a-293">Например:</span><span class="sxs-lookup"><span data-stu-id="d553a-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="d553a-294">`WithOrigins("https://localhost:<port>");` следует использовать только для тестирования примера приложения, подобного [образцу кода для загрузки](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="d553a-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="d553a-295">Создайте проект веб-приложения (Razor Pages или MVC).</span><span class="sxs-lookup"><span data-stu-id="d553a-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="d553a-296">В примере используется Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="d553a-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="d553a-297">Вы можете создать веб-приложение в том же решении, что и проект API.</span><span class="sxs-lookup"><span data-stu-id="d553a-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="d553a-298">Добавьте выделенный ниже код в файл *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="d553a-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="d553a-299">В приведенном выше коде замените `url: 'https://<web app>.azurewebsites.net/api/values/1',` URL-адресом развернутого приложения.</span><span class="sxs-lookup"><span data-stu-id="d553a-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="d553a-300">Разверните проект API.</span><span class="sxs-lookup"><span data-stu-id="d553a-300">Deploy the API project.</span></span> <span data-ttu-id="d553a-301">Например, выполните [развертывание в Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="d553a-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="d553a-302">Запустите приложение Razor Pages или MVC на рабочем столе и нажмите кнопку **Test (тест** ).</span><span class="sxs-lookup"><span data-stu-id="d553a-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="d553a-303">Используйте средства F12 для просмотра сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="d553a-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="d553a-304">Удалите источник localhost из `WithOrigins` и разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="d553a-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="d553a-305">Кроме того, можно запустить клиентское приложение с другим портом.</span><span class="sxs-lookup"><span data-stu-id="d553a-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="d553a-306">Например, запустите из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d553a-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="d553a-307">Протестируйте клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="d553a-307">Test with the client app.</span></span> <span data-ttu-id="d553a-308">Ошибки CORS возвращают ошибку, но сообщение об ошибке недоступно для JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d553a-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="d553a-309">Чтобы просмотреть ошибку, используйте вкладку консоль в средствах F12.</span><span class="sxs-lookup"><span data-stu-id="d553a-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="d553a-310">В зависимости от браузера вы получаете ошибку (в консоли средств F12), как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="d553a-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="d553a-311">Использование Microsoft ребр:</span><span class="sxs-lookup"><span data-stu-id="d553a-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="d553a-312">**SEC7120: [CORS] `https://localhost:44375` источника не удалось найти `https://localhost:44375` в заголовке ответа Access-Control-Allow-Origin для ресурса между источниками в `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="d553a-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="d553a-313">Использование Chrome:</span><span class="sxs-lookup"><span data-stu-id="d553a-313">Using Chrome:</span></span>

     <span data-ttu-id="d553a-314">**Доступ к XMLHttpRequest на `https://webapi.azurewebsites.net/api/values/1` из исходной `https://localhost:44375` заблокирован политикой CORS: в запрошенном ресурсе отсутствует заголовок "Access-Control-Allow-Origin".**</span><span class="sxs-lookup"><span data-stu-id="d553a-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="d553a-315">Конечные точки с поддержкой CORS можно тестировать с помощью средства, такого как [Fiddler](https://www.telerik.com/fiddler) или [POST](https://www.getpostman.com/).</span><span class="sxs-lookup"><span data-stu-id="d553a-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="d553a-316">При использовании средства источник запроса, указанный в заголовке `Origin`, должен отличаться от узла, получающего запрос.</span><span class="sxs-lookup"><span data-stu-id="d553a-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="d553a-317">Если запрос не имеет *источника* на основе значения заголовка `Origin`:</span><span class="sxs-lookup"><span data-stu-id="d553a-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="d553a-318">Для обработки запроса по промежуточного слоя CORS не требуется.</span><span class="sxs-lookup"><span data-stu-id="d553a-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="d553a-319">Заголовки CORS не возвращаются в ответе.</span><span class="sxs-lookup"><span data-stu-id="d553a-319">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="d553a-320">CORS в IIS</span><span class="sxs-lookup"><span data-stu-id="d553a-320">CORS in IIS</span></span>

<span data-ttu-id="d553a-321">При развертывании в IIS CORS необходимо запустить перед проверкой подлинности Windows, если сервер не настроен на разрешение анонимного доступа.</span><span class="sxs-lookup"><span data-stu-id="d553a-321">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="d553a-322">Для поддержки этого сценария необходимо установить и настроить [модуль IIS CORS](https://www.iis.net/downloads/microsoft/iis-cors-module) для приложения.</span><span class="sxs-lookup"><span data-stu-id="d553a-322">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d553a-323">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d553a-323">Additional resources</span></span>

* [<span data-ttu-id="d553a-324">Общий доступ к ресурсам между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="d553a-324">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="d553a-325">Приступая к работе с модулем IIS CORS</span><span class="sxs-lookup"><span data-stu-id="d553a-325">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
