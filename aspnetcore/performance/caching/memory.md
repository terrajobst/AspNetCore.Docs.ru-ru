---
title: "Кэширование в памяти в ASP.NET Core"
author: rick-anderson
description: "Показано, как кэширование данных в памяти в ASP.NET Core."
keywords: "ASP.NET Core, производительность в памяти, кэша,"
ms.author: riande
manager: wpickett
ms.date: 12/14/2016
ms.topic: article
ms.assetid: 819511cf-d33e-410a-b5a9-bef7fa64d2f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/memory
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b5dca6a81a66ce2a8771f1a16e63834d6504d8b6
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-in-memory-caching-in-aspnet-core"></a><span data-ttu-id="7f044-104">Введение в ASP.NET Core кэширование в памяти</span><span class="sxs-lookup"><span data-stu-id="7f044-104">Introduction to in-memory caching in ASP.NET Core</span></span>

<span data-ttu-id="7f044-105">По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Luo Джон](https://github.com/JunTaoLuo), и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7f044-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="7f044-106">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="7f044-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample)

## <a name="caching-basics"></a><span data-ttu-id="7f044-107">Основные принципы кэширования</span><span class="sxs-lookup"><span data-stu-id="7f044-107">Caching basics</span></span>

<span data-ttu-id="7f044-108">Кэширование может значительно повысить производительность и масштабируемость приложения за счет снижения работ, необходимый для создания содержимого.</span><span class="sxs-lookup"><span data-stu-id="7f044-108">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="7f044-109">Кэширование будет работать наилучшим образом с данными, которые редко изменяются.</span><span class="sxs-lookup"><span data-stu-id="7f044-109">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="7f044-110">Кэширование делает копию данных, которые могут быть возвращены намного быстрее, чем из исходного источника.</span><span class="sxs-lookup"><span data-stu-id="7f044-110">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="7f044-111">Следует писать и тестировать приложения никогда не зависят от кэшированных данных.</span><span class="sxs-lookup"><span data-stu-id="7f044-111">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="7f044-112">ASP.NET Core поддерживает несколько разных кэша.</span><span class="sxs-lookup"><span data-stu-id="7f044-112">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="7f044-113">Простейший кэша на основе [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), который представляет кэша, хранящийся в памяти веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="7f044-113">The simplest cache is based on the [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="7f044-114">Приложения, которые выполняются на ферме серверов из нескольких серверов следует убедиться, прикрепленные сеансы при использовании кэша в памяти.</span><span class="sxs-lookup"><span data-stu-id="7f044-114">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="7f044-115">Прикрепленные сеансы убедитесь, что последующие запросы от клиента все перейти на том же сервере.</span><span class="sxs-lookup"><span data-stu-id="7f044-115">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="7f044-116">Например, использование приложения Azure Web [маршрутизации запросов приложений](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), чтобы перенаправлять все последующие запросы на тот же сервер.</span><span class="sxs-lookup"><span data-stu-id="7f044-116">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="7f044-117">Non прикрепленных сеансов в веб-ферме требуется [распределенный кэш](distributed.md) во избежание проблем с согласованностью кэша.</span><span class="sxs-lookup"><span data-stu-id="7f044-117">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="7f044-118">Для некоторых приложений распределенного кэша может поддерживать более масштабного чем кэша в памяти.</span><span class="sxs-lookup"><span data-stu-id="7f044-118">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="7f044-119">Использование распределенного кэша разгружает кэш-памяти во внешний процесс.</span><span class="sxs-lookup"><span data-stu-id="7f044-119">Using a distributed cache offloads the cache memory to an external process.</span></span> 

<span data-ttu-id="7f044-120">`IMemoryCache` Кэша исключим записей кэша, свободной памяти, если не [кэшировать приоритет](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) равно `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="7f044-120">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="7f044-121">Можно задать `CacheItemPriority` Настройка приоритета кэша исключает элементы в условиях нехватки памяти.</span><span class="sxs-lookup"><span data-stu-id="7f044-121">You can set the `CacheItemPriority` to adjust the priority the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="7f044-122">Кэш в памяти можно хранить любой объект; интерфейс распределенного кэша ограничен `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="7f044-122">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="7f044-123">С помощью IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="7f044-123">Using IMemoryCache</span></span>

<span data-ttu-id="7f044-124">Кэширование в памяти *службы* , на который ссылается приложение, нажав [внедрения зависимостей](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7f044-124">In-memory caching is a *service* that is referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="7f044-125">Вызовите `AddMemoryCache` в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="7f044-125">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Startup.cs?highlight=8)] 

<span data-ttu-id="7f044-126">Запрос `IMemoryCache` экземпляр в конструкторе:</span><span class="sxs-lookup"><span data-stu-id="7f044-126">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-)] 

<span data-ttu-id="7f044-127">`IMemoryCache`требуется пакет NuGet «Microsoft.Extensions.Caching.Memory».</span><span class="sxs-lookup"><span data-stu-id="7f044-127">`IMemoryCache` requires NuGet package "Microsoft.Extensions.Caching.Memory".</span></span>

<span data-ttu-id="7f044-128">В следующем коде используется [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) проверки, если текущее время в кэше.</span><span class="sxs-lookup"><span data-stu-id="7f044-128">The following code uses [TryGetValue](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if the current time is in the cache.</span></span> <span data-ttu-id="7f044-129">Если элемент не кэшируются, новая запись создается и добавляется в кэш с [задать](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).</span><span class="sxs-lookup"><span data-stu-id="7f044-129">If the item is not cached, a new entry is created and added to the cache with [Set](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_).</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="7f044-130">Отображается текущее время и время кэширования:</span><span class="sxs-lookup"><span data-stu-id="7f044-130">The current time and the cached time is displayed:</span></span>

[!code-html[Main](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="7f044-131">Кэшированные `DateTime` значение будет оставаться в кэше, пока имеются запросы в течение периода ожидания (и без вытеснения, из-за нехватки памяти).</span><span class="sxs-lookup"><span data-stu-id="7f044-131">The cached `DateTime` value will remain in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="7f044-132">На следующем рисунке показано текущее время и время извлечен из кэша:</span><span class="sxs-lookup"><span data-stu-id="7f044-132">The image below shows the current time and an older time retrieved from cache:</span></span>

![Индекс представления с помощью двух разных времени отображаются в](memory/_static/time.png)

<span data-ttu-id="7f044-134">В следующем коде используется [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) и [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) для кэширования данных.</span><span class="sxs-lookup"><span data-stu-id="7f044-134">The following code uses [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="7f044-135">Следующий код вызывает [получить](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) выбирать время кэширования:</span><span class="sxs-lookup"><span data-stu-id="7f044-135">The following code calls [Get](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="7f044-136">В разделе [методы IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) и [CacheExtensions методы](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) описание методов кэша.</span><span class="sxs-lookup"><span data-stu-id="7f044-136">See [IMemoryCache methods](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="7f044-137">С помощью MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="7f044-137">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="7f044-138">Следующий пример:</span><span class="sxs-lookup"><span data-stu-id="7f044-138">The following sample:</span></span>

- <span data-ttu-id="7f044-139">Задает абсолютный срок действия.</span><span class="sxs-lookup"><span data-stu-id="7f044-139">Sets the absolute expiration time.</span></span> <span data-ttu-id="7f044-140">Это — это максимальное время, кэшировании записи и блокирует элемент устаревшими при скользящий срок действия постоянно обновляется.</span><span class="sxs-lookup"><span data-stu-id="7f044-140">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="7f044-141">Задает скользящего срока.</span><span class="sxs-lookup"><span data-stu-id="7f044-141">Sets a sliding expiration time.</span></span> <span data-ttu-id="7f044-142">Запросы, которые обращаются к этой кэшированного элемента приведет к сбросу скользящего срока.</span><span class="sxs-lookup"><span data-stu-id="7f044-142">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="7f044-143">Задает приоритет кэша `CacheItemPriority.NeverRemove`.</span><span class="sxs-lookup"><span data-stu-id="7f044-143">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span> 
- <span data-ttu-id="7f044-144">Наборы [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) , будет вызван после удаления записи из кэша.</span><span class="sxs-lookup"><span data-stu-id="7f044-144">Sets a [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="7f044-145">Обратный вызов выполняется в другом потоке, от кода, который удаляет элемент из кэша.</span><span class="sxs-lookup"><span data-stu-id="7f044-145">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a><span data-ttu-id="7f044-146">Зависимости кэша</span><span class="sxs-lookup"><span data-stu-id="7f044-146">Cache dependencies</span></span>

<span data-ttu-id="7f044-147">Следующий пример показано, как срок действия записи кэша после истечения срока действия зависимую запись.</span><span class="sxs-lookup"><span data-stu-id="7f044-147">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="7f044-148">Объект `CancellationChangeToken` добавляется кэшированного элемента.</span><span class="sxs-lookup"><span data-stu-id="7f044-148">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="7f044-149">Когда `Cancel` будет вызван на `CancellationTokenSource`, вытесняются обе записи кэша.</span><span class="sxs-lookup"><span data-stu-id="7f044-149">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span> 

[!code-csharp[Main](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="7f044-150">С помощью `CancellationTokenSource` позволяет нескольких записей кэша для удаления в группу.</span><span class="sxs-lookup"><span data-stu-id="7f044-150">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="7f044-151">С `using` шаблон в приведенном выше коде записей кэша, созданных в `using` блок будет наследовать триггеры и параметры истечения срока действия.</span><span class="sxs-lookup"><span data-stu-id="7f044-151">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="7f044-152">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="7f044-152">Additional notes</span></span>

- <span data-ttu-id="7f044-153">При использовании обратного вызова для повторного заполнения элемента кэша:</span><span class="sxs-lookup"><span data-stu-id="7f044-153">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="7f044-154">Несколько запросов можно найти кэшированное значение ключа пустой тем, что обратный вызов еще не завершена.</span><span class="sxs-lookup"><span data-stu-id="7f044-154">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span> 
  - <span data-ttu-id="7f044-155">Это может привести к повторному заполнению кэшированного элемента нескольких потоков.</span><span class="sxs-lookup"><span data-stu-id="7f044-155">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="7f044-156">При использовании одной записи кэша для создания другого дочернего копирует истечение срока действия маркеров и параметры истечения срока действия на основе времени родительскую запись.</span><span class="sxs-lookup"><span data-stu-id="7f044-156">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="7f044-157">Дочерние не истек, удаление вручную, или обновление родительской записи.</span><span class="sxs-lookup"><span data-stu-id="7f044-157">The child is not expired by manual removal or updating of the parent entry.</span></span>

### <a name="other-resources"></a><span data-ttu-id="7f044-158">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="7f044-158">Other Resources</span></span>

* [<span data-ttu-id="7f044-159">Работа с распределенным кэшем</span><span class="sxs-lookup"><span data-stu-id="7f044-159">Working with a Distributed Cache</span></span>](distributed.md)
* [<span data-ttu-id="7f044-160">ПО промежуточного слоя для кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="7f044-160">Response caching middleware</span></span>](middleware.md)
