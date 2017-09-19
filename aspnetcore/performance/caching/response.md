---
title: "Кэширование ответов"
author: rick-anderson
description: "Описание способов использования ответа кэширование снизить пропускную способность и повысить производительность."
keywords: "ASP.NET Core, кэширование, HTTP-заголовки ответа"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 97bc56a3cfe7383f207a4f621ab3fe8b8dc258ef
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2017
---
# <a name="response-caching"></a><span data-ttu-id="215b2-104">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="215b2-104">Response Caching</span></span>

<span data-ttu-id="215b2-105">По [Luo Джон](https://github.com/JunTaoLuo), [Рик Андерсон](https://twitter.com/RickAndMSFT), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="215b2-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="215b2-106">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="215b2-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a><span data-ttu-id="215b2-107">Что такое кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="215b2-107">What is Response Caching</span></span>

<span data-ttu-id="215b2-108">*Кэширование ответов* добавляет относящиеся к кэшированию заголовки ответов.</span><span class="sxs-lookup"><span data-stu-id="215b2-108">*Response caching* adds cache-related headers to responses.</span></span> <span data-ttu-id="215b2-109">Эти заголовки укажите способ клиента, прокси-сервера и по промежуточного слоя для кэширования ответов.</span><span class="sxs-lookup"><span data-stu-id="215b2-109">These headers specify how you want client, proxy and middleware to cache responses.</span></span> <span data-ttu-id="215b2-110">Кэширование ответов можно уменьшить количество запросов, выполненных клиентом или прокси-сервера веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="215b2-110">Response caching can reduce the number of requests a client or proxy makes to the web server.</span></span> <span data-ttu-id="215b2-111">Кэширование ответов можно также уменьшить объем работы выполняет веб-сервер для формирования ответа.</span><span class="sxs-lookup"><span data-stu-id="215b2-111">Response caching can also reduce the amount of work the web server performs to generate the response.</span></span> 

<span data-ttu-id="215b2-112">Основной заголовок HTTP для кэширования `Cache-Control`.</span><span class="sxs-lookup"><span data-stu-id="215b2-112">The primary HTTP header used for caching is `Cache-Control`.</span></span> <span data-ttu-id="215b2-113">В разделе [кэширования HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="215b2-113">See the [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) for more information.</span></span> <span data-ttu-id="215b2-114">Общий кэш директивы:</span><span class="sxs-lookup"><span data-stu-id="215b2-114">Common cache directives:</span></span>

* [<span data-ttu-id="215b2-115">public</span><span class="sxs-lookup"><span data-stu-id="215b2-115">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [<span data-ttu-id="215b2-116">private</span><span class="sxs-lookup"><span data-stu-id="215b2-116">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [<span data-ttu-id="215b2-117">Нет-cache</span><span class="sxs-lookup"><span data-stu-id="215b2-117">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [<span data-ttu-id="215b2-118">Директивы pragma</span><span class="sxs-lookup"><span data-stu-id="215b2-118">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)
* [<span data-ttu-id="215b2-119">Различаются</span><span class="sxs-lookup"><span data-stu-id="215b2-119">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)

<span data-ttu-id="215b2-120">Веб-сервер может кэшировать ответы, добавив в ответ, кэширование по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="215b2-120">The web server can cache responses by adding the response caching middleware.</span></span> <span data-ttu-id="215b2-121">В разделе [по промежуточного слоя кэширование ответов](middleware.md) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="215b2-121">See [Response caching middleware](middleware.md) for more information.</span></span>

## <a name="distributed-cache-tag-helper"></a><span data-ttu-id="215b2-122">Вспомогательный тег распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="215b2-122">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="215b2-123">[Распределенного кэша тег вспомогательный](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) включает распределенное кэширование.</span><span class="sxs-lookup"><span data-stu-id="215b2-123">The [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) enables distributed caching.</span></span>


## <a name="responsecache-attribute"></a><span data-ttu-id="215b2-124">Атрибут ResponseCache</span><span class="sxs-lookup"><span data-stu-id="215b2-124">ResponseCache Attribute</span></span>

<span data-ttu-id="215b2-125">`ResponseCacheAttribute` Указывает параметры, необходимые для установки соответствующие заголовки кэширование ответов.</span><span class="sxs-lookup"><span data-stu-id="215b2-125">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="215b2-126">В разделе [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) описание параметров.</span><span class="sxs-lookup"><span data-stu-id="215b2-126">See [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)  for a description of the parameters.</span></span>

>[!WARNING]
> <span data-ttu-id="215b2-127">Отключение кэширования для содержимого, которое содержит сведения о прошедших проверку подлинности клиентов.</span><span class="sxs-lookup"><span data-stu-id="215b2-127">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="215b2-128">Кэширование должны включаться только для содержимого, которые не изменяются в зависимости от идентификатора пользователя или пользователь вошел.</span><span class="sxs-lookup"><span data-stu-id="215b2-128">Caching should only be enabled for content that does not change based on a user's identity, or whether a user is logged in.</span></span>

<span data-ttu-id="215b2-129">`VaryByQueryKeys string[]`(требуется ASP.NET Core 1.1.0 и более поздние версии): при задании ответ, кэширование по промежуточного слоя будет изменять хранимых ответ значения из заданного списка ключей запроса.</span><span class="sxs-lookup"><span data-stu-id="215b2-129">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1.0 and higher): When set, the response caching middleware will vary the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="215b2-130">Для установки необходимо включить кэширование по промежуточного слоя ответов `VaryByQueryKeys` свойство, в противном случае среда выполнения будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="215b2-130">The response caching middleware must be enabled to set the `VaryByQueryKeys` property, otherwise a runtime exception will be thrown.</span></span> <span data-ttu-id="215b2-131">Отсутствует соответствующий заголовок HTTP для `VaryByQueryKeys` свойства.</span><span class="sxs-lookup"><span data-stu-id="215b2-131">There is no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="215b2-132">Это свойство является функцией HTTP, обрабатываемых по промежуточного слоя кэширование ответов.</span><span class="sxs-lookup"><span data-stu-id="215b2-132">This property is an HTTP feature handled by the response caching middleware.</span></span> <span data-ttu-id="215b2-133">По промежуточного слоя обслуживать кэшированный ответ строка запроса и значение строки запроса должно соответствовать предыдущего запроса.</span><span class="sxs-lookup"><span data-stu-id="215b2-133">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="215b2-134">Например рассмотрим следующую последовательность:</span><span class="sxs-lookup"><span data-stu-id="215b2-134">For example, consider the following sequence:</span></span>

| <span data-ttu-id="215b2-135">Запрос</span><span class="sxs-lookup"><span data-stu-id="215b2-135">Request</span></span>          | <span data-ttu-id="215b2-136">Результат</span><span class="sxs-lookup"><span data-stu-id="215b2-136">Result</span></span> |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | <span data-ttu-id="215b2-137">возвращенный от сервера</span><span class="sxs-lookup"><span data-stu-id="215b2-137">returned from server</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="215b2-138">возвращаемые по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="215b2-138">returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="215b2-139">возвращенный от сервера</span><span class="sxs-lookup"><span data-stu-id="215b2-139">returned from server</span></span> |

<span data-ttu-id="215b2-140">Первый запрос возвращается сервером и кэшируются в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="215b2-140">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="215b2-141">Второй запрос возвращается по промежуточного слоя, так как предыдущий запрос соответствует строке запроса.</span><span class="sxs-lookup"><span data-stu-id="215b2-141">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="215b2-142">Третий запрос не является в кэше по промежуточного слоя, так как значение строки запроса не соответствует предыдущего запроса.</span><span class="sxs-lookup"><span data-stu-id="215b2-142">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="215b2-143">`ResponseCacheAttribute` Используется для настройки и создания (через `IFilterFactory`) `ResponseCacheFilter`.</span><span class="sxs-lookup"><span data-stu-id="215b2-143">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="215b2-144">`ResponseCacheFilter` Выполняет обновление компонентов ответа и соответствующие заголовки HTTP.</span><span class="sxs-lookup"><span data-stu-id="215b2-144">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="215b2-145">Фильтр:</span><span class="sxs-lookup"><span data-stu-id="215b2-145">The filter:</span></span>

* <span data-ttu-id="215b2-146">Удаляет все существующие заголовки для `Vary`, `Cache-Control`, и `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="215b2-146">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="215b2-147">Записывает соответствующие заголовки, в зависимости от свойств, заданных `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="215b2-147">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="215b2-148">Обновляет ответ HTTP функции кэширования, если `VaryByQueryKeys` имеет значение.</span><span class="sxs-lookup"><span data-stu-id="215b2-148">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="215b2-149">Различаются</span><span class="sxs-lookup"><span data-stu-id="215b2-149">Vary</span></span>

<span data-ttu-id="215b2-150">Этот заголовок записывается только когда `VaryByHeader` свойству.</span><span class="sxs-lookup"><span data-stu-id="215b2-150">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="215b2-151">Ему присваивается `Vary` значение свойства.</span><span class="sxs-lookup"><span data-stu-id="215b2-151">It is set to the `Vary` property's value.</span></span> <span data-ttu-id="215b2-152">В следующем примере используется `VaryByHeader` свойство.</span><span class="sxs-lookup"><span data-stu-id="215b2-152">The following sample uses the `VaryByHeader` property.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

<span data-ttu-id="215b2-153">Заголовки ответа можно просмотреть с помощью сетевых средств браузеры.</span><span class="sxs-lookup"><span data-stu-id="215b2-153">You can view the response headers with your browsers network tools.</span></span> <span data-ttu-id="215b2-154">На следующем рисунке показаны границы F12, выведенных в **сети** вкладке при `About2` обновляется метод действия.</span><span class="sxs-lookup"><span data-stu-id="215b2-154">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed.</span></span> 

![Ребро F12 выходные данные на ** сети ** вкладка при вызове метода действия «About2»](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="215b2-156">NoStore и Location.None</span><span class="sxs-lookup"><span data-stu-id="215b2-156">NoStore and Location.None</span></span>

<span data-ttu-id="215b2-157">`NoStore`переопределяет большинство других свойств.</span><span class="sxs-lookup"><span data-stu-id="215b2-157">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="215b2-158">Если значение этого свойства `true`, `Cache-Control` заголовок будет присвоено «нет-store».</span><span class="sxs-lookup"><span data-stu-id="215b2-158">When this property is set to `true`, the `Cache-Control` header will be set to "no-store".</span></span> <span data-ttu-id="215b2-159">Если `Location` равно `None`:</span><span class="sxs-lookup"><span data-stu-id="215b2-159">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="215b2-160">Параметру `Cache-Control` задается значение `"no-store, no-cache"`.</span><span class="sxs-lookup"><span data-stu-id="215b2-160">`Cache-Control` is set to `"no-store, no-cache"`.</span></span> 
* <span data-ttu-id="215b2-161">Параметру `Pragma` задается значение `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="215b2-161">`Pragma` is set to `no-cache`.</span></span> 

<span data-ttu-id="215b2-162">Если `NoStore` — `false` и `Location` — `None`, `Cache-Control` и `Pragma` будет присвоено `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="215b2-162">If `NoStore` is `false` and `Location` is `None`,  `Cache-Control` and `Pragma` will be set to `no-cache`.</span></span>

<span data-ttu-id="215b2-163">Обычно устанавливается `NoStore` для `true` на страницы ошибок.</span><span class="sxs-lookup"><span data-stu-id="215b2-163">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="215b2-164">Пример:</span><span class="sxs-lookup"><span data-stu-id="215b2-164">For example:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

<span data-ttu-id="215b2-165">В результате следующие заголовки:</span><span class="sxs-lookup"><span data-stu-id="215b2-165">This will result in the following headers:</span></span>

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="215b2-166">Расположение и длительность</span><span class="sxs-lookup"><span data-stu-id="215b2-166">Location and Duration</span></span>

<span data-ttu-id="215b2-167">Чтобы включить кэширование, `Duration` должно быть присвоено положительное значение и `Location` должно равняться `Any` (по умолчанию) или `Client`.</span><span class="sxs-lookup"><span data-stu-id="215b2-167">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="215b2-168">В этом случае `Cache-Control` заголовок будет присвоено значение расположения, следуют «max-age» ответа.</span><span class="sxs-lookup"><span data-stu-id="215b2-168">In this case, the `Cache-Control` header will be set to the location value followed by the "max-age" of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="215b2-169">`Location`в параметры `Any` и `Client` переводиться `Cache-Control` значения заголовка `public` и `private`соответственно.</span><span class="sxs-lookup"><span data-stu-id="215b2-169">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="215b2-170">Как отмечалось ранее, параметр `Location` для `None` установит оба `Cache-Control` и `Pragma` заголовки `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="215b2-170">As noted previously, setting `Location` to `None` will set both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="215b2-171">Ниже приведен пример заголовки создается, задав `Duration` и оставить значение по умолчанию `Location` значение.</span><span class="sxs-lookup"><span data-stu-id="215b2-171">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value.</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

<span data-ttu-id="215b2-172">Выводятся следующие заголовки:</span><span class="sxs-lookup"><span data-stu-id="215b2-172">Produces the following headers:</span></span>

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a><span data-ttu-id="215b2-173">Профили кэша</span><span class="sxs-lookup"><span data-stu-id="215b2-173">Cache Profiles</span></span>

<span data-ttu-id="215b2-174">Вместо повторения `ResponseCache` параметров на множество атрибутов действия контроллера, профилей кэша можно настроить как параметры, при настройке MVC в `ConfigureServices` метод в `Startup`.</span><span class="sxs-lookup"><span data-stu-id="215b2-174">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="215b2-175">Значения, обнаруженные в профиль кэша, на которую указывает ссылка будет использоваться по умолчанию, `ResponseCache` атрибута и будет переопределено любого свойства, указанные в атрибуте.</span><span class="sxs-lookup"><span data-stu-id="215b2-175">Values found in a referenced cache profile will be used as the defaults by the `ResponseCache` attribute, and will be overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="215b2-176">Настройка профиля кэша.</span><span class="sxs-lookup"><span data-stu-id="215b2-176">Setting up a cache profile:</span></span>

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

<span data-ttu-id="215b2-177">Ссылка на профиль кэша:</span><span class="sxs-lookup"><span data-stu-id="215b2-177">Referencing a cache profile:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

<span data-ttu-id="215b2-178">`ResponseCache` Атрибут может применяться как для действия (методы), а также контроллеров (классы).</span><span class="sxs-lookup"><span data-stu-id="215b2-178">The `ResponseCache` attribute can be applied both to actions (methods) as well as controllers (classes).</span></span> <span data-ttu-id="215b2-179">Атрибуты на уровне метода переопределяют параметры, указанные в атрибуты уровня класса.</span><span class="sxs-lookup"><span data-stu-id="215b2-179">Method-level attributes will override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="215b2-180">В приведенном выше примере атрибут уровня класса указывает в течение 30 секунд, пока атрибуты уровня метод ссылается на профиль кэша с длительностью 60 секунд.</span><span class="sxs-lookup"><span data-stu-id="215b2-180">In the above example, a class-level attribute specifies a duration of 30 seconds while a method-level attributes references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="215b2-181">Полученный заголовок:</span><span class="sxs-lookup"><span data-stu-id="215b2-181">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a><span data-ttu-id="215b2-182">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="215b2-182">Additional Resources</span></span>

* [<span data-ttu-id="215b2-183">Кэширование в HTTP из спецификации</span><span class="sxs-lookup"><span data-stu-id="215b2-183">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="215b2-184">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="215b2-184">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
