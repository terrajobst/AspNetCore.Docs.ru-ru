---
title: Работа с распределенным кэшем в ASP.NET Core
author: ardalis
description: Сведения об использовании ASP.NET Core распределенного кэширования для повышения производительности приложения и масштабируемость, особенно в среде фермы облако или сервера.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/distributed
ms.openlocfilehash: 6c595572641604d241c0c8f702d4f392afe34f71
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734462"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="f9d68-103">Работа с распределенным кэшем в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f9d68-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="f9d68-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="f9d68-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="f9d68-105">Распределенные кэши могут улучшить производительность и масштабируемость приложений ASP.NET Core, особенно в том случае, если размещенные в среде фермы облако или сервера.</span><span class="sxs-lookup"><span data-stu-id="f9d68-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="f9d68-106">В этой статье объясняется, как работать с ASP.NET Core встроенных распределенного кэша абстракции и реализации.</span><span class="sxs-lookup"><span data-stu-id="f9d68-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

<span data-ttu-id="f9d68-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f9d68-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="f9d68-108">Что такое распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="f9d68-108">What is a distributed cache</span></span>

<span data-ttu-id="f9d68-109">Распределенный кэш является общим для нескольких серверов приложений (см. [основы кэша](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="f9d68-109">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="f9d68-110">Информация в кэше не хранятся в памяти отдельных веб-серверов и кэшированных данных доступен для всех серверов приложений.</span><span class="sxs-lookup"><span data-stu-id="f9d68-110">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="f9d68-111">Это обеспечивает несколько преимуществ:</span><span class="sxs-lookup"><span data-stu-id="f9d68-111">This provides several advantages:</span></span>

1. <span data-ttu-id="f9d68-112">Кэшированные данные согласованного на всех веб-серверах.</span><span class="sxs-lookup"><span data-stu-id="f9d68-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="f9d68-113">Пользователи не видеть разные результаты в зависимости от веб-сервер обрабатывает их запроса</span><span class="sxs-lookup"><span data-stu-id="f9d68-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="f9d68-114">Кэшированные данные сохраняется после перезагрузки сервера web и развертываний.</span><span class="sxs-lookup"><span data-stu-id="f9d68-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="f9d68-115">Отдельные веб-серверов можно удалить или добавить без ущерба для кэша.</span><span class="sxs-lookup"><span data-stu-id="f9d68-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="f9d68-116">Хранилище данных источника содержит меньшее число запросов, выполненных на него (не с нескольких кэшей в памяти или нет кэшировать вообще).</span><span class="sxs-lookup"><span data-stu-id="f9d68-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="f9d68-117">Если используется SQL Server распределенного кэша, некоторые из этих преимуществ доступны только в значение true, если экземпляр отдельную базу данных для кэша, чем для источника данных приложения.</span><span class="sxs-lookup"><span data-stu-id="f9d68-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="f9d68-118">Как и любой кэш распределенного кэша могут существенно повысить скорость реагирования приложения, поскольку обычно можно получить данные из кэша намного быстрее, чем из реляционной базы данных (или веб-службы).</span><span class="sxs-lookup"><span data-stu-id="f9d68-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="f9d68-119">Конфигурация кэша зависит от конкретной реализации.</span><span class="sxs-lookup"><span data-stu-id="f9d68-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="f9d68-120">В этой статье описывается настройка оба Redis и кэширует распределенных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f9d68-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="f9d68-121">Независимо от того, какая реализация установлен, приложение взаимодействует с кэш, используя общий `IDistributedCache` интерфейс.</span><span class="sxs-lookup"><span data-stu-id="f9d68-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="f9d68-122">Интерфейс IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="f9d68-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="f9d68-123">`IDistributedCache` Интерфейс включает синхронные и асинхронные методы.</span><span class="sxs-lookup"><span data-stu-id="f9d68-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="f9d68-124">Интерфейс позволяет элементы добавлены, получение и удалены из реализации распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="f9d68-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="f9d68-125">`IDistributedCache` Интерфейс содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="f9d68-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="f9d68-126">**Get-GetAsync**</span><span class="sxs-lookup"><span data-stu-id="f9d68-126">**Get, GetAsync**</span></span>

<span data-ttu-id="f9d68-127">Принимает строковый ключ и извлекает кэшированного элемента как `byte[]` Если найден в кэше.</span><span class="sxs-lookup"><span data-stu-id="f9d68-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="f9d68-128">**Набор SetAsync**</span><span class="sxs-lookup"><span data-stu-id="f9d68-128">**Set, SetAsync**</span></span>

<span data-ttu-id="f9d68-129">Добавляет элемент (как `byte[]`) в кэш с помощью строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="f9d68-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="f9d68-130">**Обновление RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="f9d68-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="f9d68-131">Обновляет объект в кэше, по его ключу, сброс его скользящий срок хранения (если таковые имеются).</span><span class="sxs-lookup"><span data-stu-id="f9d68-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="f9d68-132">**Удалите асинхронно удаляет**</span><span class="sxs-lookup"><span data-stu-id="f9d68-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="f9d68-133">Удаляет запись по его ключу.</span><span class="sxs-lookup"><span data-stu-id="f9d68-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="f9d68-134">Для использования `IDistributedCache` интерфейс:</span><span class="sxs-lookup"><span data-stu-id="f9d68-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="f9d68-135">Добавьте необходимые пакеты NuGet в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="f9d68-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="f9d68-136">Настройка деталей реализации `IDistributedCache` в ваш `Startup` класса `ConfigureServices` метода и добавьте его в контейнер существует.</span><span class="sxs-lookup"><span data-stu-id="f9d68-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="f9d68-137">В приложении [по промежуточного слоя](xref:fundamentals/middleware/index) или классов контроллеров MVC, запросить экземпляр `IDistributedCache` из конструктора.</span><span class="sxs-lookup"><span data-stu-id="f9d68-137">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="f9d68-138">Экземпляр будут предоставляться [внедрения зависимостей](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="f9d68-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="f9d68-139">Нет необходимости использовать единственный элемент или Scoped время существования `IDistributedCache` экземпляров (хотя бы для встроенных реализаций).</span><span class="sxs-lookup"><span data-stu-id="f9d68-139">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="f9d68-140">Можно также создать экземпляр везде, где может потребоваться одно (вместо использования [внедрения зависимостей](../../fundamentals/dependency-injection.md)), но это может усложнить код для тестирования и приводит к нарушению [явные зависимости принцип](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="f9d68-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="f9d68-141">В следующем примере показано, как использовать экземпляр `IDistributedCache` в простой программный компонент:</span><span class="sxs-lookup"><span data-stu-id="f9d68-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="f9d68-142">В приведенном выше коде кэшированное значение является считываются, но никогда не записываются.</span><span class="sxs-lookup"><span data-stu-id="f9d68-142">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="f9d68-143">В этом примере значение устанавливается только при запуске сервера, а также не изменяется.</span><span class="sxs-lookup"><span data-stu-id="f9d68-143">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="f9d68-144">В случае многосерверной последний сервер запуск перезапишет все предыдущие значения, заданные на других серверах.</span><span class="sxs-lookup"><span data-stu-id="f9d68-144">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="f9d68-145">`Get` И `Set` методы используют `byte[]` типа.</span><span class="sxs-lookup"><span data-stu-id="f9d68-145">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="f9d68-146">Таким образом, строковое значение необходимо преобразовывать с помощью `Encoding.UTF8.GetString` (для `Get`) и `Encoding.UTF8.GetBytes` (для `Set`).</span><span class="sxs-lookup"><span data-stu-id="f9d68-146">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="f9d68-147">В следующем примере кода из *файла Startup.cs* показывает, что будет задаваться значение:</span><span class="sxs-lookup"><span data-stu-id="f9d68-147">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="f9d68-148">Поскольку `IDistributedCache` настраивается в `ConfigureServices` метода, она доступна для `Configure` методу в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="f9d68-148">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="f9d68-149">Добавив его в качестве параметра позволит настроенный экземпляр должен предоставляться через DI.</span><span class="sxs-lookup"><span data-stu-id="f9d68-149">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="f9d68-150">С помощью Redis распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="f9d68-150">Using a Redis distributed cache</span></span>

<span data-ttu-id="f9d68-151">[Redis](https://redis.io/) -хранилище данных в памяти открытым исходным кодом, который часто используется в качестве распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="f9d68-151">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="f9d68-152">Можно использовать локально, и можно настроить [кэш Azure Redis](https://azure.microsoft.com/services/cache/) для приложений ASP.NET Core, размещенной в Azure.</span><span class="sxs-lookup"><span data-stu-id="f9d68-152">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="f9d68-153">Настраивает приложение ASP.NET Core реализации кэша с помощью `RedisDistributedCache` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="f9d68-153">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="f9d68-154">Настройте реализацию Redis в `ConfigureServices` и откройте его в коде приложения, запросив экземпляр `IDistributedCache` (см. приведенный выше код).</span><span class="sxs-lookup"><span data-stu-id="f9d68-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="f9d68-155">В образце кода `RedisCache` реализация используется в том случае, если сервер настроен для `Staging` среды.</span><span class="sxs-lookup"><span data-stu-id="f9d68-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="f9d68-156">Таким образом `ConfigureStagingServices` настраивает метод `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="f9d68-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="f9d68-157">Чтобы установить Redis на локальном компьютере, необходимо установить пакет chocolatey [ https://chocolatey.org/packages/redis-64/ ](https://chocolatey.org/packages/redis-64/) и запустите `redis-server` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="f9d68-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="f9d68-158">Использование SQL Server распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="f9d68-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="f9d68-159">Реализация SqlServerCache допускает распределенного кэша для использования базы данных SQL Server в качестве резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="f9d68-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="f9d68-160">Создание SQL Server таблицы, можно использовать средство кэша sql, средство создает таблицу с именем и схемой, которые вы задаете.</span><span class="sxs-lookup"><span data-stu-id="f9d68-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="f9d68-161">Добавить `SqlConfig.Tools` для `<ItemGroup>` элемент файла проекта и выполнения `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="f9d68-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="f9d68-162">Протестируйте SqlConfig.Tools, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f9d68-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="f9d68-163">SqlConfig.Tools отображает использования, параметров и команду справки.</span><span class="sxs-lookup"><span data-stu-id="f9d68-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="f9d68-164">Создание таблицы в SQL Server, выполнив `sql-cache create` команды:</span><span class="sxs-lookup"><span data-stu-id="f9d68-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="f9d68-165">Созданный таблица имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="f9d68-165">The created table has the following schema:</span></span>

![Таблица кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="f9d68-167">Как и все реализации кэша приложения следует получать и задавать значения кэша, используя экземпляр `IDistributedCache`, а не `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="f9d68-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="f9d68-168">В этом образце реализуется `SqlServerCache` в рабочей среде (поэтому оно настроено в `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="f9d68-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="f9d68-169">`ConnectionString` (И при необходимости `SchemaName` и `TableName`) обычно должны храниться вне системы управления версиями (например, объектов UserSecrets), так как они могут содержать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="f9d68-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="f9d68-170">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="f9d68-170">Recommendations</span></span>

<span data-ttu-id="f9d68-171">При выборе реализация `IDistributedCache` для приложения, выбор между Redis и SQL Server, основанное на существующей инфраструктуре и среду, требований к производительности и качества работы команды.</span><span class="sxs-lookup"><span data-stu-id="f9d68-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="f9d68-172">Если команда является более удобно работать с Redis, это лучший выбор.</span><span class="sxs-lookup"><span data-stu-id="f9d68-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="f9d68-173">Если предпочитает ваша команда SQL Server, можно быть уверенным, что реализация также.</span><span class="sxs-lookup"><span data-stu-id="f9d68-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="f9d68-174">Обратите внимание, что традиционные решения кэширования сохраняет данные в памяти, что позволяет для быстрого получения данных.</span><span class="sxs-lookup"><span data-stu-id="f9d68-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="f9d68-175">Необходимо хранить в кэше часто используемых данных и хранить все данные в постоянном хранилище серверной части, например SQL Server или хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="f9d68-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="f9d68-176">Кэш redis — это кэширования решение, которое обеспечивает высокую пропускную способность и низкую задержку по сравнению с кэша SQL.</span><span class="sxs-lookup"><span data-stu-id="f9d68-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f9d68-177">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f9d68-177">Additional resources</span></span>

* [<span data-ttu-id="f9d68-178">Кэш Azure redis</span><span class="sxs-lookup"><span data-stu-id="f9d68-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="f9d68-179">База данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="f9d68-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* [<span data-ttu-id="f9d68-180">Кэш в памяти</span><span class="sxs-lookup"><span data-stu-id="f9d68-180">Cache in-memory</span></span>](xref:performance/caching/memory)
* [<span data-ttu-id="f9d68-181">Обнаружение изменений с помощью маркеров изменений</span><span class="sxs-lookup"><span data-stu-id="f9d68-181">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="f9d68-182">Кэширование ответов</span><span class="sxs-lookup"><span data-stu-id="f9d68-182">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="f9d68-183">ПО промежуточного слоя для кэширования ответов</span><span class="sxs-lookup"><span data-stu-id="f9d68-183">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="f9d68-184">Вспомогательная функция тега кэша</span><span class="sxs-lookup"><span data-stu-id="f9d68-184">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="f9d68-185">Вспомогательная функция тега распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="f9d68-185">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
