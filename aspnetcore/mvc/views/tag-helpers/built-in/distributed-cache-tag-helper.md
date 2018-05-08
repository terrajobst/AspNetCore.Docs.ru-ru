---
title: Вспомогательная функция тега распределенного кэша в ASP.NET Core
author: pkellner
description: Сведения о работе со вспомогательной функцией тега кэша
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 929156633048b8ee68a66290f44b12026a08c8c9
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега распределенного кэша в ASP.NET Core

Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) 


Вспомогательная функция тега распределенного кэша позволяет существенно повысить производительность приложения ASP.NET Core за счет кэширования его содержимого в источник распределенного кэша.

Вспомогательная функция тега распределенного кэша наследуется от того же базового класса, что и вспомогательная функция тега кэша.  Все атрибуты, связанные со вспомогательной функцией тега кэша, будут также работать во вспомогательной функции тега распределенного кэша.


Во вспомогательной функции тега распределенного кэша соблюдается **принцип явных зависимостей**, который также известен как **внедрение конструктора**.  В частности, контейнер интерфейса `IDistributedCache` передается в конструктор вспомогательной функции тега распределенного кэша.  Если в методе `ConfigureServices`, который обычно находится в файле startup.cs, не создана определенная реализация интерфейса `IDistributedCache`, вспомогательная функция тега распределенного кэша будет использовать для хранения кэшированных данных тот же поставщик в памяти, что и базовая вспомогательная функция тега кэша.

## <a name="distributed-cache-tag-helper-attributes"></a>Атрибуты вспомогательной функции тега распределенного кэша

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>enabled expires-on expires-after expires-sliding vary-by-header vary-by-query vary-by-route vary-by-cookie vary-by-user vary-by priority

Описание этих атрибутов см. в статье, посвященной вспомогательной функции тега кэша. Вспомогательная функция тега распределенного кэша наследуется от того же класса, что и вспомогательная функция тега кэша, поэтому все эти атрибуты являются для них общими.

- - -

### <a name="name-required"></a>name (обязательный)

| Тип атрибута    | Пример значения     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

Обязательный атрибут `name` служит ключом кэша, хранящегося для каждого экземпляра вспомогательной функции тега распределенного кэша.  В отличие от базовой вспомогательной функции тега кэша, каждому экземпляру которой ключ присваивается на основе имени страницы Razor и расположения вспомогательной функции тега на странице Razor, ключ вспомогательной функции тега распределенного кэша основан только на атрибуте `name`.

Пример использования:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Реализации IDistributedCache для вспомогательной функции тега распределенного кэша

В ASP.NET Core есть две встроенные реализации интерфейса `IDistributedCache`.  Одна из них основана на **Sql Server**, а другая — на **Redis**. Подробные сведения об этих реализациях см. в статье "Работа с распределенным кэшем", ссылка на которую приведена ниже. Обе реализации предусматривают задание экземпляра `IDistributedCache` в файле **startup.cs** ASP.NET Core.

Атрибуты тегов, связанные с использованием определенной реализации `IDistributedCache`, отсутствуют.



- - -



## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
