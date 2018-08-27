---
title: Кэш в памяти в ASP.NET Core
author: rick-anderson
description: Узнайте, как кэширование данных в памяти в ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 7/22/2018
uid: performance/caching/memory
ms.openlocfilehash: 091d00ca7a30b61bdd83618e055bf23e0f2753c4
ms.sourcegitcommit: 67a0a04ebb3b21c826e5b9600bacfc897abd6a46
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/24/2018
ms.locfileid: "42899848"
---
# <a name="cache-in-memory-in-aspnet-core"></a>Кэш в памяти в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Luo Джон](https://github.com/JunTaoLuo), и [Стив Смит](https://ardalis.com/)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Кэширование основы

Кэширование может значительно повысить производительность и масштабируемость приложения путем уменьшения работ, необходимый для создания содержимого. Кэширование будет работать лучше всего с данными, которые редко изменяются. Кэширование делает копию данных, которые могут быть возвращены большую быстрее, чем из исходного источника. Вы должны писать и тестировать приложения никогда не зависят от кэшированных данных.

ASP.NET Core поддерживает несколько разных кэшах. Самый простой кэш основан на [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), который представляет кэш хранится в памяти веб-сервера. Приложения, которые выполняются на нескольких серверах фермы следует убедиться, что прикрепленные сеансы не при использовании кэша в памяти. Прикрепленные сеансы убедитесь, что последующие запросы от всех клиента перейдите к тому же серверу. Например, приложения используют Azure Web [маршрутизации запросов приложений](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), чтобы перенаправлять все последующие запросы на тот же сервер.

Non прикрепленные сеансы на веб-ферме требуется [распределенный кеш](distributed.md) во избежание проблем с согласованностью кэша. Для некоторых приложений распределенного кэша может поддерживать более горизонтальное масштабирование, чем кэш, расположенный в памяти. С помощью распределенного кэша разгружает кэш-памяти во внешний процесс.

`IMemoryCache` Кэша вытеснит записей кэша при недостатке свободной памяти, если не [кэшировать приоритет](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) присваивается `CacheItemPriority.NeverRemove`. Можно задать `CacheItemPriority` изменять приоритет, с которым кэша исключает элементы при недостатке свободной памяти.

Кэш в памяти можно хранить любой объект; интерфейс распределенного кэша ограничен `byte[]`.

### <a name="cache-guidelines"></a>Руководство по использованию кэша

* Код всегда будет иметь способа возврата для выборки данных и **не** зависят от кэшированное значение, его доступность.
* Кэш использует дефицитных ресурсов памяти. Ограничить рост кэша:
  * Сделать **не** использовать внешние входные данные в качестве ключей кэша.
  * Используйте срок действия, чтобы ограничить рост кэша.
  * [Позволяет ограничить размер кэша SetSize, размер и SizeLimit](#use-setsize-size-and-sizelimit-to-limit-cache-size)

## <a name="using-imemorycache"></a>С помощью IMemoryCache

Кэширование в памяти является *службы* , на которую ссылается приложение, нажав [внедрения зависимостей](../../fundamentals/dependency-injection.md). Вызовите `AddMemoryCache` в `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

Запросить `IMemoryCache` экземпляр в конструкторе:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), которая доступна в [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="> aspnetcore-2.0"

`IMemoryCache` требуется пакет NuGet [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), которая доступна в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

В следующем коде используется [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) проверки, если время в кэше. Если время не кэшируется, новая запись создается и добавляется в кэш с [задать](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp [](memory/sample/WebCache/CacheKeys.cs)]

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Отображается текущее время и время кэширования:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Кэшированный `DateTime` значение остается в кэше, пока имеются запросы в истечения времени ожидания (и не вытеснение из-за нехватки памяти). На следующем рисунке показана текущее время и время извлечен из кэша:

![Представление index отображаются два различных времени](memory/_static/time.png)

В следующем коде используется [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) и [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) для кэширования данных. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Следующий код вызывает [получить](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) получить время кэширования:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

См. в разделе [методы IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) и [CacheExtensions методы](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) описание методов кэша.

## <a name="memorycacheentryoptions"></a>MemoryCacheEntryOptions

Следующий пример:

- Задает абсолютный срок действия. Это максимальное время, операции могут быть кэшированы и запрещает устаревание при скользящий срок действия постоянно обновляется элемент.
- Задает скользящего срока. Запросы, которые обращаются к этого кэшированного элемента приведет к сбросу скользящего срока.
- Задает приоритет кэша с `CacheItemPriority.NeverRemove`.
- Наборы [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) , будет вызван после удаления записи из кэша. Обратный вызов выполняется в другом потоке из кода, который удаляет элемент из кэша.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

::: moniker range=">= aspnetcore-2.0"

## <a name="use-setsize-size-and-sizelimit-to-limit-cache-size"></a>Позволяет ограничить размер кэша SetSize, размер и SizeLimit

Объект `MemoryCache` экземпляр может при необходимости указывать и обеспечивать максимального размера. Предельный размер памяти не имеет определенных единица измерения, поскольку отсутствует механизм для измерения размера записей кэша. Если предельный размер кэш-памяти, все записи необходимо указать размер. Указанный размер измеряется в выбору разработчика.

Пример:

* Если веб-приложения был главным образом кэширование строк, каждый размер кэша запись может быть длину строки.
* Приложение может определить размер всех записей как 1, а максимальный размер составляет количество записей.

Следующий код создает единицами фиксированный размер [MemoryCache](/dotnet/api/microsoft.extensions.caching.memory.memorycache?view=aspnetcore-2.1) доступном [внедрения зависимостей](xref:fundamentals/dependency-injection):

[!code-csharp[](memory/sample/RPcache/Services/MyMemoryCache.cs?name=snippet)]

[SizeLimit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheoptions.sizelimit?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheOptions_SizeLimit) имеет единицы. Записей в кэше необходимо указать размер в независимо от единиц, они считают наиболее подходящий в том случае, если установлен размер кэш-памяти. Все пользователи экземпляра кэша следует использовать одну и ту же систему единицы. Запись не будут кэшироваться, если сумма размеров кэшированной записи превышает значение, заданное параметром `SizeLimit`. Если нет ограничения размера кэша, задайте запись размер кэша будет игнорироваться.

В следующем коде регистрах `MyMemoryCache` с [внедрения зависимостей](xref:fundamentals/dependency-injection) контейнера.

[!code-csharp[](memory/sample/RPcache/Startup.cs?name=snippet&highlight=5)]

`MyMemoryCache` будет создан в качестве кэша памяти для компонентов, которые поддерживают этот размер ограничен кэш и знаете, как правильно настроить размер кэша записи.

В следующем коде используется `MyMemoryCache`:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet)]

Размер записи кэша задаются [размер](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.size?view=aspnetcore-2.1#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_Size) или [SetSize](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.setsize?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_MemoryCacheEntryExtensions_SetSize_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_System_Int64_) метод расширения:

[!code-csharp[](memory/sample/RPcache/Pages/About.cshtml.cs?name=snippet2&highlight=9,10,14,15)]

::: moniker-end

## <a name="cache-dependencies"></a>Зависимости кэша

Следующий пример показано, как срок действия записи кэша, по истечении зависимую запись. Объект `CancellationChangeToken` добавляется кэшированного элемента. Когда `Cancel` вызывается для `CancellationTokenSource`, обе записи кэша удаляются.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

С помощью `CancellationTokenSource` позволяет несколько записей кэша для удаления как группу. С помощью `using` шаблон в приведенном выше коде записей кэша создан внутри `using` блок будет наследовать триггеры и параметры срока действия.

## <a name="additional-notes"></a>Дополнительные сведения

- При использовании обратного вызова для повторного заполнения элемента кэша:

  - Несколько запросов можно найти кэшированное значение ключа пустой тем, что обратный вызов еще не завершена.
  - Это может привести несколько потоков, при повторном заполнении кэшированного элемента.

- Когда одна запись кэша используется для создания другой, дочерние копирует истечения срока действия маркеров родительская запись и параметры срока действия на основе времени. Дочерние не истекшим сроком действия путем ручного удаления или обновления родительского элемента.

- Используйте [PostEvictionCallbacks](/dotnet/api/microsoft.extensions.caching.memory.icacheentry.postevictioncallbacks#Microsoft_Extensions_Caching_Memory_ICacheEntry_PostEvictionCallbacks) для установки обратных вызовов, которые будут срабатывать после удаления записи кэша из кэша.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Работа с распределенным кэшем](xref:performance/caching/distributed)
* [Обнаружение изменений с помощью маркеров изменений](xref:fundamentals/primitives/change-tokens)
* [Кэширование ответов](xref:performance/caching/response)
* [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware)
* [Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
