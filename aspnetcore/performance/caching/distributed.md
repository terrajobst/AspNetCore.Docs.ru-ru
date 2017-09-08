---
title: "Работа с распределенного кэша"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 870f082d-6d43-453d-b311-45f3aeb4d2c5
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/caching/distributed
ms.openlocfilehash: 09a1a30de38b9eb40d4fa6a684a7d43ac3e0413c
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-a-distributed-cache"></a><span data-ttu-id="1118c-103">Работа с распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="1118c-103">Working with a distributed cache</span></span>

<span data-ttu-id="1118c-104">По [Стив Смит](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="1118c-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="1118c-105">Распределенные кэши могут улучшить производительность и масштабируемость приложений ASP.NET Core, особенно в том случае, если размещенные в среде фермы облако или сервера.</span><span class="sxs-lookup"><span data-stu-id="1118c-105">Distributed caches can improve the performance and scalability of ASP.NET Core apps, especially when hosted in a cloud or server farm environment.</span></span> <span data-ttu-id="1118c-106">В этой статье объясняется, как работать с ASP.NET Core встроенных распределенного кэша абстракции и реализации.</span><span class="sxs-lookup"><span data-stu-id="1118c-106">This article explains how to work with ASP.NET Core's built-in distributed cache abstractions and implementations.</span></span>

[<span data-ttu-id="1118c-107">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="1118c-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/sample)

## <a name="what-is-a-distributed-cache"></a><span data-ttu-id="1118c-108">Что такое распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="1118c-108">What is a Distributed Cache</span></span>

<span data-ttu-id="1118c-109">Распределенный кэш является общим для нескольких серверов приложений (см. [кэширование основы](memory.md#caching-basics)).</span><span class="sxs-lookup"><span data-stu-id="1118c-109">A distributed cache is shared by multiple app servers (see [Caching Basics](memory.md#caching-basics)).</span></span> <span data-ttu-id="1118c-110">Сведения в кэше не хранятся в памяти отдельных веб-серверов и кэшированных данных доступен для всех серверов приложений.</span><span class="sxs-lookup"><span data-stu-id="1118c-110">The information in the cache is not stored in the memory of individual web servers, and the cached data is available to all of the app's servers.</span></span> <span data-ttu-id="1118c-111">Это обеспечивает несколько преимуществ:</span><span class="sxs-lookup"><span data-stu-id="1118c-111">This provides several advantages:</span></span>

1. <span data-ttu-id="1118c-112">Кэшированные данные согласованного на всех веб-серверах.</span><span class="sxs-lookup"><span data-stu-id="1118c-112">Cached data is coherent on all web servers.</span></span> <span data-ttu-id="1118c-113">Пользователи не видеть разные результаты в зависимости от веб-сервер обрабатывает их запроса</span><span class="sxs-lookup"><span data-stu-id="1118c-113">Users don't see different results depending on which web server handles their request</span></span>

2. <span data-ttu-id="1118c-114">Кэшированные данные сохраняется после перезагрузки сервера web и развертываний.</span><span class="sxs-lookup"><span data-stu-id="1118c-114">Cached data survives web server restarts and deployments.</span></span> <span data-ttu-id="1118c-115">Отдельные веб-серверов можно удалить или добавить без ущерба для кэша.</span><span class="sxs-lookup"><span data-stu-id="1118c-115">Individual web servers can be removed or added without impacting the cache.</span></span>

3. <span data-ttu-id="1118c-116">Хранилище данных источника содержит меньшее число запросов, выполненных на него (не с нескольких кэшей в памяти или нет кэшировать вообще).</span><span class="sxs-lookup"><span data-stu-id="1118c-116">The source data store has fewer requests made to it (than with multiple in-memory caches or no cache at all).</span></span>

> [!NOTE]
> <span data-ttu-id="1118c-117">Если используется SQL Server распределенного кэша, некоторые из этих преимуществ доступны только в значение true, если экземпляр отдельную базу данных для кэша, чем для источника данных приложения.</span><span class="sxs-lookup"><span data-stu-id="1118c-117">If using a SQL Server Distributed Cache, some of these advantages are only true if a separate database instance is used for the cache than for the app's source data.</span></span>

<span data-ttu-id="1118c-118">Как и любой кэш распределенного кэша могут существенно повысить скорость реагирования приложения, поскольку обычно можно получить данные из кэша намного быстрее, чем из реляционной базы данных (или веб-службы).</span><span class="sxs-lookup"><span data-stu-id="1118c-118">Like any cache, a distributed cache can dramatically improve an app's responsiveness, since typically data can be retrieved from the cache much faster than from a relational database (or web service).</span></span>

<span data-ttu-id="1118c-119">Конфигурация кэша зависит от конкретной реализации.</span><span class="sxs-lookup"><span data-stu-id="1118c-119">Cache configuration is implementation specific.</span></span> <span data-ttu-id="1118c-120">В этой статье описывается настройка оба Redis и кэширует распределенных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1118c-120">This article describes how to configure both Redis and SQL Server distributed caches.</span></span> <span data-ttu-id="1118c-121">Независимо от того, какая реализация установлен, приложение взаимодействует с кэш, используя общий `IDistributedCache` интерфейс.</span><span class="sxs-lookup"><span data-stu-id="1118c-121">Regardless of which implementation is selected, the app interacts with the cache using a common `IDistributedCache` interface.</span></span>

## <a name="the-idistributedcache-interface"></a><span data-ttu-id="1118c-122">Интерфейс IDistributedCache</span><span class="sxs-lookup"><span data-stu-id="1118c-122">The IDistributedCache Interface</span></span>

<span data-ttu-id="1118c-123">`IDistributedCache` Интерфейс включает синхронные и асинхронные методы.</span><span class="sxs-lookup"><span data-stu-id="1118c-123">The `IDistributedCache` interface includes synchronous and asynchronous methods.</span></span> <span data-ttu-id="1118c-124">Интерфейс позволяет элементы добавлены, получение и удалены из реализации распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="1118c-124">The interface allows items to be added, retrieved, and removed from the distributed cache implementation.</span></span> <span data-ttu-id="1118c-125">`IDistributedCache` Интерфейс содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="1118c-125">The `IDistributedCache` interface includes the following methods:</span></span>

<span data-ttu-id="1118c-126">**Get-GetAsync**</span><span class="sxs-lookup"><span data-stu-id="1118c-126">**Get, GetAsync**</span></span>

<span data-ttu-id="1118c-127">Принимает строковый ключ и извлекает кэшированного элемента как `byte[]` Если найден в кэше.</span><span class="sxs-lookup"><span data-stu-id="1118c-127">Takes a string key and retrieves a cached item as a `byte[]` if found in the cache.</span></span>

<span data-ttu-id="1118c-128">**Набор SetAsync**</span><span class="sxs-lookup"><span data-stu-id="1118c-128">**Set, SetAsync**</span></span>

<span data-ttu-id="1118c-129">Добавляет элемент (как `byte[]`) в кэш с помощью строкового ключа.</span><span class="sxs-lookup"><span data-stu-id="1118c-129">Adds an item (as `byte[]`) to the cache using a string key.</span></span>

<span data-ttu-id="1118c-130">**Обновление RefreshAsync**</span><span class="sxs-lookup"><span data-stu-id="1118c-130">**Refresh, RefreshAsync**</span></span>

<span data-ttu-id="1118c-131">Обновляет объект в кэше, по его ключу, сброс его скользящий срок хранения (если таковые имеются).</span><span class="sxs-lookup"><span data-stu-id="1118c-131">Refreshes an item in the cache based on its key, resetting its sliding expiration timeout (if any).</span></span>

<span data-ttu-id="1118c-132">**Удалите асинхронно удаляет**</span><span class="sxs-lookup"><span data-stu-id="1118c-132">**Remove, RemoveAsync**</span></span>

<span data-ttu-id="1118c-133">Удаляет запись по его ключу.</span><span class="sxs-lookup"><span data-stu-id="1118c-133">Removes a cache entry based on its key.</span></span>

<span data-ttu-id="1118c-134">Для использования `IDistributedCache` интерфейс:</span><span class="sxs-lookup"><span data-stu-id="1118c-134">To use the `IDistributedCache` interface:</span></span>

   1. <span data-ttu-id="1118c-135">Добавьте необходимые пакеты NuGet в файл проекта.</span><span class="sxs-lookup"><span data-stu-id="1118c-135">Add the required NuGet packages to your project file.</span></span>

   2. <span data-ttu-id="1118c-136">Настройка деталей реализации `IDistributedCache` в ваш `Startup` класса `ConfigureServices` метода и добавьте его в контейнер существует.</span><span class="sxs-lookup"><span data-stu-id="1118c-136">Configure the specific implementation of `IDistributedCache` in your `Startup` class's `ConfigureServices` method, and add it to the container there.</span></span>

   3. <span data-ttu-id="1118c-137">В приложении [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache "из конструктора.</span><span class="sxs-lookup"><span data-stu-id="1118c-137">From the app's [`Middleware](../../fundamentals/middleware.md) or MVC controller classes, request an instance of `IDistributedCache\` from the constructor.</span></span> <span data-ttu-id="1118c-138">Экземпляр будут предоставляться [внедрения зависимостей](../../fundamentals/dependency-injection.md) (DI).</span><span class="sxs-lookup"><span data-stu-id="1118c-138">The instance will be provided by [Dependency Injection](../../fundamentals/dependency-injection.md) (DI).</span></span>

> [!NOTE]
> <span data-ttu-id="1118c-139">Нет необходимости использовать единственный элемент или Scoped время существования `IDistributedCache` экземпляров (хотя бы для встроенных реализаций).</span><span class="sxs-lookup"><span data-stu-id="1118c-139">There is no need to use a Singleton or Scoped lifetime for `IDistributedCache` instances (at least for the built-in implementations).</span></span> <span data-ttu-id="1118c-140">Можно также создать экземпляр везде, где может потребоваться одно (вместо использования [внедрения зависимостей](../../fundamentals/dependency-injection.md)), но это может усложнить код для тестирования и приводит к нарушению [явные зависимости принцип](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="1118c-140">You can also create an instance wherever you might need one (instead of using [Dependency Injection](../../fundamentals/dependency-injection.md)), but this can make your code harder to test, and violates the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span>

<span data-ttu-id="1118c-141">В следующем примере показано, как использовать экземпляр `IDistributedCache` в простой программный компонент:</span><span class="sxs-lookup"><span data-stu-id="1118c-141">The following example shows how to use an instance of `IDistributedCache` in a simple middleware component:</span></span>

<span data-ttu-id="1118c-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span><span class="sxs-lookup"><span data-stu-id="1118c-142">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/StartTimeHeader.cs?highlight=15,18,21,27,28,29,30,31)]</span></span>

<span data-ttu-id="1118c-143">В приведенном выше коде кэшированное значение является считываются, но никогда не записываются.</span><span class="sxs-lookup"><span data-stu-id="1118c-143">In the code above, the cached value is read, but never written.</span></span> <span data-ttu-id="1118c-144">В этом примере значение устанавливается только при запуске сервера, а также не изменяется.</span><span class="sxs-lookup"><span data-stu-id="1118c-144">In this sample, the value is only set when a server starts up, and doesn't change.</span></span> <span data-ttu-id="1118c-145">В случае многосерверной последний сервер запуск перезапишет все предыдущие значения, заданные на других серверах.</span><span class="sxs-lookup"><span data-stu-id="1118c-145">In a multi-server scenario, the most recent server to start will overwrite any previous values that were set by other servers.</span></span> <span data-ttu-id="1118c-146">`Get` И `Set` методы используют `byte[]` типа.</span><span class="sxs-lookup"><span data-stu-id="1118c-146">The `Get` and `Set` methods use the `byte[]` type.</span></span> <span data-ttu-id="1118c-147">Таким образом, строковое значение необходимо преобразовывать с помощью `Encoding.UTF8.GetString` (для `Get`) и `Encoding.UTF8.GetBytes` (для `Set`).</span><span class="sxs-lookup"><span data-stu-id="1118c-147">Therefore, the string value must be converted using `Encoding.UTF8.GetString` (for `Get`) and `Encoding.UTF8.GetBytes` (for `Set`).</span></span>

<span data-ttu-id="1118c-148">В следующем примере кода из *файла Startup.cs* показывает, что будет задаваться значение:</span><span class="sxs-lookup"><span data-stu-id="1118c-148">The following code from *Startup.cs* shows the value being set:</span></span>

<span data-ttu-id="1118c-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span><span class="sxs-lookup"><span data-stu-id="1118c-149">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=2,4,5,6&range=58-66)]</span></span>

> [!NOTE]
> <span data-ttu-id="1118c-150">Поскольку `IDistributedCache` настраивается в `ConfigureServices` метода, она доступна для `Configure` методу в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="1118c-150">Since `IDistributedCache` is configured in the `ConfigureServices` method, it is available to the `Configure` method as a parameter.</span></span> <span data-ttu-id="1118c-151">Добавив его в качестве параметра позволит настроенный экземпляр должен предоставляться через DI.</span><span class="sxs-lookup"><span data-stu-id="1118c-151">Adding it as a parameter will allow the configured instance to be provided through DI.</span></span>

## <a name="using-a-redis-distributed-cache"></a><span data-ttu-id="1118c-152">С помощью Redis распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="1118c-152">Using a Redis Distributed Cache</span></span>

<span data-ttu-id="1118c-153">[Redis](http://redis.io) -хранилище данных в памяти открытым исходным кодом, который часто используется в качестве распределенного кэша.</span><span class="sxs-lookup"><span data-stu-id="1118c-153">[Redis](http://redis.io) is an open source in-memory data store, which is often used as a distributed cache.</span></span> <span data-ttu-id="1118c-154">Можно использовать локально, и можно настроить [кэш Azure Redis](https://azure.microsoft.com/services/cache/) для приложений ASP.NET Core, размещенной в Azure.</span><span class="sxs-lookup"><span data-stu-id="1118c-154">You can use it locally, and you can configure an [Azure Redis Cache](https://azure.microsoft.com/services/cache/) for your Azure-hosted ASP.NET Core apps.</span></span> <span data-ttu-id="1118c-155">Настраивает приложение ASP.NET Core реализации кэша с помощью `RedisDistributedCache` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="1118c-155">Your ASP.NET Core app configures the cache implementation using a `RedisDistributedCache` instance.</span></span>

<span data-ttu-id="1118c-156">Настройте реализацию Redis в `ConfigureServices` и откройте его в коде приложения, запросив экземпляр `IDistributedCache` (см. приведенный выше код).</span><span class="sxs-lookup"><span data-stu-id="1118c-156">You configure the Redis implementation in `ConfigureServices` and access it in your app code by requesting an instance of `IDistributedCache` (see the code above).</span></span>

<span data-ttu-id="1118c-157">В образце кода `RedisCache` реализация используется в том случае, если сервер настроен для `Staging` среды.</span><span class="sxs-lookup"><span data-stu-id="1118c-157">In the sample code, a `RedisCache` implementation is used when the server is configured for a `Staging` environment.</span></span> <span data-ttu-id="1118c-158">Таким образом `ConfigureStagingServices` настраивает метод `RedisCache`:</span><span class="sxs-lookup"><span data-stu-id="1118c-158">Thus the `ConfigureStagingServices` method configures the `RedisCache`:</span></span>

<span data-ttu-id="1118c-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span><span class="sxs-lookup"><span data-stu-id="1118c-159">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=8,9,10,11,12,13&range=27-40)]</span></span>

> [!NOTE]
> <span data-ttu-id="1118c-160">Чтобы установить Redis на локальном компьютере, необходимо установить пакет chocolatey [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) и запустите `redis-server` из командной строки.</span><span class="sxs-lookup"><span data-stu-id="1118c-160">To install Redis on your local machine, install the chocolatey package [http://chocolatey.org/packages/redis-64/](http://chocolatey.org/packages/redis-64/) and run `redis-server` from a command prompt.</span></span>

## <a name="using-a-sql-server-distributed-cache"></a><span data-ttu-id="1118c-161">Использование SQL Server распределенного кэша</span><span class="sxs-lookup"><span data-stu-id="1118c-161">Using a SQL Server Distributed Cache</span></span>

<span data-ttu-id="1118c-162">Реализация SqlServerCache допускает распределенного кэша для использования базы данных SQL Server в качестве резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="1118c-162">The SqlServerCache implementation allows the distributed cache to use a SQL Server database as its backing store.</span></span> <span data-ttu-id="1118c-163">Создание SQL Server таблицы, можно использовать средство кэша sql, средство создает таблицу с именем и схемой, которые вы задаете.</span><span class="sxs-lookup"><span data-stu-id="1118c-163">To create SQL Server table you can use sql-cache tool, the tool creates a table with the name and schema you specify.</span></span>

<span data-ttu-id="1118c-164">Чтобы использовать средство кэша sql, добавьте `SqlConfig.Tools` для `<ItemGroup>` элемент *.csproj* файл и запустите восстановление dotnet.</span><span class="sxs-lookup"><span data-stu-id="1118c-164">To use the sql-cache tool, add `SqlConfig.Tools` to the `<ItemGroup>` element of the *.csproj* file and run dotnet restore.</span></span>

<span data-ttu-id="1118c-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span><span class="sxs-lookup"><span data-stu-id="1118c-165">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/DistCacheSample.csproj?range=23-25)]</span></span>

<span data-ttu-id="1118c-166">Протестируйте SqlConfig.Tools, выполнив следующую команду</span><span class="sxs-lookup"><span data-stu-id="1118c-166">Test SqlConfig.Tools by running the following command</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create --help
   ```

<span data-ttu-id="1118c-167">Средство кэша SQL для отображения справки об использовании, параметры и команды, теперь можно создавать таблицы в sql server, выполнив команду «Создать кэш sql»:</span><span class="sxs-lookup"><span data-stu-id="1118c-167">sql-cache tool  will display usage, options and command help, now you can create tables into sql server, running "sql-cache create" command :</span></span>

```none
C:\DistCacheSample\src\DistCacheSample>dotnet sql-cache create "Data Source=(localdb)\v11.0;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
   info: Microsoft.Extensions.Caching.SqlConfig.Tools.Program[0]
       Table and index were created successfully.
   ```

<span data-ttu-id="1118c-168">Созданный таблица имеет следующую схему:</span><span class="sxs-lookup"><span data-stu-id="1118c-168">The created table has the following schema:</span></span>

![Таблица кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

<span data-ttu-id="1118c-170">Как и все реализации кэша приложения следует получать и задавать значения кэша, используя экземпляр `IDistributedCache`, а не `SqlServerCache`.</span><span class="sxs-lookup"><span data-stu-id="1118c-170">Like all cache implementations, your app should get and set cache values using an instance of `IDistributedCache`, not a `SqlServerCache`.</span></span> <span data-ttu-id="1118c-171">В этом образце реализуется `SqlServerCache` в `Production` среды (поэтому оно настроено в `ConfigureProductionServices`).</span><span class="sxs-lookup"><span data-stu-id="1118c-171">The sample implements `SqlServerCache` in the `Production` environment (so it is configured in `ConfigureProductionServices`).</span></span>

<span data-ttu-id="1118c-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span><span class="sxs-lookup"><span data-stu-id="1118c-172">[!code-csharp[Main](./distributed/sample/src/DistCacheSample/Startup.cs?highlight=7,8,9,10,11,12&range=42-56)]</span></span>

> [!NOTE]
> <span data-ttu-id="1118c-173">`ConnectionString` (И при необходимости `SchemaName` и `TableName`) обычно должны храниться вне системы управления версиями (например, объектов UserSecrets), так как они могут содержать учетные данные.</span><span class="sxs-lookup"><span data-stu-id="1118c-173">The `ConnectionString` (and optionally, `SchemaName` and `TableName`) should typically be stored outside of source control (such as UserSecrets), as they may contain credentials.</span></span>

## <a name="recommendations"></a><span data-ttu-id="1118c-174">Рекомендации</span><span class="sxs-lookup"><span data-stu-id="1118c-174">Recommendations</span></span>

<span data-ttu-id="1118c-175">При выборе реализация `IDistributedCache` для приложения, выбор между Redis и SQL Server, основанное на существующей инфраструктуре и среду, требований к производительности и качества работы команды.</span><span class="sxs-lookup"><span data-stu-id="1118c-175">When deciding which implementation of `IDistributedCache` is right for your app, choose between Redis and SQL Server based on your existing infrastructure and environment, your performance requirements, and your team's experience.</span></span> <span data-ttu-id="1118c-176">Если команда является более удобно работать с Redis, это лучший выбор.</span><span class="sxs-lookup"><span data-stu-id="1118c-176">If your team is more comfortable working with Redis, it's an excellent choice.</span></span> <span data-ttu-id="1118c-177">Если предпочитает ваша команда SQL Server, можно быть уверенным, что реализация также.</span><span class="sxs-lookup"><span data-stu-id="1118c-177">If your team prefers SQL Server, you can be confident in that implementation as well.</span></span> <span data-ttu-id="1118c-178">Обратите внимание, что традиционные решения кэширования сохраняет данные в памяти, что позволяет для быстрого получения данных.</span><span class="sxs-lookup"><span data-stu-id="1118c-178">Note that A traditional caching solution stores data in-memory which allows for fast retrieval of data.</span></span> <span data-ttu-id="1118c-179">Необходимо хранить в кэше часто используемых данных и хранить все данные в постоянном хранилище серверной части, например SQL Server или хранилища Azure.</span><span class="sxs-lookup"><span data-stu-id="1118c-179">You should store commonly used data in a cache and store the entire data in a backend persistent store such as SQL Server or Azure Storage.</span></span> <span data-ttu-id="1118c-180">Кэш redis — это кэширования решение, которое обеспечивает высокую пропускную способность и низкую задержку по сравнению с кэша SQL.</span><span class="sxs-lookup"><span data-stu-id="1118c-180">Redis Cache is a caching solution which gives you high throughput and low latency as compared to SQL Cache.</span></span>

<span data-ttu-id="1118c-181">Дополнительные ресурсы:</span><span class="sxs-lookup"><span data-stu-id="1118c-181">Additional resources:</span></span>

* [<span data-ttu-id="1118c-182">В памяти кэша</span><span class="sxs-lookup"><span data-stu-id="1118c-182">In memory caching</span></span>](memory.md)
* [<span data-ttu-id="1118c-183">Кэш Azure redis</span><span class="sxs-lookup"><span data-stu-id="1118c-183">Redis Cache on Azure</span></span>](https://azure.microsoft.com/documentation/services/redis-cache/)
* [<span data-ttu-id="1118c-184">База данных SQL в Azure</span><span class="sxs-lookup"><span data-stu-id="1118c-184">SQL Database on Azure</span></span>](https://azure.microsoft.com/documentation/services/sql-database/)
