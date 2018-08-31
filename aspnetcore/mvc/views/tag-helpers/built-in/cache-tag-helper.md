---
title: Вспомогательная функция тегов кэша в MVC-моделях ASP.NET Core
author: pkellner
description: Сведения о работе со вспомогательной функцией тега кэша
ms.author: riande
ms.date: 02/14/2017
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 425d8c2235f0070665bc0c967d2498f2cff2a4a6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751680"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="62a8d-103">Вспомогательная функция тегов кэша в MVC-моделях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="62a8d-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="62a8d-104">Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="62a8d-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="62a8d-105">Вспомогательная функция тегов кэша позволяет существенно повысить производительность приложения ASP.NET Core за счет кэширования его содержимого во внутренний поставщик кэша ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62a8d-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="62a8d-106">Модуль представлений Razor задает для `expires-after` по умолчанию значение 20 минут.</span><span class="sxs-lookup"><span data-stu-id="62a8d-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="62a8d-107">Приведенная ниже разметка Razor кэширует значения даты и времени:</span><span class="sxs-lookup"><span data-stu-id="62a8d-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="62a8d-108">Первый запрос к странице, содержащей `CacheTagHelper`, отобразит текущую дату и время.</span><span class="sxs-lookup"><span data-stu-id="62a8d-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="62a8d-109">Последующие запросы будут показывать кэшированное значение, пока срок действия кэша не истечет (по умолчанию — 20 минут) или пока кэш не будет удален из-за нехватки памяти.</span><span class="sxs-lookup"><span data-stu-id="62a8d-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="62a8d-110">Вы можете задать продолжительность хранения кэша с помощью следующих атрибутов:</span><span class="sxs-lookup"><span data-stu-id="62a8d-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="62a8d-111">Атрибуты вспомогательной функции тегов кэша</span><span class="sxs-lookup"><span data-stu-id="62a8d-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="62a8d-112">enabled</span><span class="sxs-lookup"><span data-stu-id="62a8d-112">enabled</span></span>    


| <span data-ttu-id="62a8d-113">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-113">Attribute Type</span></span>    | <span data-ttu-id="62a8d-114">Допустимые значения</span><span class="sxs-lookup"><span data-stu-id="62a8d-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="62a8d-115">boolean</span><span class="sxs-lookup"><span data-stu-id="62a8d-115">boolean</span></span>           | <span data-ttu-id="62a8d-116">"true" (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="62a8d-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="62a8d-117">"false"</span><span class="sxs-lookup"><span data-stu-id="62a8d-117">"false"</span></span>   |


<span data-ttu-id="62a8d-118">Определяет, кэшируется ли содержимое, охватываемое вспомогательной функцией тегов кэша.</span><span class="sxs-lookup"><span data-stu-id="62a8d-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="62a8d-119">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="62a8d-119">The default is `true`.</span></span>  <span data-ttu-id="62a8d-120">Если задано значение `false`, функция не применяет кэширование к выводимым данным.</span><span class="sxs-lookup"><span data-stu-id="62a8d-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="62a8d-121">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="62a8d-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="62a8d-122">expires-on</span></span> 

| <span data-ttu-id="62a8d-123">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-123">Attribute Type</span></span> |           <span data-ttu-id="62a8d-124">Пример значения</span><span class="sxs-lookup"><span data-stu-id="62a8d-124">Example Value</span></span>            |
|----------------|------------------------------------|
| <span data-ttu-id="62a8d-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="62a8d-125">DateTimeOffset</span></span> | <span data-ttu-id="62a8d-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="62a8d-126">"@new DateTime(2025,1,29,17,02,0)"</span></span> |

<span data-ttu-id="62a8d-127">Задает абсолютную дату окончания срока действия.</span><span class="sxs-lookup"><span data-stu-id="62a8d-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="62a8d-128">В следующем примере содержимое вспомогательной функции тегов кэша будет кэшировано до 29 января 2025 г., 17:02.</span><span class="sxs-lookup"><span data-stu-id="62a8d-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="62a8d-129">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="62a8d-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="62a8d-130">expires-after</span></span>

| <span data-ttu-id="62a8d-131">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-131">Attribute Type</span></span> |        <span data-ttu-id="62a8d-132">Пример значения</span><span class="sxs-lookup"><span data-stu-id="62a8d-132">Example Value</span></span>         |
|----------------|------------------------------|
|    <span data-ttu-id="62a8d-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="62a8d-133">TimeSpan</span></span>    | <span data-ttu-id="62a8d-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="62a8d-134">"@TimeSpan.FromSeconds(120)"</span></span> |

<span data-ttu-id="62a8d-135">Задает интервал времени для кэширования содержимого с момента первого запроса.</span><span class="sxs-lookup"><span data-stu-id="62a8d-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="62a8d-136">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="62a8d-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="62a8d-137">expires-sliding</span></span>

| <span data-ttu-id="62a8d-138">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-138">Attribute Type</span></span> |        <span data-ttu-id="62a8d-139">Пример значения</span><span class="sxs-lookup"><span data-stu-id="62a8d-139">Example Value</span></span>        |
|----------------|-----------------------------|
|    <span data-ttu-id="62a8d-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="62a8d-140">TimeSpan</span></span>    | <span data-ttu-id="62a8d-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="62a8d-141">"@TimeSpan.FromSeconds(60)"</span></span> |

<span data-ttu-id="62a8d-142">Задает время, по истечении которого запись кэша следует удалить, если к ней не было обращений.</span><span class="sxs-lookup"><span data-stu-id="62a8d-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="62a8d-143">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="62a8d-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="62a8d-144">vary-by-header</span></span>

| <span data-ttu-id="62a8d-145">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-145">Attribute Type</span></span>    | <span data-ttu-id="62a8d-146">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="62a8d-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62a8d-147">String</span><span class="sxs-lookup"><span data-stu-id="62a8d-147">String</span></span>            | <span data-ttu-id="62a8d-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="62a8d-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="62a8d-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="62a8d-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="62a8d-150">Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при их изменении.</span><span class="sxs-lookup"><span data-stu-id="62a8d-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="62a8d-151">Следующий пример показывает отслеживание значения заголовка `User-Agent`.</span><span class="sxs-lookup"><span data-stu-id="62a8d-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="62a8d-152">Содержимое будет кэшироваться для каждого отдельного заголовка `User-Agent`, представленного на веб-сервере.</span><span class="sxs-lookup"><span data-stu-id="62a8d-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="62a8d-153">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="62a8d-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="62a8d-154">vary-by-query</span></span>

| <span data-ttu-id="62a8d-155">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-155">Attribute Type</span></span>    | <span data-ttu-id="62a8d-156">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="62a8d-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62a8d-157">String</span><span class="sxs-lookup"><span data-stu-id="62a8d-157">String</span></span>            | <span data-ttu-id="62a8d-158">"Make"</span><span class="sxs-lookup"><span data-stu-id="62a8d-158">"Make"</span></span>                |
|                   | <span data-ttu-id="62a8d-159">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="62a8d-159">"Make,Model"</span></span> |

<span data-ttu-id="62a8d-160">Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при их изменении.</span><span class="sxs-lookup"><span data-stu-id="62a8d-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="62a8d-161">Следующий пример рассматривает значения `Make` и `Model`.</span><span class="sxs-lookup"><span data-stu-id="62a8d-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="62a8d-162">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="62a8d-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="62a8d-163">vary-by-route</span></span>

| <span data-ttu-id="62a8d-164">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-164">Attribute Type</span></span>    | <span data-ttu-id="62a8d-165">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="62a8d-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62a8d-166">String</span><span class="sxs-lookup"><span data-stu-id="62a8d-166">String</span></span>            | <span data-ttu-id="62a8d-167">"Make"</span><span class="sxs-lookup"><span data-stu-id="62a8d-167">"Make"</span></span>                |
|                   | <span data-ttu-id="62a8d-168">"Make,Model"</span><span class="sxs-lookup"><span data-stu-id="62a8d-168">"Make,Model"</span></span> |

<span data-ttu-id="62a8d-169">Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при изменении значений параметров данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="62a8d-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="62a8d-170">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-170">Example:</span></span>

<span data-ttu-id="62a8d-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="62a8d-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="62a8d-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a8d-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="62a8d-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="62a8d-173">vary-by-cookie</span></span>

| <span data-ttu-id="62a8d-174">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-174">Attribute Type</span></span>    | <span data-ttu-id="62a8d-175">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="62a8d-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62a8d-176">String</span><span class="sxs-lookup"><span data-stu-id="62a8d-176">String</span></span>            | <span data-ttu-id="62a8d-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="62a8d-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="62a8d-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="62a8d-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="62a8d-179">Принимает одно значение заголовка или список разделенных запятыми значений заголовков, запускающих обновление кэша при их изменении.</span><span class="sxs-lookup"><span data-stu-id="62a8d-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="62a8d-180">Следующий пример рассматривает файл cookie, связанный с удостоверением ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="62a8d-180">The following example looks at the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="62a8d-181">Когда пользователь проходит проверку подлинности, запрос задает файл cookie, запускающий обновление кэша.</span><span class="sxs-lookup"><span data-stu-id="62a8d-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="62a8d-182">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="62a8d-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="62a8d-183">vary-by-user</span></span>

| <span data-ttu-id="62a8d-184">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-184">Attribute Type</span></span>    | <span data-ttu-id="62a8d-185">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="62a8d-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62a8d-186">Boolean</span><span class="sxs-lookup"><span data-stu-id="62a8d-186">Boolean</span></span>             | <span data-ttu-id="62a8d-187">"true"</span><span class="sxs-lookup"><span data-stu-id="62a8d-187">"true"</span></span>                  |
|                     | <span data-ttu-id="62a8d-188">"false" (по умолчанию)</span><span class="sxs-lookup"><span data-stu-id="62a8d-188">"false" (default)</span></span> |

<span data-ttu-id="62a8d-189">Указывает, следует ли сбрасывать кэш при изменении вошедшего в систему пользователя (или участника контекста).</span><span class="sxs-lookup"><span data-stu-id="62a8d-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="62a8d-190">Текущий пользователь также называется участником контекста запроса и доступен для просмотра в представлении Razor с помощью ссылки на `@User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="62a8d-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="62a8d-191">В следующем примере показан текущий пользователь, выполнивший вход.</span><span class="sxs-lookup"><span data-stu-id="62a8d-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="62a8d-192">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="62a8d-193">При использовании этого атрибута содержимое сохраняется в кэше в течение цикла входа и выхода.</span><span class="sxs-lookup"><span data-stu-id="62a8d-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="62a8d-194">При использовании `vary-by-user="true"` действие входа или выхода сбрасывает кэш для прошедшего проверку подлинности пользователя.</span><span class="sxs-lookup"><span data-stu-id="62a8d-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="62a8d-195">Кэш становится недействительным, так как при входе в систему создается уникальное значение файла cookie.</span><span class="sxs-lookup"><span data-stu-id="62a8d-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="62a8d-196">Кэш сохраняется в состоянии анонимности, когда значения cookie не существует или срок его действия истек.</span><span class="sxs-lookup"><span data-stu-id="62a8d-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="62a8d-197">Это означает, что когда никто не вошел в систему, кэш будет сохраняться.</span><span class="sxs-lookup"><span data-stu-id="62a8d-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="62a8d-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="62a8d-198">vary-by</span></span>

| <span data-ttu-id="62a8d-199">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-199">Attribute Type</span></span> | <span data-ttu-id="62a8d-200">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="62a8d-200">Example Values</span></span> |
|----------------|----------------|
|     <span data-ttu-id="62a8d-201">String</span><span class="sxs-lookup"><span data-stu-id="62a8d-201">String</span></span>     |    <span data-ttu-id="62a8d-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="62a8d-202">"@Model"</span></span>    |

<span data-ttu-id="62a8d-203">Позволяет настраивать, какие данные кэшируются.</span><span class="sxs-lookup"><span data-stu-id="62a8d-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="62a8d-204">Содержимое вспомогательной функции тегов кэша обновляется при изменении объекта, на который ссылается строковое значение атрибута.</span><span class="sxs-lookup"><span data-stu-id="62a8d-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="62a8d-205">Часто этому атрибуту назначается объединенная строка значений модели.</span><span class="sxs-lookup"><span data-stu-id="62a8d-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="62a8d-206">По сути это означает, что обновление любого из объединенных значений приводит к сбросу кэша.</span><span class="sxs-lookup"><span data-stu-id="62a8d-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="62a8d-207">В следующем примере предполагается, что метод контроллера, визуализирующий представление, суммирует целочисленные значения двух параметров маршрута (`myParam1` и `myParam2`) и возвращает итог как одно свойство модели.</span><span class="sxs-lookup"><span data-stu-id="62a8d-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="62a8d-208">При изменении этой суммы содержимое вспомогательной функции тегов кэша визуализируется и кэшируется заново.</span><span class="sxs-lookup"><span data-stu-id="62a8d-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="62a8d-209">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-209">Example:</span></span>

<span data-ttu-id="62a8d-210">Действие:</span><span class="sxs-lookup"><span data-stu-id="62a8d-210">Action:</span></span>

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

<span data-ttu-id="62a8d-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="62a8d-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="62a8d-212">priority</span><span class="sxs-lookup"><span data-stu-id="62a8d-212">priority</span></span>

| <span data-ttu-id="62a8d-213">Тип атрибута</span><span class="sxs-lookup"><span data-stu-id="62a8d-213">Attribute Type</span></span>    | <span data-ttu-id="62a8d-214">Примеры значений</span><span class="sxs-lookup"><span data-stu-id="62a8d-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="62a8d-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="62a8d-215">CacheItemPriority</span></span>  | <span data-ttu-id="62a8d-216">"High"</span><span class="sxs-lookup"><span data-stu-id="62a8d-216">"High"</span></span>                   |
|                    | <span data-ttu-id="62a8d-217">"Low"</span><span class="sxs-lookup"><span data-stu-id="62a8d-217">"Low"</span></span> |
|                    | <span data-ttu-id="62a8d-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="62a8d-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="62a8d-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="62a8d-219">"Normal"</span></span> |

<span data-ttu-id="62a8d-220">Предоставляет встроенному поставщику кэша инструкции по удалению кэша.</span><span class="sxs-lookup"><span data-stu-id="62a8d-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="62a8d-221">При нехватке памяти веб-сервер будет первыми удалять записи кэша с приоритетом `Low`.</span><span class="sxs-lookup"><span data-stu-id="62a8d-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="62a8d-222">Пример</span><span class="sxs-lookup"><span data-stu-id="62a8d-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="62a8d-223">Атрибут `priority` не гарантирует определенный уровень периода удержания кэша.</span><span class="sxs-lookup"><span data-stu-id="62a8d-223">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="62a8d-224">`CacheItemPriority` носит лишь рекомендательный характер.</span><span class="sxs-lookup"><span data-stu-id="62a8d-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="62a8d-225">Установка для этого атрибута значения `NeverRemove` не гарантирует постоянное хранение кэша.</span><span class="sxs-lookup"><span data-stu-id="62a8d-225">Setting this attribute to `NeverRemove` doesn't guarantee that the cache will always be retained.</span></span> <span data-ttu-id="62a8d-226">См. [Дополнительные ресурсы](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="62a8d-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="62a8d-227">Вспомогательная функция тегов кэша зависит от [службы кэширования в памяти](xref:performance/caching/memory).</span><span class="sxs-lookup"><span data-stu-id="62a8d-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="62a8d-228">Вспомогательная функция тегов кэша добавляет эту службу, если она не добавлена.</span><span class="sxs-lookup"><span data-stu-id="62a8d-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="62a8d-229">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="62a8d-229">Additional resources</span></span>

* [<span data-ttu-id="62a8d-230">Кэш в памяти</span><span class="sxs-lookup"><span data-stu-id="62a8d-230">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="62a8d-231">Общие сведения об Identity</span><span class="sxs-lookup"><span data-stu-id="62a8d-231">Introduction to Identity</span></span>](xref:security/authentication/identity)
