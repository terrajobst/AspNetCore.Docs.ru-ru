---
title: "Распределенного кэша тег вспомогательный | Документы Microsoft"
author: pkellner
description: "Показано, как работать с вспомогательный тег кэша"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: f5844dade218fdba1169a55fe3ce251a9cc03db2
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="distributed-cache-tag-helper"></a>Вспомогательный тег распределенного кэша

Автор: [Питер Кельнер (Peter Kellner)](http://peterkellner.net) 


Тег вспомогательный объект распределенного кэша позволяет существенно повысить производительность приложения ASP.NET Core, кэшируя его содержимого с источником распределенного кэша.

Вспомогательный распределенного кэша тег наследует тот же базовый класс как вспомогательный тег кэша.  Все атрибуты, связанные со вспомогательным методом тег кэша также будет работать в вспомогательный распределенных тег.


Вспомогательный распределенного кэша тег соответствует **явные зависимости принцип** называется **внедрение конструктора**.  В частности `IDistributedCache` интерфейс контейнера передается в конструктор распределенного кэша тег помощника.  Если нет конкретные реализации `IDistributedCache` была создана в `ConfigureServices`, обычно можно найти в файле startup.cs, а затем распределенного кэша тег вспомогательное приложение будет использовать одного поставщика в памяти для хранения кэшированных данных в виде basic вспомогательные тег кэша.

## <a name="distributed-cache-tag-helper-attributes"></a>Распределенные атрибуты вспомогательного тег кэша

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>включить истечения срока действия на срок действия истекает после истечения срока действия скользящий различаются по заголовок различаются по запроса различаются по Маршрутизация различаются по cookie изменяться на уровне пользователей различаются с приоритетом

Для определения в разделе вспомогательные тег кэша. Вспомогательный тег распределенного кэша наследует от одного класса в качестве вспомогательной тег кэша, поэтому эти атрибуты являются общими из вспомогательного тег кэша.

- - -

### <a name="name-required"></a>Имя (обязательно)

| Тип атрибута    | Пример значения     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

Необходимая `name` атрибут используется в качестве ключа, кэш сохраняется для каждого экземпляра вспомогательный тег распределенного кэша.  В отличие от основных кэш тег вспомогательного приложения, назначающий ключа к каждому экземпляру вспомогательный тег кэша на основе имени страницы Razor и расположение тега вспомогательного метода на странице razor вспомогательные распределенного кэша тег только сформирует его ключ на атрибут`name`

Пример использования:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>Распределенного кэша тег вспомогательные IDistributedCache реализации

Существует два способа реализации `IDistributedCache` встроенной в ASP.NET Core.  Один основан на **Sql Server** и на основе другого **Redis**. Подробности этих реализаций можно найти в ресурсе, упоминаемой ниже именованный «работа с распределенного кэша». Обе реализации включают задание экземпляра `IDistributedCache` в ASP.NET Core **файла startup.cs**.

Нет никаких атрибутов тега, в частности, связанные с использованием любой конкретной реализации `IDistributedCache`.



- - -



## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
