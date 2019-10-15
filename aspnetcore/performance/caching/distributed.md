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
# <a name="distributed-caching-in-aspnet-core"></a>Распределенное кэширование в ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Стив Смит](https://ardalis.com/)

Распределенный кэш — это кэш, совместно используемый несколькими серверами приложений, обычно поддерживаемый как внешняя служба для серверов приложений, обращающихся к ней. Распределенный кэш может повысить производительность и масштабируемость приложения ASP.NET Core, особенно если приложение размещено в облачной службе или ферме серверов.

Распределенный кэш имеет несколько преимуществ по сравнению с другими сценариями кэширования, в которых кэшированные данные хранятся на отдельных серверах приложений.

При распределении кэшированных данных данные:

* — Согласованность между запросами к нескольким серверам.
* Выдерживает перезапуски сервера и развертывание приложений.
* Не использует локальную память.

Конфигурация распределенного кэша зависит от конкретной реализации. В этой статье описывается настройка распределенных кэшей SQL Server и Redis. Также доступны реализации сторонних производителей, например [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на GitHub](https://github.com/Alachisoft/NCache)). Независимо от выбранной реализации приложение взаимодействует с кэшем с помощью <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейса.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">= aspnetcore-3.0"

Чтобы использовать SQL Server распределенный кэш, добавьте ссылку на пакет в пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .

Чтобы использовать распределенный кэш Redis, добавьте ссылку на пакет в пакет [Microsoft. Extensions. Caching. стаккексчанжередис](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) .

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Чтобы использовать SQL Server распределенный кэш, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .

Чтобы использовать распределенный кэш Redis, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft. Extensions. Caching. стаккексчанжередис](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis) . Пакет Redis не входит в `Microsoft.AspNetCore.App` пакет, поэтому необходимо отдельно ссылаться на пакет Redis в файле проекта.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Чтобы использовать SQL Server распределенный кэш, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. Extensions. Caching. SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) .

Чтобы использовать распределенный кэш Redis, укажите ссылку на [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft. Extensions. Caching. Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) . Пакет Redis не входит в `Microsoft.AspNetCore.App` пакет, поэтому необходимо отдельно ссылаться на пакет Redis в файле проекта.

::: moniker-end

## <a name="idistributedcache-interface"></a>Интерфейс IDistributedCache

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Интерфейс предоставляет следующие методы для управления элементами в реализации распределенного кэша:

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> `byte[]` Принимает строковый ключ и получает кэшированный элемент в виде массива, если он найден в кэше. &ndash;
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>`byte[]` , <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> Добавляет&ndash; элемент (в виде массива) в кэш с помощью строкового ключа.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> Обновляетэлементвкэшенаосновеегоключа,переустанавливаяскользящийтайм-аутистечениясрокадействия(&ndash; если он есть).
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*><xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> ,&ndash; Удаляет элемент кэша на основе его строкового ключа.

## <a name="establish-distributed-caching-services"></a>Установка служб распределенного кэширования

Зарегистрируйте реализацию <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `Startup.ConfigureServices`. Реализации, предоставляемые платформой, описанные в этом разделе, включают:

* [Кэш распределенной памяти](#distributed-memory-cache)
* [Распределенный кэш SQL Server](#distributed-sql-server-cache)
* [Распределенный кэш Redis](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>Кэш распределенной памяти

Кэш распределенной памяти (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) — это предоставляемая платформой <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> реализация, которая хранит элементы в памяти. Кэш распределенной памяти не является фактическим распределенным кэшем. Кэшированные элементы хранятся в экземпляре приложения на сервере, где выполняется приложение.

Кэш распределенной памяти — это полезная реализация:

* В сценариях разработки и тестирования.
* Если в рабочей среде используется один сервер, а использование памяти не является проблемой. Реализация кэша распределенной памяти абстрагирует хранилище кэшированных данных. Она позволяет реализовать истинное решение распределенного кэширования в будущем, если потребуется несколько узлов или отказоустойчивость.

Пример приложения использует распределенный кэш памяти при запуске приложения в среде разработки в `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedMemoryCache)]

::: moniker-end

### <a name="distributed-sql-server-cache"></a>Распределенный кэш SQL Server

Реализация кэша распределенного SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) позволяет распределенному кэшу использовать базу данных SQL Server в качестве резервного хранилища. Чтобы создать SQL Server таблицу кэшированных элементов в экземпляре SQL Server, можно использовать `sql-cache` средство. Средство создает таблицу с указанными именем и схемой.

Создайте таблицу в SQL Server, выполнив `sql-cache create` команду. Укажите`Data Source`SQL Server экземпляр (), базу данных (`Initial Catalog`), схему (например, `dbo`) и имя таблицы (например, `TestCache`):

```dotnetcli
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

В журнал заносится сообщение, указывающее, что средство прошло успешно:

```console
Table and index were created successfully.
```

Таблица, созданная `sql-cache` средством, имеет следующую схему:

![Таблица кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Приложение должно манипулировать значениями кэша с помощью экземпляра <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, а <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>не.

Пример приложения реализует <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> в среде, не являющейся средой разработки `Startup.ConfigureServices`, в:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddDistributedSqlServerCache)]

::: moniker-end

> [!NOTE]
> <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (И, при необходимости, <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> и <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) обычно хранятся вне системы управления версиями (например, хранится в [диспетчере секретов](xref:security/app-secrets) или в *appsettings.json*/*appsettings. {Файлы среды}. JSON* ). Строка подключения может содержать учетные данные, которые должны содержаться в системе управления версиями.

### <a name="distributed-redis-cache"></a>Распределенный кэш Redis

[Redis](https://redis.io/) -это хранилище данных в памяти с открытым исходным кодом, которое часто используется в качестве распределенного кэша. Вы можете использовать Redis локально, и вы можете настроить [кэш Redis для Azure](https://azure.microsoft.com/services/cache/) для приложения ASP.NET Core, размещенного в Azure.

::: moniker range=">= aspnetcore-3.0"

Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) в среде, не являющейся средой разработки, `Startup.ConfigureServices`в:

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.StackExchangeRedis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.StackExchangeRedisCacheServiceCollectionExtensions.AddStackExchangeRedisCache*>) в среде, не являющейся средой разработки, `Startup.ConfigureServices`в:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_AddStackExchangeRedisCache)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Приложение настраивает реализацию кэша с помощью <xref:Microsoft.Extensions.Caching.Redis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

::: moniker-end

Чтобы установить Redis на локальном компьютере, выполните следующие действия.

* Установите [пакет шоколадного Redis](https://chocolatey.org/packages/redis-64/).
* Запустите `redis-server` из командной строки.

## <a name="use-the-distributed-cache"></a>Использование распределенного кэша

Чтобы использовать <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейс, запросите <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> экземпляр из любого конструктора в приложении. Экземпляр предоставляется путем [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).

::: moniker range=">= aspnetcore-3.0"

При запуске <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> примера приложения внедряется в `Startup.Configure`. Текущее время кэшируется с помощью <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (Дополнительные сведения см. в [разделе универсальный узел: Ихостаппликатионлифетиме](xref:fundamentals/host/generic-host#ihostapplicationlifetime)):

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

При запуске <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> примера приложения внедряется в `Startup.Configure`. Текущее время кэшируется с помощью <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Дополнительные сведения см. в [разделе веб-узел: Интерфейс](xref:fundamentals/host/web-host#iapplicationlifetime-interface)иаппликатионлифетиме):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

::: moniker-end

Пример приложения внедряет <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в объект `IndexModel` для использования на странице индекса.

При каждой загрузке страницы индекса кэш проверяется на наличие кэшированного времени в `OnGetAsync`. Если время кэширования не истекло, отображается время. Если прошло 20 секунд с момента последнего обращения к кэшированному времени (время последней загрузки этой страницы), на странице отображается *время ожидания кэширования*.

Немедленно обновите кэшированное время на текущее время, нажав кнопку **сбросить кэшированное** время. Кнопка запускает `OnPostResetCachedTime` метод обработчика.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](distributed/samples/3.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

::: moniker-end

> [!NOTE]
> Время жизни экземпляров <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (по крайней мере, для встроенных реализаций) не обязательно должно быть ограничено одним объектом или блоком.
>
> Кроме того, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> экземпляр можно создать везде, где вам может понадобиться, а не использовать di, но создание экземпляра в коде может усложнить тестирование и нарушает [принцип явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Рекомендации

При принятии решения о том, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> какая реализация лучше подходит для вашего приложения, учитывайте следующее.

* Существующая инфраструктура
* Требования к производительности
* Стоимость
* Опыт работы в группе

Для обеспечения быстрого извлечения кэшированных данных решения кэширования обычно используют хранилище в памяти, но память является ограниченным ресурсом и может быть расширена. Хранение часто используемых данных в кэше.

Как правило, кэш Redis обеспечивает более высокую пропускную способность и меньшую задержку, чем кэш SQL Server. Однако для определения характеристик производительности стратегий кэширования обычно требуется тестирование производительности.

Если SQL Server используется в качестве резервного хранилища распределенного кэша, использование той же базы данных для кэша и обычного хранилища данных и извлечения приложения может негативно сказаться на производительности обоих. Рекомендуется использовать выделенный экземпляр SQL Server для резервного хранилища распределенного кэша.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Кэш Redis в Azure](/azure/azure-cache-for-redis/)
* [База данных SQL в Azure](/azure/sql-database/)
* [Поставщик ASP.NET Core IDistributedCache для NCache в веб-фермах](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на GitHub](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
