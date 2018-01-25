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
ms.openlocfilehash: 10aa1b493dbd0672cac789f6e48ddf2f14ba35dc
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="4fce1-103">Кэшировать вспомогательный тег в ядре ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="4fce1-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="4fce1-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="4fce1-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4fce1-105">Вспомогательный тег кэша позволяет существенно повысить производительность приложения ASP.NET Core путем кэширования ее содержимое, чтобы внутренний поставщик кэша ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4fce1-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="4fce1-106">Значение по умолчанию задает представлений Razor `expires-after` до 20 минут.</span><span class="sxs-lookup"><span data-stu-id="4fce1-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="4fce1-107">Приведенная ниже разметка Razor кэширует даты и времени:</span><span class="sxs-lookup"><span data-stu-id="4fce1-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="4fce1-108">Первый запрос страницы, содержащей `CacheTagHelper` отобразит текущее значение даты и времени.</span><span class="sxs-lookup"><span data-stu-id="4fce1-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="4fce1-109">Дополнительные запросы покажет кэшированное значение, до истечения срока действия (по умолчанию 20 минут) или исключен с нехваткой памяти в кэше.</span><span class="sxs-lookup"><span data-stu-id="4fce1-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="4fce1-110">Можно задать длительность кэширования со следующими атрибутами:</span><span class="sxs-lookup"><span data-stu-id="4fce1-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="4fce1-111">Кэшировать вспомогательные атрибуты тега</span><span class="sxs-lookup"><span data-stu-id="4fce1-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="4fce1-112">enabled</span><span class="sxs-lookup"><span data-stu-id="4fce1-112">enabled</span></span>    


| <span data-ttu-id="4fce1-113">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-113">Attribute Type</span></span>    | <span data-ttu-id="4fce1-114">Допустимые значения</span><span class="sxs-lookup"><span data-stu-id="4fce1-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="4fce1-115">boolean</span><span class="sxs-lookup"><span data-stu-id="4fce1-115">boolean</span></span>           | <span data-ttu-id="4fce1-116">«true» (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="4fce1-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="4fce1-117">"false"</span><span class="sxs-lookup"><span data-stu-id="4fce1-117">"false"</span></span>   |


<span data-ttu-id="4fce1-118">Определяет, кэшируются ли содержимое, заключенное в тег вспомогательный объект кэша.</span><span class="sxs-lookup"><span data-stu-id="4fce1-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="4fce1-119">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="4fce1-119">The default is `true`.</span></span>  <span data-ttu-id="4fce1-120">Если значение `false` этого вспомогательного объекта тег кэша не будет действовать кэширования на выводимых данных.</span><span class="sxs-lookup"><span data-stu-id="4fce1-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="4fce1-121">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="4fce1-122">Срок действия истекает в</span><span class="sxs-lookup"><span data-stu-id="4fce1-122">expires-on</span></span> 

| <span data-ttu-id="4fce1-123">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-123">Attribute Type</span></span>    | <span data-ttu-id="4fce1-124">Пример значения</span><span class="sxs-lookup"><span data-stu-id="4fce1-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4fce1-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4fce1-125">DateTimeOffset</span></span>    | <span data-ttu-id="4fce1-126">«@new DateTime(2025,1,29,17,02,0)»</span><span class="sxs-lookup"><span data-stu-id="4fce1-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="4fce1-127">Задает абсолютный срок действия.</span><span class="sxs-lookup"><span data-stu-id="4fce1-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="4fce1-128">Следующий пример будет кэшировать содержимое тега вспомогательный объект кэша до 17:02:00 на 29 января 2025.</span><span class="sxs-lookup"><span data-stu-id="4fce1-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="4fce1-129">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="4fce1-130">После истечения срока действия</span><span class="sxs-lookup"><span data-stu-id="4fce1-130">expires-after</span></span>

| <span data-ttu-id="4fce1-131">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-131">Attribute Type</span></span>    | <span data-ttu-id="4fce1-132">Пример значения</span><span class="sxs-lookup"><span data-stu-id="4fce1-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4fce1-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4fce1-133">TimeSpan</span></span>    | <span data-ttu-id="4fce1-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="4fce1-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="4fce1-135">Задает интервал времени с момента первого запроса для кэширования содержимого.</span><span class="sxs-lookup"><span data-stu-id="4fce1-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="4fce1-136">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="4fce1-137">скользящий срок действия истекает</span><span class="sxs-lookup"><span data-stu-id="4fce1-137">expires-sliding</span></span>

| <span data-ttu-id="4fce1-138">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-138">Attribute Type</span></span>    | <span data-ttu-id="4fce1-139">Пример значения</span><span class="sxs-lookup"><span data-stu-id="4fce1-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="4fce1-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4fce1-140">TimeSpan</span></span>    | <span data-ttu-id="4fce1-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="4fce1-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="4fce1-142">Задает время, следует удалить запись кэша, если оно не было обращений.</span><span class="sxs-lookup"><span data-stu-id="4fce1-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="4fce1-143">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="4fce1-144">различаются по заголовок</span><span class="sxs-lookup"><span data-stu-id="4fce1-144">vary-by-header</span></span>

| <span data-ttu-id="4fce1-145">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-145">Attribute Type</span></span>    | <span data-ttu-id="4fce1-146">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="4fce1-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4fce1-147">String</span><span class="sxs-lookup"><span data-stu-id="4fce1-147">String</span></span>            | <span data-ttu-id="4fce1-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="4fce1-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="4fce1-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="4fce1-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="4fce1-150">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, запускающих обновление кэша, при их изменения.</span><span class="sxs-lookup"><span data-stu-id="4fce1-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="4fce1-151">Следующий пример отслеживает значение заголовка `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="4fce1-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="4fce1-152">Пример будет кэшировать содержимое для каждого разные `User-Agent` представлены на веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="4fce1-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="4fce1-153">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="4fce1-154">различаются по запроса</span><span class="sxs-lookup"><span data-stu-id="4fce1-154">vary-by-query</span></span>

| <span data-ttu-id="4fce1-155">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-155">Attribute Type</span></span>    | <span data-ttu-id="4fce1-156">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="4fce1-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4fce1-157">String</span><span class="sxs-lookup"><span data-stu-id="4fce1-157">String</span></span>            | <span data-ttu-id="4fce1-158">«Обеспечить»</span><span class="sxs-lookup"><span data-stu-id="4fce1-158">"Make"</span></span>                |
|                   | <span data-ttu-id="4fce1-159">«Создание модели»</span><span class="sxs-lookup"><span data-stu-id="4fce1-159">"Make,Model"</span></span> |

<span data-ttu-id="4fce1-160">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, при изменении значения заголовка.</span><span class="sxs-lookup"><span data-stu-id="4fce1-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="4fce1-161">Следующий пример просматривает значения `Make` и `Model`.</span><span class="sxs-lookup"><span data-stu-id="4fce1-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="4fce1-162">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="4fce1-163">различаются по Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="4fce1-163">vary-by-route</span></span>

| <span data-ttu-id="4fce1-164">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-164">Attribute Type</span></span>    | <span data-ttu-id="4fce1-165">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="4fce1-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4fce1-166">String</span><span class="sxs-lookup"><span data-stu-id="4fce1-166">String</span></span>            | <span data-ttu-id="4fce1-167">«Обеспечить»</span><span class="sxs-lookup"><span data-stu-id="4fce1-167">"Make"</span></span>                |
|                   | <span data-ttu-id="4fce1-168">«Создание модели»</span><span class="sxs-lookup"><span data-stu-id="4fce1-168">"Make,Model"</span></span> |

<span data-ttu-id="4fce1-169">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, когда изменение значения параметра данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="4fce1-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="4fce1-170">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-170">Example:</span></span>

<span data-ttu-id="4fce1-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="4fce1-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="4fce1-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4fce1-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="4fce1-173">различаются по cookie</span><span class="sxs-lookup"><span data-stu-id="4fce1-173">vary-by-cookie</span></span>

| <span data-ttu-id="4fce1-174">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-174">Attribute Type</span></span>    | <span data-ttu-id="4fce1-175">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="4fce1-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4fce1-176">String</span><span class="sxs-lookup"><span data-stu-id="4fce1-176">String</span></span>            | <span data-ttu-id="4fce1-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="4fce1-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="4fce1-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="4fce1-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="4fce1-179">Принимает значение одного заголовка или список разделенных запятыми значений заголовка, которые запускают обновления кэша, когда изменение (s) значения заголовка.</span><span class="sxs-lookup"><span data-stu-id="4fce1-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="4fce1-180">Следующий пример просматривает файл cookie, связанные с ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="4fce1-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="4fce1-181">Когда пользователь проходит проверку подлинности запроса куки-файл задать активизирующий обновления кэша.</span><span class="sxs-lookup"><span data-stu-id="4fce1-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="4fce1-182">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="4fce1-183">изменяться на уровне пользователей</span><span class="sxs-lookup"><span data-stu-id="4fce1-183">vary-by-user</span></span>

| <span data-ttu-id="4fce1-184">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-184">Attribute Type</span></span>    | <span data-ttu-id="4fce1-185">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="4fce1-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4fce1-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="4fce1-186">Boolean</span></span>             | <span data-ttu-id="4fce1-187">"true"</span><span class="sxs-lookup"><span data-stu-id="4fce1-187">"true"</span></span>                  |
|                     | <span data-ttu-id="4fce1-188">«false» (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="4fce1-188">"false" (default)</span></span> |

<span data-ttu-id="4fce1-189">Указывает, должна ли сбросить кэш, при изменении пользователя, выполнившего вход (или участника контекста).</span><span class="sxs-lookup"><span data-stu-id="4fce1-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="4fce1-190">Текущий пользователь, также называемая участника контекста запроса и можно просмотреть с помощью ссылки на представления Razor `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="4fce1-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="4fce1-191">Следующий пример просматривает текущего вошедшего пользователя.</span><span class="sxs-lookup"><span data-stu-id="4fce1-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="4fce1-192">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="4fce1-193">С помощью этого атрибута сохраняет содержимое в кэше журнала и журнала цикла.</span><span class="sxs-lookup"><span data-stu-id="4fce1-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="4fce1-194">При использовании `vary-by-user="true"`, журнала и журнала действий делает недействительным кэш для проверенного пользователя.</span><span class="sxs-lookup"><span data-stu-id="4fce1-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="4fce1-195">Кэш становится недействительным, поскольку новое значение уникальный файл cookie создается при входе в систему.</span><span class="sxs-lookup"><span data-stu-id="4fce1-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="4fce1-196">Кэш хранится для анонимных состояния при объекты cookie не существует или уже истек.</span><span class="sxs-lookup"><span data-stu-id="4fce1-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="4fce1-197">Это означает, что если пользователь не входит в систему, будет поддерживаться кэша.</span><span class="sxs-lookup"><span data-stu-id="4fce1-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="4fce1-198">изменяющиеся</span><span class="sxs-lookup"><span data-stu-id="4fce1-198">vary-by</span></span>

| <span data-ttu-id="4fce1-199">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-199">Attribute Type</span></span>    | <span data-ttu-id="4fce1-200">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="4fce1-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4fce1-201">String</span><span class="sxs-lookup"><span data-stu-id="4fce1-201">String</span></span>             | <span data-ttu-id="4fce1-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="4fce1-202">"@Model"</span></span>                 |


<span data-ttu-id="4fce1-203">Позволяет настраивать Возвращает кэшированные данные.</span><span class="sxs-lookup"><span data-stu-id="4fce1-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="4fce1-204">При обновлении объект, упоминаемый в строковое значение атрибута изменяется, содержимое тега вспомогательный объект кэша.</span><span class="sxs-lookup"><span data-stu-id="4fce1-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="4fce1-205">Часто объединение строк значений модели назначаются данному атрибуту.</span><span class="sxs-lookup"><span data-stu-id="4fce1-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="4fce1-206">По сути, это означает, что делает кэш недействительным обновления к любому из объединенных значений.</span><span class="sxs-lookup"><span data-stu-id="4fce1-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="4fce1-207">В следующем примере предполагается визуализации представления суммы целочисленное значение между двумя параметрами маршрута метода контроллера `myParam1` и `myParam2`и возвращает, как свойство одной модели.</span><span class="sxs-lookup"><span data-stu-id="4fce1-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="4fce1-208">При изменении этой суммы, содержимое тега вспомогательного метода кэша к просмотру и снова кэшируется.</span><span class="sxs-lookup"><span data-stu-id="4fce1-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="4fce1-209">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-209">Example:</span></span>

<span data-ttu-id="4fce1-210">Действие:</span><span class="sxs-lookup"><span data-stu-id="4fce1-210">Action:</span></span>

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

<span data-ttu-id="4fce1-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4fce1-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="4fce1-212">priority</span><span class="sxs-lookup"><span data-stu-id="4fce1-212">priority</span></span>

| <span data-ttu-id="4fce1-213">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="4fce1-213">Attribute Type</span></span>    | <span data-ttu-id="4fce1-214">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="4fce1-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="4fce1-215">Перечисления CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="4fce1-215">CacheItemPriority</span></span>  | <span data-ttu-id="4fce1-216">«Высокие»</span><span class="sxs-lookup"><span data-stu-id="4fce1-216">"High"</span></span>                   |
|                    | <span data-ttu-id="4fce1-217">«Низкое»</span><span class="sxs-lookup"><span data-stu-id="4fce1-217">"Low"</span></span> |
|                    | <span data-ttu-id="4fce1-218">«NeverRemove»</span><span class="sxs-lookup"><span data-stu-id="4fce1-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="4fce1-219">«Normal»</span><span class="sxs-lookup"><span data-stu-id="4fce1-219">"Normal"</span></span> |

<span data-ttu-id="4fce1-220">Руководство вытеснения кэша поставщику встроенного кэша.</span><span class="sxs-lookup"><span data-stu-id="4fce1-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="4fce1-221">Веб-сервер будет вытеснять `Low` первым кэша записи при нехватке памяти.</span><span class="sxs-lookup"><span data-stu-id="4fce1-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="4fce1-222">Пример</span><span class="sxs-lookup"><span data-stu-id="4fce1-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="4fce1-223">`priority` Атрибута не гарантирует определенный уровень хранения кэша.</span><span class="sxs-lookup"><span data-stu-id="4fce1-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="4fce1-224">`CacheItemPriority`— только это предложение.</span><span class="sxs-lookup"><span data-stu-id="4fce1-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="4fce1-225">Задав этому атрибуту значение `NeverRemove` не гарантирует, что кэша всегда будет сохранено.</span><span class="sxs-lookup"><span data-stu-id="4fce1-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="4fce1-226">В разделе [дополнительные ресурсы](#additional-resources) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="4fce1-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="4fce1-227">Вспомогательный тег кэша зависит от [памяти служба кэша](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="4fce1-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="4fce1-228">Вспомогательный тег кэша добавляет службу, если он не был добавлен.</span><span class="sxs-lookup"><span data-stu-id="4fce1-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4fce1-229">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4fce1-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
