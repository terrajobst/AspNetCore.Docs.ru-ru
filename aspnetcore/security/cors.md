---
title: Включение запросов между источниками (CORS) в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать CORS в качестве стандарта для разрешения или отклонения запросов между источниками в ASP.NET Core приложении.
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a02b3497684979c1a9e792437f9f1a4c467600f0
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187252"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="7c448-103">Включение запросов между источниками (CORS) в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c448-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="7c448-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7c448-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c448-105">В этой статье показано, как включить CORS в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7c448-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="7c448-106">Безопасность в браузере предотвращает выполнение веб-страницей запросов к домену, отличному от того, который обслуживает веб-страницу.</span><span class="sxs-lookup"><span data-stu-id="7c448-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="7c448-107">Это ограничение называется *политикой того же происхождения*.</span><span class="sxs-lookup"><span data-stu-id="7c448-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="7c448-108">Политика того же источника не позволит вредоносному сайту считывать конфиденциальные данные с другого сайта.</span><span class="sxs-lookup"><span data-stu-id="7c448-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="7c448-109">Иногда может потребоваться разрешить другим сайтам выполнять запросы между источниками в приложении.</span><span class="sxs-lookup"><span data-stu-id="7c448-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="7c448-110">Дополнительные сведения см. в [статье Mozilla CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span><span class="sxs-lookup"><span data-stu-id="7c448-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="7c448-111">[Совместное использование ресурсов в разных источниках](https://www.w3.org/TR/cors/) (CORS):</span><span class="sxs-lookup"><span data-stu-id="7c448-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="7c448-112">— Это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="7c448-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="7c448-113">**Не** является функцией безопасности, CORS ослабляет безопасность.</span><span class="sxs-lookup"><span data-stu-id="7c448-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="7c448-114">Интерфейс API не обеспечивает безопасную поддержку CORS.</span><span class="sxs-lookup"><span data-stu-id="7c448-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="7c448-115">Дополнительные сведения см. в разделе [как работает CORS](#how-cors).</span><span class="sxs-lookup"><span data-stu-id="7c448-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="7c448-116">Позволяет серверу явно разрешать некоторые запросы между источниками и отклонять другие.</span><span class="sxs-lookup"><span data-stu-id="7c448-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="7c448-117">Является более безопасным и более гибким, чем более ранние методики, например [JSONP](/dotnet/framework/wcf/samples/jsonp).</span><span class="sxs-lookup"><span data-stu-id="7c448-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="7c448-118">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c448-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="7c448-119">Тот же источник</span><span class="sxs-lookup"><span data-stu-id="7c448-119">Same origin</span></span>

<span data-ttu-id="7c448-120">Два URL-адреса имеют одинаковый источник, если они имеют идентичные схемы, узлы и порты ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span><span class="sxs-lookup"><span data-stu-id="7c448-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="7c448-121">Эти два URL-адреса имеют один и тот же источник:</span><span class="sxs-lookup"><span data-stu-id="7c448-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="7c448-122">Эти URL-адреса имеют разные источники, чем предыдущие два URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="7c448-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="7c448-123">`https://example.net`&ndash; Другой домен</span><span class="sxs-lookup"><span data-stu-id="7c448-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="7c448-124">`https://www.example.com/foo.html`&ndash; Другой поддомен</span><span class="sxs-lookup"><span data-stu-id="7c448-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="7c448-125">`http://example.com/foo.html`&ndash; Другая схема</span><span class="sxs-lookup"><span data-stu-id="7c448-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="7c448-126">`https://example.com:9000/foo.html`&ndash; Другой порт</span><span class="sxs-lookup"><span data-stu-id="7c448-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="7c448-127">При сравнении источников Internet Explorer не учитывает порт.</span><span class="sxs-lookup"><span data-stu-id="7c448-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="7c448-128">CORS с именованной политикой и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="7c448-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="7c448-129">По промежуточного слоя CORS обрабатывает запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="7c448-130">Следующий код включает CORS для всего приложения с указанным источником:</span><span class="sxs-lookup"><span data-stu-id="7c448-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="7c448-131">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="7c448-131">The preceding code:</span></span>

* <span data-ttu-id="7c448-132">Задает имя политики "\_мялловспеЦификоригинс".</span><span class="sxs-lookup"><span data-stu-id="7c448-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="7c448-133">Имя политики является произвольным.</span><span class="sxs-lookup"><span data-stu-id="7c448-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="7c448-134">Вызывает метод <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> расширения, который включает CORS.</span><span class="sxs-lookup"><span data-stu-id="7c448-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="7c448-135">Вызывает <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [лямбда-выражение](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="7c448-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="7c448-136">Лямбда-выражение принимает <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> объект.</span><span class="sxs-lookup"><span data-stu-id="7c448-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="7c448-137">[Параметры конфигурации](#cors-policy-options), такие как `WithOrigins`, описаны далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="7c448-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="7c448-138">Вызов <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> метода добавляет службы CORS в контейнер службы приложения:</span><span class="sxs-lookup"><span data-stu-id="7c448-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="7c448-139">Дополнительные сведения см. в разделе [Параметры политики CORS](#cpo) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="7c448-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="7c448-140">Метод <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> может привести к цепочке методов, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="7c448-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="7c448-141">Примечание. URL-адрес **не** должен содержать косую черту`/`().</span><span class="sxs-lookup"><span data-stu-id="7c448-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="7c448-142">Если URL-адрес завершается `/`с, то сравнение `false` возвращает значение и заголовок не возвращается.</span><span class="sxs-lookup"><span data-stu-id="7c448-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7c448-143">Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:</span><span class="sxs-lookup"><span data-stu-id="7c448-143">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="7c448-144">При маршрутизации конечных точек по промежуточного слоя CORS должно быть настроено для выполнения `UseRouting` между `UseEndpoints`вызовами функций и.</span><span class="sxs-lookup"><span data-stu-id="7c448-144">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="7c448-145">Неправильная конфигурация приведет к тому, что по промежуточного слоя будет работать правильно.</span><span class="sxs-lookup"><span data-stu-id="7c448-145">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="7c448-146">Следующий код применяет политики CORS ко всем конечным точкам приложений через по промежуточного слоя CORS:</span><span class="sxs-lookup"><span data-stu-id="7c448-146">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="7c448-147">Примечание. `UseCors` необходимо вызвать до `UseMvc`.</span><span class="sxs-lookup"><span data-stu-id="7c448-147">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="7c448-148">См. раздел [Включение CORS в Razor Pages, контроллерах и методах действий](#ecors) для применения политики CORS на уровне страницы, контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="7c448-148">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="7c448-149">Инструкции по тестированию приведенного выше кода см. в разделе [тестирование CORS](#test) .</span><span class="sxs-lookup"><span data-stu-id="7c448-149">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="7c448-150">Включение CORS с маршрутизацией конечных точек</span><span class="sxs-lookup"><span data-stu-id="7c448-150">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="7c448-151">При маршрутизации конечных точек CORS можно включить для каждой конечной точки с помощью `RequireCors` набора методов расширения.</span><span class="sxs-lookup"><span data-stu-id="7c448-151">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="7c448-152">Кроме того, CORS можно также включить для всех контроллеров:</span><span class="sxs-lookup"><span data-stu-id="7c448-152">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="7c448-153">Включение CORS с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="7c448-153">Enable CORS with attributes</span></span>

<span data-ttu-id="7c448-154">Атрибут [EnableCors&rbrack;предоставляет альтернативу глобальному применению CORS. &lbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="7c448-154">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="7c448-155">`[EnableCors]` Атрибут включает CORS для выбранных конечных точек, а не всех конечных точек.</span><span class="sxs-lookup"><span data-stu-id="7c448-155">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="7c448-156">Используйте `[EnableCors]` , чтобы указать политику по умолчанию и `[EnableCors("{Policy String}")]` указать политику.</span><span class="sxs-lookup"><span data-stu-id="7c448-156">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="7c448-157">`[EnableCors]` Атрибут может быть применен к:</span><span class="sxs-lookup"><span data-stu-id="7c448-157">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="7c448-158">Страница Razor`PageModel`</span><span class="sxs-lookup"><span data-stu-id="7c448-158">Razor Page `PageModel`</span></span>
* <span data-ttu-id="7c448-159">Контроллер</span><span class="sxs-lookup"><span data-stu-id="7c448-159">Controller</span></span>
* <span data-ttu-id="7c448-160">Метод действия контроллера</span><span class="sxs-lookup"><span data-stu-id="7c448-160">Controller action method</span></span>

<span data-ttu-id="7c448-161">С `[EnableCors]` атрибутом можно применить различные политики для контроллера, модели страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="7c448-161">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="7c448-162">`[EnableCors]` Если атрибут применяется к контроллеру, модели страницы или методу действия, а CORS включен по промежуточного слоя, применяются обе политики.</span><span class="sxs-lookup"><span data-stu-id="7c448-162">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="7c448-163">Мы советуем использовать сочетание политик.</span><span class="sxs-lookup"><span data-stu-id="7c448-163">We recommend against combining policies.</span></span> <span data-ttu-id="7c448-164">Используйте атрибут `[EnableCors]` или по промежуточного слоя, а не оба в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="7c448-164">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="7c448-165">Следующий код применяет к каждому методу другую политику:</span><span class="sxs-lookup"><span data-stu-id="7c448-165">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="7c448-166">Следующий код создает политику CORS по умолчанию и политику с именем `"AnotherPolicy"`:</span><span class="sxs-lookup"><span data-stu-id="7c448-166">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="7c448-167">Отключение CORS</span><span class="sxs-lookup"><span data-stu-id="7c448-167">Disable CORS</span></span>

<span data-ttu-id="7c448-168">Атрибут [дисаблекорс&rbrack;отключает CORS для контроллера, модели страницы или действия. &lbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="7c448-168">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="7c448-169">Параметры политики CORS</span><span class="sxs-lookup"><span data-stu-id="7c448-169">CORS policy options</span></span>

<span data-ttu-id="7c448-170">В этом разделе описаны различные параметры, которые можно задать в политике CORS:</span><span class="sxs-lookup"><span data-stu-id="7c448-170">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="7c448-171">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="7c448-171">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="7c448-172">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="7c448-172">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="7c448-173">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="7c448-173">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="7c448-174">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="7c448-174">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="7c448-175">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="7c448-175">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="7c448-176">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="7c448-176">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="7c448-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>метод вызывается `Startup.ConfigureServices`в.</span><span class="sxs-lookup"><span data-stu-id="7c448-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7c448-178">Для некоторых параметров может оказаться полезным сначала ознакомиться с разделом " [работа CORS](#how-cors) ".</span><span class="sxs-lookup"><span data-stu-id="7c448-178">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="7c448-179">Установка разрешенных источников</span><span class="sxs-lookup"><span data-stu-id="7c448-179">Set the allowed origins</span></span>

<span data-ttu-id="7c448-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>Разрешает запросы CORS из всех источников с любой схемой (`http` или `https`). &ndash;</span><span class="sxs-lookup"><span data-stu-id="7c448-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="7c448-181">`AllowAnyOrigin`не является безопасным, так как *любой веб-сайт* может выполнять запросы между источниками в приложение.</span><span class="sxs-lookup"><span data-stu-id="7c448-181">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="7c448-182">Указание `AllowAnyOrigin` и`AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="7c448-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7c448-183">Служба CORS возвращает недопустимый ответ CORS, если приложение настроено с обоими методами.</span><span class="sxs-lookup"><span data-stu-id="7c448-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="7c448-184">Указание `AllowAnyOrigin` и`AllowCredentials` является небезопасной конфигурацией и может привести к подделке межсайтовых запросов.</span><span class="sxs-lookup"><span data-stu-id="7c448-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7c448-185">Для безопасного приложения укажите точный список источников, если клиент должен авторизоваться для доступа к ресурсам сервера.</span><span class="sxs-lookup"><span data-stu-id="7c448-185">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="7c448-186">`AllowAnyOrigin`влияет на предпечатные запросы `Access-Control-Allow-Origin` и заголовок.</span><span class="sxs-lookup"><span data-stu-id="7c448-186">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="7c448-187">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="7c448-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7c448-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash; Задает свойство политики как функцию, которая позволяет источникам соответствовать настроенному домену с подстановочными знаками при оценке того, разрешен ли источник. <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*></span><span class="sxs-lookup"><span data-stu-id="7c448-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="7c448-189">Задание допустимых методов HTTP</span><span class="sxs-lookup"><span data-stu-id="7c448-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="7c448-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>.</span><span class="sxs-lookup"><span data-stu-id="7c448-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="7c448-191">Разрешает любой метод HTTP:</span><span class="sxs-lookup"><span data-stu-id="7c448-191">Allows any HTTP method:</span></span>
* <span data-ttu-id="7c448-192">Влияет на предпечатные запросы `Access-Control-Allow-Methods` и заголовок.</span><span class="sxs-lookup"><span data-stu-id="7c448-192">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="7c448-193">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="7c448-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="7c448-194">Задание разрешенных заголовков запроса</span><span class="sxs-lookup"><span data-stu-id="7c448-194">Set the allowed request headers</span></span>

<span data-ttu-id="7c448-195">Чтобы разрешить отправку конкретных заголовков в запрос CORS, именуемый *заголовком запроса на создание*, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> и укажите разрешенные заголовки:</span><span class="sxs-lookup"><span data-stu-id="7c448-195">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7c448-196">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="7c448-196">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7c448-197">Этот параметр влияет на предпечатные запросы `Access-Control-Request-Headers` и заголовок.</span><span class="sxs-lookup"><span data-stu-id="7c448-197">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="7c448-198">Дополнительные сведения см. в разделе [предпечатные запросы](#preflight-requests) .</span><span class="sxs-lookup"><span data-stu-id="7c448-198">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7c448-199">Политика по промежуточного слоя CORS соответствует определенным заголовкам `WithHeaders` , указанным в параметре, только если `Access-Control-Request-Headers` отправляемые заголовки в точности совпадают `WithHeaders`с заголовками, указанными в.</span><span class="sxs-lookup"><span data-stu-id="7c448-199">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="7c448-200">Например, рассмотрим приложение, настроенное следующим образом:</span><span class="sxs-lookup"><span data-stu-id="7c448-200">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7c448-201">По промежуточного слоя CORS отклоняет Предпечатный запрос со следующим заголовком запроса, так как `Content-Language` ([хеадернамес. контентлангуаже](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) нет в списке в: `WithHeaders`</span><span class="sxs-lookup"><span data-stu-id="7c448-201">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="7c448-202">Приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно.</span><span class="sxs-lookup"><span data-stu-id="7c448-202">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7c448-203">Поэтому браузер не пытается выполнить запрос между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-203">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7c448-204">По промежуточного слоя CORS всегда позволяет отправлять `Access-Control-Request-Headers` четыре заголовка в, независимо от значений, настроенных в корсполици. Headers.</span><span class="sxs-lookup"><span data-stu-id="7c448-204">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="7c448-205">Этот список заголовков включает:</span><span class="sxs-lookup"><span data-stu-id="7c448-205">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="7c448-206">Например, рассмотрим приложение, настроенное следующим образом:</span><span class="sxs-lookup"><span data-stu-id="7c448-206">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7c448-207">По промежуточного слоя CORS успешно реагирует на предварительный запрос со следующим заголовком `Content-Language` запроса, поскольку всегда имеет значение список разрешений:</span><span class="sxs-lookup"><span data-stu-id="7c448-207">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="7c448-208">Задание предоставленных заголовков ответа</span><span class="sxs-lookup"><span data-stu-id="7c448-208">Set the exposed response headers</span></span>

<span data-ttu-id="7c448-209">По умолчанию браузер не предоставляет все заголовки ответа приложению.</span><span class="sxs-lookup"><span data-stu-id="7c448-209">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="7c448-210">Дополнительные сведения см. в [разделе Общий доступ к ресурсам между источниками W3C (терминология). Простой заголовок](https://www.w3.org/TR/cors/#simple-response-header)ответа.</span><span class="sxs-lookup"><span data-stu-id="7c448-210">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="7c448-211">По умолчанию доступны заголовки ответов:</span><span class="sxs-lookup"><span data-stu-id="7c448-211">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="7c448-212">Спецификация CORS вызывает эти заголовки *простых заголовков ответа*.</span><span class="sxs-lookup"><span data-stu-id="7c448-212">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="7c448-213">Чтобы сделать другие заголовки доступными для приложения, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="7c448-213">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="7c448-214">Учетные данные в запросах между источниками</span><span class="sxs-lookup"><span data-stu-id="7c448-214">Credentials in cross-origin requests</span></span>

<span data-ttu-id="7c448-215">Учетные данные требует специальной обработки в запросе CORS.</span><span class="sxs-lookup"><span data-stu-id="7c448-215">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="7c448-216">По умолчанию браузер не отправляет учетные данные с запросом между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-216">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="7c448-217">Учетные данные включают файлы cookie и схемы проверки подлинности HTTP.</span><span class="sxs-lookup"><span data-stu-id="7c448-217">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="7c448-218">Чтобы отправить учетные данные с запросом между источниками, клиент должен `XMLHttpRequest.withCredentials` установить `true`в значение.</span><span class="sxs-lookup"><span data-stu-id="7c448-218">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="7c448-219">Использование `XMLHttpRequest` напрямую:</span><span class="sxs-lookup"><span data-stu-id="7c448-219">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="7c448-220">Использование jQuery:</span><span class="sxs-lookup"><span data-stu-id="7c448-220">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="7c448-221">Использование [API выборки](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="7c448-221">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="7c448-222">Сервер должен разрешать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="7c448-222">The server must allow the credentials.</span></span> <span data-ttu-id="7c448-223">Чтобы разрешить учетные данные для разных источников <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>, вызовите:</span><span class="sxs-lookup"><span data-stu-id="7c448-223">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="7c448-224">HTTP-ответ содержит `Access-Control-Allow-Credentials` заголовок, который сообщает браузеру, что сервер разрешает учетные данные для запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-224">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="7c448-225">Если браузер отправляет учетные данные, но ответ не включает допустимый `Access-Control-Allow-Credentials` заголовок, браузер не предоставляет ответ на приложение, и запрос на перекрестное происхождение завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="7c448-225">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="7c448-226">Разрешение учетных данных между источниками является угрозой безопасности.</span><span class="sxs-lookup"><span data-stu-id="7c448-226">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="7c448-227">Веб-сайт в другом домене может отправить учетные данные пользователя, выполнившего вход, в приложение от имени пользователя без ведома пользователя.</span><span class="sxs-lookup"><span data-stu-id="7c448-227">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="7c448-228">В спецификации CORS также указывается, что `"*"` `Access-Control-Allow-Credentials` при наличии заголовка недопустимыми являются исходные значения (все источники).</span><span class="sxs-lookup"><span data-stu-id="7c448-228">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="7c448-229">Предпечатные запросы</span><span class="sxs-lookup"><span data-stu-id="7c448-229">Preflight requests</span></span>

<span data-ttu-id="7c448-230">Для некоторых запросов CORS браузер отправляет дополнительный запрос перед выполнением фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="7c448-230">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="7c448-231">Этот запрос называется *предпечатным запросом*.</span><span class="sxs-lookup"><span data-stu-id="7c448-231">This request is called a *preflight request*.</span></span> <span data-ttu-id="7c448-232">Браузер может пропустить Предпечатный запрос, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="7c448-232">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="7c448-233">Метод запроса — GET, HEAD или POST.</span><span class="sxs-lookup"><span data-stu-id="7c448-233">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="7c448-234">Приложение не устанавливает заголовки `Accept`запроса, кроме, `Accept-Language`, `Content-Language`, `Content-Type`или `Last-Event-ID`.</span><span class="sxs-lookup"><span data-stu-id="7c448-234">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="7c448-235">`Content-Type` Заголовок, если он задан, имеет одно из следующих значений:</span><span class="sxs-lookup"><span data-stu-id="7c448-235">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="7c448-236">Правило для заголовков запросов, заданных для запроса клиента, применяется к заголовкам, которые `setRequestHeader` устанавливаются `XMLHttpRequest` приложением путем вызова для объекта.</span><span class="sxs-lookup"><span data-stu-id="7c448-236">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="7c448-237">Спецификация CORS вызывает заголовки *запроса автора*заголовков.</span><span class="sxs-lookup"><span data-stu-id="7c448-237">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="7c448-238">Правило не применяется к заголовкам, которые может задать браузер, например `User-Agent`, `Host`или `Content-Length`.</span><span class="sxs-lookup"><span data-stu-id="7c448-238">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="7c448-239">Ниже приведен пример предпечатного запроса.</span><span class="sxs-lookup"><span data-stu-id="7c448-239">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="7c448-240">Запрос перед рейсом использует метод HTTP OPTIONS.</span><span class="sxs-lookup"><span data-stu-id="7c448-240">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="7c448-241">Он включает два специальных заголовка:</span><span class="sxs-lookup"><span data-stu-id="7c448-241">It includes two special headers:</span></span>

* <span data-ttu-id="7c448-242">`Access-Control-Request-Method`. Метод HTTP, который будет использоваться для фактического запроса.</span><span class="sxs-lookup"><span data-stu-id="7c448-242">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="7c448-243">`Access-Control-Request-Headers`. Список заголовков запросов, которые приложение устанавливает на фактическом запросе.</span><span class="sxs-lookup"><span data-stu-id="7c448-243">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="7c448-244">Как упоминалось ранее, в него не входят заголовки, заданных браузером `User-Agent`, например.</span><span class="sxs-lookup"><span data-stu-id="7c448-244">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="7c448-245">Предпечатный запрос CORS может включать `Access-Control-Request-Headers` заголовок, указывающий серверу на заголовки, отправляемые с фактическим запросом.</span><span class="sxs-lookup"><span data-stu-id="7c448-245">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="7c448-246">Чтобы разрешить определенные заголовки, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="7c448-246">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7c448-247">Чтобы разрешить все заголовки запроса автора, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="7c448-247">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7c448-248">Браузеры не полностью согласуются с тем `Access-Control-Request-Headers`, как они заданы.</span><span class="sxs-lookup"><span data-stu-id="7c448-248">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="7c448-249">Если для заголовков задано значение `"*"` , отличное от <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>(или использование), следует включить `Accept`как `Content-Type`минимум, `Origin`, и, а также любые настраиваемые заголовки, которые требуется поддерживать.</span><span class="sxs-lookup"><span data-stu-id="7c448-249">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="7c448-250">Ниже приведен пример ответа на Предпечатный запрос (при условии, что сервер разрешает запрос):</span><span class="sxs-lookup"><span data-stu-id="7c448-250">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="7c448-251">Ответ содержит `Access-Control-Allow-Methods` заголовок со списком допустимых методов и, при необходимости `Access-Control-Allow-Headers` , заголовок, в котором перечислены разрешенные заголовки.</span><span class="sxs-lookup"><span data-stu-id="7c448-251">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="7c448-252">Если Предпечатный запрос выполнен, браузер отправляет фактический запрос.</span><span class="sxs-lookup"><span data-stu-id="7c448-252">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="7c448-253">Если Предпечатный запрос отклонен, приложение возвращает ответ *ок 200* , но не ОТПРАВЛЯЕТ заголовки CORS обратно.</span><span class="sxs-lookup"><span data-stu-id="7c448-253">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7c448-254">Поэтому браузер не пытается выполнить запрос между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-254">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="7c448-255">Задать срок действия предпечатного срока</span><span class="sxs-lookup"><span data-stu-id="7c448-255">Set the preflight expiration time</span></span>

<span data-ttu-id="7c448-256">`Access-Control-Max-Age` Заголовок указывает время, в течение которого может быть кэширован ответ на Предпечатный запрос.</span><span class="sxs-lookup"><span data-stu-id="7c448-256">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="7c448-257">Чтобы задать этот заголовок, вызовите <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span><span class="sxs-lookup"><span data-stu-id="7c448-257">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="7c448-258">Принцип работы CORS</span><span class="sxs-lookup"><span data-stu-id="7c448-258">How CORS works</span></span>

<span data-ttu-id="7c448-259">В этом разделе описывается, что происходит в запросе [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) на уровне HTTP-сообщений.</span><span class="sxs-lookup"><span data-stu-id="7c448-259">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="7c448-260">CORS **не** является функцией безопасности.</span><span class="sxs-lookup"><span data-stu-id="7c448-260">CORS is **not** a security feature.</span></span> <span data-ttu-id="7c448-261">CORS — это стандарт консорциума W3C, позволяющий серверу ослабить политику того же источника.</span><span class="sxs-lookup"><span data-stu-id="7c448-261">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="7c448-262">Например, вредоносный субъект может использовать для веб [-узла предотвращение межсайтовых сценариев (XSS)](xref:security/cross-site-scripting) и выполнить межсайтовый запрос к сайту с поддержкой CORS, чтобы украсть информацию.</span><span class="sxs-lookup"><span data-stu-id="7c448-262">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="7c448-263">Интерфейс API не обеспечивает безопасную работу, разрешая CORS.</span><span class="sxs-lookup"><span data-stu-id="7c448-263">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="7c448-264">Для применения CORS требуется клиент (браузер).</span><span class="sxs-lookup"><span data-stu-id="7c448-264">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="7c448-265">Сервер выполняет запрос и возвращает ответ. это клиент, который возвращает сообщение об ошибке и блокирует ответ.</span><span class="sxs-lookup"><span data-stu-id="7c448-265">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="7c448-266">Например, в любом из следующих средств будет отображаться ответ сервера:</span><span class="sxs-lookup"><span data-stu-id="7c448-266">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="7c448-267">Fiddler</span><span class="sxs-lookup"><span data-stu-id="7c448-267">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="7c448-268">Postman</span><span class="sxs-lookup"><span data-stu-id="7c448-268">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="7c448-269">HttpClient .NET</span><span class="sxs-lookup"><span data-stu-id="7c448-269">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="7c448-270">Веб-браузер, введя URL-адрес в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="7c448-270">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="7c448-271">Сервер может позволить обозревателям выполнять запросы API [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) или [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) , которые в противном случае будут запрещены.</span><span class="sxs-lookup"><span data-stu-id="7c448-271">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="7c448-272">Браузеры (без CORS) не могут выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-272">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="7c448-273">Перед CORS использовалась технология [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) для обхода этого ограничения.</span><span class="sxs-lookup"><span data-stu-id="7c448-273">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="7c448-274">JSONP не использует XHR, он использует `<script>` тег для получения ответа.</span><span class="sxs-lookup"><span data-stu-id="7c448-274">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="7c448-275">Скрипты могут загружаться в разных источниках.</span><span class="sxs-lookup"><span data-stu-id="7c448-275">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="7c448-276">В [спецификации CORS](https://www.w3.org/TR/cors/) появились несколько новых HTTP-заголовков, которые позволяют выполнять запросы между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-276">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="7c448-277">Если браузер поддерживает CORS, эти заголовки автоматически устанавливаются для запросов между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-277">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="7c448-278">Для включения CORS не требуется пользовательский код JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c448-278">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="7c448-279">Ниже приведен пример запроса между источниками.</span><span class="sxs-lookup"><span data-stu-id="7c448-279">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="7c448-280">`Origin` Заголовок предоставляет домен сайта, выполняющего запрос:</span><span class="sxs-lookup"><span data-stu-id="7c448-280">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="7c448-281">Если сервер разрешает запрос, он устанавливает `Access-Control-Allow-Origin` заголовок в ответе.</span><span class="sxs-lookup"><span data-stu-id="7c448-281">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="7c448-282">Значение этого заголовка либо соответствует `Origin` заголовку запроса, либо является подстановочным значением `"*"`, что означает, что любой источник разрешен:</span><span class="sxs-lookup"><span data-stu-id="7c448-282">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="7c448-283">Если ответ не содержит `Access-Control-Allow-Origin` заголовок, запрос на перекрестное происхождение завершается с ошибкой.</span><span class="sxs-lookup"><span data-stu-id="7c448-283">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="7c448-284">В частности, браузер не разрешает запрос.</span><span class="sxs-lookup"><span data-stu-id="7c448-284">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="7c448-285">Даже если сервер возвращает успешный ответ, браузер не делает ответ доступным клиентскому приложению.</span><span class="sxs-lookup"><span data-stu-id="7c448-285">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="7c448-286">Тестирование CORS</span><span class="sxs-lookup"><span data-stu-id="7c448-286">Test CORS</span></span>

<span data-ttu-id="7c448-287">Тестирование CORS:</span><span class="sxs-lookup"><span data-stu-id="7c448-287">To test CORS:</span></span>

1. <span data-ttu-id="7c448-288">[Создайте проект API](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="7c448-288">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="7c448-289">Кроме того, можно [загрузить пример](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="7c448-289">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="7c448-290">Включите CORS с помощью одного из подходов, описанных в этом документе.</span><span class="sxs-lookup"><span data-stu-id="7c448-290">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="7c448-291">Например:</span><span class="sxs-lookup"><span data-stu-id="7c448-291">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="7c448-292">`WithOrigins("https://localhost:<port>");`следует использовать только для тестирования примера приложения, аналогичного [образцу кода для скачивания](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span><span class="sxs-lookup"><span data-stu-id="7c448-292">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="7c448-293">Создайте проект веб-приложения (Razor Pages или MVC).</span><span class="sxs-lookup"><span data-stu-id="7c448-293">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="7c448-294">В примере используется Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7c448-294">The sample uses Razor Pages.</span></span> <span data-ttu-id="7c448-295">Вы можете создать веб-приложение в том же решении, что и проект API.</span><span class="sxs-lookup"><span data-stu-id="7c448-295">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="7c448-296">Добавьте выделенный ниже код в файл *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c448-296">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="7c448-297">В приведенном выше коде замените `url: 'https://<web app>.azurewebsites.net/api/values/1',` URL-адресом развернутого приложения.</span><span class="sxs-lookup"><span data-stu-id="7c448-297">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="7c448-298">Разверните проект API.</span><span class="sxs-lookup"><span data-stu-id="7c448-298">Deploy the API project.</span></span> <span data-ttu-id="7c448-299">Например, выполните [развертывание в Azure](xref:host-and-deploy/azure-apps/index).</span><span class="sxs-lookup"><span data-stu-id="7c448-299">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="7c448-300">Запустите приложение Razor Pages или MVC на рабочем столе и нажмите кнопку **Test (тест** ).</span><span class="sxs-lookup"><span data-stu-id="7c448-300">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="7c448-301">Используйте средства F12 для просмотра сообщений об ошибках.</span><span class="sxs-lookup"><span data-stu-id="7c448-301">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="7c448-302">Удалите источник localhost из `WithOrigins` и разверните приложение.</span><span class="sxs-lookup"><span data-stu-id="7c448-302">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="7c448-303">Кроме того, можно запустить клиентское приложение с другим портом.</span><span class="sxs-lookup"><span data-stu-id="7c448-303">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="7c448-304">Например, запустите из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7c448-304">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="7c448-305">Протестируйте клиентское приложение.</span><span class="sxs-lookup"><span data-stu-id="7c448-305">Test with the client app.</span></span> <span data-ttu-id="7c448-306">Ошибки CORS возвращают ошибку, но сообщение об ошибке недоступно для JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c448-306">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="7c448-307">Чтобы просмотреть ошибку, используйте вкладку консоль в средствах F12.</span><span class="sxs-lookup"><span data-stu-id="7c448-307">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="7c448-308">В зависимости от браузера вы получаете ошибку (в консоли средств F12), как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="7c448-308">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="7c448-309">Использование Microsoft ребр:</span><span class="sxs-lookup"><span data-stu-id="7c448-309">Using Microsoft Edge:</span></span>

     <span data-ttu-id="7c448-310">**SEC7120: [CORS] не удалось `https://localhost:44375` найти `https://localhost:44375` источник в заголовке ответа Access-Control-Allow-Origin для ресурса-источника в`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="7c448-310">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="7c448-311">Использование Chrome:</span><span class="sxs-lookup"><span data-stu-id="7c448-311">Using Chrome:</span></span>

     <span data-ttu-id="7c448-312">**Доступ к XMLHttpRequest `https://webapi.azurewebsites.net/api/values/1` из источника `https://localhost:44375` заблокирован политикой CORS: В запрошенном ресурсе отсутствует заголовок "Access-Control-Allow-Origin".**</span><span class="sxs-lookup"><span data-stu-id="7c448-312">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c448-313">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7c448-313">Additional resources</span></span>

* [<span data-ttu-id="7c448-314">Общий доступ к ресурсам между источниками (CORS)</span><span class="sxs-lookup"><span data-stu-id="7c448-314">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
