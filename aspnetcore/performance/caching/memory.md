---
title: Кэш в памяти в ASP.NET Core
author: rick-anderson
description: Узнайте, как кэширование данных в памяти в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 7/22/2018
uid: performance/caching/memory
ms.openlocfilehash: b57e29965edc791ad4ecfe1b6b863a4a3dbe3f09
ms.sourcegitcommit: 506a199274e9fe5fb4070b273ba94f29f14cb619
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/28/2018
ms.locfileid: "39332305"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="1afa6-103">Кэш в памяти в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1afa6-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="1afa6-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Luo Джон](https://github.com/JunTaoLuo), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1afa6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1afa6-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1afa6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="1afa6-106">Кэширование основы</span><span class="sxs-lookup"><span data-stu-id="1afa6-106">Caching basics</span></span>

<span data-ttu-id="1afa6-107">Кэширование может значительно повысить производительность и масштабируемость приложения путем уменьшения работ, необходимый для создания содержимого.</span><span class="sxs-lookup"><span data-stu-id="1afa6-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="1afa6-108">Кэширование будет работать лучше всего с данными, которые редко изменяются.</span><span class="sxs-lookup"><span data-stu-id="1afa6-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="1afa6-109">Кэширование делает копию данных, которые могут быть возвращены большую быстрее, чем из исходного источника.</span><span class="sxs-lookup"><span data-stu-id="1afa6-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="1afa6-110">Вы должны писать и тестировать приложения никогда не зависят от кэшированных данных.</span><span class="sxs-lookup"><span data-stu-id="1afa6-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="1afa6-111">ASP.NET Core поддерживает несколько разных кэшах.</span><span class="sxs-lookup"><span data-stu-id="1afa6-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="1afa6-112">Самый простой кэш основан на [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), который представляет кэш хранится в памяти веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="1afa6-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="1afa6-113">Приложения, которые выполняются на нескольких серверах фермы следует убедиться, что прикрепленные сеансы не при использовании кэша в памяти.</span><span class="sxs-lookup"><span data-stu-id="1afa6-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="1afa6-114">Прикрепленные сеансы убедитесь, что последующие запросы от всех клиента перейдите к тому же серверу.</span><span class="sxs-lookup"><span data-stu-id="1afa6-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="1afa6-115">Например, приложения используют Azure Web [маршрутизации запросов приложений](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), чтобы перенаправлять все последующие запросы на тот же сервер.</span><span class="sxs-lookup"><span data-stu-id="1afa6-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="1afa6-116">Non прикрепленные сеансы на веб-ферме требуется [распределенный кеш](distributed.md) во избежание проблем с согласованностью кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="1afa6-117">Для некоторых приложений распределенного кэша может поддерживать более горизонтальное масштабирование, чем кэш, расположенный в памяти.</span><span class="sxs-lookup"><span data-stu-id="1afa6-117">For some apps, a distributed cache can support higher scale-out than an in-memory cache.</span></span> <span data-ttu-id="1afa6-118">С помощью распределенного кэша разгружает кэш-памяти во внешний процесс.</span><span class="sxs-lookup"><span data-stu-id="1afa6-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="1afa6-119">`IMemoryCache` Кэша вытеснит записей кэша при недостатке свободной памяти, если не [кэшировать приоритет](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) присваивается `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="1afa6-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="1afa6-120">Можно задать `CacheItemPriority` изменять приоритет, с которым кэша исключает элементы при недостатке свободной памяти.</span><span class="sxs-lookup"><span data-stu-id="1afa6-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="1afa6-121">Кэш в памяти можно хранить любой объект; интерфейс распределенного кэша ограничен `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1afa6-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

### <a name="cache-guidelines"></a><span data-ttu-id="1afa6-122">Руководство по использованию кэша</span><span class="sxs-lookup"><span data-stu-id="1afa6-122">Cache guidelines</span></span>

* <span data-ttu-id="1afa6-123">Код всегда будет иметь способа возврата для выборки данных и **не** зависят от кэшированное значение, его доступность.</span><span class="sxs-lookup"><span data-stu-id="1afa6-123">Code should always have a fallback option to fetch data and **not** depend on a cached value being available.</span></span>
* <span data-ttu-id="1afa6-124">Scare ресурс, память используется в кэше.</span><span class="sxs-lookup"><span data-stu-id="1afa6-124">The cache uses a scare resource, memory.</span></span> <span data-ttu-id="1afa6-125">Ограничить рост кэша:</span><span class="sxs-lookup"><span data-stu-id="1afa6-125">Limit cache growth:</span></span>
  * <span data-ttu-id="1afa6-126">Сделать **не** использовать внешние входные данные в качестве ключей кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-126">Do **not** use external input as cache keys.</span></span>
  * <span data-ttu-id="1afa6-127">Используйте срок действия, чтобы ограничить рост кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-127">Use expirations to limit cache growth.</span></span>
  * [<span data-ttu-id="1afa6-128">Позволяет ограничить размер кэша SetSize, размер и SizeLimit</span><span class="sxs-lookup"><span data-stu-id="1afa6-128">Use SetSize, Size, and SizeLimit to limit cache size</span></span>](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a><span data-ttu-id="1afa6-129">С помощью IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1afa6-129">Using IMemoryCache</span></span>

<span data-ttu-id="1afa6-130">Кэширование в памяти является *службы* , на которую ссылается приложение, нажав [внедрения зависимостей](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="1afa6-130">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="1afa6-131">Вызовите `AddMemoryCache` в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1afa6-131">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="1afa6-132">Запросить `IMemoryCache` экземпляр в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="1afa6-132">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="1afa6-133">`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="1afa6-133">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="1afa6-134">`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), которая доступна в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="1afa6-134">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="1afa6-135">`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), которая доступна в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="1afa6-135">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="1afa6-136">В следующем коде используется [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) проверки, если время в кэше.</span><span class="sxs-lookup"><span data-stu-id="1afa6-136">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="1afa6-137">Если время не кэшируется, новая запись создается и добавляется в кэш с [задать](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="1afa6-137">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="1afa6-138">Отображается текущее время и время кэширования:</span><span class="sxs-lookup"><span data-stu-id="1afa6-138">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="1afa6-139">Кэшированный `DateTime` значение остается в кэше, пока имеются запросы в истечения времени ожидания (и не вытеснение из-за нехватки памяти).</span><span class="sxs-lookup"><span data-stu-id="1afa6-139">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="1afa6-140">На следующем рисунке показана текущее время и время извлечен из кэша:</span><span class="sxs-lookup"><span data-stu-id="1afa6-140">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Представление index отображаются два различных времени](memory/_static/time.png)

<span data-ttu-id="1afa6-142">В следующем коде используется [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) и [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) для кэширования данных.</span><span class="sxs-lookup"><span data-stu-id="1afa6-142">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="1afa6-143">Следующий код вызывает [получить](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) получить время кэширования:</span><span class="sxs-lookup"><span data-stu-id="1afa6-143">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="1afa6-144">См. в разделе [методы IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) и [CacheExtensions методы](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) описание методов кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-144">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="memorycacheentryoptions"></a><span data-ttu-id="1afa6-145">MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="1afa6-145">MemoryCacheEntryOptions</span></span>

<span data-ttu-id="1afa6-146">Следующий пример:</span><span class="sxs-lookup"><span data-stu-id="1afa6-146">The following sample:</span></span>

- <span data-ttu-id="1afa6-147">Задает абсолютный срок действия.</span><span class="sxs-lookup"><span data-stu-id="1afa6-147">Sets the absolute expiration time.</span></span> <span data-ttu-id="1afa6-148">Это максимальное время, операции могут быть кэшированы и запрещает устаревание при скользящий срок действия постоянно обновляется элемент.</span><span class="sxs-lookup"><span data-stu-id="1afa6-148">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="1afa6-149">Задает скользящего срока.</span><span class="sxs-lookup"><span data-stu-id="1afa6-149">Sets a sliding expiration time.</span></span> <span data-ttu-id="1afa6-150">Запросы, которые обращаются к этого кэшированного элемента приведет к сбросу скользящего срока.</span><span class="sxs-lookup"><span data-stu-id="1afa6-150">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="1afa6-151">Задает приоритет кэша с `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="1afa6-151">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="1afa6-152">Наборы [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , будет вызван после удаления записи из кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-152">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="1afa6-153">Обратный вызов выполняется в другом потоке из кода, который удаляет элемент из кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-153">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a><span data-ttu-id="1afa6-154">Позволяет ограничить размер кэша SetSize, размер и SizeLimit</span><span class="sxs-lookup"><span data-stu-id="1afa6-154">Use SetSize, Size, and SizeLimit to limit cache size</span></span>

<span data-ttu-id="1afa6-155">Объект `MemoryCache` экземпляр может при необходимости указывать и обеспечивать максимального размера.</span><span class="sxs-lookup"><span data-stu-id="1afa6-155">A `MemoryCache` instance may optionally specify and enforce a size limit.</span></span> <span data-ttu-id="1afa6-156">Предельный размер памяти не имеет определенных единица измерения, поскольку отсутствует механизм для измерения размера записей кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-156">The memory size limit does not have a defined unit of measure because the cache has no mechanism to measure the size of entries.</span></span> <span data-ttu-id="1afa6-157">Если предельный размер кэш-памяти, все записи необходимо указать размер.</span><span class="sxs-lookup"><span data-stu-id="1afa6-157">If the cache memory size limit is set, all entries must specify size.</span></span> <span data-ttu-id="1afa6-158">Указанный размер измеряется в выбору разработчика.</span><span class="sxs-lookup"><span data-stu-id="1afa6-158">The size specified is in units the developer chooses.</span></span>

<span data-ttu-id="1afa6-159">Пример:</span><span class="sxs-lookup"><span data-stu-id="1afa6-159">For example:</span></span>

* <span data-ttu-id="1afa6-160">Если веб-приложения был главным образом кэширование строк, каждый размер кэша запись может быть длину строки.</span><span class="sxs-lookup"><span data-stu-id="1afa6-160">If the web app was primarily caching strings, each cache entry size could be the string length.</span></span>
* <span data-ttu-id="1afa6-161">Приложение может определить размер всех записей как 1, а максимальный размер составляет количество записей.</span><span class="sxs-lookup"><span data-stu-id="1afa6-161">The app could specify the size of all entries as 1, and the size limit is the count of entries.</span></span>

<span data-ttu-id="1afa6-162">Следующий код создает единицами фиксированный размер [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) доступном [внедрения зависимостей](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="1afa6-162">The following code creates a unitless fixed size [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) accessible by [dependency injection](xref:fundamentals/dependency-injection):</span></span>

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

<span data-ttu-id="1afa6-163">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) имеет единицы.</span><span class="sxs-lookup"><span data-stu-id="1afa6-163">[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) does not have units.</span></span> <span data-ttu-id="1afa6-164">Записей в кэше необходимо указать размер в независимо от единиц, они считают наиболее подходящий в том случае, если установлен размер кэш-памяти.</span><span class="sxs-lookup"><span data-stu-id="1afa6-164">Cached entries must specify size in whatever units they deem most appropriate if the cache memory size has been set.</span></span> <span data-ttu-id="1afa6-165">Все пользователи экземпляра кэша следует использовать одну и ту же систему единицы.</span><span class="sxs-lookup"><span data-stu-id="1afa6-165">All users of a cache instance should use the same unit system.</span></span> <span data-ttu-id="1afa6-166">Запись не будут кэшироваться, если сумма размеров кэшированной записи превышает значение, заданное параметром `SizeLimit`.</span><span class="sxs-lookup"><span data-stu-id="1afa6-166">An entry will not be cached if the sum of the cached entry sizes exceeds the value specified by `SizeLimit`.</span></span> <span data-ttu-id="1afa6-167">Если нет ограничения размера кэша, задайте запись размер кэша будет игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="1afa6-167">If no cache size limit is set, the cache size set on the entry will be ignored.</span></span>

<span data-ttu-id="1afa6-168">В следующем коде регистрах `MyMemoryCache` с [внедрения зависимостей](xref:fundamentals/dependency-injection) контейнера.</span><span class="sxs-lookup"><span data-stu-id="1afa6-168">The following code registers `MyMemoryCache` with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span>

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

<span data-ttu-id="1afa6-169">`MyMemoryCache` будет создан в качестве кэша памяти для компонентов, которые поддерживают этот размер ограничен кэш и знаете, как правильно настроить размер кэша записи.</span><span class="sxs-lookup"><span data-stu-id="1afa6-169">`MyMemoryCache` is created as an independent memory cache for components that are aware of this size limited cache and know how to set cache entry size appropriately.</span></span>

<span data-ttu-id="1afa6-170">В следующем коде используется `MyMemoryCache`:</span><span class="sxs-lookup"><span data-stu-id="1afa6-170">The following code uses `MyMemoryCache`:</span></span>

<span data-ttu-id="1afa6-171">[! [] кода csharp (memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet) [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="1afa6-171">[!code-csharp [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet) [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]</span></span>

<span data-ttu-id="1afa6-172">Размер записи кэша задаются [размер](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) или [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) метод расширения:</span><span class="sxs-lookup"><span data-stu-id="1afa6-172">The size of the cache entry can be set by [Size](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) or the [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) extension method:</span></span>

<span data-ttu-id="1afa6-173">[! [] кода csharp (memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2 & Выделение = 9, 10, 14, 15) [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]</span><span class="sxs-lookup"><span data-stu-id="1afa6-173">[!code-csharp [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15) [](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]</span></span>

::: moniker-end

## <a name="cache-dependencies"></a><span data-ttu-id="1afa6-174">Зависимости кэша</span><span class="sxs-lookup"><span data-stu-id="1afa6-174">Cache dependencies</span></span>

<span data-ttu-id="1afa6-175">Следующий пример показано, как срок действия записи кэша, по истечении зависимую запись.</span><span class="sxs-lookup"><span data-stu-id="1afa6-175">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="1afa6-176">Объект `CancellationChangeToken` добавляется кэшированного элемента.</span><span class="sxs-lookup"><span data-stu-id="1afa6-176">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="1afa6-177">Когда `Cancel` вызывается для `CancellationTokenSource`, обе записи кэша удаляются.</span><span class="sxs-lookup"><span data-stu-id="1afa6-177">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="1afa6-178">С помощью `CancellationTokenSource` позволяет несколько записей кэша для удаления как группу.</span><span class="sxs-lookup"><span data-stu-id="1afa6-178">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="1afa6-179">С помощью `using` шаблон в приведенном выше коде записей кэша создан внутри `using` блок будет наследовать триггеры и параметры срока действия.</span><span class="sxs-lookup"><span data-stu-id="1afa6-179">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="1afa6-180">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="1afa6-180">Additional notes</span></span>

- <span data-ttu-id="1afa6-181">При использовании обратного вызова для повторного заполнения элемента кэша:</span><span class="sxs-lookup"><span data-stu-id="1afa6-181">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="1afa6-182">Несколько запросов можно найти кэшированное значение ключа пустой тем, что обратный вызов еще не завершена.</span><span class="sxs-lookup"><span data-stu-id="1afa6-182">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="1afa6-183">Это может привести несколько потоков, при повторном заполнении кэшированного элемента.</span><span class="sxs-lookup"><span data-stu-id="1afa6-183">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="1afa6-184">Когда одна запись кэша используется для создания другой, дочерние копирует истечения срока действия маркеров родительская запись и параметры срока действия на основе времени.</span><span class="sxs-lookup"><span data-stu-id="1afa6-184">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="1afa6-185">Дочерние не истекшим сроком действия путем ручного удаления или обновления родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="1afa6-185">The child isn't expired by manual removal or updating of the parent entry.</span></span>

- <span data-ttu-id="1afa6-186">Используйте [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) для установки обратных вызовов, которые будут срабатывать после удаления записи кэша из кэша.</span><span class="sxs-lookup"><span data-stu-id="1afa6-186">Use [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) to set the callbacks that will be fired after the cache entry is evicted from the cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1afa6-187">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1afa6-187">Additional resources</span></span>

* [<span data-ttu-id="1afa6-188">Работа с распределенным кэшем</span><span class="sxs-lookup"><span data-stu-id="1afa6-188">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="1afa6-189">Обнаружение изменений с помощью маркеров изменений</span><span class="sxs-lookup"><span data-stu-id="1afa6-189">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="1afa6-190">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="1afa6-190">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="1afa6-191">ПО промежуточного слоя для кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="1afa6-191">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="1afa6-192">Вспомогательная функция тега кэша</span><span class="sxs-lookup"><span data-stu-id="1afa6-192">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="1afa6-193">Вспомогательная функция тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="1afa6-193">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)