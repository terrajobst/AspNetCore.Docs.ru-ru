---
title: Кэширование ответов в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать кэширование ответов, чтобы снизить требования к пропускной способности и повысить производительность приложений ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: 99093cd281ffa8dddc574dc27254c0175e2651b3
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207372"
---
# <a name="response-caching-in-aspnet-core"></a>Кэширование ответов в ASP.NET Core

По [Luo Джон](https://github.com/JunTaoLuo), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Люк Лэтем](https://github.com/guardrex)

> [!NOTE]
> Кэширование ответов в Razor Pages в ASP.NET Core 2.1 или более поздней версии.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Кэширование ответов уменьшает количество запросов, выполненных клиентского приложения или прокси-сервера веб-сервера. Кэширование ответов также сокращает время работы веб-сервер выполняет для создания ответа. Кэширование ответов управляется заголовки, указывающие способ клиента, прокси-сервера и по промежуточного слоя для кэширования ответов.

Веб-сервер может кэшировать ответы, при добавлении [по промежуточного слоя, кэширование ответов](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>Кэширование ответов на основе HTTP

[Спецификации кэширования HTTP 1.1](https://tools.ietf.org/html/rfc7234) описано поведение Internet кэшей. Основной заголовок HTTP, используемый для кэширования [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), который используется для указания кэша *директивы*. Директивы Управление поведением кэширования запросов все же встречаются несущественные от клиентов к серверам и принимая ответов свой путь с серверов клиентам. Запросы и ответы перемещения между прокси-серверами и прокси-серверы также должны соответствовать спецификации кэширования HTTP 1.1.

Распространенные `Cache-Control` директивы показаны в следующей таблице.

| Директива                                                       | Действие |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Кэш может хранить ответ. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Ответ не должны храниться в общий кэш. Частный кэш может хранить и повторно использовать ответ. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Клиент не будет принимать ответ, возраст которых больше, чем указанное число секунд. Примеры: `max-age=60` (60 секунд), `max-age=2592000` (1 месяц) |
| [без кэша](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **При запросах**: кэш не должны использовать хранимые ответ для удовлетворения запроса. Примечание: На сервере-источнике повторно формирует ответ для клиента, и по промежуточного слоя обновляет ответов в своем кэше.<br><br>**При ответах**: ответ не должны использоваться для последующих запросов без проверки на исходном сервере. |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **При запросах**: кэш не может храниться запрос.<br><br>**При ответах**: кэш не должен хранить любую часть ответа. |

В следующей таблице показаны другие заголовки кэша, которые влияют на кэширование.

| Header                                                     | Функция |
| ---------------------------------------------------------- | -------- |
| [Срок действия](https://tools.ietf.org/html/rfc7234#section-5.1)     | Приблизительное количество времени в секундах с момента ответа сформирован или создан успешно прошли проверку на исходном сервере. |
| [Срок действия истекает](https://tools.ietf.org/html/rfc7234#section-5.3) | Дата и время, после чего ответ считается устаревшей. |
| [Директивы pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Существует для обеспечения обратной совместимости с HTTP/1.0 кэширует параметра `no-cache` поведение. Если `Cache-Control` заголовок присутствует, `Pragma` заголовка учитывается. |
| [Различаются](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Указывает, что кэшированный ответ должен быть отправляется, только если все из `Vary` заголовок поля согласуются в кэшированный ответ исходного запроса и новый запрос. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>Отношениях кэширования на основе HTTP запроса директивы Cache-Control

[Спецификации кэширования HTTP 1.1 для заголовка Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) требует кэша соблюдать является допустимым для `Cache-Control` заголовка, отправленные клиентом. Клиент может выполнять запросы с `no-cache` значение заголовка и принудительного создания нового ответа для каждого запроса сервером.

Всегда учитывает клиента `Cache-Control` заголовки запроса смысл, если вы считаете цель HTTP-кэширования. В официальной спецификации кэширование призвана сократить расходы на задержку и сети для удовлетворения запросов по сети, прокси-серверов, серверов и клиентов. Это не обязательно для контроля нагрузки на сервер-источник.

Нет не текущий контроль разработчика над это поведение кэширования, при использовании [по промежуточного слоя для кэширования ответа](xref:performance/caching/middleware) так, как по промежуточного слоя, соответствует официальной спецификации кэширования. [В будущем в по промежуточного слоя](https://github.com/aspnet/ResponseCaching/issues/96) позволит Настройка по промежуточного слоя, чтобы игнорировать запроса `Cache-Control` заголовка при принятии решения о обслуживать кэшированный ответ. Это предложит вам возможность более эффективно контролировать загрузку сервера при использовании по промежуточного слоя.

## <a name="other-caching-technology-in-aspnet-core"></a>Другие технология кэширования в ASP.NET Core

### <a name="in-memory-caching"></a>Кэширование в памяти

Кэширование в памяти использует память сервера для хранения кэшированных данных. Этот тип кэширования подходит для одного или нескольких серверов с использованием *прикрепленные сеансы*. Прикрепленные сеансы означает, что запросы от клиента всегда направляются на один и тот же сервер для обработки.

Дополнительные сведения см. в разделе [кэшировать в памяти](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Распределенного кэша

Используйте распределенный кэш для хранения данных в памяти, когда приложение будет размещено в облаке или сервер фермы. Кэш распределяется по всем серверам, которые обрабатывают запросы. Клиент может отправить запрос, обрабатываемый любой сервер в группе, при наличии кэшированных данных для клиента. ASP.NET Core предлагает SQL Server и кэши Redis распределенных.

Дополнительные сведения см. в разделе <xref:performance/caching/distributed>.

### <a name="cache-tag-helper"></a>Вспомогательная функция тега кэша

Вы можете кэшировать содержимое из представления MVC или страница Razor со вспомогательной функцией тега кэша. Вспомогательная функция тега кэша использует кэширование в памяти для хранения данных.

Дополнительные сведения см. в разделе [вспомогательная функция тега кэша в ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Вспомогательная функция тега распределенного кэша

Вы можете кэшировать содержимое из представления MVC или страницы Razor в распределенных облачных или веб-ферм с вспомогательная функция тега распределенного кэша. Для хранения данных кэша вспомогательной функции тега распределенного использует SQL Server или Redis.

Дополнительные сведения см. в разделе [вспомогательной функции тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Атрибута ResponseCache

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) указывает параметры, необходимые для задания соответствующих заголовков в кэширование ответов.

> [!WARNING]
> Отключите кэширование для содержимого, которое содержит сведения о прошедших проверку подлинности клиентов. Кэширование можно включать только для содержимого, которое не меняется на основе удостоверения пользователя или ли пользователь выполнил вход.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) изменяет хранимую ответ по значениям указанного списка ключей запроса. Если одно значение `*` не указан, по промежуточного слоя, зависит от ответов для всех запросов параметров строки запроса. `VaryByQueryKeys` требуется ASP.NET Core 1.1 или более поздней.

Необходимо включить по промежуточного слоя кэширования ответов для задания `VaryByQueryKeys` свойство; в противном случае создается исключение времени выполнения. Нет соответствующего заголовка HTTP для `VaryByQueryKeys` свойство. Свойство — это функция HTTP, обрабатываемых по промежуточного слоя кэширования ответов. Для по промежуточного слоя для обслуживания кэшированный ответ строку запроса и значения строки запроса, должны совпадать предыдущего запроса. Например рассмотрим последовательность запросов и результаты, показанные в следующей таблице.

| Запрос                          | Результат                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Сервер вернул     |
| `http://example.com?key1=value1` | Возвращаемые по промежуточного слоя |
| `http://example.com?key1=value2` | Сервер вернул     |

Первый запрос возвращается сервером и кэшируются в по промежуточного слоя. Второй запрос возвращается по промежуточного слоя, так как строка запроса совпадает с предыдущим запросом. Третий запрос не в кэше по промежуточного слоя, поскольку значение строки запроса не соответствует предыдущего запроса. 

`ResponseCacheAttribute` Используется для настройки и создания (с помощью `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). `ResponseCacheFilter` Выполняет обновление соответствующие заголовки HTTP и функции ответа. Фильтр:

* Удаляет любые существующие заголовки для `Vary`, `Cache-Control`, и `Pragma`. 
* Записывает соответствующие заголовки, в зависимости от свойств, заданных `ResponseCacheAttribute`. 
* Обновляет ответ HTTP функции кэширования, если `VaryByQueryKeys` имеет значение.

### <a name="vary"></a>Различаются

Этот заголовок записывается только когда `VaryByHeader` свойству. Ему будет присвоено `Vary` значение свойства. В следующем примере используется `VaryByHeader` свойство:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

Вы можете просматривать заголовки ответа с помощью средства обозревателя сети. На следующем рисунке показана F12 Edge, выведенных в **сети** вкладке `About2` обновляется метод действия:

![Граничный узел вывод F12 на вкладке «сети» при вызове метода действия About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore и Location.None

`NoStore` переопределяет большинство других свойств. Если присвоить этому свойству `true`, `Cache-Control` заголовок имеет значение `no-store`. Если `Location` присваивается `None`:

* Параметру `Cache-Control` задается значение `no-store,no-cache`.
* Параметру `Pragma` задается значение `no-cache`.

Если `NoStore` — `false` и `Location` — `None`, `Cache-Control` и `Pragma` присваивается `no-cache`.

Обычно устанавливается `NoStore` для `true` на страницы ошибок. Пример:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

Это приводит к следующие заголовки:

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a>Расположение и длительность

Чтобы включить кэширование, `Duration` должно быть присвоено положительное значение и `Location` должен быть либо `Any` (по умолчанию) или `Client`. В этом случае `Cache-Control` заголовок присваивается значение расположения, за которым следует `max-age` ответа.

> [!NOTE]
> `Location`в параметры `Any` и `Client` переводиться `Cache-Control` значения заголовка `public` и `private`, соответственно. Как отмечалось ранее, параметр `Location` для `None` задает оба `Cache-Control` и `Pragma` заголовки `no-cache`.

Ниже пример, показывающий, заголовки, создаваемое путем установки `Duration` и оставить значение по умолчанию `Location` значение:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

В результате получается следующий заголовок:

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Профили кэша

Вместо повторения `ResponseCache` параметров на множество атрибутов действий контроллера, профили кэша можно настроить как параметры, при настройке MVC в `ConfigureServices` метод в `Startup`. Значения в конфигурации кэша, на которую указывает ссылка, используются значения по умолчанию, `ResponseCache` атрибут и переопределяются все свойства, заданные в атрибуте.

Настройка профиля кэша.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

Ссылка на профиль кэша:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

`ResponseCache` Атрибут может применяться как для действия (методы) и контроллеры (классы). Атрибуты на уровне метода переопределяют параметры, указанные в атрибуты уровня класса.

В приведенном выше примере атрибут уровня класса указывает в течение 30 секунд, а атрибутом уровня методов ссылок на профиль кэша с длительностью 60 секунд.

Итоговый заголовка:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Сохранения ответов в кэше](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
