---
title: Вспомогательная функция тега распределенного кэша в ASP.NET Core
author: pkellner
description: Сведения об использовании вспомогательной функции для тэга распределенного кэша.
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: a5b33451a763c297c6d7885855a321c43435abb4
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325215"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>Вспомогательная функция тега распределенного кэша в ASP.NET Core

Авторы: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) и [Люк Лэтэм (Luke Latham)](https://github.com/guardrex)

Вспомогательная функция тега распределенного кэша позволяет существенно повысить производительность приложения ASP.NET Core за счет кэширования его содержимого в источник распределенного кэша.

Общие сведения о вспомогательных функциях тегов см. в разделе <xref:mvc/views/tag-helpers/intro>.

Вспомогательная функция тега распределенного кэша наследуется от того же базового класса, что и вспомогательная функция тега кэша. Все атрибуты [вспомогательной функции тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper) доступны вспомогательной функции тега распределенного кэша.

Вспомогательная функция тега распределенного кэша использует [внедрение через конструктор](xref:fundamentals/dependency-injection#constructor-injection-behavior). Интерфейс <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> передается в конструктор вспомогательной функции тега распределенного кэша. Если конкретная реализация `IDistributedCache` не создается в `Startup.ConfigureServices` (*Startup.cs*), вспомогательная функция тега распределенного кэша использует тот же поставщик в памяти для хранения кэшированных данных, что и [вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

## <a name="distributed-cache-tag-helper-attributes"></a>Атрибуты вспомогательной функции тега распределенного кэша

### <a name="attributes-shared-with-the-cache-tag-helper"></a>Атрибуты, используемые совместно с вспомогательной функцией тега кэша

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

Вспомогательная функция тега распределенного кэша наследует от того же класса, что и вспомогательная функция тега кэша. Описание этих атрибутов см. в разделе [Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="name"></a>имя

| Тип атрибута | Пример                               |
| -------------- | ------------------------------------- |
| String         | `my-distributed-cache-unique-key-101` |

`name` является обязательным. Атрибут `name` используется в качестве ключа для каждого хранимого экземпляра кэша. В отличие от вспомогательной функции тега кэша, которая присваивает ключ кэша каждому экземпляру на основе имени страницы Razor и расположения на странице Razor, ключ вспомогательной функции тега распределенного кэша основан только на атрибуте `name`.

Пример

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Реализации IDistributedCache для вспомогательной функции тега распределенного кэша

В ASP.NET Core есть две встроенные реализации интерфейса <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache>. Одна из них основана на SQL Server, а другая — на Redis. Сведения об этих реализациях можно найти по адресу <xref:performance/caching/distributed>. Обе реализации предусматривают задание экземпляра `IDistributedCache` в `Startup`.

Атрибуты тегов, связанные с использованием определенной реализации `IDistributedCache`, отсутствуют.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
