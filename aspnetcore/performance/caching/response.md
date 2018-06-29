---
title: Кэширование ответов в ASP.NET Core
author: rick-anderson
description: Сведения об использовании ответ, кэширование, более низкие требования к пропускной способности и повышения производительности приложений ASP.NET Core.
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: c53ae3f6ab8d26588533772dd4fdacb36ec12059
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077768"
---
# <a name="response-caching-in-aspnet-core"></a>Кэширование ответов в ASP.NET Core

По [Luo Джон](https://github.com/JunTaoLuo), [Рик Андерсон](https://twitter.com/RickAndMSFT), [Стив Смит](https://ardalis.com/), и [Latham Люк](https://github.com/guardrex)

> [!NOTE]
> Кэширование ответов на страницах Razor ASP.NET Core 2.1 или более поздних версий.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Кэширование ответов снижает количество запросов, выполненных клиентом или прокси-сервера веб-сервера. Кэширование ответов также сокращает время работы веб-сервер выполняет для формирования ответа. Кэширование ответов управляется заголовки, указывающие способ клиента, прокси-сервера и по промежуточного слоя для кэширования ответов.

Веб-сервер может кэшировать ответы, при добавлении [по промежуточного слоя кэширования ответа](xref:performance/caching/middleware).

## <a name="http-based-response-caching"></a>Кэширование ответов на основе HTTP

[Кэширования HTTP 1.1 спецификации](https://tools.ietf.org/html/rfc7234) описано поведение кэша Интернета. Основной заголовок HTTP для кэширования [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), который используется для указания кэша *директивы*. Директивы управляют поведением по мере запросов продвижения от клиентов на серверах и ответов продвижения от серверов обратно клиентам. Запросы и ответы перемещения между прокси-серверами и прокси-серверы также должны соответствовать спецификации кэширования HTTP 1.1.

Общие `Cache-Control` директивы показаны в следующей таблице.

| Директива                                                       | Действие |
| --------------------------------------------------------------- | ------ |
| [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | Кэш может хранить ответ. |
| [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | Ответ не должны храниться с общего кэша. Закрытый кэша может хранить и повторно использовать ответа. |
| [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | Клиент не будет принимать ответ возраст больше, чем указанное количество секунд. Примеры: `max-age=60` (60 секунд), `max-age=2592000` (1 месяц) |
| [Нет-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **В запросах,**: кэш не должны использовать хранимые ответа для удовлетворения запроса. Примечание: На исходном сервере повторно формирует ответ для клиента и по промежуточного слоя обновляет ответ хранимых в кэше.<br><br>**При ответах**: ответ не должны использоваться для последующего запроса без проверки на исходном сервере. |
| [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **В запросах,**: кэш не следует хранить запроса.<br><br>**При ответах**: кэш не следует хранить любую часть ответа. |

В следующей таблице показаны другие заголовки кэша, играющих роль в кэшировании.

| Header                                                     | Функция |
| ---------------------------------------------------------- | -------- |
| [Срок действия](https://tools.ietf.org/html/rfc7234#section-5.1)     | Приблизительное количество времени в секундах с момента ответ был создан или успешно прошли проверку на исходном сервере. |
| [Срок действия истекает](https://tools.ietf.org/html/rfc7234#section-5.3) | Дата и время, после которого ответ считается устаревшей. |
| [Директивы pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Существует для обеспечения обратной совместимости с HTTP/1.0 кэширует параметр `no-cache` поведение. Если `Cache-Control` заголовок присутствует `Pragma` заголовок учитывается. |
| [Различаются](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Указывает, что кэшированный ответ должен не отправляться, если не все из `Vary` поля заголовка совпадает в исходном запросе кэшированный ответ и новый запрос. |

## <a name="http-based-caching-respects-request-cache-control-directives"></a>На основе HTTP кэширования отношениях запрос директивы управления кэшем

[Кэширования HTTP 1.1 спецификации заголовок Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) требуется соблюдать является допустимым для кэша `Cache-Control` заголовок, отправленных клиентом. Клиент может выполнять запросы с `no-cache` значение заголовка и force которого сервер создает новый ответ для каждого запроса.

Учитывая клиента всегда `Cache-Control` заголовки запроса имеет смысл при желании цель кэширования HTTP. В разделе официальная спецификация кэширования предназначен для сократить задержку и сети расходы на удовлетворяющие запросы клиентов, прокси-серверов и серверов в сети. Это не обязательно для контроля нагрузки на исходном сервере.

Отсутствует текущий контроль разработчика над этот режим кэширования при использовании [по промежуточного слоя кэширования ответа](xref:performance/caching/middleware) так, как по промежуточного слоя следует должностное лицо, кэширование спецификации. [Будущих улучшений по промежуточного слоя](https://github.com/aspnet/ResponseCaching/issues/96) разрешает настройки по промежуточного слоя, чтобы игнорировать запрос на `Cache-Control` заголовок при принятии решения о обслуживать кэшированный ответ. Это предложит вам возможность для улучшения контроля нагрузки на сервере при использовании по промежуточного слоя.

## <a name="other-caching-technology-in-aspnet-core"></a>Другие технологии кэширования в ASP.NET Core

### <a name="in-memory-caching"></a>Кэширование в памяти

Кэширование в памяти использует память сервера для хранения кэшированных данных. Этот тип кэширования подходит для одного или нескольких серверов с использованием *прикрепленные сеансы*. Прикрепленные сеансы означает, что запросы от клиента всегда направляются на один и тот же сервер для обработки.

Дополнительные сведения см. в разделе [кэша в памяти](xref:performance/caching/memory).

### <a name="distributed-cache"></a>Распределенного кэша

Используйте распределенный кэш для хранения данных в памяти, когда приложение размещается в облаке или сервер фермы. Кэш распределяется по всем серверам, которые обрабатывают запросы. Клиент может отправить запрос, обрабатываемый любой сервер в группе, при наличии кэшированных данных для клиента. ASP.NET Core предлагает SQL Server и Redis распределенного кэша.

Дополнительные сведения см. в разделе [работать с распределенным кэшем](xref:performance/caching/distributed).

### <a name="cache-tag-helper"></a>Вспомогательный тег кэша

Можно кэшировать содержимое из представления MVC или страница Razor со вспомогательным методом тег кэша. Вспомогательный тег кэша использует кэширование в памяти для хранения данных.

Дополнительные сведения см. в разделе [вспомогательный тег кэша в ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).

### <a name="distributed-cache-tag-helper"></a>Вспомогательная функция тега распределенного кэша

Можно кэшировать содержимое из страницы Razor в распределенных облачных или в сценариях веб-ферм или представление MVC со вспомогательным методом тег распределенного кэша. Вспомогательный распределенного кэша тег использует SQL Server или Redis для хранения данных.

Дополнительные сведения см. в разделе [распределенного кэша тег вспомогательный](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).

## <a name="responsecache-attribute"></a>Атрибут ResponseCache

[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) указывает параметры, необходимые для установки соответствующие заголовки кэширование ответов.

> [!WARNING]
> Отключение кэширования для содержимого, которое содержит сведения о прошедших проверку подлинности клиентов. Кэширование должны включаться только для содержимого, которое не меняется на основе удостоверения пользователя или ли пользователь выполнил вход.

[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) хранимых ответа зависит от значения из заданного списка ключей запроса. Если отдельное значение `*` не предоставлен, зависит от по промежуточного слоя ответов для всех запросов параметров строки запроса. `VaryByQueryKeys` требуется ASP.NET Core 1.1 или более поздней версии.

По промежуточного слоя кэширования ответа необходимо включить, чтобы задать `VaryByQueryKeys` свойства; в противном случае создается исключение времени выполнения. Нет соответствующего заголовка HTTP для `VaryByQueryKeys` свойства. Свойство является функцией HTTP, обрабатываемых по промежуточного слоя кэширования ответа. По промежуточного слоя обслуживать кэшированный ответ строка запроса и значение строки запроса должно соответствовать предыдущего запроса. Например рассмотрим последовательности запросов и результаты, показанные в следующей таблице.

| Запрос                          | Результат                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | Возвращенный от сервера     |
| `http://example.com?key1=value1` | Возвращаемые по промежуточного слоя |
| `http://example.com?key1=value2` | Возвращенный от сервера     |

Первый запрос возвращается сервером и кэшируются в по промежуточного слоя. Второй запрос возвращается по промежуточного слоя, так как предыдущий запрос соответствует строке запроса. Третий запрос не в кэше по промежуточного слоя, так как значение строки запроса не соответствует предыдущего запроса. 

`ResponseCacheAttribute` Используется для настройки и создания (через `IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter). `ResponseCacheFilter` Выполняет обновление компонентов ответа и соответствующие заголовки HTTP. Фильтр:

* Удаляет все существующие заголовки для `Vary`, `Cache-Control`, и `Pragma`. 
* Записывает соответствующие заголовки, в зависимости от свойств, заданных `ResponseCacheAttribute`. 
* Обновляет ответ HTTP функции кэширования, если `VaryByQueryKeys` имеет значение.

### <a name="vary"></a>Различаются

Этот заголовок записывается только когда `VaryByHeader` свойству. Ему присваивается `Vary` значение свойства. В следующем примере используется `VaryByHeader` свойство:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

Можно просматривать заголовки ответа с помощью средства сетевой вашего браузера. На следующем рисунке показаны границы F12, выведенных в **сети** вкладке при `About2` обновляется метода действия:

![Ребро F12 выходные данные на вкладке «сети», при вызове метода действия About2](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a>NoStore и Location.None

`NoStore` переопределяет большинство других свойств. Если значение этого свойства `true`, `Cache-Control` заголовок имеет значение `no-store`. Если `Location` равно `None`:

* Параметру `Cache-Control` задается значение `no-store,no-cache`.
* Параметру `Pragma` задается значение `no-cache`.

Если `NoStore` — `false` и `Location` — `None`, `Cache-Control` и `Pragma` присваиваются `no-cache`.

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

Чтобы включить кэширование, `Duration` должно быть присвоено положительное значение и `Location` должно равняться `Any` (по умолчанию) или `Client`. В этом случае `Cache-Control` заголовок присвоено значение расположения, за которым следует `max-age` ответа.

> [!NOTE]
> `Location`в параметры `Any` и `Client` переводиться `Cache-Control` значения заголовка `public` и `private`соответственно. Как отмечалось ранее, параметр `Location` для `None` задает оба `Cache-Control` и `Pragma` заголовки `no-cache`.

Ниже приведен пример заголовки создается, задав `Duration` и оставить значение по умолчанию `Location` значение:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

Получается следующий заголовок.

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a>Профили кэша

Вместо повторения `ResponseCache` параметров на множество атрибутов действия контроллера, профилей кэша можно настроить как параметры, при настройке MVC в `ConfigureServices` метод в `Startup`. Значения в конфигурации кэша, на которую указывает ссылка, используются значения по умолчанию, `ResponseCache` атрибута и переопределяются все свойства, указанные в атрибуте.

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

`ResponseCache` Атрибут может применяться к действия (методы) и контроллеры (классы). Атрибуты на уровне метода переопределяют параметры, указанные в атрибуты уровня класса.

В приведенном выше примере атрибут уровня класса указывает в течение 30 секунд, пока атрибут уровня метод ссылается на профиль кэша с длительностью 60 секунд.

Полученный заголовок:

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Хранение ответы в кэше](https://tools.ietf.org/html/rfc7234#section-3)
* [Cache-Control](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Кэш в памяти](xref:performance/caching/memory)
* [Работа с распределенным кэшем](xref:performance/caching/distributed)
* [Обнаружение изменений с помощью маркеров изменений](xref:fundamentals/primitives/change-tokens)
* [ПО промежуточного слоя для кэширования ответов](xref:performance/caching/middleware)
* [Вспомогательная функция тега кэша](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Вспомогательная функция тега распределенного кэша](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
