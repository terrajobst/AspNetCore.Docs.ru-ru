---
title: Кэш в памяти в ASP.NET Core
author: rick-anderson
description: Узнайте, как кэширование данных в памяти в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: c2eae83219e8995a614b2933b1290d061f1b7869
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="cache-in-memory-in-aspnet-core"></a>Кэш в памяти в ASP.NET Core

По [Рик Андерсон](https://twitter.com/RickAndMSFT), [Luo Джон](https://github.com/JunTaoLuo), и [Стив Смит](https://ardalis.com/)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="caching-basics"></a>Основные принципы кэширования

Кэширование может значительно повысить производительность и масштабируемость приложения за счет снижения работ, необходимый для создания содержимого. Кэширование будет работать наилучшим образом с данными, которые редко изменяются. Кэширование делает копию данных, которые могут быть возвращены намного быстрее, чем из исходного источника. Следует писать и тестировать приложения никогда не зависят от кэшированных данных.

ASP.NET Core поддерживает несколько разных кэша. Простейший кэша на основе [IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache), который представляет кэша, хранящийся в памяти веб-сервера. Приложения, которые выполняются на ферме серверов из нескольких серверов следует убедиться, прикрепленные сеансы при использовании кэша в памяти. Прикрепленные сеансы убедитесь, что последующие запросы от клиента все перейти на том же сервере. Например, использование приложения Azure Web [маршрутизации запросов приложений](https://www.iis.net/learn/extensions/planning-for-arr) (ARR), чтобы перенаправлять все последующие запросы на тот же сервер.

Non прикрепленных сеансов в веб-ферме требуется [распределенный кэш](distributed.md) во избежание проблем с согласованностью кэша. Для некоторых приложений распределенного кэша может поддерживать более масштабного чем кэша в памяти. Использование распределенного кэша разгружает кэш-памяти во внешний процесс. 

`IMemoryCache` Кэша исключим записей кэша, свободной памяти, если не [кэшировать приоритет](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheitempriority) равно `CacheItemPriority.NeverRemove`. Можно задать `CacheItemPriority` Настройка приоритета, с которым кэша исключает элементы в условиях нехватки памяти.

Кэш в памяти можно хранить любой объект; интерфейс распределенного кэша ограничен `byte[]`.

## <a name="using-imemorycache"></a>С помощью IMemoryCache

Кэширование в памяти *службы* , на который ссылается приложение, нажав [внедрения зависимостей](../../fundamentals/dependency-injection.md). Вызовите `AddMemoryCache` в `ConfigureServices`:

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=8)] 

Запрос `IMemoryCache` экземпляр в конструкторе:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor&highlight=3,5-999)] 

`IMemoryCache` требуется пакет NuGet «Microsoft.Extensions.Caching.Memory».

В следующем коде используется [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) проверки, если время в кэше. Если время не кэшируется, новая запись создается и добавляется в кэш с [задать](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

Отображается текущее время и время кэширования:

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

Кэшированные `DateTime` значение остается в кэше, пока имеются запросы в течение периода ожидания (и без вытеснения, из-за нехватки памяти). На следующем рисунке показана текущего времени и получить из кэша, время:

![Индекс представления с помощью двух разных времени отображаются в](memory/_static/time.png)

В следующем коде используется [GetOrCreate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) и [GetOrCreateAsync](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) для кэширования данных. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

Следующий код вызывает [получить](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) выбирать время кэширования:

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

В разделе [методы IMemoryCache](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.imemorycache) и [CacheExtensions методы](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.cacheextensions) описание методов кэша.

## <a name="using-memorycacheentryoptions"></a>С помощью MemoryCacheEntryOptions

Следующий пример:

- Задает абсолютный срок действия. Это — это максимальное время, кэшировании записи и блокирует элемент устаревшими при скользящий срок действия постоянно обновляется.
- Задает скользящего срока. Запросы, которые обращаются к этой кэшированного элемента приведет к сбросу скользящего срока.
- Задает приоритет кэша `CacheItemPriority.NeverRemove`. 
- Наборы [PostEvictionDelegate](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.caching.memory.postevictiondelegate) , будет вызван после удаления записи из кэша. Обратный вызов выполняется в другом потоке, от кода, который удаляет элемент из кэша.

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-20)]

## <a name="cache-dependencies"></a>Зависимости кэша

Следующий пример показано, как срок действия записи кэша после истечения срока действия зависимую запись. Объект `CancellationChangeToken` добавляется кэшированного элемента. Когда `Cancel` будет вызван на `CancellationTokenSource`, вытесняются обе записи кэша. 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

С помощью `CancellationTokenSource` позволяет нескольких записей кэша для удаления в группу. С `using` шаблон в приведенном выше коде записей кэша, созданных в `using` блок будет наследовать триггеры и параметры истечения срока действия.

## <a name="additional-notes"></a>Дополнительные сведения

- При использовании обратного вызова для повторного заполнения элемента кэша:

  - Несколько запросов можно найти кэшированное значение ключа пустой тем, что обратный вызов еще не завершена. 
  - Это может привести к повторному заполнению кэшированного элемента нескольких потоков.

- При использовании одной записи кэша для создания другого дочернего копирует истечение срока действия маркеров и параметры истечения срока действия на основе времени родительскую запись. Дочерние не истек, удаление вручную, или обновление родительской записи.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Работа с распределенным кэшем](xref:performance/caching/distributed)
* [Обнаружение изменений с помощью маркеров изменений](xref:fundamentals/primitives/change-tokens)
* [Кэширование ответов](xref:performance/caching/response)
* [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware)
* [Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
