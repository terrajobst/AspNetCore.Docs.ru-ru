---
title: Распределенное кэширование в ASP.NET Core
author: guardrex
description: Узнайте, как использовать распределенный кэш ASP.NET Core для повышения производительности и масштабируемости приложений, особенно в среде облака или фермы серверов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
uid: performance/caching/distributed
ms.openlocfilehash: 3e039a26505aed1bcc0299880039760fef19fd67
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76727241"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="28000-103">Распределенное кэширование в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="28000-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="28000-104">[Люк ЛаСаМ](https://github.com/guardrex), [Мохсин Насир](https://github.com/mohsinnasir)и [Виктор Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="28000-104">By [Luke Latham](https://github.com/guardrex), [Mohsin Nasir](https://github.com/mohsinnasir), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="28000-105">Распределенный кэш — это кэш, совместно используемый несколькими серверами приложений, обычно поддерживаемый как внешняя служба для серверов приложений, обращающихся к ней.</span><span class="sxs-lookup"><span data-stu-id="28000-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="28000-106">Распределенный кэш может повысить производительность и масштабируемость приложения ASP.NET Core, особенно если приложение размещено в облачной службе или ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="28000-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="28000-107">Распределенный кэш имеет несколько преимуществ по сравнению с другими сценариями кэширования, в которых кэшированные данные хранятся на отдельных серверах приложений.</span><span class="sxs-lookup"><span data-stu-id="28000-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="28000-108">При распределении кэшированных данных данные:</span><span class="sxs-lookup"><span data-stu-id="28000-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="28000-109">— Согласованность между запросами к нескольким серверам.</span><span class="sxs-lookup"><span data-stu-id="28000-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="28000-110">Выдерживает перезапуски сервера и развертывание приложений.</span><span class="sxs-lookup"><span data-stu-id="28000-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="28000-111">Не использует локальную память.</span><span class="sxs-lookup"><span data-stu-id="28000-111">Doesn't use local memory.</span></span>

<span data-ttu-id="28000-112">Конфигурация распределенного кэша зависит от конкретной реализации.</span><span class="sxs-lookup"><span data-stu-id="28000-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="28000-113">В этой статье описывается настройка распределенных кэшей SQL Server и Redis.</span><span class="sxs-lookup"><span data-stu-id="28000-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="28000-114">Также доступны реализации сторонних производителей, например [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="28000-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="28000-115">Независимо от выбранной реализации приложение взаимодействует с кэшем с помощью интерфейса <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>.</span><span class="sxs-lookup"><span data-stu-id="28000-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="28000-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="28000-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="28000-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="28000-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="28000-118">Чтобы использовать SQL Server распределенный кэш, добавьте ссылку на пакет в пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="28000-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="28000-119">Чтобы использовать распределенный кэш Redis, добавьте ссылку на пакет в пакет [Microsoft. Extensions. Caching. стаккексчанжередис](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="28000-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

<span data-ttu-id="28000-120">Чтобы использовать распределенный кэш NCache, добавьте ссылку на пакет в пакет [NCache. Microsoft. Extensions. Caching. конвертер](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="28000-120">To use NCache distributed cache, add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="28000-121">Чтобы использовать SQL Server распределенный кэш, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="28000-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="28000-122">Чтобы использовать распределенный кэш Redis, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft. Extensions. Caching. стаккексчанжередис](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="28000-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="28000-123">Пакет Redis не входит в пакет `Microsoft.AspNetCore.App`, поэтому необходимо по отдельности ссылаться на пакет Redis в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="28000-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="28000-124">Чтобы использовать распределенный кэш NCache, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [NCache. Microsoft. Extensions. Caching. конвертер](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="28000-124">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="28000-125">Пакет NCache не входит в пакет `Microsoft.AspNetCore.App`, поэтому необходимо по отдельности ссылаться на пакет NCache в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="28000-125">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="28000-126">Чтобы использовать SQL Server распределенный кэш, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="28000-126">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="28000-127">Чтобы использовать распределенный кэш Redis, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft. Extensions. Caching. Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="28000-127">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="28000-128">Пакет Redis не входит в пакет `Microsoft.AspNetCore.App`, поэтому необходимо по отдельности ссылаться на пакет Redis в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="28000-128">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

<span data-ttu-id="28000-129">Чтобы использовать распределенный кэш NCache, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [NCache. Microsoft. Extensions. Caching. конвертер](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) .</span><span class="sxs-lookup"><span data-stu-id="28000-129">To use NCache distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [NCache.Microsoft.Extensions.Caching.OpenSource](https://www.nuget.org/packages/NCache.Microsoft.Extensions.Caching.OpenSource) package.</span></span> <span data-ttu-id="28000-130">Пакет NCache не входит в пакет `Microsoft.AspNetCore.App`, поэтому необходимо по отдельности ссылаться на пакет NCache в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="28000-130">The NCache package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the NCache package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="28000-131">Интерфейс IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="28000-131">IDistributedCache interface</span></span>

<span data-ttu-id="28000-132">Интерфейс <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> предоставляет следующие методы для управления элементами в реализации распределенного кэша:</span><span class="sxs-lookup"><span data-stu-id="28000-132">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="28000-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; принимает строковый ключ и получает кэшированный элемент в виде массива `byte[]`, если он найден в кэше.</span><span class="sxs-lookup"><span data-stu-id="28000-133"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="28000-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; добавляет элемент (как `byte[]` массив) в кэш с помощью строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="28000-134"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="28000-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; обновляет элемент в кэше на основе его ключа, передавая время ожидания скользящего срока действия (если оно есть).</span><span class="sxs-lookup"><span data-stu-id="28000-135"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="28000-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; удаляет элемент кэша на основе его строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="28000-136"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="28000-137">Установка служб распределенного кэширования</span><span class="sxs-lookup"><span data-stu-id="28000-137">Establish distributed caching services</span></span>

<span data-ttu-id="28000-138">Зарегистрируйте реализацию <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="28000-138">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="28000-139">Реализации, предоставляемые платформой, описанные в этом разделе, включают:</span><span class="sxs-lookup"><span data-stu-id="28000-139">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="28000-140">Кэш распределенной памяти</span><span class="sxs-lookup"><span data-stu-id="28000-140">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="28000-141">Распределенный кэш SQL Server</span><span class="sxs-lookup"><span data-stu-id="28000-141">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="28000-142">Распределенный кэш Redis</span><span class="sxs-lookup"><span data-stu-id="28000-142">Distributed Redis cache</span></span>](#distributed-redis-cache)
* [<span data-ttu-id="28000-143">Распределенный кэш NCache</span><span class="sxs-lookup"><span data-stu-id="28000-143">Distributed NCache cache</span></span>](#distributed-ncache-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="28000-144">Кэш распределенной памяти</span><span class="sxs-lookup"><span data-stu-id="28000-144">Distributed Memory Cache</span></span>

<span data-ttu-id="28000-145">Кэш распределенной памяти (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) — это предоставляемая платформой реализация <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, которая хранит элементы в памяти.</span><span class="sxs-lookup"><span data-stu-id="28000-145">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="28000-146">Кэш распределенной памяти не является фактическим распределенным кэшем.</span><span class="sxs-lookup"><span data-stu-id="28000-146">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="28000-147">Кэшированные элементы хранятся в экземпляре приложения на сервере, где выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="28000-147">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="28000-148">Кэш распределенной памяти — это полезная реализация:</span><span class="sxs-lookup"><span data-stu-id="28000-148">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="28000-149">В сценариях разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="28000-149">In development and testing scenarios.</span></span>
* <span data-ttu-id="28000-150">Если в рабочей среде используется один сервер, а использование памяти не является проблемой.</span><span class="sxs-lookup"><span data-stu-id="28000-150">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="28000-151">Реализация кэша распределенной памяти абстрагирует хранилище кэшированных данных.</span><span class="sxs-lookup"><span data-stu-id="28000-151">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="28000-152">Она позволяет реализовать истинное решение распределенного кэширования в будущем, если потребуется несколько узлов или отказоустойчивость.</span><span class="sxs-lookup"><span data-stu-id="28000-152">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="28000-153">Пример приложения использует распределенный кэш памяти при запуске приложения в среде разработки в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="28000-153">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="28000-154">Распределенный кэш SQL Server</span><span class="sxs-lookup"><span data-stu-id="28000-154">Distributed SQL Server Cache</span></span>

<span data-ttu-id="28000-155">Реализация кэша распределенного SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) позволяет распределенному кэшу использовать базу данных SQL Server в качестве резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="28000-155">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="28000-156">Чтобы создать SQL Server таблицу кэшированных элементов в экземпляре SQL Server, можно использовать средство `sql-cache`.</span><span class="sxs-lookup"><span data-stu-id="28000-156">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="28000-157">Средство создает таблицу с указанными именем и схемой.</span><span class="sxs-lookup"><span data-stu-id="28000-157">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="28000-158">Создайте таблицу в SQL Server, выполнив команду `sql-cache create`.</span><span class="sxs-lookup"><span data-stu-id="28000-158">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="28000-159">Укажите SQL Server экземпляр (`Data Source`), базу данных (`Initial Catalog`), схему (например, `dbo`) и имя таблицы (например, `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="28000-159">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="28000-160">В журнал заносится сообщение, указывающее, что средство прошло успешно:</span><span class="sxs-lookup"><span data-stu-id="28000-160">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="28000-161">Таблица, созданная средством `sql-cache`, имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="28000-161">The table created by the `sql-cache` tool has the following schema:</span></span>

![Таблица кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="28000-163">Приложение должно манипулировать значениями кэша с помощью экземпляра <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, а не <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="28000-163">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="28000-164">Пример приложения реализует <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> в среде, отличной от разработки, в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="28000-164">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="28000-165"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (И, при необходимости, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> и <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) обычно хранятся вне системы управления версиями (например, хранится в [диспетчере секретов](xref:security/app-secrets) или в *appsettings.json*/*appsettings. {Файлы среды}. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="28000-165">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="28000-166">Строка подключения может содержать учетные данные, которые должны содержаться в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="28000-166">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="28000-167">Распределенный кэш Redis</span><span class="sxs-lookup"><span data-stu-id="28000-167">Distributed Redis Cache</span></span>

<span data-ttu-id="28000-168">[Redis](https://redis.io/) -это хранилище данных в памяти с открытым исходным кодом, которое часто используется в качестве распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="28000-168">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="28000-169">Вы можете использовать Redis локально, и вы можете настроить [кэш Redis для Azure](https://azure.microsoft.com/services/cache/) для приложения ASP.NET Core, размещенного в Azure.</span><span class="sxs-lookup"><span data-stu-id="28000-169">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="28000-170">Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) в среде, не являющейся средой разработки, в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="28000-170">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="28000-171">Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) в среде, не являющейся средой разработки, в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="28000-171">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="28000-172">Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.Redis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="28000-172">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="28000-173">Чтобы установить Redis на локальном компьютере, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="28000-173">To install Redis on your local machine:</span></span>

1. <span data-ttu-id="28000-174">Установите [пакет шоколадного Redis](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="28000-174">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
1. <span data-ttu-id="28000-175">Запустите `redis-server` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="28000-175">Run `redis-server` from a command prompt.</span></span>

### <a name="distributed-ncache-cache"></a><span data-ttu-id="28000-176">Распределенный кэш NCache</span><span class="sxs-lookup"><span data-stu-id="28000-176">Distributed NCache Cache</span></span>

<span data-ttu-id="28000-177">[NCache](https://github.com/Alachisoft/NCache) — это распределенный кэш в памяти с открытым исходным кодом, разработанный изначально в .NET и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="28000-177">[NCache](https://github.com/Alachisoft/NCache) is an open source in-memory distributed cache developed natively in .NET and .NET Core.</span></span> <span data-ttu-id="28000-178">NCache работает как локально, так и настроенный как кластер распределенного кэша для ASP.NET Core приложения, работающего в Azure или на других платформах размещения.</span><span class="sxs-lookup"><span data-stu-id="28000-178">NCache works both locally and configured as a distributed cache cluster for an ASP.NET Core app running in Azure or on other hosting platforms.</span></span>

<span data-ttu-id="28000-179">Чтобы установить и настроить NCache на локальном компьютере, ознакомьтесь с [руководством по NCache начало работы для Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span><span class="sxs-lookup"><span data-stu-id="28000-179">To install and configure NCache on your local machine, see [NCache Getting Started Guide for Windows](https://www.alachisoft.com/resources/docs/ncache-oss/getting-started-guide-windows/).</span></span>

<span data-ttu-id="28000-180">Чтобы настроить NCache, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="28000-180">To configure NCache:</span></span>

1. <span data-ttu-id="28000-181">Установите [NuGet с открытым исходным кодом NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span><span class="sxs-lookup"><span data-stu-id="28000-181">Install [NCache open source NuGet](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/).</span></span>
1. <span data-ttu-id="28000-182">Настройте кластер кэша в [Client. нкконф](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span><span class="sxs-lookup"><span data-stu-id="28000-182">Configure the cache cluster in [client.ncconf](https://www.alachisoft.com/resources/docs/ncache-oss/admin-guide/client-config.html).</span></span>
1. <span data-ttu-id="28000-183">Добавьте следующий код в файл `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="28000-183">Add the following code to `Startup.ConfigureServices`:</span></span>

   ```csharp
   services.AddNCacheDistributedCache(configuration =>    
   {        
       configuration.CacheName = "demoClusteredCache";
       configuration.EnableLogs = true;
       configuration.ExceptionsEnabled = true;
   });
   ```

## <a name="use-the-distributed-cache"></a><span data-ttu-id="28000-184">Использование распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="28000-184">Use the distributed cache</span></span>

<span data-ttu-id="28000-185">Чтобы использовать интерфейс <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, запросите экземпляр <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> из любого конструктора в приложении.</span><span class="sxs-lookup"><span data-stu-id="28000-185">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="28000-186">Экземпляр предоставляется путем [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="28000-186">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="28000-187">При запуске примера приложения <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> вставляется в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="28000-187">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="28000-188">Текущее время кэшируется с помощью <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (Дополнительные сведения см. в разделе [универсальный узел: ихостаппликатионлифетиме](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span><span class="sxs-lookup"><span data-stu-id="28000-188">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="28000-189">При запуске примера приложения <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> вставляется в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="28000-189">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="28000-190">Текущее время кэшируется с помощью <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Дополнительные сведения см. в разделе [веб-узел: интерфейс иаппликатионлифетиме](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="28000-190">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="28000-191">Пример приложения внедряет <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `IndexModel` для использования на странице индекса.</span><span class="sxs-lookup"><span data-stu-id="28000-191">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="28000-192">При каждой загрузке страницы индекса кэш проверяется на наличие кэшированного времени в `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="28000-192">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="28000-193">Если время кэширования не истекло, отображается время.</span><span class="sxs-lookup"><span data-stu-id="28000-193">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="28000-194">Если прошло 20 секунд с момента последнего обращения к кэшированному времени (время последней загрузки этой страницы), на странице отображается *время ожидания кэширования*.</span><span class="sxs-lookup"><span data-stu-id="28000-194">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="28000-195">Немедленно обновите кэшированное время на текущее время, нажав кнопку **сбросить кэшированное** время.</span><span class="sxs-lookup"><span data-stu-id="28000-195">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="28000-196">Кнопка запускает метод обработчика `OnPostResetCachedTime`.</span><span class="sxs-lookup"><span data-stu-id="28000-196">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="28000-197">Время жизни экземпляров <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (по крайней мере, для встроенных реализаций) не обязательно должно быть ограничено одним объектом или блоком.</span><span class="sxs-lookup"><span data-stu-id="28000-197">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="28000-198">Можно также создать экземпляр <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> везде, где это может понадобиться, вместо использования DI, но создание экземпляра в коде может усложнить тестирование и нарушение [принципа явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="28000-198">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="28000-199">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="28000-199">Recommendations</span></span>

<span data-ttu-id="28000-200">При принятии решения о том, какая реализация <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> лучше подходит для вашего приложения, учитывайте следующее.</span><span class="sxs-lookup"><span data-stu-id="28000-200">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="28000-201">Существующая инфраструктура</span><span class="sxs-lookup"><span data-stu-id="28000-201">Existing infrastructure</span></span>
* <span data-ttu-id="28000-202">Требования к производительности</span><span class="sxs-lookup"><span data-stu-id="28000-202">Performance requirements</span></span>
* <span data-ttu-id="28000-203">Стоимость</span><span class="sxs-lookup"><span data-stu-id="28000-203">Cost</span></span>
* <span data-ttu-id="28000-204">Опыт работы в группе</span><span class="sxs-lookup"><span data-stu-id="28000-204">Team experience</span></span>

<span data-ttu-id="28000-205">Для обеспечения быстрого извлечения кэшированных данных решения кэширования обычно используют хранилище в памяти, но память является ограниченным ресурсом и может быть расширена.</span><span class="sxs-lookup"><span data-stu-id="28000-205">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="28000-206">Хранение часто используемых данных в кэше.</span><span class="sxs-lookup"><span data-stu-id="28000-206">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="28000-207">Как правило, кэш Redis обеспечивает более высокую пропускную способность и меньшую задержку, чем кэш SQL Server.</span><span class="sxs-lookup"><span data-stu-id="28000-207">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="28000-208">Однако для определения характеристик производительности стратегий кэширования обычно требуется тестирование производительности.</span><span class="sxs-lookup"><span data-stu-id="28000-208">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="28000-209">Если SQL Server используется в качестве резервного хранилища распределенного кэша, использование той же базы данных для кэша и обычного хранилища данных и извлечения приложения может негативно сказаться на производительности обоих.</span><span class="sxs-lookup"><span data-stu-id="28000-209">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="28000-210">Рекомендуется использовать выделенный экземпляр SQL Server для резервного хранилища распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="28000-210">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28000-211">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="28000-211">Additional resources</span></span>

* [<span data-ttu-id="28000-212">Кэш Redis в Azure</span><span class="sxs-lookup"><span data-stu-id="28000-212">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="28000-213">База данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="28000-213">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="28000-214">[Поставщик ASP.NET Core IDistributedCache для NCache в веб-фермах](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на сайте GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="28000-214">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
