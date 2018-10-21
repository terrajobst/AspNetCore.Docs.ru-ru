---
title: Работа с распределенным кэшем в ASP.NET Core
author: ardalis
description: Узнайте, как использовать распределенное кэширование ASP.NET Core, чтобы повысить производительность и улучшить масштабируемость приложения, особенно в среде фермы серверов или в облаке.
ms.author: riande
ms.custom: mvc
ms.date: 02/14/2017
uid: performance/caching/distributed
ms.openlocfilehash: 85da734f3ae7bcf0936888edfb6ac91d4362eef2
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477480"
---
# <a name="work-with-a-distributed-cache-in-aspnet-core"></a><span data-ttu-id="4cce1-103">Работа с распределенным кэшем в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4cce1-103">Work with a distributed cache in ASP.NET Core</span></span>

<span data-ttu-id="4cce1-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="4cce1-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4cce1-105">Распределенные кэши могут улучшить производительность и масштабируемость приложений ASP.NET Core, особенно в том случае, если размещенные в облаке или ферме серверов.</span><span class="sxs-lookup"><span data-stu-id="4cce1-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in the cloud or a server farm.</span></span>

<span data-ttu-id="4cce1-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4cce1-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="4cce1-107">Что такое распределенный кэш</span><span class="sxs-lookup"><span data-stu-id="4cce1-107">What is a distributed cache</span></span>

<span data-ttu-id="4cce1-108">Распределенный кэш является общим для нескольких серверов приложений (см. [основы кэша](memory.md#caching-basics)). </span><span class="sxs-lookup"><span data-stu-id="4cce1-108">A distributed cache is shared by multiple app servers (see [Cache Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="4cce1-109">Сведения в кэше не хранятся в памяти отдельных веб-серверов и кэшированных данных доступен для всех серверов приложений.</span><span class="sxs-lookup"><span data-stu-id="4cce1-109">The information in the cache isn't stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="4cce1-110">Это обеспечивает ряд преимуществ:</span><span class="sxs-lookup"><span data-stu-id="4cce1-110">This provides several advantages:</span></span>

1. <span data-ttu-id="4cce1-111">Кэшированные данные согласованны на всех веб-серверах.</span><span class="sxs-lookup"><span data-stu-id="4cce1-111">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="4cce1-112">Пользователи получают одинаковые результаты вне зависимости от того, какой веб-сервер обрабатывает их запрос.</span><span class="sxs-lookup"><span data-stu-id="4cce1-112">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="4cce1-113">Кэшированные данные сохраняются при перезагрузке и развертывании веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="4cce1-113">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="4cce1-114">Веб-серверы можно удалять или добавлять без воздействия на кэш.</span><span class="sxs-lookup"><span data-stu-id="4cce1-114">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="4cce1-115">Исходное хранилище данных получает меньшее число запросов (по сравнению с несколькими кэшами в памяти или отсутствием кэша вообще).</span><span class="sxs-lookup"><span data-stu-id="4cce1-115">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="4cce1-116">Если используется распределенный кэш SQL Server, некоторые из этих преимуществ верны только в том случае, если для кеша используется отдельный экземпляр базы данных, отличный от того, который используется для исходных данных приложения.</span><span class="sxs-lookup"><span data-stu-id="4cce1-116">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="4cce1-117">Как и любой кэш, распределенный кэш может значительно повысить быстродействие приложения, поскольку обычно данные из кэша можно извлечь намного быстрее, чем из реляционной базы данных (или веб-службы).</span><span class="sxs-lookup"><span data-stu-id="4cce1-117">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="4cce1-118">Конфигурация кэша зависит от конкретной реализации.</span><span class="sxs-lookup"><span data-stu-id="4cce1-118">Cache configuration is implementation specific.</span></span> <span data-ttu-id="4cce1-119">В этой статье описывается, как настроить распределенные кэши для Redis и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4cce1-119">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="4cce1-120">Независимо от того, какая реализация выбрана, приложение взаимодействует с кэшем через общий интерфейс `IDistributedCache`.</span><span class="sxs-lookup"><span data-stu-id="4cce1-120">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="4cce1-121">Интерфейс IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="4cce1-121">The IDistributedCache Interface</span></span>

<span data-ttu-id="4cce1-122">Интерфейс `IDistributedCache` включает синхронные и асинхронные методы.</span><span class="sxs-lookup"><span data-stu-id="4cce1-122">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="4cce1-123">Он позволяет добавлять, получать и удалять элементы из реализации распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="4cce1-123">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="4cce1-124">Интерфейс `IDistributedCache` содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="4cce1-124">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="4cce1-125">**Get, GetAsync**</span><span class="sxs-lookup"><span data-stu-id="4cce1-125">**Get, GetAsync**</span></span>

<span data-ttu-id="4cce1-126">Принимает строковый ключ и извлекает кэшированный элемент в виде `byte[]`, если он найден в кэше.</span><span class="sxs-lookup"><span data-stu-id="4cce1-126">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="4cce1-127">**Set, SetAsync**</span><span class="sxs-lookup"><span data-stu-id="4cce1-127">**Set, SetAsync**</span></span>

<span data-ttu-id="4cce1-128">Добавляет элемент (как `byte[]`) в кэш с использованием строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="4cce1-128">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="4cce1-129">**Refresh, RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="4cce1-129">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="4cce1-130">Обновляет элемент в кеше на основе его ключа, сбросив время ожидания его переменного срока действия (если есть).</span><span class="sxs-lookup"><span data-stu-id="4cce1-130">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="4cce1-131">**Remove, RemoveAsync**</span><span class="sxs-lookup"><span data-stu-id="4cce1-131">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="4cce1-132">Удаляет запись в кэше по его ключу.</span><span class="sxs-lookup"><span data-stu-id="4cce1-132">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="4cce1-133">Для использования интерфейса `IDistributedCache`:</span><span class="sxs-lookup"><span data-stu-id="4cce1-133">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="4cce1-134">Добавьте необходимые пакеты NuGet в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="4cce1-134">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="4cce1-135">Настройте конкретную реализацию `IDistributedCache` в методе`ConfigureServices` класса `Startup` и добавьте ее в контейнер.</span><span class="sxs-lookup"><span data-stu-id="4cce1-135">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="4cce1-136">Из классов [ромежуточного уровня](xref:fundamentals/middleware/index) или MVC-контроллера приложения запросите экземпляр `IDistributedCache` в конструкторе. </span><span class="sxs-lookup"><span data-stu-id="4cce1-136">From the app's [Middleware](xref:fundamentals/middleware/index) or MVC controller classes, request an instance of `IDistributedCache` from the constructor.</span></span> <span data-ttu-id="4cce1-137">Экземпляр будет предоставлен через [внедрение зависимостей](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="4cce1-137">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="4cce1-138">Время жизни экземпляров `IDistributedCache` (по крайней мере, для встроенных реализаций) не обязательно должно быть ограничено одним объектом или блоком.</span><span class="sxs-lookup"><span data-stu-id="4cce1-138">There's no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="4cce1-139">Вы можете создавать экземпляр где угодно, где это необходимо (вместо использования [внедрения зависимостей](../../fundamentals/dependency-injection.md)), но это может сделать ваш код более сложным для тестирования и нарушает [принцип явных зависимостей](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="4cce1-139">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="4cce1-140">В следующем примере показано, как использовать экземпляр `IDistributedCache` в простом компоненте промежуточного уровня:</span><span class="sxs-lookup"><span data-stu-id="4cce1-140">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/StartTimeHeader.cs)]

<span data-ttu-id="4cce1-141">В приведенном выше коде кэшированное значение считывается, но никогда не записывается.</span><span class="sxs-lookup"><span data-stu-id="4cce1-141">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="4cce1-142">В этом примере значение устанавливается только при запуске сервера и не изменяется.</span><span class="sxs-lookup"><span data-stu-id="4cce1-142">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="4cce1-143">В сценарии с множеством серверов самый последний запущенный сервер перезапишет все предыдущие значения, установленные другими серверами.</span><span class="sxs-lookup"><span data-stu-id="4cce1-143">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="4cce1-144">Методы `Get` и `Set` используют тип `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="4cce1-144">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="4cce1-145">Поэтому строковое значение необходимо преобразовывать с помощью `Encoding.UTF8.GetString` (для `Get`) и `Encoding.UTF8.GetBytes` (для `Set`)</span><span class="sxs-lookup"><span data-stu-id="4cce1-145">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="4cce1-146">В следующем примере кода из *Startup.cs* показывается, как задать значение:</span><span class="sxs-lookup"><span data-stu-id="4cce1-146">The following code from *Startup.cs* shows the value being set:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="4cce1-147">Поскольку `IDistributedCache` настраивается в методе`ConfigureServices`, он доступен для метода `Configure` в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="4cce1-147">Since `IDistributedCache` is configured in the `ConfigureServices` method, it's available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="4cce1-148">Добавление его в качестве параметра позволит получить настроенный экземпляр через DI.</span><span class="sxs-lookup"><span data-stu-id="4cce1-148">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="4cce1-149">Использование распределенного кеша Redis</span><span class="sxs-lookup"><span data-stu-id="4cce1-149">Using a Redis distributed cache</span></span>

<span data-ttu-id="4cce1-150">[Redis](https://redis.io/) -это хранилище данных в памяти с открытым исходным кодом, которое часто используется в качестве распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="4cce1-150">[Redis](https://redis.io/) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="4cce1-151">Вы можете использовать его локально, и вы можете настроить [кэш Azure Redis](https://azure.microsoft.com/services/cache/) для приложений ASP.NET Core, размещенных в Azure. </span><span class="sxs-lookup"><span data-stu-id="4cce1-151">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="4cce1-152">Приложение ASP.NET Core настраивает реализации кэша с помощью `RedisDistributedCache` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="4cce1-152">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="4cce1-153">Кэш Redis требует [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span><span class="sxs-lookup"><span data-stu-id="4cce1-153">The Redis cache requires [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis/)</span></span>

<span data-ttu-id="4cce1-154">Настройте реализацию Redis в `ConfigureServices` и получите доступ к нему в коде приложения, запросив экземпляр `IDistributedCache` (см. код выше).</span><span class="sxs-lookup"><span data-stu-id="4cce1-154">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="4cce1-155">В примере кода реализация `RedisCache` используется в том случае, если сервер настроен для среды `Staging`.</span><span class="sxs-lookup"><span data-stu-id="4cce1-155">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="4cce1-156">Таким образом, метод `ConfigureStagingServices` настраивает `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="4cce1-156">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet2)]

<span data-ttu-id="4cce1-157">Чтобы установить Redis на локальном компьютере, необходимо установить пакет chocolatey [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) и запустить `redis-server` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="4cce1-157">To install Redis on your local machine, install the chocolatey package [https://chocolatey.org/packages/redis-64/](https://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="4cce1-158">Использование распределенного кеша SQL Server</span><span class="sxs-lookup"><span data-stu-id="4cce1-158">Using a SQL Server distributed cache</span></span>

<span data-ttu-id="4cce1-159">Реализация SqlServerCache позволяет распределенному кэшу использовать базу данных SQL Server в качестве хранилища.</span><span class="sxs-lookup"><span data-stu-id="4cce1-159">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="4cce1-160">Чтобы создать таблицу SQL Server, вы можете использовать инструмент sql-cache, который создает таблицу с указанным именем и схемой.</span><span class="sxs-lookup"><span data-stu-id="4cce1-160">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="4cce1-161">Добавить `SqlConfig.Tools` для `<ItemGroup>` элемент файла проекта и выполните `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="4cce1-161">Add `SqlConfig.Tools` to the `<ItemGroup>` element of the project file and run `dotnet restore`.</span></span>

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools" 
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

<span data-ttu-id="4cce1-162">Проверьте SqlConfig.Tools, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="4cce1-162">Test SqlConfig.Tools by running the following command:</span></span>

```console
dotnet sql-cache create --help
```

<span data-ttu-id="4cce1-163">SqlConfig.Tools отображает использование, параметры и справку по команде.</span><span class="sxs-lookup"><span data-stu-id="4cce1-163">SqlConfig.Tools displays usage, options, and command help.</span></span>

<span data-ttu-id="4cce1-164">Создайте таблицу в SQL Server, выполнив команду `sql-cache create`:</span><span class="sxs-lookup"><span data-stu-id="4cce1-164">Create a table in SQL Server by running the `sql-cache create` command :</span></span>

```console
dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
Table and index were created successfully.
```

<span data-ttu-id="4cce1-165">Созданная таблица имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="4cce1-165">The created table has the following schema:</span></span>

![Таблицы кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="4cce1-167">Как и все реализации кэша, ваше приложение должно получать и задавать значения кэша, используя экземпляр `IDistributedCache`, а не `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="4cce1-167">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="4cce1-168">Этот пример реализует `SqlServerCache` в рабочей среде (с настройкой в `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="4cce1-168">The sample implements `SqlServerCache` in the Production environment (so it's configured in `ConfigureProductionServices`).</span></span>

[!code-csharp[](distributed/sample/src/DistCacheSample/Startup.cs?name=snippet3)]

> [!NOTE]
> <span data-ttu-id="4cce1-169">`ConnectionString` (и, при необходимости, `SchemaName` и `TableName`) обычно должны храниться вне системы управления версиями (например, UserSecrets), так как они могут содержать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="4cce1-169">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="4cce1-170">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="4cce1-170">Recommendations</span></span>

<span data-ttu-id="4cce1-171">Когда вы решаете, какая реализация `IDistributedCache` подходит для вашего приложения, выбирайте между Redis и SQL Server на основе вашей существующей инфраструктуры и среды, ваших требований к производительности и опыта вашей команды.</span><span class="sxs-lookup"><span data-stu-id="4cce1-171">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="4cce1-172">Если вашей команде комфортней работать с Redis, это отличный выбор.</span><span class="sxs-lookup"><span data-stu-id="4cce1-172">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="4cce1-173">Если ваша команда предпочитает SQL Server, вы можете быть уверены и в этой реализации.</span><span class="sxs-lookup"><span data-stu-id="4cce1-173">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="4cce1-174">Обратите внимание, что традиционные решения кэширования хранят данные в памяти, что позволяет быстро их извлекать.</span><span class="sxs-lookup"><span data-stu-id="4cce1-174">Note that a traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="4cce1-175">Храните часто используемые данные в кэше и все данные — в постоянном хранилище, например SQL Server или Azure Storage. </span><span class="sxs-lookup"><span data-stu-id="4cce1-175">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="4cce1-176">Redis Cache — это решение кэширования, которое обеспечивает более высокую пропускную способность и низкую задержку</span><span class="sxs-lookup"><span data-stu-id="4cce1-176">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4cce1-177">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4cce1-177">Additional resources</span></span>

* [<span data-ttu-id="4cce1-178">Кэш в Azure redis</span><span class="sxs-lookup"><span data-stu-id="4cce1-178">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="4cce1-179">База данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="4cce1-179">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
