---
title: Кэширование ответа по промежуточного слоя в ASP.NET Core
author: guardrex
description: Узнайте, как настроить и использовать ПО промежуточного слоя для кэширование ответов в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: performance/caching/middleware
ms.openlocfilehash: 6371f42b100f70c6042064a6372c7b9e41fd5c73
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914994"
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="4f1b9-103">Кэширование ответа по промежуточного слоя в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f1b9-103">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="4f1b9-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Джон Луо](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="4f1b9-104">By [Luke Latham](https://github.com/guardrex) and [John Luo](https://github.com/JunTaoLuo)</span></span>

<span data-ttu-id="4f1b9-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4f1b9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4f1b9-106">В этой статье объясняется, как настроить по промежуточного слоя кэширования ответов в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-106">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="4f1b9-107">По промежуточного слоя определяет, когда ответы кэшируются, сохраняет ответы и обслуживает ответы из кэша.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-107">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="4f1b9-108">Общие сведения о кэшировании HTTP и атрибуте [[респонсекаче]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) см. в разделе [кэширование ответов](xref:performance/caching/response).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-108">For an introduction to HTTP caching and the [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

## <a name="configuration"></a><span data-ttu-id="4f1b9-109">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="4f1b9-109">Configuration</span></span>

<span data-ttu-id="4f1b9-110">Используйте [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. AspNetCore. респонсекачинг](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) .</span><span class="sxs-lookup"><span data-stu-id="4f1b9-110">Use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

<span data-ttu-id="4f1b9-111">В `Startup.ConfigureServices`добавьте по промежуточного слоя кэширования ответа в коллекцию служб:</span><span class="sxs-lookup"><span data-stu-id="4f1b9-111">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

<span data-ttu-id="4f1b9-112">Настройте приложение для использования по промежуточного слоя с <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> помощью метода расширения, который добавляет по промежуточного слоя в конвейер обработки `Startup.Configure`запросов в:</span><span class="sxs-lookup"><span data-stu-id="4f1b9-112">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

::: moniker-end

<span data-ttu-id="4f1b9-113">Пример приложения добавляет заголовки для управления кэшированием последующих запросов:</span><span class="sxs-lookup"><span data-stu-id="4f1b9-113">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="4f1b9-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Кэширует ответы в кэше до 10 секунд.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="4f1b9-115">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) Настраивает по промежуточного слоя для обслуживания кэшированного ответа только [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) в том случае, если заголовок последующих запросов совпадает с заголовком исходного запроса. &ndash;</span><span class="sxs-lookup"><span data-stu-id="4f1b9-115">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Configures the middleware to serve a cached response only if the [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

::: moniker-end

<span data-ttu-id="4f1b9-116">По промежуточного слоя кэширования ответов кэширует только ответы сервера, приводящие к коду состояния 200 (ОК).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-116">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="4f1b9-117">Любые другие ответы, включая [страницы ошибок](xref:fundamentals/error-handling), по промежуточного слоя игнорируются.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-117">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="4f1b9-118">Ответы, содержащие содержимое для прошедших проверку клиентов, должны быть помечены как недоступные для кэширования, чтобы предотвратить хранение и обслуживание этих ответов по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-118">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="4f1b9-119">Сведения о том, как по промежуточного слоя определяет, является ли ответ кэшированным, см. в разделе [условия кэширования](#conditions-for-caching) .</span><span class="sxs-lookup"><span data-stu-id="4f1b9-119">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="4f1b9-120">Параметры</span><span class="sxs-lookup"><span data-stu-id="4f1b9-120">Options</span></span>

<span data-ttu-id="4f1b9-121">Параметры кэширования ответов приведены в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-121">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="4f1b9-122">Параметр</span><span class="sxs-lookup"><span data-stu-id="4f1b9-122">Option</span></span> | <span data-ttu-id="4f1b9-123">Описание</span><span class="sxs-lookup"><span data-stu-id="4f1b9-123">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="4f1b9-124">Самый крупный размер кэша для текста ответа в байтах.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-124">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="4f1b9-125">Значение по умолчанию `64 * 1024 * 1024` — (64 МБ).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-125">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="4f1b9-126">Предельный размер для по промежуточного слоя кэша ответов в байтах.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-126">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="4f1b9-127">Значение по умолчанию `100 * 1024 * 1024` — (100 МБ).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-127">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="4f1b9-128">Определяет, кэшируются ли ответы в путях с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-128">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="4f1b9-129">Значение по умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-129">The default value is `false`.</span></span> |

<span data-ttu-id="4f1b9-130">В следующем примере по промежуточного слоя настраивается:</span><span class="sxs-lookup"><span data-stu-id="4f1b9-130">The following example configures the middleware to:</span></span>

* <span data-ttu-id="4f1b9-131">Ответы кэша с размером текста меньше или равным 1 024 байт.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-131">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="4f1b9-132">Храните ответы по путям с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-132">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="4f1b9-133">Например, `/page1` и `/Page1` хранятся отдельно.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-133">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="4f1b9-134">варибикуерикэйс</span><span class="sxs-lookup"><span data-stu-id="4f1b9-134">VaryByQueryKeys</span></span>

<span data-ttu-id="4f1b9-135">При использовании контроллеров MVC, веб-API или Razor Pagesных моделей страниц атрибут [[респонсекаче]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) задает параметры, необходимые для установки соответствующих заголовков для кэширования ответов.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-135">When using MVC / web API controllers or Razor Pages page models, the [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="4f1b9-136">Единственный параметр `[ResponseCache]` атрибута, для которого строго требуется по <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>промежуточного слоя, не соответствует фактическому заголовку HTTP.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-136">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="4f1b9-137">Дополнительные сведения см. в разделе <xref:performance/caching/response#responsecache-attribute>.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-137">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="4f1b9-138">Если `[ResponseCache]` атрибут не используется, кэширование ответов можно изменять с помощью `VaryByQueryKeys`.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-138">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="4f1b9-139">Используйте непосредственно из [функции HttpContext. Features:](xref:Microsoft.AspNetCore.Http.HttpContext.Features) <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature></span><span class="sxs-lookup"><span data-stu-id="4f1b9-139">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="4f1b9-140">При использовании одного значения, `*` равного `VaryByQueryKeys` , аргумент Cache изменяется в зависимости от всех параметров запроса запроса.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-140">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="4f1b9-141">Заголовки HTTP, используемые по промежуточного слоя кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="4f1b9-141">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="4f1b9-142">В следующей таблице приведены сведения о заголовках HTTP, влияющих на кэширование ответов.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-142">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="4f1b9-143">Header</span><span class="sxs-lookup"><span data-stu-id="4f1b9-143">Header</span></span> | <span data-ttu-id="4f1b9-144">Подробные сведения</span><span class="sxs-lookup"><span data-stu-id="4f1b9-144">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="4f1b9-145">Ответ не кэшируется, если заголовок существует.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-145">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="4f1b9-146">По промежуточного слоя рассматривает только ответы кэширования, отмеченные `public` директивой Cache.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-146">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="4f1b9-147">Управление кэшированием со следующими параметрами:</span><span class="sxs-lookup"><span data-stu-id="4f1b9-147">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="4f1b9-148">максимальный возраст</span><span class="sxs-lookup"><span data-stu-id="4f1b9-148">max-age</span></span></li><li><span data-ttu-id="4f1b9-149">максимум — устаревший&#8224;</span><span class="sxs-lookup"><span data-stu-id="4f1b9-149">max-stale&#8224;</span></span></li><li><span data-ttu-id="4f1b9-150">min-свежая</span><span class="sxs-lookup"><span data-stu-id="4f1b9-150">min-fresh</span></span></li><li><span data-ttu-id="4f1b9-151">необходимо — повторно проверить</span><span class="sxs-lookup"><span data-stu-id="4f1b9-151">must-revalidate</span></span></li><li><span data-ttu-id="4f1b9-152">без кэша</span><span class="sxs-lookup"><span data-stu-id="4f1b9-152">no-cache</span></span></li><li><span data-ttu-id="4f1b9-153">без магазина</span><span class="sxs-lookup"><span data-stu-id="4f1b9-153">no-store</span></span></li><li><span data-ttu-id="4f1b9-154">только в случае кэширования</span><span class="sxs-lookup"><span data-stu-id="4f1b9-154">only-if-cached</span></span></li><li><span data-ttu-id="4f1b9-155">private</span><span class="sxs-lookup"><span data-stu-id="4f1b9-155">private</span></span></li><li><span data-ttu-id="4f1b9-156">public</span><span class="sxs-lookup"><span data-stu-id="4f1b9-156">public</span></span></li><li><span data-ttu-id="4f1b9-157">s-maxage</span><span class="sxs-lookup"><span data-stu-id="4f1b9-157">s-maxage</span></span></li><li><span data-ttu-id="4f1b9-158">прокси — повторная проверка&#8225;</span><span class="sxs-lookup"><span data-stu-id="4f1b9-158">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="4f1b9-159">&#8224;Если ограничение не задано `max-stale`, по промежуточного слоя не выполняет никаких действий.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-159">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="4f1b9-160">&#8225;`proxy-revalidate`имеет тот же результат, `must-revalidate`что и.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-160">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="4f1b9-161">Дополнительные сведения см [. в RFC 7231: Запрос директив](https://tools.ietf.org/html/rfc7234#section-5.2.1)управления Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-161">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="4f1b9-162">Заголовок в запросе дает тот же результат, что `Cache-Control: no-cache`и. `Pragma: no-cache`</span><span class="sxs-lookup"><span data-stu-id="4f1b9-162">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="4f1b9-163">Этот заголовок переопределяется соответствующими директивами в `Cache-Control` заголовке, если он имеется.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-163">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="4f1b9-164">Рассматривается для обеспечения обратной совместимости с HTTP/1.0.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-164">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="4f1b9-165">Ответ не кэшируется, если заголовок существует.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-165">The response isn't cached if the header exists.</span></span> <span data-ttu-id="4f1b9-166">Любое по промежуточного слоя в конвейере обработки запросов, которое задает один или несколько файлов cookie, предотвращает кэширование ответа по промежуточного слоя (например, [поставщик TempData на основе файлов cookie](xref:fundamentals/app-state#tempdata)).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-166">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="4f1b9-167">`Vary` Заголовок используется для изменения кэшированного ответа другим заголовком.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-167">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="4f1b9-168">Например, ответы кэшируются по кодировке путем включения `Vary: Accept-Encoding` заголовка, который кэширует ответы для запросов с заголовками `Accept-Encoding: gzip` и `Accept-Encoding: text/plain` по отдельности.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-168">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="4f1b9-169">Ответ со значением заголовка объекта `*` никогда не сохраняется.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-169">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="4f1b9-170">Ответ, который считается устаревшим по этому заголовку, не сохраняется или не извлекается, если он не переопределен другими `Cache-Control` заголовками.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-170">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="4f1b9-171">Полный ответ обрабатывается из кэша, если значение не `*` равно, а ответ не совпадает ни с `ETag` одним из указанных значений.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-171">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="4f1b9-172">В противном случае выдается ответ 304 (не изменено).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-172">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="4f1b9-173">`If-None-Match` Если заголовок отсутствует, полный ответ передается из кэша, если значение кэшированной даты ответа новее заданного значения.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-173">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="4f1b9-174">В противном случае обслуживается ответ *304 — не изменено* .</span><span class="sxs-lookup"><span data-stu-id="4f1b9-174">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="4f1b9-175">При обслуживании из кэша `Date` заголовок задается по промежуточного слоя, если оно не было указано в исходном ответе.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-175">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="4f1b9-176">При обслуживании из кэша `Content-Length` заголовок задается по промежуточного слоя, если оно не было указано в исходном ответе.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-176">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="4f1b9-177">`Age` Заголовок, отправленный в исходном ответе, игнорируется.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-177">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="4f1b9-178">По промежуточного слоя выполняет вычисление нового значения при обработке кэшированного ответа.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-178">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="4f1b9-179">Кэширование директив запроса Cache-Control</span><span class="sxs-lookup"><span data-stu-id="4f1b9-179">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="4f1b9-180">По промежуточного слоя учитывает правила [спецификации кэширования HTTP 1,1](https://tools.ietf.org/html/rfc7234#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-180">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="4f1b9-181">Для правил требуется, чтобы кэш учитывал допустимый `Cache-Control` заголовок, отправленный клиентом.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-181">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="4f1b9-182">В соответствии со спецификацией клиент может выполнять запросы со `no-cache` значением заголовка и принудительно создавать новый ответ для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-182">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="4f1b9-183">В настоящее время разработчик не управляет этим поведением кэширования при использовании по промежуточного слоя, поскольку по промежуточного слоя соответствует официальной спецификации кэширования.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-183">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="4f1b9-184">Чтобы получить более полный контроль над поведением кэширования, изучите другие функции кэширования ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-184">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="4f1b9-185">См. указанные ниже разделы.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-185">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="4f1b9-186">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="4f1b9-186">Troubleshooting</span></span>

<span data-ttu-id="4f1b9-187">Если поведение кэширования не так, как ожидалось, убедитесь, что ответы кэшируются и могут обрабатываться из кэша.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-187">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="4f1b9-188">Изучите Входящие заголовки запроса и исходящие заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-188">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="4f1b9-189">Включите [ведение журнала](xref:fundamentals/logging/index) для помощи при отладке.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-189">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="4f1b9-190">При тестировании и устранении неполадок кэширования браузер может задавать заголовки запросов, которые влияют на кэширование нежелательным образом.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-190">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="4f1b9-191">Например, браузер может задать `Cache-Control` для `no-cache` заголовка или `max-age=0` при обновлении страницы.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-191">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="4f1b9-192">Следующие средства могут явно задавать заголовки запроса и являются предпочтительными для тестирования кэширования.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-192">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="4f1b9-193">Fiddler</span><span class="sxs-lookup"><span data-stu-id="4f1b9-193">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="4f1b9-194">Postman</span><span class="sxs-lookup"><span data-stu-id="4f1b9-194">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="4f1b9-195">Условия для кэширования</span><span class="sxs-lookup"><span data-stu-id="4f1b9-195">Conditions for caching</span></span>

* <span data-ttu-id="4f1b9-196">Запрос должен привести к отклику сервера с кодом состояния 200 (ОК).</span><span class="sxs-lookup"><span data-stu-id="4f1b9-196">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="4f1b9-197">Метод запроса должен быть GET или HEAD.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-197">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="4f1b9-198">В `Startup.Configure`по промежуточного слоя, для которого требуется кэширование, должно быть помещено в состояние кэширования ответа.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-198">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="4f1b9-199">Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index>.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-199">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="4f1b9-200">`Authorization` Заголовок не должен присутствовать.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-200">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="4f1b9-201">`Cache-Control`параметры заголовка должны быть допустимыми, и ответ должен быть помечен `public` и не помечен `private`.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-201">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="4f1b9-202">Заголовок не должен присутствовать, `Cache-Control` если заголовок отсутствует, `Pragma` так как `Cache-Control` заголовок переопределяет заголовок при его наличии. `Pragma: no-cache`</span><span class="sxs-lookup"><span data-stu-id="4f1b9-202">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="4f1b9-203">`Set-Cookie` Заголовок не должен присутствовать.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-203">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="4f1b9-204">`Vary`параметры заголовка должны быть допустимыми и не равны `*`.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-204">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="4f1b9-205">Значение `Content-Length` заголовка (если оно задано) должно соответствовать размеру текста ответа.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-205">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="4f1b9-206">Объект <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> не используется.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-206">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="4f1b9-207">Ответ не должен быть устаревшим, как указано в `Expires` заголовках `max-age` и директивах кэша и `s-maxage` .</span><span class="sxs-lookup"><span data-stu-id="4f1b9-207">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="4f1b9-208">Буферизация ответов должна быть успешной.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-208">Response buffering must be successful.</span></span> <span data-ttu-id="4f1b9-209">Размер ответа должен быть меньше заданного значения или по умолчанию <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-209">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="4f1b9-210">Размер текста ответа должен быть меньше заданного значения или по умолчанию <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-210">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="4f1b9-211">Ответ должен быть кэширован согласно спецификациям [RFC 7234](https://tools.ietf.org/html/rfc7234) .</span><span class="sxs-lookup"><span data-stu-id="4f1b9-211">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="4f1b9-212">Например, `no-store` директива не должна существовать в полях заголовка запроса или ответа.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-212">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="4f1b9-213">См *. раздел 3. Хранение ответов в кэшах* [RFC 7234](https://tools.ietf.org/html/rfc7234) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-213">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="4f1b9-214">Система защиты от подделки для создания безопасных маркеров для предотвращения подделки межсайтовых запросов (CSRF) устанавливает `Cache-Control` заголовки и `Pragma` так, чтобы `no-cache` ответы не кэшируются.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-214">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="4f1b9-215">Сведения об отключении маркеров подделки для элементов HTML-форм см. в разделе <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span><span class="sxs-lookup"><span data-stu-id="4f1b9-215">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f1b9-216">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4f1b9-216">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
