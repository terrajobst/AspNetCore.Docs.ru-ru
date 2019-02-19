---
title: По промежуточного слоя в ASP.NET Core кэширования ответов
author: guardrex
description: Узнайте, как настроить и использовать ПО промежуточного слоя для кэширование ответов в ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/16/2019
uid: performance/caching/middleware
ms.openlocfilehash: bb265d04022ec2f8fdb3f2f3bc42f6b3f0b2b338
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410327"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>По промежуточного слоя в ASP.NET Core кэширования ответов

По [Люк Лэтем](https://github.com/guardrex) и [Джон Luo](https://github.com/JunTaoLuo)

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).

В этой статье описывается настройка по промежуточного слоя для кэширования ответа в приложении ASP.NET Core. По промежуточного слоя определяет, когда кэшируемых ответов, ответы на магазины и служит ответы из кэша. Общие сведения о HTTP-кэширования и `ResponseCache` атрибут, см. в разделе [кэширование ответов](xref:performance/caching/response).

## <a name="package"></a>Пакет

::: moniker range=">= aspnetcore-2.1"

Справочник по [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) пакета.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Справочник по [метапакет Microsoft.AspNetCore.All](xref:fundamentals/metapackage) или добавьте ссылку на пакет [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) пакета.

::: moniker-end

::: moniker range="= aspnetcore-1.1"

Добавьте ссылку на пакет [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) пакета.

::: moniker-end

## <a name="configuration"></a>Параметр Configuration

В `Startup.ConfigureServices`, добавьте по промежуточного слоя в коллекцию служб.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=9)]

Настройка приложения для использования по промежуточного слоя с `UseResponseCaching` метод расширения, который добавляет по промежуточного слоя в конвейере обработки запросов. Пример приложения добавляет [ `Cache-Control` ](https://tools.ietf.org/html/rfc7234#section-5.2) заголовка в ответ, который кэширует ответы кэшируемые до 10 секунд. Пример отправляет [ `Vary` ](https://tools.ietf.org/html/rfc7231#section-7.1.4) заголовка для настройки по промежуточного слоя для обслуживания только если кэшированный ответ [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) заголовок последующих запросов совпадает с исходного запроса. В следующем примере кода [CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) и [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) требуют `using` инструкции для [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) пространство имен.

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17,22-29)]

По промежуточного слоя, кэширование ответа только кэширует ответы сервера, которые приводят к код состояния 200 (ОК). Любые другие ответы, включая [страницы ошибок](xref:fundamentals/error-handling), учитываются по промежуточного слоя.

> [!WARNING]
> Ответов, содержащий содержимое для прошедших проверку подлинности клиентов должен быть помечен как некэшируемый во избежание по промежуточного слоя, хранение и обслуживание этих ответов. См. в разделе [условий для кэширования](#conditions-for-caching) Дополнительные сведения о том, как по промежуточного слоя определяет, является ли кэшируемый ответ.

## <a name="options"></a>Параметры

По промежуточного слоя предлагает три варианта управления кэширование ответов.

| Параметр                | Описание: |
| --------------------- | ----------- |
| UseCaseSensitivePaths | Определяет, если ответы кэшируются на пути с учетом регистра. Значение по умолчанию — `false`. |
| MaximumBodySize       | Максимальный размер кэшируемого текст ответа в байтах. Значение по умолчанию — `64 * 1024 * 1024` (64 МБ). |
| SizeLimit             | Предельный размер, по промежуточного слоя кэш ответа в байтах. Значение по умолчанию — `100 * 1024 * 1024` (100 МБ). |

В следующем примере настраивается по промежуточного слоя для:

* Кэшировать ответы, меньше или равно 1024 байта.
* Store ответы по пути, с учетом регистра (например, `/page1` и `/Page1` хранятся отдельно).

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

При использовании контроллеров MVC или веб-API или моделях страниц Razor Pages, `ResponseCache` атрибут задает параметры, необходимые для настройки соответствующие заголовки для кэширования ответов. Единственным параметром `ResponseCache` атрибут, строго требует по промежуточного слоя — `VaryByQueryKeys`, которые не соответствуют фактический заголовок HTTP. Дополнительные сведения см. в разделе [атрибута ResponseCache](xref:performance/caching/response#responsecache-attribute).

Если не используется `ResponseCache` атрибут, кэширование ответов может изменяться с `VaryByQueryKeys` функции. Используйте `ResponseCachingFeature` непосредственно из `IFeatureCollection` из `HttpContext`:

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

С помощью одного значением, равным `*` в `VaryByQueryKeys` зависит от кэша все параметры запроса для запроса.

## <a name="http-headers-used-by-response-caching-middleware"></a>Заголовки HTTP, используемые по промежуточного слоя для кэширования ответа

Кэширование ответов по промежуточного слоя настраивается с помощью заголовков HTTP.

| Header | Подробные сведения |
| ------ | ------- |
| Авторизация | Ответ не кэшируется, если заголовок существует. |
| Cache-Control | По промежуточного слоя рассматривает только кэширования ответов, отмеченные `public` директива кэша. Управлять кэшированием со следующими параметрами:<ul><li>max-age</li><li>max-stale&#8224;</li><li>Min новые</li><li>должен revalidate</li><li>без кэша</li><li>Нет-store</li><li>только if-cached</li><li>private</li><li>public</li><li>s-maxage</li><li>Proxy-revalidate&#8225;</li></ul>&#8224;Если задано неограниченное `max-stale`, по промежуточного слоя не предпринимает никаких действий.<br>&#8225;`proxy-revalidate`имеет тот же эффект, что `must-revalidate`.<br><br>Дополнительные сведения см. в разделе [RFC 7231: Запросить директивы управления кэшем](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| Директивы pragma | Объект `Pragma: no-cache` заголовка в запросе дает тот же эффект, как `Cache-Control: no-cache`. Этот заголовок переопределяется соответствующие директив `Cache-Control` заголовка, если он имеется. Учитывать для обеспечения обратной совместимости с HTTP/1.0. |
| Set-Cookie | Ответ не кэшируется, если заголовок существует. Любое по промежуточного слоя в конвейере обработки запросов, который задает один или несколько файлов cookie предотвращает кэширование ответа по промежуточного слоя кэширования ответа (например, [поставщик TempData на основе файлов cookie](xref:fundamentals/app-state#tempdata)).  |
| Различаются | `Vary` Заголовок используется другой заголовок для изменения кэшированного ответа. Например, при кодировании, включив могут кэшировать ответы `Vary: Accept-Encoding` заголовок, который кэширует ответы для запросов с заголовками `Accept-Encoding: gzip` и `Accept-Encoding: text/plain` отдельно. Ответ со значением заголовка `*` никогда не хранится. |
| Срок действия истекает | Считается устаревшей, этот заголовок ответа не сохраняемый или извлекаемый, если это не переопределено другим `Cache-Control` заголовки. |
| If-None-Match | Полный ответ обрабатывается из кэша, если значение не `*` и `ETag` ответа не совпадает с указанными значениями. В противном случае выдается ответ 304 (не изменено). |
| If-Modified-Since | Если `If-None-Match` заголовок отсутствует, полный ответ обрабатывается из кэша, если новее, чем значение, предоставленное Дата кэшированного ответа. В противном случае выдается ответ 304 (не изменено). |
| Дата | При выполнении из кэша, `Date` заголовок имеет значение по промежуточного слоя, если он не был указан в исходном запросе. |
| Content-Length | При выполнении из кэша, `Content-Length` заголовок имеет значение по промежуточного слоя, если он не был указан в исходном запросе. |
| Срок действия | `Age` Заголовок, отправленный в исходном запросе учитывается. По промежуточного слоя вычисляет новое значение, при выполнении кэшированного ответа. |

## <a name="caching-respects-request-cache-control-directives"></a>Кэширование учитывает директивы запрос Cache-Control

По промежуточного слоя подчиняется правилам [спецификации кэширования HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2). Правила требуется кэш соблюдать является допустимым для `Cache-Control` заголовка, отправленные клиентом. В разделе спецификации, клиент может выполнять запросы с `no-cache` значение заголовка и принудительного создания нового ответа для каждого запроса сервером. В настоящее время нет не контроль разработчика над это поведение кэширования, при использовании по промежуточного слоя, так как по промежуточного слоя, соответствует официальной спецификации кэширования.

Для большего контроля над поведением кэширования Изучите другие функции кэширования ASP.NET Core. См. указанные ниже разделы.

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Устранение неполадок

Если поведение кэширования не должным образом, убедитесь, что ответы кэшируемого, так и может обслуживать из кэша. Изучите заголовки входящего запроса и ответа Исходящие заголовки. Включить [ведение журнала](xref:fundamentals/logging/index) для отладки.

При тестировании и устранении неполадок поведение кэширования, браузер может задать заголовки запросов, затрагивающих кэширование негативно. Например, браузер может задать `Cache-Control` заголовок `no-cache` или `max-age=0` при обновлении страницы. Следующие средства можно явно задать заголовки запроса и являются предпочтительными для тестирования кэширования:

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Условия для кэширования

* Запрос должен иметь результаты в ответе сервера с кодом состояния 200 (ОК).
* Метод запроса должен быть GET или HEAD.
* Терминалов по промежуточного слоя не должен обработать ответ до по промежуточного слоя кэширования ответов.
* `Authorization` Заголовка не должен присутствовать.
* `Cache-Control` Параметры заголовка должен быть допустимым и должен быть помечен ответ `public` и не помечен как `private`.
* `Pragma: no-cache` Заголовка не должно быть Если `Cache-Control` заголовок файла нет, как `Cache-Control` переопределяет заголовок `Pragma` заголовка, если он присутствует.
* `Set-Cookie` Заголовка не должен присутствовать.
* `Vary` Параметры заголовка должно быть допустимым и не равно `*`.
* `Content-Length` Значение заголовка (если задать) должно соответствовать размеру текста ответа.
* [IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) не используется.
* Ответ не должно быть устаревшим в соответствии с `Expires` заголовка и `max-age` и `s-maxage` кэшировать директивы.
* Буферизацию ответов должны быть успешными и размер ответа должен быть меньше, чем настроенное или по умолчанию `SizeLimit`.
* Ответ должен быть кэшируемого в соответствии с [RFC 7234](https://tools.ietf.org/html/rfc7234) спецификации. Например `no-store` директива не должен существовать в полях заголовка запроса или ответа. См. в разделе *раздел 3: Сохранения ответов в кэшах* из [RFC 7234](https://tools.ietf.org/html/rfc7234) подробные сведения.

> [!NOTE]
> Наборы атак против подделки системы для создания маркеров безопасности для предотвращения подделки межсайтовых запросов (CSRF) `Cache-Control` и `Pragma` заголовки `no-cache` таким образом, чтобы ответы не кэшируются. Сведения о том, как отключить против подделки маркеры для элементы HTML-формы, см. в разделе [против подделки конфигурации ASP.NET Core](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
