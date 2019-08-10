---
title: Кэширование ответа по промежуточного слоя в ASP.NET Core
author: guardrex
description: Узнайте, как настроить и использовать ПО промежуточного слоя для кэширование ответов в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/08/2019
uid: performance/caching/middleware
ms.openlocfilehash: 6371f42b100f70c6042064a6372c7b9e41fd5c73
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68914994"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Кэширование ответа по промежуточного слоя в ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Джон Луо](https://github.com/JunTaoLuo)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([как скачивать](xref:index#how-to-download-a-sample))

В этой статье объясняется, как настроить по промежуточного слоя кэширования ответов в приложении ASP.NET Core. По промежуточного слоя определяет, когда ответы кэшируются, сохраняет ответы и обслуживает ответы из кэша. Общие сведения о кэшировании HTTP и атрибуте [[респонсекаче]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) см. в разделе [кэширование ответов](xref:performance/caching/response).

## <a name="configuration"></a>Параметр Configuration

Используйте [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. AspNetCore. респонсекачинг](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) .

В `Startup.ConfigureServices`добавьте по промежуточного слоя кэширования ответа в коллекцию служб:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

::: moniker-end

Настройте приложение для использования по промежуточного слоя с <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> помощью метода расширения, который добавляет по промежуточного слоя в конвейер обработки `Startup.Configure`запросов в:

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

::: moniker-end

Пример приложения добавляет заголовки для управления кэшированием последующих запросов:

* [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Кэширует ответы в кэше до 10 секунд.
* [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) Настраивает по промежуточного слоя для обслуживания кэшированного ответа только [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) в том случае, если заголовок последующих запросов совпадает с заголовком исходного запроса. &ndash;

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

::: moniker-end

По промежуточного слоя кэширования ответов кэширует только ответы сервера, приводящие к коду состояния 200 (ОК). Любые другие ответы, включая [страницы ошибок](xref:fundamentals/error-handling), по промежуточного слоя игнорируются.

> [!WARNING]
> Ответы, содержащие содержимое для прошедших проверку клиентов, должны быть помечены как недоступные для кэширования, чтобы предотвратить хранение и обслуживание этих ответов по промежуточного слоя. Сведения о том, как по промежуточного слоя определяет, является ли ответ кэшированным, см. в разделе [условия кэширования](#conditions-for-caching) .

## <a name="options"></a>Параметры

Параметры кэширования ответов приведены в следующей таблице.

| Параметр | Описание |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Самый крупный размер кэша для текста ответа в байтах. Значение по умолчанию `64 * 1024 * 1024` — (64 МБ). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Предельный размер для по промежуточного слоя кэша ответов в байтах. Значение по умолчанию `100 * 1024 * 1024` — (100 МБ). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Определяет, кэшируются ли ответы в путях с учетом регистра. Значение по умолчанию — `false`. |

В следующем примере по промежуточного слоя настраивается:

* Ответы кэша с размером текста меньше или равным 1 024 байт.
* Храните ответы по путям с учетом регистра. Например, `/page1` и `/Page1` хранятся отдельно.

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a>варибикуерикэйс

При использовании контроллеров MVC, веб-API или Razor Pagesных моделей страниц атрибут [[респонсекаче]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) задает параметры, необходимые для установки соответствующих заголовков для кэширования ответов. Единственный параметр `[ResponseCache]` атрибута, для которого строго требуется по <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>промежуточного слоя, не соответствует фактическому заголовку HTTP. Дополнительные сведения см. в разделе <xref:performance/caching/response#responsecache-attribute>.

Если `[ResponseCache]` атрибут не используется, кэширование ответов можно изменять с помощью `VaryByQueryKeys`. Используйте непосредственно из [функции HttpContext. Features:](xref:Microsoft.AspNetCore.Http.HttpContext.Features) <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

При использовании одного значения, `*` равного `VaryByQueryKeys` , аргумент Cache изменяется в зависимости от всех параметров запроса запроса.

## <a name="http-headers-used-by-response-caching-middleware"></a>Заголовки HTTP, используемые по промежуточного слоя кэширования ответов

В следующей таблице приведены сведения о заголовках HTTP, влияющих на кэширование ответов.

| Header | Подробные сведения |
| ------ | ------- |
| `Authorization` | Ответ не кэшируется, если заголовок существует. |
| `Cache-Control` | По промежуточного слоя рассматривает только ответы кэширования, отмеченные `public` директивой Cache. Управление кэшированием со следующими параметрами:<ul><li>максимальный возраст</li><li>максимум — устаревший&#8224;</li><li>min-свежая</li><li>необходимо — повторно проверить</li><li>без кэша</li><li>без магазина</li><li>только в случае кэширования</li><li>private</li><li>public</li><li>s-maxage</li><li>прокси — повторная проверка&#8225;</li></ul>&#8224;Если ограничение не задано `max-stale`, по промежуточного слоя не выполняет никаких действий.<br>&#8225;`proxy-revalidate`имеет тот же результат, `must-revalidate`что и.<br><br>Дополнительные сведения см [. в RFC 7231: Запрос директив](https://tools.ietf.org/html/rfc7234#section-5.2.1)управления Cache-Control. |
| `Pragma` | Заголовок в запросе дает тот же результат, что `Cache-Control: no-cache`и. `Pragma: no-cache` Этот заголовок переопределяется соответствующими директивами в `Cache-Control` заголовке, если он имеется. Рассматривается для обеспечения обратной совместимости с HTTP/1.0. |
| `Set-Cookie` | Ответ не кэшируется, если заголовок существует. Любое по промежуточного слоя в конвейере обработки запросов, которое задает один или несколько файлов cookie, предотвращает кэширование ответа по промежуточного слоя (например, [поставщик TempData на основе файлов cookie](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | `Vary` Заголовок используется для изменения кэшированного ответа другим заголовком. Например, ответы кэшируются по кодировке путем включения `Vary: Accept-Encoding` заголовка, который кэширует ответы для запросов с заголовками `Accept-Encoding: gzip` и `Accept-Encoding: text/plain` по отдельности. Ответ со значением заголовка объекта `*` никогда не сохраняется. |
| `Expires` | Ответ, который считается устаревшим по этому заголовку, не сохраняется или не извлекается, если он не переопределен другими `Cache-Control` заголовками. |
| `If-None-Match` | Полный ответ обрабатывается из кэша, если значение не `*` равно, а ответ не совпадает ни с `ETag` одним из указанных значений. В противном случае выдается ответ 304 (не изменено). |
| `If-Modified-Since` | `If-None-Match` Если заголовок отсутствует, полный ответ передается из кэша, если значение кэшированной даты ответа новее заданного значения. В противном случае обслуживается ответ *304 — не изменено* . |
| `Date` | При обслуживании из кэша `Date` заголовок задается по промежуточного слоя, если оно не было указано в исходном ответе. |
| `Content-Length` | При обслуживании из кэша `Content-Length` заголовок задается по промежуточного слоя, если оно не было указано в исходном ответе. |
| `Age` | `Age` Заголовок, отправленный в исходном ответе, игнорируется. По промежуточного слоя выполняет вычисление нового значения при обработке кэшированного ответа. |

## <a name="caching-respects-request-cache-control-directives"></a>Кэширование директив запроса Cache-Control

По промежуточного слоя учитывает правила [спецификации кэширования HTTP 1,1](https://tools.ietf.org/html/rfc7234#section-5.2). Для правил требуется, чтобы кэш учитывал допустимый `Cache-Control` заголовок, отправленный клиентом. В соответствии со спецификацией клиент может выполнять запросы со `no-cache` значением заголовка и принудительно создавать новый ответ для каждого запроса. В настоящее время разработчик не управляет этим поведением кэширования при использовании по промежуточного слоя, поскольку по промежуточного слоя соответствует официальной спецификации кэширования.

Чтобы получить более полный контроль над поведением кэширования, изучите другие функции кэширования ASP.NET Core. См. указанные ниже разделы.

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Устранение неполадок

Если поведение кэширования не так, как ожидалось, убедитесь, что ответы кэшируются и могут обрабатываться из кэша. Изучите Входящие заголовки запроса и исходящие заголовки ответа. Включите [ведение журнала](xref:fundamentals/logging/index) для помощи при отладке.

При тестировании и устранении неполадок кэширования браузер может задавать заголовки запросов, которые влияют на кэширование нежелательным образом. Например, браузер может задать `Cache-Control` для `no-cache` заголовка или `max-age=0` при обновлении страницы. Следующие средства могут явно задавать заголовки запроса и являются предпочтительными для тестирования кэширования.

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Условия для кэширования

* Запрос должен привести к отклику сервера с кодом состояния 200 (ОК).
* Метод запроса должен быть GET или HEAD.
* В `Startup.Configure`по промежуточного слоя, для которого требуется кэширование, должно быть помещено в состояние кэширования ответа. Дополнительные сведения см. в разделе <xref:fundamentals/middleware/index>.
* `Authorization` Заголовок не должен присутствовать.
* `Cache-Control`параметры заголовка должны быть допустимыми, и ответ должен быть помечен `public` и не помечен `private`.
* Заголовок не должен присутствовать, `Cache-Control` если заголовок отсутствует, `Pragma` так как `Cache-Control` заголовок переопределяет заголовок при его наличии. `Pragma: no-cache`
* `Set-Cookie` Заголовок не должен присутствовать.
* `Vary`параметры заголовка должны быть допустимыми и не равны `*`.
* Значение `Content-Length` заголовка (если оно задано) должно соответствовать размеру текста ответа.
* Объект <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> не используется.
* Ответ не должен быть устаревшим, как указано в `Expires` заголовках `max-age` и директивах кэша и `s-maxage` .
* Буферизация ответов должна быть успешной. Размер ответа должен быть меньше заданного значения или по умолчанию <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>. Размер текста ответа должен быть меньше заданного значения или по умолчанию <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.
* Ответ должен быть кэширован согласно спецификациям [RFC 7234](https://tools.ietf.org/html/rfc7234) . Например, `no-store` директива не должна существовать в полях заголовка запроса или ответа. См *. раздел 3. Хранение ответов в кэшах* [RFC 7234](https://tools.ietf.org/html/rfc7234) для получения дополнительных сведений.

> [!NOTE]
> Система защиты от подделки для создания безопасных маркеров для предотвращения подделки межсайтовых запросов (CSRF) устанавливает `Cache-Control` заголовки и `Pragma` так, чтобы `no-cache` ответы не кэшируются. Сведения об отключении маркеров подделки для элементов HTML-форм см. в разделе <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
