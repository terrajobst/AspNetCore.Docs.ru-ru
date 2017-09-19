---
title: "Кэширование ответов"
author: rick-anderson
description: "Описание способов использования ответа кэширование снизить пропускную способность и повысить производительность."
keywords: "ASP.NET Core, кэширование, HTTP-заголовки ответа"
ms.author: riande
manager: wpickett
ms.date: 07/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 97bc56a3cfe7383f207a4f621ab3fe8b8dc258ef
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2017
---
# <a name="response-caching"></a>Кэширование ответов

По [Luo Джон](https://github.com/JunTaoLuo), [Рик Андерсон](https://twitter.com/RickAndMSFT), и [Стив Смит](https://ardalis.com/)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a>Что такое кэширование ответов

*Кэширование ответов* добавляет относящиеся к кэшированию заголовки ответов. Эти заголовки укажите способ клиента, прокси-сервера и по промежуточного слоя для кэширования ответов. Кэширование ответов можно уменьшить количество запросов, выполненных клиентом или прокси-сервера веб-сервера. Кэширование ответов можно также уменьшить объем работы выполняет веб-сервер для формирования ответа. 

Основной заголовок HTTP для кэширования `Cache-Control`. В разделе [кэширования HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2) для получения дополнительной информации. Общий кэш директивы:

* [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [Нет-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [Директивы pragma](https://tools.ietf.org/html/rfc7234#section-5.4)
* [Различаются](https://tools.ietf.org/html/rfc7231#section-7.1.4)

Веб-сервер может кэшировать ответы, добавив в ответ, кэширование по промежуточного слоя. В разделе [по промежуточного слоя кэширование ответов](middleware.md) для получения дополнительной информации.

## <a name="distributed-cache-tag-helper"></a>Вспомогательный тег распределенного кэша

[Распределенного кэша тег вспомогательный](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) включает распределенное кэширование.


## <a name="responsecache-attribute"></a>Атрибут ResponseCache

`ResponseCacheAttribute` Указывает параметры, необходимые для установки соответствующие заголовки кэширование ответов. В разделе [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) описание параметров.

>[!WARNING]
> Отключение кэширования для содержимого, которое содержит сведения о прошедших проверку подлинности клиентов. Кэширование должны включаться только для содержимого, которые не изменяются в зависимости от идентификатора пользователя или пользователь вошел.

`VaryByQueryKeys string[]`(требуется ASP.NET Core 1.1.0 и более поздние версии): при задании ответ, кэширование по промежуточного слоя будет изменять хранимых ответ значения из заданного списка ключей запроса. Для установки необходимо включить кэширование по промежуточного слоя ответов `VaryByQueryKeys` свойство, в противном случае среда выполнения будет вызвано исключение. Отсутствует соответствующий заголовок HTTP для `VaryByQueryKeys` свойства. Это свойство является функцией HTTP, обрабатываемых по промежуточного слоя кэширование ответов. По промежуточного слоя обслуживать кэшированный ответ строка запроса и значение строки запроса должно соответствовать предыдущего запроса. Например рассмотрим следующую последовательность:

| Запрос          | Результат |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | возвращенный от сервера |
| `http://example.com?key1=value1` | возвращаемые по промежуточного слоя |
| `http://example.com?key1=value2` | возвращенный от сервера |

Первый запрос возвращается сервером и кэшируются в по промежуточного слоя. Второй запрос возвращается по промежуточного слоя, так как предыдущий запрос соответствует строке запроса. Третий запрос не является в кэше по промежуточного слоя, так как значение строки запроса не соответствует предыдущего запроса. 

`ResponseCacheAttribute` Используется для настройки и создания (через `IFilterFactory`) `ResponseCacheFilter`. `ResponseCacheFilter` Выполняет обновление компонентов ответа и соответствующие заголовки HTTP. Фильтр:

* Удаляет все существующие заголовки для `Vary`, `Cache-Control`, и `Pragma`. 
* Записывает соответствующие заголовки, в зависимости от свойств, заданных `ResponseCacheAttribute`. 
* Обновляет ответ HTTP функции кэширования, если `VaryByQueryKeys` имеет значение.

### <a name="vary"></a>Различаются

Этот заголовок записывается только когда `VaryByHeader` свойству. Ему присваивается `Vary` значение свойства. В следующем примере используется `VaryByHeader` свойство.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

Заголовки ответа можно просмотреть с помощью сетевых средств браузеры. На следующем рисунке показаны границы F12, выведенных в **сети** вкладке при `About2` обновляется метод действия. 

![Ребро F12 выходные данные на ** сети ** вкладка при вызове метода действия «About2»](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore и Location.None

`NoStore`переопределяет большинство других свойств. Если значение этого свойства `true`, `Cache-Control` заголовок будет присвоено «нет-store». Если `Location` равно `None`:

* Параметру `Cache-Control` задается значение `"no-store, no-cache"`. 
* Параметру `Pragma` задается значение `no-cache`. 

Если `NoStore` — `false` и `Location` — `None`, `Cache-Control` и `Pragma` будет присвоено `no-cache`.

Обычно устанавливается `NoStore` для `true` на страницы ошибок. Пример:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

В результате следующие заголовки:

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Расположение и длительность

Чтобы включить кэширование, `Duration` должно быть присвоено положительное значение и `Location` должно равняться `Any` (по умолчанию) или `Client`. В этом случае `Cache-Control` заголовок будет присвоено значение расположения, следуют «max-age» ответа.

> [!NOTE]
> `Location`в параметры `Any` и `Client` переводиться `Cache-Control` значения заголовка `public` и `private`соответственно. Как отмечалось ранее, параметр `Location` для `None` установит оба `Cache-Control` и `Pragma` заголовки `no-cache`.

Ниже приведен пример заголовки создается, задав `Duration` и оставить значение по умолчанию `Location` значение.

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

Выводятся следующие заголовки:

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a>Профили кэша

Вместо повторения `ResponseCache` параметров на множество атрибутов действия контроллера, профилей кэша можно настроить как параметры, при настройке MVC в `ConfigureServices` метод в `Startup`. Значения, обнаруженные в профиль кэша, на которую указывает ссылка будет использоваться по умолчанию, `ResponseCache` атрибута и будет переопределено любого свойства, указанные в атрибуте.

Настройка профиля кэша.

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

Ссылка на профиль кэша:

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

`ResponseCache` Атрибут может применяться как для действия (методы), а также контроллеров (классы). Атрибуты на уровне метода переопределяют параметры, указанные в атрибуты уровня класса.

В приведенном выше примере атрибут уровня класса указывает в течение 30 секунд, пока атрибуты уровня метод ссылается на профиль кэша с длительностью 60 секунд.

Полученный заголовок:

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a>Дополнительные ресурсы

* [Кэширование в HTTP из спецификации](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
