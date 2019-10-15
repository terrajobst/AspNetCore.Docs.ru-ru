---
title: Распределенное кэширование в ASP.NET Core
author: guardrex
description: Узнайте, как использовать распределенный кэш ASP.NET Core для повышения производительности и масштабируемости приложений, особенно в среде облака или фермы серверов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: performance/caching/distributed
ms.openlocfilehash: dbcdfcd07877fabfe6d18cd4d840b5597afa1afd
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081551"
---
# <a name="distributed-caching-in-aspnet-core"></a><span data-ttu-id="75a0f-103">Распределенное кэширование в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="75a0f-103">Distributed caching in ASP.NET Core</span></span>

<span data-ttu-id="75a0f-104">[Люк ЛаСаМ](https://github.com/guardrex) и [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="75a0f-104">By [Luke Latham](https://github.com/guardrex) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="75a0f-105">Распределенный кэш — это кэш, совместно используемый несколькими серверами приложений, обычно поддерживаемый как внешняя служба для серверов приложений, обращающихся к ней.</span><span class="sxs-lookup"><span data-stu-id="75a0f-105">A distributed cache is a cache shared by multiple app servers, typically maintained as an external service to the app servers that access it.</span></span> <span data-ttu-id="75a0f-106">Распределенный кэш может повысить производительность и масштабируемость приложения ASP.NET Core, особенно если приложение размещено в облачной службе или ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="75a0f-106">A distributed cache can improve the performance and scalability of an ASP.NET Core app, especially when the app is hosted by a cloud service or a server farm.</span></span>

<span data-ttu-id="75a0f-107">Распределенный кэш имеет несколько преимуществ по сравнению с другими сценариями кэширования, в которых кэшированные данные хранятся на отдельных серверах приложений.</span><span class="sxs-lookup"><span data-stu-id="75a0f-107">A distributed cache has several advantages over other caching scenarios where cached data is stored on individual app servers.</span></span>

<span data-ttu-id="75a0f-108">При распределении кэшированных данных данные:</span><span class="sxs-lookup"><span data-stu-id="75a0f-108">When cached data is distributed, the data:</span></span>

* <span data-ttu-id="75a0f-109">— Согласованность между запросами к нескольким серверам.</span><span class="sxs-lookup"><span data-stu-id="75a0f-109">Is *coherent* (consistent) across requests to multiple servers.</span></span>
* <span data-ttu-id="75a0f-110">Выдерживает перезапуски сервера и развертывание приложений.</span><span class="sxs-lookup"><span data-stu-id="75a0f-110">Survives server restarts and app deployments.</span></span>
* <span data-ttu-id="75a0f-111">Не использует локальную память.</span><span class="sxs-lookup"><span data-stu-id="75a0f-111">Doesn't use local memory.</span></span>

<span data-ttu-id="75a0f-112">Конфигурация распределенного кэша зависит от конкретной реализации.</span><span class="sxs-lookup"><span data-stu-id="75a0f-112">Distributed cache configuration is implementation specific.</span></span> <span data-ttu-id="75a0f-113">В этой статье описывается настройка распределенных кэшей SQL Server и Redis.</span><span class="sxs-lookup"><span data-stu-id="75a0f-113">This article describes how to configure SQL Server and Redis distributed caches.</span></span> <span data-ttu-id="75a0f-114">Также доступны реализации сторонних производителей, например [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на GitHub](https://github.com/Alachisoft/NCache)).</span><span class="sxs-lookup"><span data-stu-id="75a0f-114">Third party implementations are also available, such as [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache)).</span></span> <span data-ttu-id="75a0f-115">Независимо от выбранной реализации приложение взаимодействует с кэшем с помощью <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейса.</span><span class="sxs-lookup"><span data-stu-id="75a0f-115">Regardless of which implementation is selected, the app interacts with the cache using the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface.</span></span>

<span data-ttu-id="75a0f-116">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="75a0f-116">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75a0f-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="75a0f-117">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75a0f-118">Чтобы использовать SQL Server распределенный кэш, добавьте ссылку на пакет в пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="75a0f-118">To use a SQL Server distributed cache, add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="75a0f-119">Чтобы использовать распределенный кэш Redis, добавьте ссылку на пакет в пакет [Microsoft. Extensions. Caching. стаккексчанжередис](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="75a0f-119">To use a Redis distributed cache, add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="75a0f-120">Чтобы использовать SQL Server распределенный кэш, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="75a0f-120">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="75a0f-121">Чтобы использовать распределенный кэш Redis, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft. Extensions. Caching. стаккексчанжередис](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .</span><span class="sxs-lookup"><span data-stu-id="75a0f-121">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) package.</span></span> <span data-ttu-id="75a0f-122">Пакет Redis не входит в `Microsoft.AspNetCore.App` пакет, поэтому необходимо отдельно ссылаться на пакет Redis в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="75a0f-122">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="75a0f-123">Чтобы использовать SQL Server распределенный кэш, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .</span><span class="sxs-lookup"><span data-stu-id="75a0f-123">To use a SQL Server distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) package.</span></span>

<span data-ttu-id="75a0f-124">Чтобы использовать распределенный кэш Redis, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft. Extensions. Caching. Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) .</span><span class="sxs-lookup"><span data-stu-id="75a0f-124">To use a Redis distributed cache, reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and add a package reference to the [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) package.</span></span> <span data-ttu-id="75a0f-125">Пакет Redis не входит в `Microsoft.AspNetCore.App` пакет, поэтому необходимо отдельно ссылаться на пакет Redis в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="75a0f-125">The Redis package isn't included in the `Microsoft.AspNetCore.App` package, so you must reference the Redis package separately in your project file.</span></span>

::: moniker-end

## <a name="idistributedcache-interface"></a><span data-ttu-id="75a0f-126">Интерфейс IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="75a0f-126">IDistributedCache interface</span></span>

<span data-ttu-id="75a0f-127"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Интерфейс предоставляет следующие методы для управления элементами в реализации распределенного кэша:</span><span class="sxs-lookup"><span data-stu-id="75a0f-127">The <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface provides the following methods to manipulate items in the distributed cache implementation:</span></span>

* <span data-ttu-id="75a0f-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> `byte[]` Принимает строковый ключ и получает кэшированный элемент в виде массива, если он найден в кэше. &ndash;</span><span class="sxs-lookup"><span data-stu-id="75a0f-128"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Accepts a string key and retrieves a cached item as a `byte[]` array if found in the cache.</span></span>
* <span data-ttu-id="75a0f-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>`byte[]` , <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> Добавляет&ndash; элемент (в виде массива) в кэш с помощью строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="75a0f-129"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Adds an item (as `byte[]` array) to the cache using a string key.</span></span>
* <span data-ttu-id="75a0f-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> Обновляетэлементвкэшенаосновеегоключа,переустанавливаяскользящийтайм-аутистечениясрокадействия(&ndash; если он есть).</span><span class="sxs-lookup"><span data-stu-id="75a0f-130"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>
* <span data-ttu-id="75a0f-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ,&ndash; Удаляет элемент кэша на основе его строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="75a0f-131"><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Removes a cache item based on its string key.</span></span>

## <a name="establish-distributed-caching-services"></a><span data-ttu-id="75a0f-132">Установка служб распределенного кэширования</span><span class="sxs-lookup"><span data-stu-id="75a0f-132">Establish distributed caching services</span></span>

<span data-ttu-id="75a0f-133">Зарегистрируйте реализацию <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="75a0f-133">Register an implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="75a0f-134">Реализации, предоставляемые платформой, описанные в этом разделе, включают:</span><span class="sxs-lookup"><span data-stu-id="75a0f-134">Framework-provided implementations described in this topic include:</span></span>

* [<span data-ttu-id="75a0f-135">Кэш распределенной памяти</span><span class="sxs-lookup"><span data-stu-id="75a0f-135">Distributed Memory Cache</span></span>](#distributed-memory-cache)
* [<span data-ttu-id="75a0f-136">Распределенный кэш SQL Server</span><span class="sxs-lookup"><span data-stu-id="75a0f-136">Distributed SQL Server cache</span></span>](#distributed-sql-server-cache)
* [<span data-ttu-id="75a0f-137">Распределенный кэш Redis</span><span class="sxs-lookup"><span data-stu-id="75a0f-137">Distributed Redis cache</span></span>](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a><span data-ttu-id="75a0f-138">Кэш распределенной памяти</span><span class="sxs-lookup"><span data-stu-id="75a0f-138">Distributed Memory Cache</span></span>

<span data-ttu-id="75a0f-139">Кэш распределенной памяти (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) — это предоставляемая платформой <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> реализация, которая хранит элементы в памяти.</span><span class="sxs-lookup"><span data-stu-id="75a0f-139">The Distributed Memory Cache (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) is a framework-provided implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> that stores items in memory.</span></span> <span data-ttu-id="75a0f-140">Кэш распределенной памяти не является фактическим распределенным кэшем.</span><span class="sxs-lookup"><span data-stu-id="75a0f-140">The Distributed Memory Cache isn't an actual distributed cache.</span></span> <span data-ttu-id="75a0f-141">Кэшированные элементы хранятся в экземпляре приложения на сервере, где выполняется приложение.</span><span class="sxs-lookup"><span data-stu-id="75a0f-141">Cached items are stored by the app instance on the server where the app is running.</span></span>

<span data-ttu-id="75a0f-142">Кэш распределенной памяти — это полезная реализация:</span><span class="sxs-lookup"><span data-stu-id="75a0f-142">The Distributed Memory Cache is a useful implementation:</span></span>

* <span data-ttu-id="75a0f-143">В сценариях разработки и тестирования.</span><span class="sxs-lookup"><span data-stu-id="75a0f-143">In development and testing scenarios.</span></span>
* <span data-ttu-id="75a0f-144">Если в рабочей среде используется один сервер, а использование памяти не является проблемой.</span><span class="sxs-lookup"><span data-stu-id="75a0f-144">When a single server is used in production and memory consumption isn't an issue.</span></span> <span data-ttu-id="75a0f-145">Реализация кэша распределенной памяти абстрагирует хранилище кэшированных данных.</span><span class="sxs-lookup"><span data-stu-id="75a0f-145">Implementing the Distributed Memory Cache abstracts cached data storage.</span></span> <span data-ttu-id="75a0f-146">Она позволяет реализовать истинное решение распределенного кэширования в будущем, если потребуется несколько узлов или отказоустойчивость.</span><span class="sxs-lookup"><span data-stu-id="75a0f-146">It allows for implementing a true distributed caching solution in the future if multiple nodes or fault tolerance become necessary.</span></span>

<span data-ttu-id="75a0f-147">Пример приложения использует распределенный кэш памяти при запуске приложения в среде разработки в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="75a0f-147">The sample app makes use of the Distributed Memory Cache when the app is run in the Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a><span data-ttu-id="75a0f-148">Распределенный кэш SQL Server</span><span class="sxs-lookup"><span data-stu-id="75a0f-148">Distributed SQL Server Cache</span></span>

<span data-ttu-id="75a0f-149">Реализация кэша распределенного SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) позволяет распределенному кэшу использовать базу данных SQL Server в качестве резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="75a0f-149">The Distributed SQL Server Cache implementation (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="75a0f-150">Чтобы создать SQL Server таблицу кэшированных элементов в экземпляре SQL Server, можно использовать `sql-cache` средство.</span><span class="sxs-lookup"><span data-stu-id="75a0f-150">To create a SQL Server cached item table in a SQL Server instance, you can use the `sql-cache` tool.</span></span> <span data-ttu-id="75a0f-151">Средство создает таблицу с указанными именем и схемой.</span><span class="sxs-lookup"><span data-stu-id="75a0f-151">The tool creates a table with the name and schema that you specify.</span></span>

<span data-ttu-id="75a0f-152">Создайте таблицу в SQL Server, выполнив `sql-cache create` команду.</span><span class="sxs-lookup"><span data-stu-id="75a0f-152">Create a table in SQL Server by running the `sql-cache create` command.</span></span> <span data-ttu-id="75a0f-153">Укажите`Data Source`SQL Server экземпляр (), базу данных (`Initial Catalog`), схему (например, `dbo`) и имя таблицы (например, `TestCache`):</span><span class="sxs-lookup"><span data-stu-id="75a0f-153">Provide the SQL Server instance (`Data Source`), database (`Initial Catalog`), schema (for example, `dbo`), and table name (for example, `TestCache`):</span></span>

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

<span data-ttu-id="75a0f-154">В журнал заносится сообщение, указывающее, что средство прошло успешно:</span><span class="sxs-lookup"><span data-stu-id="75a0f-154">A message is logged to indicate that the tool was successful:</span></span>

```console
Table and index were created successfully.
```

<span data-ttu-id="75a0f-155">Таблица, созданная `sql-cache` средством, имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="75a0f-155">The table created by the `sql-cache` tool has the following schema:</span></span>

![Таблица кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> <span data-ttu-id="75a0f-157">Приложение должно манипулировать значениями кэша с помощью экземпляра <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, а <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>не.</span><span class="sxs-lookup"><span data-stu-id="75a0f-157">An app should manipulate cache values using an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, not a <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.</span></span>

<span data-ttu-id="75a0f-158">Пример приложения реализует <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> в среде, не являющейся средой разработки `Startup.ConfigureServices`, в:</span><span class="sxs-lookup"><span data-stu-id="75a0f-158">The sample app implements <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> in a non-Development environment in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="75a0f-159"><xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (И, при необходимости, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> и <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) обычно хранятся вне системы управления версиями (например, хранится в [диспетчере секретов](xref:security/app-secrets) или в *appsettings.json*/*appsettings. {Файлы среды}. JSON* ).</span><span class="sxs-lookup"><span data-stu-id="75a0f-159">A <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (and optionally, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> and <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) are typically stored outside of source control (for example, stored by the [Secret Manager](xref:security/app-secrets) or in *appsettings.json*/*appsettings.{ENVIRONMENT}.json* files).</span></span> <span data-ttu-id="75a0f-160">Строка подключения может содержать учетные данные, которые должны содержаться в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="75a0f-160">The connection string may contain credentials that should be kept out of source control systems.</span></span>

### <a name="distributed-redis-cache"></a><span data-ttu-id="75a0f-161">Распределенный кэш Redis</span><span class="sxs-lookup"><span data-stu-id="75a0f-161">Distributed Redis Cache</span></span>

<span data-ttu-id="75a0f-162">[Redis](https://redis.io/) -это хранилище данных в памяти с открытым исходным кодом, которое часто используется в качестве распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="75a0f-162">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="75a0f-163">Вы можете использовать Redis локально, и вы можете настроить [кэш Redis для Azure](https://azure.microsoft.com/services/cache/) для приложения ASP.NET Core, размещенного в Azure.</span><span class="sxs-lookup"><span data-stu-id="75a0f-163">You can use Redis locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for an Azure-hosted ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75a0f-164">Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) в среде, не являющейся средой разработки, `Startup.ConfigureServices`в:</span><span class="sxs-lookup"><span data-stu-id="75a0f-164">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="75a0f-165">Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) в среде, не являющейся средой разработки, `Startup.ConfigureServices`в:</span><span class="sxs-lookup"><span data-stu-id="75a0f-165">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) in a non-Development environment in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="75a0f-166">Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.Redis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span><span class="sxs-lookup"><span data-stu-id="75a0f-166">An app configures the cache implementation using a <xref:Microsoft.Extensions.Caching.Redis.RedisCache> instance (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):</span></span>

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

<span data-ttu-id="75a0f-167">Чтобы установить Redis на локальном компьютере, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="75a0f-167">To install Redis on your local machine:</span></span>

* <span data-ttu-id="75a0f-168">Установите [пакет шоколадного Redis](https://chocolatey.org/packages/redis-64/).</span><span class="sxs-lookup"><span data-stu-id="75a0f-168">Install the [Chocolatey Redis package](https://chocolatey.org/packages/redis-64/).</span></span>
* <span data-ttu-id="75a0f-169">Запустите `redis-server` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="75a0f-169">Run `redis-server` from a command prompt.</span></span>

## <a name="use-the-distributed-cache"></a><span data-ttu-id="75a0f-170">Использование распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="75a0f-170">Use the distributed cache</span></span>

<span data-ttu-id="75a0f-171">Чтобы использовать <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейс, запросите <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> экземпляр из любого конструктора в приложении.</span><span class="sxs-lookup"><span data-stu-id="75a0f-171">To use the <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> interface, request an instance of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> from any constructor in the app.</span></span> <span data-ttu-id="75a0f-172">Экземпляр предоставляется путем [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="75a0f-172">The instance is provided by [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="75a0f-173">При запуске <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> примера приложения внедряется в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="75a0f-173">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="75a0f-174">Текущее время кэшируется с помощью <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (Дополнительные сведения см. в [разделе универсальный узел: Ихостаппликатионлифетиме](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span><span class="sxs-lookup"><span data-stu-id="75a0f-174">The current time is cached using <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (for more information, see [Generic Host: IHostApplicationLifetime](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):</span></span>

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="75a0f-175">При запуске <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> примера приложения внедряется в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="75a0f-175">When the sample app starts, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is injected into `Startup.Configure`.</span></span> <span data-ttu-id="75a0f-176">Текущее время кэшируется с помощью <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Дополнительные сведения см. в [разделе веб-узел: Интерфейс](xref:fundamentals/host/web-host#iapplicationlifetime-interface)иаппликатионлифетиме):</span><span class="sxs-lookup"><span data-stu-id="75a0f-176">The current time is cached using <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (for more information, see [Web Host: IApplicationLifetime interface](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):</span></span>

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

<span data-ttu-id="75a0f-177">Пример приложения внедряет <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в объект `IndexModel` для использования на странице индекса.</span><span class="sxs-lookup"><span data-stu-id="75a0f-177">The sample app injects <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> into the `IndexModel` for use by the Index page.</span></span>

<span data-ttu-id="75a0f-178">При каждой загрузке страницы индекса кэш проверяется на наличие кэшированного времени в `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="75a0f-178">Each time the Index page is loaded, the cache is checked for the cached time in `OnGetAsync`.</span></span> <span data-ttu-id="75a0f-179">Если время кэширования не истекло, отображается время.</span><span class="sxs-lookup"><span data-stu-id="75a0f-179">If the cached time hasn't expired, the time is displayed.</span></span> <span data-ttu-id="75a0f-180">Если прошло 20 секунд с момента последнего обращения к кэшированному времени (время последней загрузки этой страницы), на странице отображается *время ожидания кэширования*.</span><span class="sxs-lookup"><span data-stu-id="75a0f-180">If 20 seconds have elapsed since the last time the cached time was accessed (the last time this page was loaded), the page displays *Cached Time Expired*.</span></span>

<span data-ttu-id="75a0f-181">Немедленно обновите кэшированное время на текущее время, нажав кнопку **сбросить кэшированное** время.</span><span class="sxs-lookup"><span data-stu-id="75a0f-181">Immediately update the cached time to the current time by selecting the **Reset Cached Time** button.</span></span> <span data-ttu-id="75a0f-182">Кнопка запускает `OnPostResetCachedTime` метод обработчика.</span><span class="sxs-lookup"><span data-stu-id="75a0f-182">The button triggers the `OnPostResetCachedTime` handler method.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="75a0f-183">Время жизни экземпляров <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (по крайней мере, для встроенных реализаций) не обязательно должно быть ограничено одним объектом или блоком.</span><span class="sxs-lookup"><span data-stu-id="75a0f-183">There's no need to use a Singleton or Scoped lifetime for <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instances (at least for the built-in implementations).</span></span>
>
> <span data-ttu-id="75a0f-184">Кроме того, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> экземпляр можно создать везде, где вам может понадобиться, а не использовать di, но создание экземпляра в коде может усложнить тестирование и нарушает [принцип явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span><span class="sxs-lookup"><span data-stu-id="75a0f-184">You can also create an <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> instance wherever you might need one instead of using DI, but creating an instance in code can make your code harder to test and violates the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).</span></span>

## <a name="recommendations"></a><span data-ttu-id="75a0f-185">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="75a0f-185">Recommendations</span></span>

<span data-ttu-id="75a0f-186">При принятии решения о том, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> какая реализация лучше подходит для вашего приложения, учитывайте следующее.</span><span class="sxs-lookup"><span data-stu-id="75a0f-186">When deciding which implementation of <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> is best for your app, consider the following:</span></span>

* <span data-ttu-id="75a0f-187">Существующая инфраструктура</span><span class="sxs-lookup"><span data-stu-id="75a0f-187">Existing infrastructure</span></span>
* <span data-ttu-id="75a0f-188">Требования к производительности</span><span class="sxs-lookup"><span data-stu-id="75a0f-188">Performance requirements</span></span>
* <span data-ttu-id="75a0f-189">Стоимость</span><span class="sxs-lookup"><span data-stu-id="75a0f-189">Cost</span></span>
* <span data-ttu-id="75a0f-190">Опыт работы в группе</span><span class="sxs-lookup"><span data-stu-id="75a0f-190">Team experience</span></span>

<span data-ttu-id="75a0f-191">Для обеспечения быстрого извлечения кэшированных данных решения кэширования обычно используют хранилище в памяти, но память является ограниченным ресурсом и может быть расширена.</span><span class="sxs-lookup"><span data-stu-id="75a0f-191">Caching solutions usually rely on in-memory storage to provide fast retrieval of cached data, but memory is a limited resource and costly to expand.</span></span> <span data-ttu-id="75a0f-192">Хранение часто используемых данных в кэше.</span><span class="sxs-lookup"><span data-stu-id="75a0f-192">Only store commonly used data in a cache.</span></span>

<span data-ttu-id="75a0f-193">Как правило, кэш Redis обеспечивает более высокую пропускную способность и меньшую задержку, чем кэш SQL Server.</span><span class="sxs-lookup"><span data-stu-id="75a0f-193">Generally, a Redis cache provides higher throughput and lower latency than a SQL Server cache.</span></span> <span data-ttu-id="75a0f-194">Однако для определения характеристик производительности стратегий кэширования обычно требуется тестирование производительности.</span><span class="sxs-lookup"><span data-stu-id="75a0f-194">However, benchmarking is usually required to determine the performance characteristics of caching strategies.</span></span>

<span data-ttu-id="75a0f-195">Если SQL Server используется в качестве резервного хранилища распределенного кэша, использование той же базы данных для кэша и обычного хранилища данных и извлечения приложения может негативно сказаться на производительности обоих.</span><span class="sxs-lookup"><span data-stu-id="75a0f-195">When SQL Server is used as a distributed cache backing store, use of the same database for the cache and the app's ordinary data storage and retrieval can negatively impact the performance of both.</span></span> <span data-ttu-id="75a0f-196">Рекомендуется использовать выделенный экземпляр SQL Server для резервного хранилища распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="75a0f-196">We recommend using a dedicated SQL Server instance for the distributed cache backing store.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="75a0f-197">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="75a0f-197">Additional resources</span></span>

* [<span data-ttu-id="75a0f-198">Кэш Redis в Azure</span><span class="sxs-lookup"><span data-stu-id="75a0f-198">Redis Cache on Azure</span></span>](/azure/azure-cache-for-redis/)
* [<span data-ttu-id="75a0f-199">База данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="75a0f-199">SQL Database on Azure</span></span>](/azure/sql-database/)
* <span data-ttu-id="75a0f-200">[Поставщик ASP.NET Core IDistributedCache для NCache в веб-фермах](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на GitHub](https://github.com/Alachisoft/NCache))</span><span class="sxs-lookup"><span data-stu-id="75a0f-200">[ASP.NET Core IDistributedCache Provider for NCache in Web Farms](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache on GitHub](https://github.com/Alachisoft/NCache))</span></span>
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
