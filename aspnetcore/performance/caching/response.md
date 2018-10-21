---
title: Кэширование ответов в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать кэширование ответов, чтобы снизить требования к пропускной способности и повысить производительность приложений ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 4bf61502738d70760679ec98c8f2f303eca9d504
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477493"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="0a2bd-103">Кэширование ответов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a2bd-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="0a2bd-104">По [Luo Джон](https://github.com/JunTaoLuo), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Люк Лэтем](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0a2bd-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="0a2bd-105">Кэширование ответов в Razor Pages в ASP.NET Core 2.1 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="0a2bd-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a2bd-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0a2bd-107">Кэширование ответов уменьшает количество запросов, выполненных клиентского приложения или прокси-сервера веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="0a2bd-108">Кэширование ответов также сокращает время работы веб-сервер выполняет для создания ответа.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="0a2bd-109">Кэширование ответов управляется заголовки, указывающие способ клиента, прокси-сервера и по промежуточного слоя для кэширования ответов.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="0a2bd-110">Веб-сервер может кэшировать ответы, при добавлении [по промежуточного слоя, кэширование ответов](xref:performance/caching/middleware).</span><span class="sxs-lookup"><span data-stu-id="0a2bd-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="0a2bd-111">Кэширование ответов на основе HTTP</span><span class="sxs-lookup"><span data-stu-id="0a2bd-111">HTTP-based response caching</span></span>

<span data-ttu-id="0a2bd-112">[Спецификации кэширования HTTP 1.1](https://tools.ietf.org/html/rfc7234) описано поведение Internet кэшей.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="0a2bd-113">Основной заголовок HTTP, используемый для кэширования [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), который используется для указания кэша *директивы*.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="0a2bd-114">Директивы Управление поведением кэширования запросов все же встречаются несущественные от клиентов к серверам и принимая ответов свой путь с серверов клиентам.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="0a2bd-115">Запросы и ответы перемещения между прокси-серверами и прокси-серверы также должны соответствовать спецификации кэширования HTTP 1.1.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="0a2bd-116">Распространенные `Cache-Control` директивы показаны в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="0a2bd-117">Директива</span><span class="sxs-lookup"><span data-stu-id="0a2bd-117">Directive</span></span>                                                       | <span data-ttu-id="0a2bd-118">Действие</span><span class="sxs-lookup"><span data-stu-id="0a2bd-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="0a2bd-119">public</span><span class="sxs-lookup"><span data-stu-id="0a2bd-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="0a2bd-120">Кэш может хранить ответ.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="0a2bd-121">private</span><span class="sxs-lookup"><span data-stu-id="0a2bd-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="0a2bd-122">Ответ не должны храниться в общий кэш.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="0a2bd-123">Частный кэш может хранить и повторно использовать ответ.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="0a2bd-124">max-age</span><span class="sxs-lookup"><span data-stu-id="0a2bd-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="0a2bd-125">Клиент не будет принимать ответ, возраст которых больше, чем указанное число секунд.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="0a2bd-126">Примеры: `max-age=60` (60 секунд), `max-age=2592000` (1 месяц)</span><span class="sxs-lookup"><span data-stu-id="0a2bd-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="0a2bd-127">без кэша</span><span class="sxs-lookup"><span data-stu-id="0a2bd-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="0a2bd-128">**При запросах**: кэш не должны использовать хранимые ответ для удовлетворения запроса.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="0a2bd-129">Примечание: На сервере-источнике повторно формирует ответ для клиента, и по промежуточного слоя обновляет ответов в своем кэше.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="0a2bd-130">**При ответах**: ответ не должны использоваться для последующих запросов без проверки на исходном сервере.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="0a2bd-131">no-store</span><span class="sxs-lookup"><span data-stu-id="0a2bd-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="0a2bd-132">**При запросах**: кэш не может храниться запрос.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="0a2bd-133">**При ответах**: кэш не должен хранить любую часть ответа.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="0a2bd-134">В следующей таблице показаны другие заголовки кэша, которые влияют на кэширование.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="0a2bd-135">Header</span><span class="sxs-lookup"><span data-stu-id="0a2bd-135">Header</span></span>                                                     | <span data-ttu-id="0a2bd-136">Функция</span><span class="sxs-lookup"><span data-stu-id="0a2bd-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="0a2bd-137">Срок действия</span><span class="sxs-lookup"><span data-stu-id="0a2bd-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="0a2bd-138">Приблизительное количество времени в секундах с момента ответа сформирован или создан успешно прошли проверку на исходном сервере.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="0a2bd-139">Срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="0a2bd-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="0a2bd-140">Дата и время, после чего ответ считается устаревшей.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="0a2bd-141">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="0a2bd-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="0a2bd-142">Существует для обеспечения обратной совместимости с HTTP/1.0 кэширует параметра `no-cache` поведение.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="0a2bd-143">Если `Cache-Control` заголовок присутствует, `Pragma` заголовка учитывается.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="0a2bd-144">Различаются</span><span class="sxs-lookup"><span data-stu-id="0a2bd-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="0a2bd-145">Указывает, что кэшированный ответ должен быть отправляется, только если все из `Vary` заголовок поля согласуются в кэшированный ответ исходного запроса и новый запрос.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="0a2bd-146">Отношениях кэширования на основе HTTP запроса директивы Cache-Control</span><span class="sxs-lookup"><span data-stu-id="0a2bd-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="0a2bd-147">[Спецификации кэширования HTTP 1.1 для заголовка Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) требует кэша соблюдать является допустимым для `Cache-Control` заголовка, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="0a2bd-148">Клиент может выполнять запросы с `no-cache` значение заголовка и принудительного создания нового ответа для каждого запроса сервером.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="0a2bd-149">Всегда учитывает клиента `Cache-Control` заголовки запроса смысл, если вы считаете цель HTTP-кэширования.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="0a2bd-150">В официальной спецификации кэширование призвана сократить расходы на задержку и сети для удовлетворения запросов по сети, прокси-серверов, серверов и клиентов.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="0a2bd-151">Это не обязательно для контроля нагрузки на сервер-источник.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="0a2bd-152">Нет не текущий контроль разработчика над это поведение кэширования, при использовании [по промежуточного слоя для кэширования ответа](xref:performance/caching/middleware) так, как по промежуточного слоя, соответствует официальной спецификации кэширования.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="0a2bd-153">[В будущем в по промежуточного слоя](https://github.com/aspnet/ResponseCaching/issues/96) позволит Настройка по промежуточного слоя, чтобы игнорировать запроса `Cache-Control` заголовка при принятии решения о обслуживать кэшированный ответ.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="0a2bd-154">Это предложит вам возможность более эффективно контролировать загрузку сервера при использовании по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="0a2bd-155">Другие технология кэширования в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a2bd-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="0a2bd-156">Кэширование в памяти</span><span class="sxs-lookup"><span data-stu-id="0a2bd-156">In-memory caching</span></span>

<span data-ttu-id="0a2bd-157">Кэширование в памяти использует память сервера для хранения кэшированных данных.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="0a2bd-158">Этот тип кэширования подходит для одного или нескольких серверов с использованием *прикрепленные сеансы*.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="0a2bd-159">Прикрепленные сеансы означает, что запросы от клиента всегда направляются на один и тот же сервер для обработки.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="0a2bd-160">Дополнительные сведения см. в разделе [кэшировать в памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="0a2bd-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="0a2bd-161">Распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="0a2bd-161">Distributed Cache</span></span>

<span data-ttu-id="0a2bd-162">Используйте распределенный кэш для хранения данных в памяти, когда приложение будет размещено в облаке или сервер фермы.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="0a2bd-163">Кэш распределяется по всем серверам, которые обрабатывают запросы.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="0a2bd-164">Клиент может отправить запрос, обрабатываемый любой сервер в группе, при наличии кэшированных данных для клиента.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="0a2bd-165">ASP.NET Core предлагает SQL Server и кэши Redis распределенных.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="0a2bd-166">Дополнительные сведения см. в разделе [Работа с распределенным кэшем](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="0a2bd-166">For more information, see [Work with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="0a2bd-167">Вспомогательная функция тега кэша</span><span class="sxs-lookup"><span data-stu-id="0a2bd-167">Cache Tag Helper</span></span>

<span data-ttu-id="0a2bd-168">Вы можете кэшировать содержимое из представления MVC или страница Razor со вспомогательной функцией тега кэша.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="0a2bd-169">Вспомогательная функция тега кэша использует кэширование в памяти для хранения данных.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="0a2bd-170">Дополнительные сведения см. в разделе [вспомогательная функция тега кэша в ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0a2bd-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="0a2bd-171">Вспомогательная функция тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="0a2bd-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="0a2bd-172">Вы можете кэшировать содержимое из представления MVC или страницы Razor в распределенных облачных или веб-ферм с вспомогательная функция тега распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="0a2bd-173">Для хранения данных кэша вспомогательной функции тега распределенного использует SQL Server или Redis.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="0a2bd-174">Дополнительные сведения см. в разделе [вспомогательной функции тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="0a2bd-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="0a2bd-175">Атрибута ResponseCache</span><span class="sxs-lookup"><span data-stu-id="0a2bd-175">ResponseCache attribute</span></span>

<span data-ttu-id="0a2bd-176">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) указывает параметры, необходимые для задания соответствующих заголовков в кэширование ответов.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="0a2bd-177">Отключите кэширование для содержимого, которое содержит сведения о прошедших проверку подлинности клиентов.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="0a2bd-178">Кэширование можно включать только для содержимого, которое не меняется на основе удостоверения пользователя или ли пользователь выполнил вход.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="0a2bd-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) изменяет хранимую ответ по значениям указанного списка ключей запроса.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="0a2bd-180">Если одно значение `*` не указан, по промежуточного слоя, зависит от ответов для всех запросов параметров строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="0a2bd-181">`VaryByQueryKeys` требуется ASP.NET Core 1.1 или более поздней.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="0a2bd-182">Необходимо включить по промежуточного слоя кэширования ответов для задания `VaryByQueryKeys` свойство; в противном случае создается исключение времени выполнения.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="0a2bd-183">Нет соответствующего заголовка HTTP для `VaryByQueryKeys` свойство.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="0a2bd-184">Свойство — это функция HTTP, обрабатываемых по промежуточного слоя кэширования ответов.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="0a2bd-185">Для по промежуточного слоя для обслуживания кэшированный ответ строку запроса и значения строки запроса, должны совпадать предыдущего запроса.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="0a2bd-186">Например рассмотрим последовательность запросов и результаты, показанные в следующей таблице.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="0a2bd-187">Запрос</span><span class="sxs-lookup"><span data-stu-id="0a2bd-187">Request</span></span>                          | <span data-ttu-id="0a2bd-188">Результат</span><span class="sxs-lookup"><span data-stu-id="0a2bd-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="0a2bd-189">Сервер вернул</span><span class="sxs-lookup"><span data-stu-id="0a2bd-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="0a2bd-190">Возвращаемые по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="0a2bd-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="0a2bd-191">Сервер вернул</span><span class="sxs-lookup"><span data-stu-id="0a2bd-191">Returned from server</span></span>     |

<span data-ttu-id="0a2bd-192">Первый запрос возвращается сервером и кэшируются в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="0a2bd-193">Второй запрос возвращается по промежуточного слоя, так как строка запроса совпадает с предыдущим запросом.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="0a2bd-194">Третий запрос не в кэше по промежуточного слоя, поскольку значение строки запроса не соответствует предыдущего запроса.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="0a2bd-195">`ResponseCacheAttribute` Используется для настройки и создания (с помощью `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span><span class="sxs-lookup"><span data-stu-id="0a2bd-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="0a2bd-196">`ResponseCacheFilter` Выполняет обновление соответствующие заголовки HTTP и функции ответа.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="0a2bd-197">Фильтр:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-197">The filter:</span></span>

* <span data-ttu-id="0a2bd-198">Удаляет любые существующие заголовки для `Vary`, `Cache-Control`, и `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="0a2bd-199">Записывает соответствующие заголовки, в зависимости от свойств, заданных `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="0a2bd-200">Обновляет ответ HTTP функции кэширования, если `VaryByQueryKeys` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="0a2bd-201">Различаются</span><span class="sxs-lookup"><span data-stu-id="0a2bd-201">Vary</span></span>

<span data-ttu-id="0a2bd-202">Этот заголовок записывается только когда `VaryByHeader` свойству.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="0a2bd-203">Ему будет присвоено `Vary` значение свойства.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="0a2bd-204">В следующем примере используется `VaryByHeader` свойство:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="0a2bd-205">Вы можете просматривать заголовки ответа с помощью средства обозревателя сети.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="0a2bd-206">На следующем рисунке показана F12 Edge, выведенных в **сети** вкладке `About2` обновляется метод действия:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![Граничный узел вывод F12 на вкладке «сети» при вызове метода действия About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="0a2bd-208">NoStore и Location.None</span><span class="sxs-lookup"><span data-stu-id="0a2bd-208">NoStore and Location.None</span></span>

<span data-ttu-id="0a2bd-209">`NoStore` переопределяет большинство других свойств.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="0a2bd-210">Если присвоить этому свойству `true`, `Cache-Control` заголовок имеет значение `no-store`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="0a2bd-211">Если `Location` присваивается `None`:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="0a2bd-212">Параметру `Cache-Control` задается значение `no-store,no-cache`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="0a2bd-213">Параметру `Pragma` задается значение `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="0a2bd-214">Если `NoStore` — `false` и `Location` — `None`, `Cache-Control` и `Pragma` присваивается `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="0a2bd-215">Обычно устанавливается `NoStore` для `true` на страницы ошибок.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="0a2bd-216">Пример:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="0a2bd-217">Это приводит к следующие заголовки:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="0a2bd-218">Расположение и длительность</span><span class="sxs-lookup"><span data-stu-id="0a2bd-218">Location and Duration</span></span>

<span data-ttu-id="0a2bd-219">Чтобы включить кэширование, `Duration` должно быть присвоено положительное значение и `Location` должен быть либо `Any` (по умолчанию) или `Client`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="0a2bd-220">В этом случае `Cache-Control` заголовок присваивается значение расположения, за которым следует `max-age` ответа.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="0a2bd-221">`Location`в параметры `Any` и `Client` переводиться `Cache-Control` значения заголовка `public` и `private`, соответственно.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="0a2bd-222">Как отмечалось ранее, параметр `Location` для `None` задает оба `Cache-Control` и `Pragma` заголовки `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="0a2bd-223">Ниже пример, показывающий, заголовки, создаваемое путем установки `Duration` и оставить значение по умолчанию `Location` значение:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="0a2bd-224">В результате получается следующий заголовок:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="0a2bd-225">Профили кэша</span><span class="sxs-lookup"><span data-stu-id="0a2bd-225">Cache profiles</span></span>

<span data-ttu-id="0a2bd-226">Вместо повторения `ResponseCache` параметров на множество атрибутов действий контроллера, профили кэша можно настроить как параметры, при настройке MVC в `ConfigureServices` метод в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="0a2bd-227">Значения в конфигурации кэша, на которую указывает ссылка, используются значения по умолчанию, `ResponseCache` атрибут и переопределяются все свойства, заданные в атрибуте.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="0a2bd-228">Настройка профиля кэша.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="0a2bd-229">Ссылка на профиль кэша:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="0a2bd-230">`ResponseCache` Атрибут может применяться как для действия (методы) и контроллеры (классы).</span><span class="sxs-lookup"><span data-stu-id="0a2bd-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="0a2bd-231">Атрибуты на уровне метода переопределяют параметры, указанные в атрибуты уровня класса.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="0a2bd-232">В приведенном выше примере атрибут уровня класса указывает в течение 30 секунд, а атрибутом уровня методов ссылок на профиль кэша с длительностью 60 секунд.</span><span class="sxs-lookup"><span data-stu-id="0a2bd-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="0a2bd-233">Итоговый заголовка:</span><span class="sxs-lookup"><span data-stu-id="0a2bd-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="0a2bd-234">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0a2bd-234">Additional resources</span></span>

* [<span data-ttu-id="0a2bd-235">Сохранения ответов в кэше</span><span class="sxs-lookup"><span data-stu-id="0a2bd-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="0a2bd-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="0a2bd-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="0a2bd-237">Кэш в памяти</span><span class="sxs-lookup"><span data-stu-id="0a2bd-237">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="0a2bd-238">Работа с распределенным кэшем</span><span class="sxs-lookup"><span data-stu-id="0a2bd-238">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="0a2bd-239">Обнаружение изменений с помощью маркеров изменений</span><span class="sxs-lookup"><span data-stu-id="0a2bd-239">Detect changes with change tokens</span></span>](xref:fundamentals/change-tokens)
* [<span data-ttu-id="0a2bd-240">ПО промежуточного слоя для кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="0a2bd-240">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="0a2bd-241">Вспомогательная функция тега кэша</span><span class="sxs-lookup"><span data-stu-id="0a2bd-241">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="0a2bd-242">Вспомогательная функция тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="0a2bd-242">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
