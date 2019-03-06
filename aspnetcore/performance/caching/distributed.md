---
title: Распределенное кэширование в ASP.NET Core
author: guardrex
description: Сведения об использовании ASP.NET Core распределенного кэша для повышения производительности приложения и масштабируемости, особенно в среде фермы облаком и сервером.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: performance/caching/distributed
ms.openlocfilehash: a7850e317dfa3b54f1980902b3dcd6b096effa15
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346123"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="17c59-103">Распределенное кэширование в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17c59-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="17c59-104">По [Люк Лэтем](https://github.com/guardrex) и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="17c59-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="17c59-105">Распределенный кэш — это кэш, который совместно используется несколькими серверами приложений, обычно поддерживаются как внешняя служба на серверы приложений, обращающихся к ней.</span><span class="sxs-lookup"><span data-stu-id="17c59-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="17c59-106">Распределенный кэш может повысить производительность и масштабируемость приложения ASP.NET Core, особенно в том случае, если приложение будет размещено в облачную службу или ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="17c59-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="17c59-107">Распределенный кэш имеет ряд преимуществ по сравнению с другими кэширования сценариев, где кэшированные данные хранятся на серверах отдельных приложений.</span><span class="sxs-lookup"><span data-stu-id="17c59-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="17c59-108">Когда кэшированные данные распределяются, эти данные:</span><span class="sxs-lookup"><span data-stu-id="17c59-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="17c59-109">— *Согласовано* (согласованный) между запросами на несколько серверов.</span><span class="sxs-lookup"><span data-stu-id="17c59-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="17c59-110">Сохранять работоспособность после перезагрузки сервера и развертывания приложений.</span><span class="sxs-lookup"><span data-stu-id="17c59-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="17c59-111">Не использует локальную память.</span><span class="sxs-lookup"><span data-stu-id="17c59-111">Doesn't use local memory.</span></span>

<span data-ttu-id="17c59-112">Конфигурация распределенного кэша зависит от реализации.</span><span class="sxs-lookup"><span data-stu-id="17c59-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="17c59-113">В этой статье описывается, как настроить SQL Server и распределенные кэши Redis.</span><span class="sxs-lookup"><span data-stu-id="17c59-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="17c59-114">Реализации сторонних также доступны, такие как [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на сайте GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="17c59-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="17c59-115">Независимо от того, какую реализацию установлен, приложение взаимодействует с кэша, используя <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейс.</span><span class="sxs-lookup"><span data-stu-id="17c59-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="17c59-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="17c59-116">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="17c59-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="17c59-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="17c59-118">Использование SQL Server распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) пакета.</span><span class="sxs-lookup"><span data-stu-id="17c59-118">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="17c59-119">Для использования Redis распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) пакета.</span><span class="sxs-lookup"><span data-stu-id="17c59-119">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="17c59-120">Пакет Redis не включен в `Microsoft.AspNetCore.App` пакета, поэтому необходимо сослаться на пакет, Redis отдельно в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="17c59-120">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="17c59-121">Использование SQL Server распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) пакета.</span><span class="sxs-lookup"><span data-stu-id="17c59-121">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="17c59-122">Для использования Redis распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) пакета.</span><span class="sxs-lookup"><span data-stu-id="17c59-122">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="17c59-123">Пакет Redis не включен в `Microsoft.AspNetCore.App` пакета, поэтому необходимо сослаться на пакет, Redis отдельно в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="17c59-123">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="17c59-124">Интерфейс IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="17c59-124">IDistributedCache interface</span></span>

<span data-ttu-id="17c59-125"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Интерфейс предоставляет следующие методы для работы с элементами в реализации распределенного кэша:</span><span class="sxs-lookup"><span data-stu-id="17c59-125">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="17c59-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Принимает ключ строкового типа и извлекает кэшированного элемента как `byte[]` массив Если найдена в кэше.</span><span class="sxs-lookup"><span data-stu-id="17c59-126"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="17c59-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Добавляет элемент (как `byte[]` массива) в кэш, с помощью строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="17c59-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="17c59-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Обновляет объект в кэше, по его ключу, сброс его скользящее время ожидания (если таковые имеются).</span><span class="sxs-lookup"><span data-stu-id="17c59-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="17c59-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Удаляет элемент кэша на основе его строку ключа.</span><span class="sxs-lookup"><span data-stu-id="17c59-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="17c59-130">Установить службы распределенного кэширования</span><span class="sxs-lookup"><span data-stu-id="17c59-130">Establish distributed caching services</span></span>

<span data-ttu-id="17c59-131">Зарегистрируйте реализацию <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="17c59-131">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="17c59-132">Предоставляемые платформой реализации, описанных в этом разделе включают:</span><span class="sxs-lookup"><span data-stu-id="17c59-132">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="17c59-133">Распределенные памяти кэша</span><span class="sxs-lookup"><span data-stu-id="17c59-133">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="17c59-134">Распределенный кэш SQL Server</span><span class="sxs-lookup"><span data-stu-id="17c59-134">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="17c59-135">Распределенный кэш Redis</span><span class="sxs-lookup"><span data-stu-id="17c59-135">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="17c59-136">Распределенные памяти кэша</span><span class="sxs-lookup"><span data-stu-id="17c59-136">Distributed Memory Cache</span></span>

<span data-ttu-id="17c59-137">Распределенного кэша в памяти (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) — это реализация предоставляемые платформой <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> , сохраняет элементы в памяти.</span><span class="sxs-lookup"><span data-stu-id="17c59-137">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="17c59-138">Распределенный кеш памяти не фактический распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="17c59-138">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="17c59-139">Кэшированные элементы хранятся в экземпляре приложения на сервере, где выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="17c59-139">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="17c59-140">Распределенным кэшем памяти — это полезные реализация:</span><span class="sxs-lookup"><span data-stu-id="17c59-140">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="17c59-141">В сценариях разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="17c59-141">In development and testing scenarios.</span></span>
* <span data-ttu-id="17c59-142">При использовании одного сервера в рабочей среде и потребление памяти не является проблемой.</span><span class="sxs-lookup"><span data-stu-id="17c59-142">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="17c59-143">Реализация краткие описания распределенным кэшем памяти в кэше хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="17c59-143">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="17c59-144">Он позволяет реализовать настоящую распределенного кэширования решения в будущем в случае нескольких узлов или привести к необходимости устойчивость к сбоям.</span><span class="sxs-lookup"><span data-stu-id="17c59-144">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="17c59-145">Пример приложения использует распределенного кэша в памяти при запуске приложения в среде разработки в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="17c59-145">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="17c59-146">Кэш распределенных SQL Server</span><span class="sxs-lookup"><span data-stu-id="17c59-146">Distributed SQL Server Cache</span></span>

<span data-ttu-id="17c59-147">Реализация распределенного кэша SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) позволяет распределенного кэша использовать базу данных SQL Server в качестве резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="17c59-147">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="17c59-148">Чтобы создать таблицу SQL Server кэшированного элемента в экземпляр SQL Server, можно использовать `sql-cache` средство.</span><span class="sxs-lookup"><span data-stu-id="17c59-148">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="17c59-149">Средство создает таблицу с именем и схемой, указанной вами.</span><span class="sxs-lookup"><span data-stu-id="17c59-149">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="17c59-150">Создать таблицу в SQL Server, выполнив `sql-cache create` команды.</span><span class="sxs-lookup"><span data-stu-id="17c59-150">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="17c59-151">Укажите экземпляр SQL Server (`Data Source`), базы данных (`Initial Catalog`), схемы (например, `dbo`) и имя таблицы (например, `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="17c59-151">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="17c59-152">Чтобы указать, что средство успешно записывается сообщение:</span><span class="sxs-lookup"><span data-stu-id="17c59-152">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="17c59-153">Таблицу, созданную `sql-cache` средство имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="17c59-153">The table created by the `sql-cache` tool has the following schema:</span></span>

![Таблицы кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="17c59-155">Приложение следует управлять значения кэша, используя экземпляр <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, а не <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span><span class="sxs-lookup"><span data-stu-id="17c59-155">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="17c59-156">Пример приложения реализует <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> в среде не разработки в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="17c59-156">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

> [!NOTE]
> <span data-ttu-id="17c59-157">Объект <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (и при необходимости <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> и <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) обычно хранятся вне системы управления версиями (например, хранимая с [Secret Manager](xref:security/app-secrets) или в *appsettings.json* / *appsettings. {СРЕДА} .json* файлов).</span><span class="sxs-lookup"><span data-stu-id="17c59-157">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="17c59-158">Строка подключения может содержать учетные данные, которые должны храниться вне системы управления версиями.</span><span class="sxs-lookup"><span data-stu-id="17c59-158">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="17c59-159">Кэш Redis для распределенных</span><span class="sxs-lookup"><span data-stu-id="17c59-159">Distributed Redis Cache</span></span>

<span data-ttu-id="17c59-160">[Redis](https://redis.io/) -это хранилище данных в памяти с открытым исходным кодом, которое часто используется в качестве распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="17c59-160">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="17c59-161">Redis можно использовать локально, и можно настроить [кэша Redis для Azure](https://azure.microsoft.com/services/cache/) для приложения ASP.NET Core, размещенных в Azure.</span><span class="sxs-lookup"><span data-stu-id="17c59-161">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="17c59-162">Приложения настраивает реализации кэша с помощью `RedisCache` экземпляра (`AddStackExchangeRedisCache`) в среде не разработки в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="17c59-162">An app configures the cache implementation using a `RedisCache` instance (`AddStackExchangeRedisCache`) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="17c59-163">Приложения настраивает реализации кэша с помощью <xref:Microsoft.Extensions.Caching.Redis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="17c59-163">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="17c59-164">Чтобы установить Redis на локальном компьютере:</span><span class="sxs-lookup"><span data-stu-id="17c59-164">To install Redis on your local machine:</span></span>

* <span data-ttu-id="17c59-165">Установка [Chocolatey Redis пакета](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="17c59-165">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="17c59-166">Запустите `redis-server` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="17c59-166">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="17c59-167">Использование распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="17c59-167">Use the distributed cache</span></span>

<span data-ttu-id="17c59-168">Чтобы использовать <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейсом, запросить экземпляр <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> из любого конструктора в приложении.</span><span class="sxs-lookup"><span data-stu-id="17c59-168">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="17c59-169">Экземпляр предоставляется [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="17c59-169">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="17c59-170">При запуске приложения, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> внедряется в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="17c59-170">When the app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="17c59-171">Текущее время кэшируется с помощью <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Дополнительные сведения см. в разделе [веб-узел: Интерфейс IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span><span class="sxs-lookup"><span data-stu-id="17c59-171">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

<span data-ttu-id="17c59-172">Пример приложения внедряет <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `IndexModel` для использования на странице индекса.</span><span class="sxs-lookup"><span data-stu-id="17c59-172">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="17c59-173">Каждый раз при загрузке страницы индекса кэша проверяется на время кэширования в `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="17c59-173">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="17c59-174">Если время кэширования еще не истек, отображается время.</span><span class="sxs-lookup"><span data-stu-id="17c59-174">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="17c59-175">Если 20 секунд, истекших с момента последнего обращения к время кэширования (последний раз страница была загружена), на странице отображается *кэшированных Time Expired*.</span><span class="sxs-lookup"><span data-stu-id="17c59-175">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="17c59-176">Немедленно обновить кэшированные время на текущий момент времени, выбрав **сбросить время кэширования** кнопки.</span><span class="sxs-lookup"><span data-stu-id="17c59-176">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="17c59-177">Триггеры кнопку `OnPostResetCachedTime` метод обработчика.</span><span class="sxs-lookup"><span data-stu-id="17c59-177">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> <span data-ttu-id="17c59-178">Время жизни экземпляров <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (по крайней мере, для встроенных реализаций) не обязательно должно быть ограничено одним объектом или блоком.</span><span class="sxs-lookup"><span data-stu-id="17c59-178">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="17c59-179">Вы также можете создать <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> экземпляра везде, где это может потребоваться, а не с помощью внедрения Зависимостей, но создание экземпляра в коде может сделать код труднее тестировать и нарушает [принципу явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="17c59-179">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="17c59-180">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="17c59-180">Recommendations</span></span>

<span data-ttu-id="17c59-181">При принятии решения о реализации <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> лучше всего подходит для вашего приложения, необходимо учитывать следующее:</span><span class="sxs-lookup"><span data-stu-id="17c59-181">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="17c59-182">Существующую инфраструктуру</span><span class="sxs-lookup"><span data-stu-id="17c59-182">Existing infrastructure</span></span>
* <span data-ttu-id="17c59-183">Требования к производительности</span><span class="sxs-lookup"><span data-stu-id="17c59-183">Performance requirements</span></span>
* <span data-ttu-id="17c59-184">Стоимость</span><span class="sxs-lookup"><span data-stu-id="17c59-184">Cost</span></span>
* <span data-ttu-id="17c59-185">Опыт рабочей группы</span><span class="sxs-lookup"><span data-stu-id="17c59-185">Team experience</span></span>

<span data-ttu-id="17c59-186">Решения кэширования обычно используют хранилище в памяти для предоставления быстрого получения кэшированных данных, но память является ограниченным ресурсом и дорогостоящей развернуть.</span><span class="sxs-lookup"><span data-stu-id="17c59-186">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="17c59-187">Только хранилище часто использовать данные в кэше.</span><span class="sxs-lookup"><span data-stu-id="17c59-187">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="17c59-188">Как правило кэш Redis обеспечивает высокую пропускную способность и меньшую задержку, чем при использовании кэша SQL Server.</span><span class="sxs-lookup"><span data-stu-id="17c59-188">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="17c59-189">Тем не менее тестирование производительности обычно требуется, чтобы определить характеристики производительности стратегии кэширования.</span><span class="sxs-lookup"><span data-stu-id="17c59-189">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="17c59-190">Когда SQL Server используется в качестве резервного хранилища распределенного кэша, использовать ту же базу данных для кэша и хранения обычных данных приложения и извлечения может отрицательно повлиять на их производительность.</span><span class="sxs-lookup"><span data-stu-id="17c59-190">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="17c59-191">Мы рекомендуем использовать выделенный экземпляр SQL Server для распределенного кэша, резервным хранилищем.</span><span class="sxs-lookup"><span data-stu-id="17c59-191">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="17c59-192">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="17c59-192">Additional resources</span></span>

* [<span data-ttu-id="17c59-193">Кэш в Azure redis</span><span class="sxs-lookup"><span data-stu-id="17c59-193">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="17c59-194">База данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="17c59-194">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="17c59-195">[Поставщик IDistributedCache для NCache в веб-ферм с ASP.NET Core](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на сайте GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="17c59-195">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
