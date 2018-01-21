---
title: "Кэшировать вспомогательный тег в ядре ASP.NET MVC"
author: pkellner
description: "Показано, как работать с вспомогательный тег кэша"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: dfd9c3c0c4e50a99e4f8703b01bd9b384930b87a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="d3b37-103">Кэшировать вспомогательный тег в ядре ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d3b37-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="d3b37-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="d3b37-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="d3b37-105">Вспомогательный тег кэша позволяет существенно повысить производительность приложения ASP.NET Core путем кэширования ее содержимое, чтобы внутренний поставщик кэша ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d3b37-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="d3b37-106">Значение по умолчанию задает представлений Razor `expires-after` до 20 минут.</span><span class="sxs-lookup"><span data-stu-id="d3b37-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="d3b37-107">Приведенная ниже разметка Razor кэширует даты и времени:</span><span class="sxs-lookup"><span data-stu-id="d3b37-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="d3b37-108">Первый запрос страницы, содержащей `CacheTagHelper` отобразит текущее значение даты и времени.</span><span class="sxs-lookup"><span data-stu-id="d3b37-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="d3b37-109">Дополнительные запросы покажет кэшированное значение, до истечения срока действия (по умолчанию 20 минут) или исключен с нехваткой памяти в кэше.</span><span class="sxs-lookup"><span data-stu-id="d3b37-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="d3b37-110">Можно задать длительность кэширования со следующими атрибутами:</span><span class="sxs-lookup"><span data-stu-id="d3b37-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="d3b37-111">Кэшировать вспомогательные атрибуты тега</span><span class="sxs-lookup"><span data-stu-id="d3b37-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="d3b37-112">enabled</span><span class="sxs-lookup"><span data-stu-id="d3b37-112">enabled</span></span>    


| <span data-ttu-id="d3b37-113">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-113">Attribute Type</span></span>    | <span data-ttu-id="d3b37-114">Допустимые значения</span><span class="sxs-lookup"><span data-stu-id="d3b37-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="d3b37-115">boolean</span><span class="sxs-lookup"><span data-stu-id="d3b37-115">boolean</span></span>           | <span data-ttu-id="d3b37-116">«true» (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="d3b37-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="d3b37-117">"false"</span><span class="sxs-lookup"><span data-stu-id="d3b37-117">"false"</span></span>   |


<span data-ttu-id="d3b37-118">Определяет, кэшируются ли содержимое, заключенное в тег вспомогательный объект кэша.</span><span class="sxs-lookup"><span data-stu-id="d3b37-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="d3b37-119">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="d3b37-119">The default is `true`.</span></span>  <span data-ttu-id="d3b37-120">Если значение `false` этого вспомогательного объекта тег кэша не будет действовать кэширования на выводимых данных.</span><span class="sxs-lookup"><span data-stu-id="d3b37-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="d3b37-121">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="d3b37-122">Срок действия истекает в</span><span class="sxs-lookup"><span data-stu-id="d3b37-122">expires-on</span></span> 

| <span data-ttu-id="d3b37-123">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-123">Attribute Type</span></span>    | <span data-ttu-id="d3b37-124">Пример значения</span><span class="sxs-lookup"><span data-stu-id="d3b37-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="d3b37-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="d3b37-125">DateTimeOffset</span></span>    | <span data-ttu-id="d3b37-126">«@new DateTime(2025,1,29,17,02,0)»</span><span class="sxs-lookup"><span data-stu-id="d3b37-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="d3b37-127">Задает абсолютный срок действия.</span><span class="sxs-lookup"><span data-stu-id="d3b37-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="d3b37-128">Следующий пример будет кэшировать содержимое тега вспомогательный объект кэша до 17:02:00 на 29 января 2025.</span><span class="sxs-lookup"><span data-stu-id="d3b37-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="d3b37-129">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="d3b37-130">После истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="d3b37-130">expires-after</span></span>

| <span data-ttu-id="d3b37-131">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-131">Attribute Type</span></span>    | <span data-ttu-id="d3b37-132">Пример значения</span><span class="sxs-lookup"><span data-stu-id="d3b37-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="d3b37-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="d3b37-133">TimeSpan</span></span>    | <span data-ttu-id="d3b37-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="d3b37-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="d3b37-135">Задает интервал времени с момента первого запроса для кэширования содержимого.</span><span class="sxs-lookup"><span data-stu-id="d3b37-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="d3b37-136">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="d3b37-137">скользящий срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="d3b37-137">expires-sliding</span></span>

| <span data-ttu-id="d3b37-138">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-138">Attribute Type</span></span>    | <span data-ttu-id="d3b37-139">Пример значения</span><span class="sxs-lookup"><span data-stu-id="d3b37-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="d3b37-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="d3b37-140">TimeSpan</span></span>    | <span data-ttu-id="d3b37-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="d3b37-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="d3b37-142">Задает время, следует удалить запись кэша, если оно не было обращений.</span><span class="sxs-lookup"><span data-stu-id="d3b37-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="d3b37-143">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="d3b37-144">различаются по заголовок</span><span class="sxs-lookup"><span data-stu-id="d3b37-144">vary-by-header</span></span>

| <span data-ttu-id="d3b37-145">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-145">Attribute Type</span></span>    | <span data-ttu-id="d3b37-146">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="d3b37-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="d3b37-147">String</span><span class="sxs-lookup"><span data-stu-id="d3b37-147">String</span></span>            | <span data-ttu-id="d3b37-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="d3b37-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="d3b37-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="d3b37-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="d3b37-150">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, запускающих обновление кэша, при их изменения.</span><span class="sxs-lookup"><span data-stu-id="d3b37-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="d3b37-151">Следующий пример отслеживает значение заголовка `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="d3b37-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="d3b37-152">Пример будет кэшировать содержимое для каждого разные `User-Agent` представлены на веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="d3b37-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="d3b37-153">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="d3b37-154">различаются по запроса</span><span class="sxs-lookup"><span data-stu-id="d3b37-154">vary-by-query</span></span>

| <span data-ttu-id="d3b37-155">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-155">Attribute Type</span></span>    | <span data-ttu-id="d3b37-156">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="d3b37-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="d3b37-157">String</span><span class="sxs-lookup"><span data-stu-id="d3b37-157">String</span></span>            | <span data-ttu-id="d3b37-158">«Обеспечить»</span><span class="sxs-lookup"><span data-stu-id="d3b37-158">"Make"</span></span>                |
|                   | <span data-ttu-id="d3b37-159">«Создание модели»</span><span class="sxs-lookup"><span data-stu-id="d3b37-159">"Make,Model"</span></span> |

<span data-ttu-id="d3b37-160">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, при изменении значения заголовка.</span><span class="sxs-lookup"><span data-stu-id="d3b37-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="d3b37-161">Следующий пример просматривает значения `Make` и `Model`.</span><span class="sxs-lookup"><span data-stu-id="d3b37-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="d3b37-162">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="d3b37-163">различаются по Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="d3b37-163">vary-by-route</span></span>

| <span data-ttu-id="d3b37-164">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-164">Attribute Type</span></span>    | <span data-ttu-id="d3b37-165">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="d3b37-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="d3b37-166">String</span><span class="sxs-lookup"><span data-stu-id="d3b37-166">String</span></span>            | <span data-ttu-id="d3b37-167">«Обеспечить»</span><span class="sxs-lookup"><span data-stu-id="d3b37-167">"Make"</span></span>                |
|                   | <span data-ttu-id="d3b37-168">«Создание модели»</span><span class="sxs-lookup"><span data-stu-id="d3b37-168">"Make,Model"</span></span> |

<span data-ttu-id="d3b37-169">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, когда изменение значения параметра данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="d3b37-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="d3b37-170">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-170">Example:</span></span>

<span data-ttu-id="d3b37-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="d3b37-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="d3b37-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d3b37-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="d3b37-173">различаются по cookie</span><span class="sxs-lookup"><span data-stu-id="d3b37-173">vary-by-cookie</span></span>

| <span data-ttu-id="d3b37-174">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-174">Attribute Type</span></span>    | <span data-ttu-id="d3b37-175">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="d3b37-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="d3b37-176">String</span><span class="sxs-lookup"><span data-stu-id="d3b37-176">String</span></span>            | <span data-ttu-id="d3b37-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="d3b37-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="d3b37-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="d3b37-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="d3b37-179">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, когда изменение (s) значения заголовка.</span><span class="sxs-lookup"><span data-stu-id="d3b37-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="d3b37-180">Следующий пример просматривает файл cookie, связанные с ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="d3b37-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="d3b37-181">Когда пользователь проходит проверку подлинности запроса куки-файл задать активизирующий обновления кэша.</span><span class="sxs-lookup"><span data-stu-id="d3b37-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="d3b37-182">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="d3b37-183">изменяться на уровне пользователей</span><span class="sxs-lookup"><span data-stu-id="d3b37-183">vary-by-user</span></span>

| <span data-ttu-id="d3b37-184">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-184">Attribute Type</span></span>    | <span data-ttu-id="d3b37-185">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="d3b37-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="d3b37-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="d3b37-186">Boolean</span></span>             | <span data-ttu-id="d3b37-187">"true"</span><span class="sxs-lookup"><span data-stu-id="d3b37-187">"true"</span></span>                  |
|                     | <span data-ttu-id="d3b37-188">«false» (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="d3b37-188">"false" (default)</span></span> |

<span data-ttu-id="d3b37-189">Указывает, должна ли сбросить кэш, при изменении пользователя, выполнившего вход (или участника контекста).</span><span class="sxs-lookup"><span data-stu-id="d3b37-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="d3b37-190">Текущий пользователь, также называемая участника контекста запроса и можно просмотреть с помощью ссылки на представления Razor `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="d3b37-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="d3b37-191">Следующий пример просматривает текущего вошедшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="d3b37-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="d3b37-192">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="d3b37-193">С помощью этого атрибута сохраняет содержимое в кэше журнала и журнала цикла.</span><span class="sxs-lookup"><span data-stu-id="d3b37-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="d3b37-194">При использовании `vary-by-user="true"`, журнала и журнала действий делает недействительным кэш для проверенного пользователя.</span><span class="sxs-lookup"><span data-stu-id="d3b37-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="d3b37-195">Кэш становится недействительным, поскольку новое значение уникальный файл cookie создается при входе в систему.</span><span class="sxs-lookup"><span data-stu-id="d3b37-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="d3b37-196">Кэш хранится для анонимных состояния при объекты cookie не существует или уже истек.</span><span class="sxs-lookup"><span data-stu-id="d3b37-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="d3b37-197">Это означает, что если пользователь не входит в систему, будет поддерживаться кэша.</span><span class="sxs-lookup"><span data-stu-id="d3b37-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="d3b37-198">изменяющиеся</span><span class="sxs-lookup"><span data-stu-id="d3b37-198">vary-by</span></span>

| <span data-ttu-id="d3b37-199">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-199">Attribute Type</span></span>    | <span data-ttu-id="d3b37-200">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="d3b37-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="d3b37-201">String</span><span class="sxs-lookup"><span data-stu-id="d3b37-201">String</span></span>             | <span data-ttu-id="d3b37-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="d3b37-202">"@Model"</span></span>                 |


<span data-ttu-id="d3b37-203">Позволяет настраивать Возвращает кэшированные данные.</span><span class="sxs-lookup"><span data-stu-id="d3b37-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="d3b37-204">При обновлении объект, упоминаемый в строковое значение атрибута изменяется, содержимое тега вспомогательный объект кэша.</span><span class="sxs-lookup"><span data-stu-id="d3b37-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="d3b37-205">Часто объединение строк значений модели назначаются данному атрибуту.</span><span class="sxs-lookup"><span data-stu-id="d3b37-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="d3b37-206">По сути, это означает, что делает кэш недействительным обновления к любому из объединенных значений.</span><span class="sxs-lookup"><span data-stu-id="d3b37-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="d3b37-207">В следующем примере предполагается визуализации представления суммы целочисленное значение между двумя параметрами маршрута метода контроллера `myParam1` и `myParam2`и возвращает, как свойство одной модели.</span><span class="sxs-lookup"><span data-stu-id="d3b37-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="d3b37-208">При изменении этой суммы, содержимое тега вспомогательного метода кэша к просмотру и снова кэшируется.</span><span class="sxs-lookup"><span data-stu-id="d3b37-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="d3b37-209">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-209">Example:</span></span>

<span data-ttu-id="d3b37-210">Действие:</span><span class="sxs-lookup"><span data-stu-id="d3b37-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="d3b37-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="d3b37-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="d3b37-212">priority</span><span class="sxs-lookup"><span data-stu-id="d3b37-212">priority</span></span>

| <span data-ttu-id="d3b37-213">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="d3b37-213">Attribute Type</span></span>    | <span data-ttu-id="d3b37-214">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="d3b37-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="d3b37-215">Перечисления CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="d3b37-215">CacheItemPriority</span></span>  | <span data-ttu-id="d3b37-216">«Высокие»</span><span class="sxs-lookup"><span data-stu-id="d3b37-216">"High"</span></span>                   |
|                    | <span data-ttu-id="d3b37-217">«Низкое»</span><span class="sxs-lookup"><span data-stu-id="d3b37-217">"Low"</span></span> |
|                    | <span data-ttu-id="d3b37-218">«NeverRemove»</span><span class="sxs-lookup"><span data-stu-id="d3b37-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="d3b37-219">«Normal»</span><span class="sxs-lookup"><span data-stu-id="d3b37-219">"Normal"</span></span> |

<span data-ttu-id="d3b37-220">Руководство вытеснения кэша поставщику встроенного кэша.</span><span class="sxs-lookup"><span data-stu-id="d3b37-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="d3b37-221">Веб-сервер будет вытеснять `Low` первым кэша записи при нехватке памяти.</span><span class="sxs-lookup"><span data-stu-id="d3b37-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="d3b37-222">Пример</span><span class="sxs-lookup"><span data-stu-id="d3b37-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="d3b37-223">`priority` Атрибута не гарантирует определенный уровень хранения кэша.</span><span class="sxs-lookup"><span data-stu-id="d3b37-223">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="d3b37-224">`CacheItemPriority`— только это предложение.</span><span class="sxs-lookup"><span data-stu-id="d3b37-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="d3b37-225">Задав этому атрибуту значение `NeverRemove` не гарантирует, что кэша всегда будет сохранено.</span><span class="sxs-lookup"><span data-stu-id="d3b37-225">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="d3b37-226">В разделе [дополнительные ресурсы](#additional-resources) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="d3b37-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="d3b37-227">Вспомогательный тег кэша зависит от [памяти служба кэша](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="d3b37-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="d3b37-228">Вспомогательный тег кэша добавляет службу, если он не был добавлен.</span><span class="sxs-lookup"><span data-stu-id="d3b37-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d3b37-229">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d3b37-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
