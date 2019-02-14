---
title: Распределенное кэширование в ASP.NET Core
author: guardrex
description: Сведения об использовании ASP.NET Core распределенного кэша для повышения производительности приложения и масштабируемости, особенно в среде фермы облаком и сервером.
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: performance/caching/distributed
ms.openlocfilehash: a157eb075874d2118e3e34b51410b539a1ec37df
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248592"
---
# <a name="distributed-caching-in-aspnet-core"></a>Распределенное кэширование в ASP.NET Core

Авторы: [Стив Смит](https://ardalis.com/) (Steve Smith) и [Люк Лэтем](https://github.com/guardrex) (Luke Latham)

Распределенный кэш — это кэш, который совместно используется несколькими серверами приложений, обычно поддерживаются как внешняя служба на серверы приложений, обращающихся к ней. Распределенный кэш может повысить производительность и масштабируемость приложения ASP.NET Core, особенно в том случае, если приложение будет размещено в облачную службу или ферме серверов.

Распределенный кэш имеет ряд преимуществ по сравнению с другими кэширования сценариев, где кэшированные данные хранятся на серверах отдельных приложений.

Когда кэшированные данные распределяются, эти данные:

* — *Согласовано* (согласованный) между запросами на несколько серверов.
* Сохранять работоспособность после перезагрузки сервера и развертывания приложений.
* Не использует локальную память.

Конфигурация распределенного кэша зависит от реализации. В этой статье описывается, как настроить SQL Server и распределенные кэши Redis. Реализации сторонних также доступны, такие как [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на сайте GitHub](https://github.com/Alachisoft/NCache)). Независимо от того, какую реализацию установлен, приложение взаимодействует с кэша, используя <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейс.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/distributed/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Предварительные требования

::: moniker range=">= aspnetcore-2.1"

Использование SQL Server распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) пакета.

Для использования Redis распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) и добавьте ссылку на пакет [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) пакета. Пакет Redis не включен в `Microsoft.AspNetCore.App` пакета, поэтому необходимо сослаться на пакет, Redis отдельно в файле проекта.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Использование SQL Server распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) или добавьте ссылку на пакет [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) пакета.

Для использования Redis распределенный кеш, справочник по [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) или добавьте ссылку на пакет [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) пакета. Redis пакет включен в `Microsoft.AspNetCore.All` пакета, поэтому не нужно сослаться на пакет Redis отдельно в файле проекта.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Чтобы использовать экземпляр SQL Server распределенный кеш, добавьте ссылку на пакет [Microsoft.Extensions.Caching.SqlServer](https://www.nuget.org/packages/Microsoft.Extensions.Caching.SqlServer) пакета.

Для использования Redis распределенный кеш, добавьте ссылку на пакет [Microsoft.Extensions.Caching.Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Redis) пакета.

::: moniker-end

## <a name="idistributedcache-interface"></a>Интерфейс IDistributedCache

<xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> Интерфейс предоставляет следующие методы для работы с элементами в реализации распределенного кэша:

* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Get*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.GetAsync*> &ndash; Принимает ключ строкового типа и извлекает кэшированного элемента как `byte[]` массив Если найдена в кэше.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Set*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.SetAsync*> &ndash; Добавляет элемент (как `byte[]` массива) в кэш, с помощью строкового ключа.
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Refresh*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RefreshAsync*> &ndash; Обновляет объект в кэше, по его ключу, сброс его скользящее время ожидания (если таковые имеются).
* <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.Remove*>, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache.RemoveAsync*> &ndash; Удаляет элемент кэша на основе его строку ключа.

## <a name="establish-distributed-caching-services"></a>Установить службы распределенного кэширования

Зарегистрируйте реализацию <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `Startup.ConfigureServices`. Предоставляемые платформой реализации, описанных в этом разделе включают:

* [Распределенные памяти кэша](#distributed-memory-cache)
* [Распределенный кэш SQL Server](#distributed-sql-server-cache)
* [Распределенный кэш Redis](#distributed-redis-cache)

### <a name="distributed-memory-cache"></a>Распределенные памяти кэша

Распределенного кэша в памяти (<xref:Microsoft.Extensions.DependencyInjection.MemoryCacheServiceCollectionExtensions.AddDistributedMemoryCache*>) — это реализация предоставляемые платформой <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> , сохраняет элементы в памяти. Распределенный кеш памяти не фактический распределенного кэша. Кэшированные элементы хранятся в экземпляре приложения на сервере, где выполняется приложение.

Распределенным кэшем памяти — это полезные реализация:

* В сценариях разработки и тестирования.
* При использовании одного сервера в рабочей среде и потребление памяти не является проблемой. Реализация краткие описания распределенным кэшем памяти в кэше хранилища данных. Он позволяет реализовать настоящую распределенного кэширования решения в будущем в случае нескольких узлов или привести к необходимости устойчивость к сбоям.

Пример приложения использует распределенного кэша в памяти при запуске приложения в среде разработки:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=5)]

### <a name="distributed-sql-server-cache"></a>Кэш распределенных SQL Server

Реализация распределенного кэша SQL Server (<xref:Microsoft.Extensions.DependencyInjection.SqlServerCachingServicesExtensions.AddDistributedSqlServerCache*>) позволяет распределенного кэша использовать базу данных SQL Server в качестве резервного хранилища. Чтобы создать таблицу SQL Server кэшированного элемента в экземпляр SQL Server, можно использовать `sql-cache` средство. Средство создает таблицу с именем и схемой, указанной вами.

::: moniker range="< aspnetcore-2.1"

Добавить `SqlConfig.Tools` для `<ItemGroup>` элемент файла проекта и выполните `dotnet restore`.

```xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.Extensions.Caching.SqlConfig.Tools"
                          Version="2.0.2" />
</ItemGroup>
```

::: moniker-end

Создать таблицу в SQL Server, выполнив `sql-cache create` команды. Укажите экземпляр SQL Server (`Data Source`), базы данных (`Initial Catalog`), схемы (например, `dbo`) и имя таблицы (например, `TestCache`):

```console
dotnet sql-cache create "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=DistCache;Integrated Security=True;" dbo TestCache
```

Чтобы указать, что средство успешно записывается сообщение:

```console
Table and index were created successfully.
```

Таблицу, созданную `sql-cache` средство имеет следующую схему:

![Таблицы кэша SqlServer](distributed/_static/SqlServerCacheTable.png)

> [!NOTE]
> Приложение следует управлять значения кэша, используя экземпляр <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>, а не <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache>.

Пример приложения реализует <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCache> в среде не разработки:

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_ConfigureServices&highlight=9-15)]

> [!NOTE]
> Объект <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.ConnectionString*> (и при необходимости <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.SchemaName*> и <xref:Microsoft.Extensions.Caching.SqlServer.SqlServerCacheOptions.TableName*>) обычно хранятся вне системы управления версиями (например, хранимая с [Secret Manager](xref:security/app-secrets) или в *appsettings.json* / *appsettings. {Среда} .json* файлов). Строка подключения может содержать учетные данные, которые должны храниться вне системы управления версиями.

### <a name="distributed-redis-cache"></a>Кэш Redis для распределенных

[Redis](https://redis.io/) -это хранилище данных в памяти с открытым исходным кодом, которое часто используется в качестве распределенного кэша. Redis можно использовать локально, и можно настроить [кэша Redis для Azure](https://azure.microsoft.com/services/cache/) для приложения ASP.NET Core, размещенных в Azure. Приложения настраивает реализации кэша с помощью <xref:Microsoft.Extensions.Caching.Redis.RedisCache> экземпляра (<xref:Microsoft.Extensions.DependencyInjection.RedisCacheServiceCollectionExtensions.AddDistributedRedisCache*>):

```csharp
services.AddDistributedRedisCache(options =>
{
    options.Configuration = "localhost";
    options.InstanceName = "SampleInstance";
});
```

Чтобы установить Redis на локальном компьютере:

* Установка [Chocolatey Redis пакета](https://chocolatey.org/packages/redis-64/).
* Запустите `redis-server` из командной строки.

## <a name="use-the-distributed-cache"></a>Использование распределенного кэша

Чтобы использовать <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> интерфейсом, запросить экземпляр <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> из любого конструктора в приложении. Экземпляр предоставляется [внедрения зависимостей (DI)](xref:fundamentals/dependency-injection).

При запуске приложения, <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> внедряется в `Startup.Configure`. Текущее время кэшируется с помощью <xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime> (Дополнительные сведения см. в разделе [веб-узел: Интерфейс IApplicationLifetime](xref:fundamentals/host/web-host#iapplicationlifetime-interface)):

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Startup.cs?name=snippet_Configure&highlight=10)]

Пример приложения внедряет <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> в `IndexModel` для использования на странице индекса.

Каждый раз при загрузке страницы индекса кэша проверяется на время кэширования в `OnGetAsync`. Если время кэширования еще не истек, отображается время. Если 20 секунд, истекших с момента последнего обращения к время кэширования (последний раз страница была загружена), на странице отображается *кэшированных Time Expired*.

Немедленно обновить кэшированные время на текущий момент времени, выбрав **сбросить время кэширования** кнопки. Триггеры кнопку `OnPostResetCachedTime` метод обработчика.

[!code-csharp[](distributed/samples/2.x/DistCacheSample/Pages/Index.cshtml.cs?name=snippet_IndexModel&highlight=7,14-20,25-29)]

> [!NOTE]
> Время жизни экземпляров <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> (по крайней мере, для встроенных реализаций) не обязательно должно быть ограничено одним объектом или блоком.
>
> Вы также можете создать <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> экземпляра везде, где это может потребоваться, а не с помощью внедрения Зависимостей, но создание экземпляра в коде может сделать код труднее тестировать и нарушает [принципу явных зависимостей](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies).

## <a name="recommendations"></a>Рекомендации

При принятии решения о реализации <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> лучше всего подходит для вашего приложения, необходимо учитывать следующее:

* Существующую инфраструктуру
* Требования к производительности
* Стоимость
* Опыт рабочей группы

Решения кэширования обычно используют хранилище в памяти для предоставления быстрого получения кэшированных данных, но память является ограниченным ресурсом и дорогостоящей развернуть. Только хранилище часто использовать данные в кэше.

Как правило кэш Redis обеспечивает высокую пропускную способность и меньшую задержку, чем при использовании кэша SQL Server. Тем не менее тестирование производительности обычно требуется, чтобы определить характеристики производительности стратегии кэширования.

Когда SQL Server используется в качестве резервного хранилища распределенного кэша, использовать ту же базу данных для кэша и хранения обычных данных приложения и извлечения может отрицательно повлиять на их производительность. Мы рекомендуем использовать выделенный экземпляр SQL Server для распределенного кэша, резервным хранилищем.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Кэш в Azure redis](https://azure.microsoft.com/documentation/services/redis-cache/)
* [База данных SQL в Azure](https://azure.microsoft.com/documentation/services/sql-database/)
* [Поставщик IDistributedCache для NCache в веб-ферм с ASP.NET Core](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html) ([NCache на сайте GitHub](https://github.com/Alachisoft/NCache))
* <xref:performance/caching/memory>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
* <xref:host-and-deploy/web-farm>
