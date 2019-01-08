---
title: Кэширование ответов в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать кэширование ответов, чтобы снизить требования к пропускной способности и повысить производительность приложений ASP.NET Core.
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098952"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="5e803-103">Кэширование ответов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e803-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="5e803-104">По [Luo Джон](https://github.com/JunTaoLuo), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5e803-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="5e803-105">Кэширование ответов в Razor Pages в ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="5e803-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="5e803-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5e803-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="5e803-107">Кэширование ответов уменьшает количество запросов, выполненных клиентского приложения или прокси-сервера веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="5e803-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="5e803-108">Кэширование ответов также сокращает время работы веб-сервер выполняет для создания ответа.</span><span class="sxs-lookup"><span data-stu-id="5e803-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="5e803-109">Кэширование ответов управляется заголовки, указывающие способ клиента, прокси-сервера и по промежуточного слоя для кэширования ответов.</span><span class="sxs-lookup"><span data-stu-id="5e803-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="5e803-110">[Атрибута ResponseCache](#responsecache-attribute) участвует в параметр кэширования заголовков, в которых клиенты могут поддерживать при кэшировании ответов ответов.</span><span class="sxs-lookup"><span data-stu-id="5e803-110">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="5e803-111">[По промежуточного слоя для кэширования ответа](xref:performance/caching/middleware) может использоваться для кэширования ответов на сервере.</span><span class="sxs-lookup"><span data-stu-id="5e803-111">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="5e803-112">Можно использовать по промежуточного слоя `ResponseCache` атрибут свойства, которые влияют на поведение кэширования на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="5e803-112">The middleware can use `ResponseCache` attribute properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="5e803-113">Кэширование ответов на основе HTTP</span><span class="sxs-lookup"><span data-stu-id="5e803-113">HTTP-based response caching</span></span>

<span data-ttu-id="5e803-114">[Спецификации кэширования HTTP 1.1](https://tools.ietf.org/html/rfc7234) описано поведение Internet кэшей.</span><span class="sxs-lookup"><span data-stu-id="5e803-114">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="5e803-115">Основной заголовок HTTP, используемый для кэширования [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), который используется для указания кэша *директивы*.</span><span class="sxs-lookup"><span data-stu-id="5e803-115">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="5e803-116">Директивы Управление поведением кэширования запросов все же встречаются несущественные от клиентов к серверам и принимая ответов свой путь с серверов клиентам.</span><span class="sxs-lookup"><span data-stu-id="5e803-116">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="5e803-117">Запросы и ответы перемещения между прокси-серверами и прокси-серверы также должны соответствовать спецификации кэширования HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="5e803-117">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="5e803-118">Распространенные `Cache-Control` директивы показаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5e803-118">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="5e803-119">Директива</span><span class="sxs-lookup"><span data-stu-id="5e803-119">Directive</span></span>                                                       | <span data-ttu-id="5e803-120">Действие</span><span class="sxs-lookup"><span data-stu-id="5e803-120">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="5e803-121">public</span><span class="sxs-lookup"><span data-stu-id="5e803-121">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="5e803-122">Кэш может хранить ответ.</span><span class="sxs-lookup"><span data-stu-id="5e803-122">A cache may store the response.</span></span> |
| [<span data-ttu-id="5e803-123">private</span><span class="sxs-lookup"><span data-stu-id="5e803-123">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="5e803-124">Ответ не должны храниться в общий кэш.</span><span class="sxs-lookup"><span data-stu-id="5e803-124">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="5e803-125">Частный кэш может хранить и повторно использовать ответ.</span><span class="sxs-lookup"><span data-stu-id="5e803-125">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="5e803-126">max-age</span><span class="sxs-lookup"><span data-stu-id="5e803-126">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="5e803-127">Клиент не будет принимать ответ, возраст которых больше, чем указанное число секунд.</span><span class="sxs-lookup"><span data-stu-id="5e803-127">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="5e803-128">Примеры `max-age=60` (60 секунд), `max-age=2592000` (1 месяц)</span><span class="sxs-lookup"><span data-stu-id="5e803-128">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="5e803-129">без кэша</span><span class="sxs-lookup"><span data-stu-id="5e803-129">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="5e803-130">**При запросах**: Кэш не должны использовать хранимые ответ для удовлетворения запроса.</span><span class="sxs-lookup"><span data-stu-id="5e803-130">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="5e803-131">Примечание. На сервере-источнике повторно формирует ответ для клиента, и по промежуточного слоя обновляет ответов в своем кэше.</span><span class="sxs-lookup"><span data-stu-id="5e803-131">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="5e803-132">**При ответах**: Ответ не должны использоваться для последующих запросов без проверки на исходном сервере.</span><span class="sxs-lookup"><span data-stu-id="5e803-132">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="5e803-133">no-store</span><span class="sxs-lookup"><span data-stu-id="5e803-133">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="5e803-134">**При запросах**: Кэш не должен хранить запроса.</span><span class="sxs-lookup"><span data-stu-id="5e803-134">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="5e803-135">**При ответах**: Кэш не должен хранить любую часть ответа.</span><span class="sxs-lookup"><span data-stu-id="5e803-135">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="5e803-136">В следующей таблице показаны другие заголовки кэша, которые влияют на кэширование.</span><span class="sxs-lookup"><span data-stu-id="5e803-136">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="5e803-137">Header</span><span class="sxs-lookup"><span data-stu-id="5e803-137">Header</span></span>                                                     | <span data-ttu-id="5e803-138">Функция</span><span class="sxs-lookup"><span data-stu-id="5e803-138">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="5e803-139">Срок действия</span><span class="sxs-lookup"><span data-stu-id="5e803-139">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="5e803-140">Приблизительное количество времени в секундах с момента ответа сформирован или создан успешно прошли проверку на исходном сервере.</span><span class="sxs-lookup"><span data-stu-id="5e803-140">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="5e803-141">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="5e803-141">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="5e803-142">Дата и время, после чего ответ считается устаревшей.</span><span class="sxs-lookup"><span data-stu-id="5e803-142">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="5e803-143">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="5e803-143">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="5e803-144">Существует для обеспечения обратной совместимости с HTTP/1.0 кэширует параметра `no-cache` поведение.</span><span class="sxs-lookup"><span data-stu-id="5e803-144">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="5e803-145">Если `Cache-Control` заголовок присутствует, `Pragma` заголовка учитывается.</span><span class="sxs-lookup"><span data-stu-id="5e803-145">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="5e803-146">Различаются</span><span class="sxs-lookup"><span data-stu-id="5e803-146">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="5e803-147">Указывает, что кэшированный ответ должен быть отправляется, только если все из `Vary` заголовок поля согласуются в кэшированный ответ исходного запроса и новый запрос.</span><span class="sxs-lookup"><span data-stu-id="5e803-147">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="5e803-148">Отношениях кэширования на основе HTTP запроса директивы Cache-Control</span><span class="sxs-lookup"><span data-stu-id="5e803-148">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="5e803-149">[Спецификации кэширования HTTP 1.1 для заголовка Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) требует кэша соблюдать является допустимым для `Cache-Control` заголовка, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="5e803-149">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="5e803-150">Клиент может выполнять запросы с `no-cache` значение заголовка и принудительного создания нового ответа для каждого запроса сервером.</span><span class="sxs-lookup"><span data-stu-id="5e803-150">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="5e803-151">Всегда учитывает клиента `Cache-Control` заголовки запроса смысл, если вы считаете цель HTTP-кэширования.</span><span class="sxs-lookup"><span data-stu-id="5e803-151">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="5e803-152">В официальной спецификации кэширование призвана сократить расходы на задержку и сети для удовлетворения запросов по сети, прокси-серверов, серверов и клиентов.</span><span class="sxs-lookup"><span data-stu-id="5e803-152">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="5e803-153">Это не обязательно для контроля нагрузки на сервер-источник.</span><span class="sxs-lookup"><span data-stu-id="5e803-153">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="5e803-154">Отсутствует контроль разработчика над это поведение кэширования при использовании [по промежуточного слоя для кэширования ответа](xref:performance/caching/middleware) так, как по промежуточного слоя, соответствует официальной спецификации кэширования.</span><span class="sxs-lookup"><span data-stu-id="5e803-154">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="5e803-155">[Запланированный усовершенствования в по промежуточного слоя](https://github.com/aspnet/AspNetCore/issues/2612) – это возможность для настройки по промежуточного слоя, чтобы игнорировать запроса `Cache-Control` заголовка при принятии решения о обслуживать кэшированный ответ.</span><span class="sxs-lookup"><span data-stu-id="5e803-155">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="5e803-156">Плановое улучшения предоставляют возможность лучше нагрузка на сервер управления.</span><span class="sxs-lookup"><span data-stu-id="5e803-156">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="5e803-157">Другие технология кэширования в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5e803-157">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="5e803-158">Кэширование в памяти</span><span class="sxs-lookup"><span data-stu-id="5e803-158">In-memory caching</span></span>

<span data-ttu-id="5e803-159">Кэширование в памяти использует память сервера для хранения кэшированных данных.</span><span class="sxs-lookup"><span data-stu-id="5e803-159">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="5e803-160">Этот тип кэширования подходит для одного или нескольких серверов с использованием *прикрепленные сеансы*.</span><span class="sxs-lookup"><span data-stu-id="5e803-160">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="5e803-161">Прикрепленные сеансы означает, что запросы от клиента всегда направляются на один и тот же сервер для обработки.</span><span class="sxs-lookup"><span data-stu-id="5e803-161">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="5e803-162">Дополнительные сведения см. в разделе [кэшировать в памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="5e803-162">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="5e803-163">Распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="5e803-163">Distributed Cache</span></span>

<span data-ttu-id="5e803-164">Используйте распределенный кэш для хранения данных в памяти, когда приложение будет размещено в облаке или сервер фермы.</span><span class="sxs-lookup"><span data-stu-id="5e803-164">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="5e803-165">Кэш распределяется по всем серверам, которые обрабатывают запросы.</span><span class="sxs-lookup"><span data-stu-id="5e803-165">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="5e803-166">Клиент может отправить запрос, обрабатываемый любой сервер в группе, при наличии кэшированных данных для клиента.</span><span class="sxs-lookup"><span data-stu-id="5e803-166">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="5e803-167">ASP.NET Core предлагает SQL Server и кэши Redis распределенных.</span><span class="sxs-lookup"><span data-stu-id="5e803-167">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="5e803-168">Дополнительные сведения см. в разделе <xref:performance/caching/distributed>.</span><span class="sxs-lookup"><span data-stu-id="5e803-168">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="5e803-169">Вспомогательная функция тега кэша</span><span class="sxs-lookup"><span data-stu-id="5e803-169">Cache Tag Helper</span></span>

<span data-ttu-id="5e803-170">Вы можете кэшировать содержимое из представления MVC или страница Razor со вспомогательной функцией тега кэша.</span><span class="sxs-lookup"><span data-stu-id="5e803-170">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="5e803-171">Вспомогательная функция тега кэша использует кэширование в памяти для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="5e803-171">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="5e803-172">Дополнительные сведения см. в разделе [вспомогательная функция тега кэша в ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="5e803-172">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="5e803-173">Вспомогательная функция тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="5e803-173">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="5e803-174">Вы можете кэшировать содержимое из представления MVC или страницы Razor в распределенных облачных или веб-ферм с вспомогательная функция тега распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="5e803-174">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="5e803-175">Для хранения данных кэша вспомогательной функции тега распределенного использует SQL Server или Redis.</span><span class="sxs-lookup"><span data-stu-id="5e803-175">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="5e803-176">Дополнительные сведения см. в разделе [вспомогательной функции тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="5e803-176">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="5e803-177">Атрибута ResponseCache</span><span class="sxs-lookup"><span data-stu-id="5e803-177">ResponseCache attribute</span></span>

<span data-ttu-id="5e803-178">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) указывает параметры, необходимые для задания соответствующих заголовков в кэширование ответов.</span><span class="sxs-lookup"><span data-stu-id="5e803-178">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="5e803-179">Отключите кэширование для содержимого, которое содержит сведения о прошедших проверку подлинности клиентов.</span><span class="sxs-lookup"><span data-stu-id="5e803-179">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="5e803-180">Кэширование можно включать только для содержимого, которое не меняется на основе удостоверения пользователя или ли пользователь выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="5e803-180">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="5e803-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) изменяет хранимую ответ по значениям указанного списка ключей запроса.</span><span class="sxs-lookup"><span data-stu-id="5e803-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="5e803-182">Если одно значение `*` не указан, по промежуточного слоя, зависит от ответов для всех запросов параметров строки запроса.</span><span class="sxs-lookup"><span data-stu-id="5e803-182">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="5e803-183">`VaryByQueryKeys` требуется ASP.NET Core 1.1 или более поздней.</span><span class="sxs-lookup"><span data-stu-id="5e803-183">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="5e803-184">[По промежуточного слоя для кэширования ответа](xref:performance/caching/middleware) необходимо включить для задания `VaryByQueryKeys` свойство; в противном случае создается исключение времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="5e803-184">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="5e803-185">Нет соответствующего заголовка HTTP для `VaryByQueryKeys` свойство.</span><span class="sxs-lookup"><span data-stu-id="5e803-185">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="5e803-186">Свойство — это функция HTTP, обрабатываемых по промежуточного слоя кэширования ответов.</span><span class="sxs-lookup"><span data-stu-id="5e803-186">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="5e803-187">Для по промежуточного слоя для обслуживания кэшированный ответ строку запроса и значения строки запроса, должны совпадать предыдущего запроса.</span><span class="sxs-lookup"><span data-stu-id="5e803-187">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="5e803-188">Например рассмотрим последовательность запросов и результаты, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="5e803-188">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="5e803-189">Запрос</span><span class="sxs-lookup"><span data-stu-id="5e803-189">Request</span></span>                          | <span data-ttu-id="5e803-190">Результат</span><span class="sxs-lookup"><span data-stu-id="5e803-190">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="5e803-191">Сервер вернул</span><span class="sxs-lookup"><span data-stu-id="5e803-191">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="5e803-192">Возвращаемые по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="5e803-192">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="5e803-193">Сервер вернул</span><span class="sxs-lookup"><span data-stu-id="5e803-193">Returned from server</span></span>     |

<span data-ttu-id="5e803-194">Первый запрос возвращается сервером и кэшируются в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="5e803-194">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="5e803-195">Второй запрос возвращается по промежуточного слоя, так как строка запроса совпадает с предыдущим запросом.</span><span class="sxs-lookup"><span data-stu-id="5e803-195">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="5e803-196">Третий запрос не в кэше по промежуточного слоя, поскольку значение строки запроса не соответствует предыдущего запроса.</span><span class="sxs-lookup"><span data-stu-id="5e803-196">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="5e803-197">`ResponseCacheAttribute` Используется для настройки и создания (с помощью `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="5e803-197">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="5e803-198">`ResponseCacheFilter` Выполняет обновление соответствующие заголовки HTTP и функции ответа.</span><span class="sxs-lookup"><span data-stu-id="5e803-198">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="5e803-199">Фильтр:</span><span class="sxs-lookup"><span data-stu-id="5e803-199">The filter:</span></span>

* <span data-ttu-id="5e803-200">Удаляет любые существующие заголовки для `Vary`, `Cache-Control`, и `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="5e803-200">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="5e803-201">Записывает соответствующие заголовки, в зависимости от свойств, заданных `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="5e803-201">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="5e803-202">Обновляет ответ HTTP функции кэширования, если `VaryByQueryKeys` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="5e803-202">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="5e803-203">Различаются</span><span class="sxs-lookup"><span data-stu-id="5e803-203">Vary</span></span>

<span data-ttu-id="5e803-204">Этот заголовок записывается только когда `VaryByHeader` свойству.</span><span class="sxs-lookup"><span data-stu-id="5e803-204">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="5e803-205">Ему будет присвоено `Vary` значение свойства.</span><span class="sxs-lookup"><span data-stu-id="5e803-205">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="5e803-206">В следующем примере используется `VaryByHeader` свойство:</span><span class="sxs-lookup"><span data-stu-id="5e803-206">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="5e803-207">Вы можете просматривать заголовки ответа с помощью средства обозревателя сети.</span><span class="sxs-lookup"><span data-stu-id="5e803-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="5e803-208">На следующем рисунке показана F12 Edge, выведенных в **сети** вкладке `About2` обновляется метод действия:</span><span class="sxs-lookup"><span data-stu-id="5e803-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Граничный узел вывод F12 на вкладке «сети» при вызове метода действия About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="5e803-210">NoStore и Location.None</span><span class="sxs-lookup"><span data-stu-id="5e803-210">NoStore and Location.None</span></span>

<span data-ttu-id="5e803-211">`NoStore` переопределяет большинство других свойств.</span><span class="sxs-lookup"><span data-stu-id="5e803-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="5e803-212">Если присвоить этому свойству `true`, `Cache-Control` заголовок имеет значение `no-store`.</span><span class="sxs-lookup"><span data-stu-id="5e803-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="5e803-213">Если `Location` присваивается `None`:</span><span class="sxs-lookup"><span data-stu-id="5e803-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="5e803-214">Параметру `Cache-Control` задается значение `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="5e803-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="5e803-215">Параметру `Pragma` задается значение `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="5e803-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="5e803-216">Если `NoStore` — `false` и `Location` — `None`, `Cache-Control` и `Pragma` присваивается `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="5e803-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="5e803-217">Обычно устанавливается `NoStore` для `true` на страницы ошибок.</span><span class="sxs-lookup"><span data-stu-id="5e803-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="5e803-218">Пример:</span><span class="sxs-lookup"><span data-stu-id="5e803-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="5e803-219">Это приводит к следующие заголовки:</span><span class="sxs-lookup"><span data-stu-id="5e803-219">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="5e803-220">Расположение и длительность</span><span class="sxs-lookup"><span data-stu-id="5e803-220">Location and Duration</span></span>

<span data-ttu-id="5e803-221">Чтобы включить кэширование, `Duration` должно быть присвоено положительное значение и `Location` должен быть либо `Any` (по умолчанию) или `Client`.</span><span class="sxs-lookup"><span data-stu-id="5e803-221">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="5e803-222">В этом случае `Cache-Control` заголовок присваивается значение расположения, за которым следует `max-age` ответа.</span><span class="sxs-lookup"><span data-stu-id="5e803-222">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="5e803-223">`Location`в параметры `Any` и `Client` переводиться `Cache-Control` значения заголовка `public` и `private`, соответственно.</span><span class="sxs-lookup"><span data-stu-id="5e803-223">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="5e803-224">Как отмечалось ранее, параметр `Location` для `None` задает оба `Cache-Control` и `Pragma` заголовки `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="5e803-224">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="5e803-225">Ниже пример, показывающий, заголовки, создаваемое путем установки `Duration` и оставить значение по умолчанию `Location` значение:</span><span class="sxs-lookup"><span data-stu-id="5e803-225">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="5e803-226">В результате получается следующий заголовок:</span><span class="sxs-lookup"><span data-stu-id="5e803-226">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="5e803-227">Профили кэша</span><span class="sxs-lookup"><span data-stu-id="5e803-227">Cache profiles</span></span>

<span data-ttu-id="5e803-228">Вместо повторения `ResponseCache` параметров на множество атрибутов действий контроллера, профили кэша можно настроить как параметры, при настройке MVC в `ConfigureServices` метод в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="5e803-228">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="5e803-229">Значения в конфигурации кэша, на которую указывает ссылка, используются значения по умолчанию, `ResponseCache` атрибут и переопределяются все свойства, заданные в атрибуте.</span><span class="sxs-lookup"><span data-stu-id="5e803-229">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="5e803-230">Настройка профиля кэша.</span><span class="sxs-lookup"><span data-stu-id="5e803-230">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="5e803-231">Ссылка на профиль кэша:</span><span class="sxs-lookup"><span data-stu-id="5e803-231">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="5e803-232">`ResponseCache` Атрибут может применяться как для действия (методы) и контроллеры (классы).</span><span class="sxs-lookup"><span data-stu-id="5e803-232">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="5e803-233">Атрибуты на уровне метода переопределяют параметры, указанные в атрибуты уровня класса.</span><span class="sxs-lookup"><span data-stu-id="5e803-233">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="5e803-234">В приведенном выше примере атрибут уровня класса указывает в течение 30 секунд, а атрибутом уровня методов ссылок на профиль кэша с длительностью 60 секунд.</span><span class="sxs-lookup"><span data-stu-id="5e803-234">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="5e803-235">Итоговый заголовка:</span><span class="sxs-lookup"><span data-stu-id="5e803-235">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="5e803-236">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5e803-236">Additional resources</span></span>

* [<span data-ttu-id="5e803-237">Сохранения ответов в кэше</span><span class="sxs-lookup"><span data-stu-id="5e803-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="5e803-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="5e803-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
