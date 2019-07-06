---
title: По промежуточного слоя в ASP.NET Core кэширования ответов
author: guardrex
description: Узнайте, как настроить и использовать ПО промежуточного слоя для кэширование ответов в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/05/2019
uid: performance/caching/middleware
ms.openlocfilehash: d6756ce16396133da643cc08ca0f48369479ad3a
ms.sourcegitcommit: b9e914ef274b5ec359582f299724af6234dce135
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67596155"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>По промежуточного слоя в ASP.NET Core кэширования ответов

По [Люк Лэтем](https://github.com/guardrex) и [Джон Luo](https://github.com/JunTaoLuo)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([как скачивать](xref:index#how-to-download-a-sample))

В этой статье описывается настройка по промежуточного слоя для кэширования ответа в приложении ASP.NET Core. По промежуточного слоя определяет, когда кэшируемых ответов, ответы на магазины и служит ответы из кэша. Общие сведения о HTTP-кэширования и [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) атрибут, см. в разделе [кэширование ответов](xref:performance/caching/response).

## <a name="configuration"></a>Параметр Configuration

Используйте [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) пакета.

В `Startup.ConfigureServices`, добавьте по промежуточного слоя кэширования ответа службы коллекции:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Настройка приложения для использования по промежуточного слоя с <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> метод расширения, который добавляет по промежуточного слоя в конвейер обработки запросов в `Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

Пример приложения добавляет заголовки для управления кэшированием при последующих запросах:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; кэширует ответы кэшируемые до 10 секунд.
* [Различаются](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; настраивает по промежуточного слоя для обслуживания только если кэшированный ответ [ `Accept-Encoding` ](https://tools.ietf.org/html/rfc7231#section-5.3.4) заголовок последующих запросов совпадает с исходного запроса.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

По промежуточного слоя, кэширование ответа только кэширует ответы сервера, которые приводят к код состояния 200 (ОК). Любые другие ответы, включая [страницы ошибок](xref:fundamentals/error-handling), учитываются по промежуточного слоя.

> [!WARNING]
> Ответов, содержащий содержимое для прошедших проверку подлинности клиентов должен быть помечен как некэшируемый во избежание по промежуточного слоя, хранение и обслуживание этих ответов. См. в разделе [условий для кэширования](#conditions-for-caching) Дополнительные сведения о том, как по промежуточного слоя определяет, является ли кэшируемый ответ.

## <a name="options"></a>Параметры

В следующей таблице показаны параметры кэширования ответа.

| Параметр | Описание |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Максимальный размер кэшируемого текст ответа в байтах. Значение по умолчанию — `64 * 1024 * 1024` (64 МБ). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Предельный размер, по промежуточного слоя кэш ответа в байтах. Значение по умолчанию — `100 * 1024 * 1024` (100 МБ). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Определяет, если ответы кэшируются на пути с учетом регистра. Значение по умолчанию — `false`. |

В следующем примере настраивается по промежуточного слоя для:

* Кэшировать ответы размер текста, меньше или равно 1024 байта.
* Store ответы по пути, с учетом регистра. Например `/page1` и `/Page1` хранятся отдельно.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>VaryByQueryKeys

При использовании MVC или веб-API контроллерах или моделях страниц Razor Pages, [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) атрибут задает параметры, необходимые для настройки соответствующие заголовки для кэширования ответов. Единственным параметром `[ResponseCache]` атрибут, строго требует по промежуточного слоя — <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, которые не соответствуют фактический заголовок HTTP. Дополнительные сведения см. в разделе <xref:performance/caching/response#responsecache-attribute>.

Если не используется `[ResponseCache]` атрибут, кэширование ответов можно изменять с помощью `VaryByQueryKeys`. Используйте <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> непосредственно из [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

С помощью одного значением, равным `*` в `VaryByQueryKeys` зависит от кэша все параметры запроса для запроса.

## <a name="http-headers-used-by-response-caching-middleware"></a>Заголовки HTTP, используемые по промежуточного слоя для кэширования ответа

Ниже приведены сведения по заголовкам HTTP, которые влияют на кэширование ответов.

| Header | Подробные сведения |
| ------ | ------- |
| `Authorization` | Ответ не кэшируется, если заголовок существует. |
| `Cache-Control` | По промежуточного слоя рассматривает только кэширования ответов, отмеченные `public` директива кэша. Управлять кэшированием со следующими параметрами:<ul><li>max-age</li><li>max-stale&#8224;</li><li>Min новые</li><li>должен revalidate</li><li>без кэша</li><li>Нет-store</li><li>только if-cached</li><li>private</li><li>public</li><li>s-maxage</li><li>Proxy-revalidate&#8225;</li></ul>&#8224;Если задано неограниченное `max-stale`, по промежуточного слоя не предпринимает никаких действий.<br>&#8225;`proxy-revalidate`имеет тот же эффект, что `must-revalidate`.<br><br>Дополнительные сведения см. в разделе [RFC 7231: Запросить директивы управления кэшем](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | Объект `Pragma: no-cache` заголовка в запросе дает тот же эффект, как `Cache-Control: no-cache`. Этот заголовок переопределяется соответствующие директив `Cache-Control` заголовка, если он имеется. Учитывать для обеспечения обратной совместимости с HTTP/1.0. |
| `Set-Cookie` | Ответ не кэшируется, если заголовок существует. Любое по промежуточного слоя в конвейере обработки запросов, который задает один или несколько файлов cookie предотвращает кэширование ответа по промежуточного слоя кэширования ответа (например, [поставщик TempData на основе файлов cookie](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | `Vary` Заголовок используется другой заголовок для изменения кэшированного ответа. Например, при кодировании, включив могут кэшировать ответы `Vary: Accept-Encoding` заголовок, который кэширует ответы для запросов с заголовками `Accept-Encoding: gzip` и `Accept-Encoding: text/plain` отдельно. Ответ со значением заголовка `*` никогда не хранится. |
| `Expires` | Считается устаревшей, этот заголовок ответа не сохраняемый или извлекаемый, если это не переопределено другим `Cache-Control` заголовки. |
| `If-None-Match` | Полный ответ обрабатывается из кэша, если значение не `*` и `ETag` ответа не совпадает с указанными значениями. В противном случае выдается ответ 304 (не изменено). |
| `If-Modified-Since` | Если `If-None-Match` заголовок отсутствует, полный ответ обрабатывается из кэша, если новее, чем значение, предоставленное Дата кэшированного ответа. В противном случае *304 - не изменено* ответа. |
| `Date` | При выполнении из кэша, `Date` заголовок имеет значение по промежуточного слоя, если он не был указан в исходном запросе. |
| `Content-Length` | При выполнении из кэша, `Content-Length` заголовок имеет значение по промежуточного слоя, если он не был указан в исходном запросе. |
| `Age` | `Age` Заголовок, отправленный в исходном запросе учитывается. По промежуточного слоя вычисляет новое значение, при выполнении кэшированного ответа. |

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
* В `Startup.Configure`, по промежуточного слоя, кэширование ответов должны предшествовать по промежуточного слоя, который хотите использовать кэширование. Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index>.
* `Authorization` Заголовка не должен присутствовать.
* `Cache-Control` Параметры заголовка должен быть допустимым и должен быть помечен ответ `public` и не помечен как `private`.
* `Pragma: no-cache` Заголовка не должно быть Если `Cache-Control` заголовок файла нет, как `Cache-Control` переопределяет заголовок `Pragma` заголовка, если он присутствует.
* `Set-Cookie` Заголовка не должен присутствовать.
* `Vary` Параметры заголовка должно быть допустимым и не равно `*`.
* `Content-Length` Значение заголовка (если задать) должно соответствовать размеру текста ответа.
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> Не используется.
* Ответ не должно быть устаревшим в соответствии с `Expires` заголовка и `max-age` и `s-maxage` кэшировать директивы.
* Буферизацию ответов должны быть успешными. Размер ответа должен быть меньше, чем настроенное или по умолчанию <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>. Размер тела ответа должен быть меньше, чем настроенное или по умолчанию <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.
* Ответ должен быть кэшируемого в соответствии с [RFC 7234](https://tools.ietf.org/html/rfc7234) спецификации. Например `no-store` директива не должен существовать в полях заголовка запроса или ответа. См. в разделе *раздел 3: Сохранения ответов в кэшах* из [RFC 7234](https://tools.ietf.org/html/rfc7234) подробные сведения.

> [!NOTE]
> Наборы атак против подделки системы для создания маркеров безопасности для предотвращения подделки межсайтовых запросов (CSRF) `Cache-Control` и `Pragma` заголовки `no-cache` таким образом, чтобы ответы не кэшируются. Сведения о том, как отключить против подделки маркеры для элементы HTML-формы, см. в разделе <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
