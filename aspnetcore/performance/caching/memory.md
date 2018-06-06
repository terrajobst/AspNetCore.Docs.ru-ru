---
title: Кэш в памяти в ASP.NET Core
author: rick-anderson
description: Узнайте, как кэширование данных в памяти в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: eca6610caf4e0a654c9a31f89a42e2ac82e94d23
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734488"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="479ab-103">Кэш в памяти в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="479ab-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="479ab-104">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Luo Джон](https://github.com/JunTaoLuo), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="479ab-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="479ab-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="479ab-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="479ab-106">Основные принципы кэширования</span><span class="sxs-lookup"><span data-stu-id="479ab-106">Caching basics</span></span>

<span data-ttu-id="479ab-107">Кэширование может значительно повысить производительность и масштабируемость приложения за счет снижения работ, необходимый для создания содержимого.</span><span class="sxs-lookup"><span data-stu-id="479ab-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="479ab-108">Кэширование будет работать наилучшим образом с данными, которые редко изменяются.</span><span class="sxs-lookup"><span data-stu-id="479ab-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="479ab-109">Кэширование делает копию данных, которые могут быть возвращены намного быстрее, чем из исходного источника.</span><span class="sxs-lookup"><span data-stu-id="479ab-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="479ab-110">Следует писать и тестировать приложения никогда не зависят от кэшированных данных.</span><span class="sxs-lookup"><span data-stu-id="479ab-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="479ab-111">ASP.NET Core поддерживает несколько разных кэша.</span><span class="sxs-lookup"><span data-stu-id="479ab-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="479ab-112">Простейший кэша на основе [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), который представляет кэша, хранящийся в памяти веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="479ab-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="479ab-113">Приложения, которые выполняются на ферме серверов из нескольких серверов следует убедиться, прикрепленные сеансы при использовании кэша в памяти.</span><span class="sxs-lookup"><span data-stu-id="479ab-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="479ab-114">Прикрепленные сеансы убедитесь, что последующие запросы от клиента все перейти на том же сервере.</span><span class="sxs-lookup"><span data-stu-id="479ab-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="479ab-115">Например, использование приложения Azure Web [маршрутизации запросов приложений](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), чтобы перенаправлять все последующие запросы на тот же сервер.</span><span class="sxs-lookup"><span data-stu-id="479ab-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="479ab-116">Non прикрепленных сеансов в веб-ферме требуется [распределенный кэш](distributed.md) во избежание проблем с согласованностью кэша.</span><span class="sxs-lookup"><span data-stu-id="479ab-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="479ab-117">Для некоторых приложений распределенного кэша может поддерживать более масштабного чем кэша в памяти.</span><span class="sxs-lookup"><span data-stu-id="479ab-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="479ab-118">Использование распределенного кэша разгружает кэш-памяти во внешний процесс.</span><span class="sxs-lookup"><span data-stu-id="479ab-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="479ab-119">`IMemoryCache` Кэша исключим записей кэша, свободной памяти, если не [кэшировать приоритет](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) равно `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="479ab-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="479ab-120">Можно задать `CacheItemPriority` Настройка приоритета, с которым кэша исключает элементы в условиях нехватки памяти.</span><span class="sxs-lookup"><span data-stu-id="479ab-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="479ab-121">Кэш в памяти можно хранить любой объект; интерфейс распределенного кэша ограничен `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="479ab-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="479ab-122">С помощью IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="479ab-122">Using IMemoryCache</span></span>

<span data-ttu-id="479ab-123">Кэширование в памяти *службы* , на который ссылается приложение, нажав [внедрения зависимостей](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="479ab-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="479ab-124">Вызовите `AddMemoryCache` в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="479ab-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="479ab-125">Запрос `IMemoryCache` экземпляр в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="479ab-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="479ab-126">`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span><span class="sxs-lookup"><span data-stu-id="479ab-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="479ab-127">`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), который является доступной в [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="479ab-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="479ab-128">`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), который является доступной в [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="479ab-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="479ab-129">В следующем коде используется [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) проверки, если время в кэше.</span><span class="sxs-lookup"><span data-stu-id="479ab-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="479ab-130">Если время не кэшируется, новая запись создается и добавляется в кэш с [задать](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span><span class="sxs-lookup"><span data-stu-id="479ab-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="479ab-131">Отображается текущее время и время кэширования:</span><span class="sxs-lookup"><span data-stu-id="479ab-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="479ab-132">Кэшированные `DateTime` значение остается в кэше, пока имеются запросы в течение периода ожидания (и без вытеснения, из-за нехватки памяти).</span><span class="sxs-lookup"><span data-stu-id="479ab-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="479ab-133">На следующем рисунке показана текущего времени и получить из кэша, время:</span><span class="sxs-lookup"><span data-stu-id="479ab-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![Индекс представления с помощью двух разных времени отображаются в](memory/_static/time.png)

<span data-ttu-id="479ab-135">В следующем коде используется [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) и [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) для кэширования данных.</span><span class="sxs-lookup"><span data-stu-id="479ab-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="479ab-136">Следующий код вызывает [получить](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) выбирать время кэширования:</span><span class="sxs-lookup"><span data-stu-id="479ab-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="479ab-137">В разделе [методы IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) и [CacheExtensions методы](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) описание методов кэша.</span><span class="sxs-lookup"><span data-stu-id="479ab-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="479ab-138">С помощью MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="479ab-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="479ab-139">Следующий пример:</span><span class="sxs-lookup"><span data-stu-id="479ab-139">The following sample:</span></span>

- <span data-ttu-id="479ab-140">Задает абсолютный срок действия.</span><span class="sxs-lookup"><span data-stu-id="479ab-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="479ab-141">Это — это максимальное время, кэшировании записи и блокирует элемент устаревшими при скользящий срок действия постоянно обновляется.</span><span class="sxs-lookup"><span data-stu-id="479ab-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="479ab-142">Задает скользящего срока.</span><span class="sxs-lookup"><span data-stu-id="479ab-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="479ab-143">Запросы, которые обращаются к этой кэшированного элемента приведет к сбросу скользящего срока.</span><span class="sxs-lookup"><span data-stu-id="479ab-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="479ab-144">Задает приоритет кэша `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="479ab-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="479ab-145">Наборы [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , будет вызван после удаления записи из кэша.</span><span class="sxs-lookup"><span data-stu-id="479ab-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="479ab-146">Обратный вызов выполняется в другом потоке, от кода, который удаляет элемент из кэша.</span><span class="sxs-lookup"><span data-stu-id="479ab-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="479ab-147">Зависимости кэша</span><span class="sxs-lookup"><span data-stu-id="479ab-147">Cache dependencies</span></span>

<span data-ttu-id="479ab-148">Следующий пример показано, как срок действия записи кэша после истечения срока действия зависимую запись.</span><span class="sxs-lookup"><span data-stu-id="479ab-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="479ab-149">Объект `CancellationChangeToken` добавляется кэшированного элемента.</span><span class="sxs-lookup"><span data-stu-id="479ab-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="479ab-150">Когда `Cancel` будет вызван на `CancellationTokenSource`, вытесняются обе записи кэша.</span><span class="sxs-lookup"><span data-stu-id="479ab-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="479ab-151">С помощью `CancellationTokenSource` позволяет нескольких записей кэша для удаления в группу.</span><span class="sxs-lookup"><span data-stu-id="479ab-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="479ab-152">С `using` шаблон в приведенном выше коде записей кэша, созданных в `using` блок будет наследовать триггеры и параметры истечения срока действия.</span><span class="sxs-lookup"><span data-stu-id="479ab-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="479ab-153">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="479ab-153">Additional notes</span></span>

- <span data-ttu-id="479ab-154">При использовании обратного вызова для повторного заполнения элемента кэша:</span><span class="sxs-lookup"><span data-stu-id="479ab-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="479ab-155">Несколько запросов можно найти кэшированное значение ключа пустой тем, что обратный вызов еще не завершена.</span><span class="sxs-lookup"><span data-stu-id="479ab-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="479ab-156">Это может привести к повторному заполнению кэшированного элемента нескольких потоков.</span><span class="sxs-lookup"><span data-stu-id="479ab-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="479ab-157">При использовании одной записи кэша для создания другого дочернего копирует истечение срока действия маркеров и параметры истечения срока действия на основе времени родительскую запись.</span><span class="sxs-lookup"><span data-stu-id="479ab-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="479ab-158">Дочерние не истек, удаление вручную, или обновление родительской записи.</span><span class="sxs-lookup"><span data-stu-id="479ab-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="479ab-159">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="479ab-159">Additional resources</span></span>

* [<span data-ttu-id="479ab-160">Работа с распределенным кэшем</span><span class="sxs-lookup"><span data-stu-id="479ab-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="479ab-161">Обнаружение изменений с помощью маркеров изменений</span><span class="sxs-lookup"><span data-stu-id="479ab-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="479ab-162">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="479ab-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="479ab-163">ПО промежуточного слоя для кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="479ab-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="479ab-164">Вспомогательная функция тега кэша</span><span class="sxs-lookup"><span data-stu-id="479ab-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="479ab-165">Вспомогательная функция тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="479ab-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
