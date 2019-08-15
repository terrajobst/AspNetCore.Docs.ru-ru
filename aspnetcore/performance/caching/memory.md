---
title: Кэширование в памяти в ASP.NET Core
author: rick-anderson
description: Узнайте, как кэшировать данные в памяти в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 8/11/2019
uid: performance/caching/memory
ms.openlocfilehash: 474f71225371ea89b023ee077d4ecc03e9751681
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030316"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Кэширование в памяти в ASP.NET Core

[Рик Андерсон (](https://twitter.com/RickAndMSFT), [Джон Луо](https://github.com/JunTaoLuo)и [Стив Смит](https://ardalis.com/)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="caching-basics"></a>Основы кэширования

Кэширование может значительно повысить производительность и масштабируемость приложения, уменьшая объем работы, необходимый для создания содержимого. Кэширование лучше всего работает с данными, которые изменяются редко. Кэширование создает копию данных, которая может быть возвращена гораздо быстрее, чем из исходного источника. Приложения должны быть написаны и протестированы, чтобы **никогда не** зависеть от кэшированных данных.

ASP.NET Core поддерживает несколько разных кэшей. Простейший кэш основан на [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), который представляет кэш, хранящийся в памяти веб-сервера. Приложения, работающие на ферме серверов с несколькими серверами, должны гарантировать, что сеансы будут прикреплены при использовании кэша в памяти. Закрепленные сеансы гарантируют, что последующие запросы от клиента отправляются на один и тот же сервер. Например, веб-приложения Azure используют [маршрутизацию запросов приложений](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) для маршрутизации всех последующих запросов к тому же серверу.

Для неприкрепленных сеансов в веб-ферме требуется [распределенный кэш](distributed.md) , чтобы избежать проблем с согласованностью кэша. Для некоторых приложений распределенный кэш может поддерживать более высокие возможности масштабирования, чем кэш в памяти. Использование распределенного кэша разгружает кэш-память во внешний процесс.

::: moniker range="< aspnetcore-2.0"

Кэш будет исключать записи кэша при нехватке памяти, если только [приоритет кэша](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) не установлен в `CacheItemPriority.NeverRemove`значение. `IMemoryCache` Можно задать, `CacheItemPriority` чтобы изменить приоритет, с которым кэш исключает элементы при нехватке памяти.

::: moniker-end

Кэш в памяти может хранить любой объект. интерфейс распределенного кэша ограничен `byte[]`. Элементы кэша в памяти и распределенного кэша хранятся в виде пар "ключ-значение".

## <a name="systemruntimecachingmemorycache"></a>System. Runtime. Caching/MemoryCache

<xref:System.Runtime.Caching>/<xref:System.Runtime.Caching.MemoryCache>([Пакет NuGet](https://www.nuget.org/packages/System.Runtime.Caching/)) можно использовать с:

* .NET Standard 2,0 или более поздней версии.
* Любая [реализация .NET](/dotnet/standard/net-standard#net-implementation-support) , предназначенная для .NET Standard 2,0 или более поздней версии. Например, ASP.NET Core 2,0 или более поздней версии.
* .NET Framework 4,5 или более поздней версии.

[Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/) / `IMemoryCache` (описывается в `System.Runtime.Caching` / `MemoryCache` этой статье) рекомендуется использовать, так как он лучше интегрирован в ASP.NET Core. Например, `IMemoryCache` работает изначально с [внедрением зависимостей](xref:fundamentals/dependency-injection)ASP.NET Core.

Используйте `System.Runtime.Caching` вкачествемоста`MemoryCache` совместимости при переносе кода из ASP.NET 4. x в ASP.NET Core. /

## <a name="cache-guidelines"></a>Рекомендации по кэшированию

* Код должен всегда иметь резервный вариант для выборки данных и **не** зависит от доступности кэшированного значения.
* Кэш использует неограниченный ресурс, память. Ограничение роста кэша:
  * **Не** используйте внешние входные данные в качестве ключей кэша.
  * Используйте срок действия, чтобы ограничить рост кэша.
  * [Используйте SetSize, Size и сизелимит для ограничения размера кэша](#use-setsize-size-and-sizelimit-to-limit-cache-size). Среда выполнения ASP.NET Core не ограничивает размер кэша на основе нехватки памяти. Разработчик может ограничить размер кэша.

## <a name="using-imemorycache"></a>Использование IMemoryCache

> [!WARNING]
> Использование кэша *общей* памяти из [внедрения зависимостей](xref:fundamentals/dependency-injection) и вызова `SetSize`, `Size`или `SizeLimit` для ограничения размера кэша может привести к сбою приложения. Если для кэша задано ограничение размера, все записи должны указывать размер при добавлении. Это может привести к проблемам, так как разработчики могут не иметь полного контроля над тем, что использует общий кэш. Например, Entity Framework Core использует общий кэш и не задает размер. Если приложение устанавливает ограничение размера кэша и использует EF Core, приложение создает исключение `InvalidOperationException`.
> При использовании `SetSize`, `Size`или `SizeLimit` для ограничения кэша создайте одноэлементный кэш для кэширования. Дополнительные сведения и пример см. в статьях [Использование SetSize, Size и сизелимит для ограничения размера кэша](#use-setsize-size-and-sizelimit-to-limit-cache-size).

Кэширование в памяти — это *Служба* , на которую можно ссылаться из приложения с помощью [внедрения зависимостей](xref:fundamentals/dependency-injection). Вызов `AddMemoryCache` в `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Запросите `IMemoryCache` экземпляр в конструкторе:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache`требуется пакет NuGet [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache`требуется пакет NuGet [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), доступный в [Microsoft. AspNetCore. ALL метапакет](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache`требуется пакет NuGet [Microsoft. Extensions. Caching. Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), доступный в [Microsoft. AspNetCore. app метапакет](xref:fundamentals/metapackage-app).

::: moniker-end

В следующем коде используется [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) для проверки, находится ли время в кэше. Если время не кэшировано, создается новая запись и добавляется в кэш с помощью [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Текущее время и кэшированное время отображаются:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Кэшированное `DateTime` значение остается в кэше во время выполнения запросов в течение периода ожидания. На следующем рисунке показано текущее время и старое время, полученное из кэша:

![Представление индекса с двумя различно отображаемым временем](memory/_static/time.png)

В следующем коде для кэширования данных используются [жеторкреате](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreate#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) и [жеторкреатеасинк](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.getorcreateasync#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) .

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Следующий код вызывает [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) для получения кэшированного времени:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreate*>методы <xref:Microsoft.Extensions.Caching.Memory.CacheExtensions.GetOrCreateAsync*>, и [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) являются методами расширения в <xref:Microsoft.Extensions.Caching.Memory.IMemoryCache>классе [качикстенсионс](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) , который расширяет возможности. Описание других методов кэширования см. в разделе [методы IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) и [методы качикстенсионс](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) .

## <a name="memorycacheentryoptions"></a>меморикачинтрйоптионс

Следующий пример:

* Задает абсолютный срок действия. Это максимальное время, в течение которого запись может быть кэширована, и она предотвращает слишком неактуальность элемента при постоянном обновлении скользящего срока действия.
* Задает скользящий срок действия. Запросы, обращающиеся к этому кэшированному элементу, будут сбрасывать скользящий срок действия.
* Устанавливает для приоритета кэша `CacheItemPriority.NeverRemove`значение.
* Задает [постевиктионделегате](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , который будет вызываться после удаления записи из кэша. Обратный вызов выполняется в другом потоке из кода, который удаляет элемент из кэша.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Использование SetSize, Size и Сизелимит для ограничения размера кэша

`MemoryCache` Экземпляр может дополнительно задавать и применять ограничение размера. Ограничение на размер памяти не имеет определенной единицы измерения, так как кэш не имеет механизма для измерения размера записей. Если установлен предел объема памяти кэша, все записи должны указывать size. Среда выполнения ASP.NET Core не ограничивает размер кэша на основе нехватки памяти. Разработчик может ограничить размер кэша. Указанный размер находится в единицах, которые выбирает разработчик.

Например:

* Если веб-приложение в основном кэширует строки, каждый размер записи кэша может быть длиной строки.
* Приложение может указать размер всех записей как 1, а максимальный размер — число записей.

Следующий код создает [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) с фиксированным размером безмодульного доступа при [внедрении зависимостей](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[Сизелимит](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) не имеет единиц. Кэшированные записи должны указывать размер в любых единицах, которые они считаются наиболее подходящими, если размер кэша памяти был установлен. Все пользователи экземпляра кэша должны использовать одну и ту же систему единиц измерения. Запись не будет кэшироваться, если сумма кэшированных размеров записей превышает значение, заданное параметром `SizeLimit`. Если ограничение размера кэша не задано, размер кэша, заданный для записи, будет проигнорирован.

Следующий код регистрируется `MyMemoryCache` в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) .

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache`создается как независимый кэш памяти для компонентов, которые знают об этом ограниченном размере кэша и знают, как задать размер записи кэша соответствующим образом.

В следующем коде используется `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Размер записи кэша можно задать по [размеру](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) или методу расширения [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) :

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Зависимости кэша

В следующем примере показано, как истечет срок действия записи кэша, если истечет срок действия зависимой записи. `CancellationChangeToken` Добавляется к кэшированному элементу. Если `Cancel` метод вызывается `CancellationTokenSource`для, то обе записи кэша будут вытеснены.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

`CancellationTokenSource` Использование позволяет удалять несколько записей кэша как группу. Используя шаблон в приведенном выше коде, записи кэша, созданные `using` внутри блока, будут наследовать параметры триггеров и срока действия. `using`

## <a name="additional-notes"></a>Дополнительные сведения

* При использовании обратного вызова для повторного заполнения элемента кэша:

  * Несколько запросов могут найти значение кэшированного ключа пустым, так как обратный вызов не завершен.
  * Это может привести к повторному заполнению кэшированного элемента несколькими потоками.

* Если одна запись кэша используется для создания другой, дочерняя запись копирует маркеры истечения срока действия родительской записи и параметры срока действия, основанные на времени. Срок действия дочернего элемента не истек, удаление или обновление родительской записи вручную.

* Используйте [постевиктионкаллбаккс](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) , чтобы задать обратные вызовы, которые будут срабатывать после удаления записи кэша из кэша.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
