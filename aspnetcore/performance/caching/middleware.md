---
title: Кэширование ответа по промежуточного слоя в ASP.NET Core
author: guardrex
description: Узнайте, как настроить и использовать ПО промежуточного слоя для кэширование ответов в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: performance/caching/middleware
ms.openlocfilehash: d034252f69f8efdc9a912a0d9c3ecde65196e7e3
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880935"
---
# <a name="response-caching-middleware-in-aspnet-core"></a>Кэширование ответа по промежуточного слоя в ASP.NET Core

[Люк ЛаСаМ](https://github.com/guardrex) и [Джон Луо](https://github.com/JunTaoLuo)

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([как скачивать](xref:index#how-to-download-a-sample))

В этой статье объясняется, как настроить по промежуточного слоя кэширования ответов в приложении ASP.NET Core. По промежуточного слоя определяет, когда ответы кэшируются, сохраняет ответы и обслуживает ответы из кэша. Общие сведения о кэшировании HTTP и атрибуте [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) см. в разделе [кэширование ответов](xref:performance/caching/response).

## <a name="configuration"></a>Конфигурация

::: moniker range=">= aspnetcore-3.0"

По промежуточного слоя кэширования ответов неявным образом доступно для ASP.NET Core приложений через общую платформу.

В `Startup.ConfigureServices`добавьте по промежуточного слоя кэширования ответа в коллекцию служб:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Настройте приложение для использования по промежуточного слоя с помощью метода расширения <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*>, который добавляет по промежуточного слоя в конвейер обработки запросов в `Startup.Configure`:

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

Пример приложения добавляет заголовки для управления кэшированием последующих запросов:

* Cache [-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; кэширует ответы в кэше до 10 секунд.
* [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; настраивает по промежуточного слоя для обработки кэшированного ответа только в том случае, если заголовок [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) последующих запросов соответствует исходному запросу.

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

По промежуточного слоя кэширования ответов кэширует только ответы сервера, приводящие к коду состояния 200 (ОК). Любые другие ответы, включая [страницы ошибок](xref:fundamentals/error-handling), по промежуточного слоя игнорируются.

> [!WARNING]
> Ответы, содержащие содержимое для прошедших проверку клиентов, должны быть помечены как недоступные для кэширования, чтобы предотвратить хранение и обслуживание этих ответов по промежуточного слоя. Сведения о том, как по промежуточного слоя определяет, является ли ответ кэшированным, см. в разделе [условия кэширования](#conditions-for-caching) .

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Используйте [метапакет Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет [Microsoft. AspNetCore. респонсекачинг](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) .

В `Startup.ConfigureServices`добавьте по промежуточного слоя кэширования ответа в коллекцию служб:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

Настройте приложение для использования по промежуточного слоя с помощью метода расширения <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*>, который добавляет по промежуточного слоя в конвейер обработки запросов в `Startup.Configure`:

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

Пример приложения добавляет заголовки для управления кэшированием последующих запросов:

* Cache [-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; кэширует ответы в кэше до 10 секунд.
* [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; настраивает по промежуточного слоя для обработки кэшированного ответа только в том случае, если заголовок [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) последующих запросов соответствует исходному запросу.

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

По промежуточного слоя кэширования ответов кэширует только ответы сервера, приводящие к коду состояния 200 (ОК). Любые другие ответы, включая [страницы ошибок](xref:fundamentals/error-handling), по промежуточного слоя игнорируются.

> [!WARNING]
> Ответы, содержащие содержимое для прошедших проверку клиентов, должны быть помечены как недоступные для кэширования, чтобы предотвратить хранение и обслуживание этих ответов по промежуточного слоя. Сведения о том, как по промежуточного слоя определяет, является ли ответ кэшированным, см. в разделе [условия кэширования](#conditions-for-caching) .

::: moniker-end

## <a name="options"></a>Options

Параметры кэширования ответов приведены в следующей таблице.

| Параметр | Описание |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | Самый крупный размер кэша для текста ответа в байтах. Значение по умолчанию — `64 * 1024 * 1024` (64 МБ). |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | Предельный размер для по промежуточного слоя кэша ответов в байтах. Значение по умолчанию — `100 * 1024 * 1024` (100 МБ). |
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

При использовании контроллеров MVC, веб-API или моделей страниц Razor Pages, атрибут [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) задает параметры, необходимые для установки соответствующих заголовков для кэширования ответа. Единственным параметром `[ResponseCache]` атрибута, который строго требует по промежуточного слоя, является <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, который не соответствует фактическому заголовку HTTP. Для получения дополнительной информации см. <xref:performance/caching/response#responsecache-attribute>.

Если атрибут `[ResponseCache]` не используется, кэширование ответов можно изменять с помощью `VaryByQueryKeys`. Используйте <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> непосредственно из [функции HttpContext. Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

При использовании одного значения, равного `*` в `VaryByQueryKeys`, кэш изменяется всеми параметрами запроса запроса.

## <a name="http-headers-used-by-response-caching-middleware"></a>Заголовки HTTP, используемые по промежуточного слоя кэширования ответов

В следующей таблице приведены сведения о заголовках HTTP, влияющих на кэширование ответов.

| Header | Подробности |
| ------ | ------- |
| `Authorization` | Ответ не кэшируется, если заголовок существует. |
| `Cache-Control` | По промежуточного слоя рассматривает только ответы кэширования, отмеченные директивой кэша `public`. Управление кэшированием со следующими параметрами:<ul><li>максимальный возраст</li><li>максимум — устаревший&#8224;</li><li>min-свежая</li><li>must-revalidate</li><li>no-cache</li><li>без магазина</li><li>только в случае кэширования</li><li>private</li><li>public</li><li>s-maxage</li><li>прокси — повторная проверка&#8225;</li></ul>&#8224;Если для `max-stale`не указано ограничение, по промежуточного слоя не выполняет никаких действий.<br>&#8225;`proxy-revalidate` действует так же, как `must-revalidate`.<br><br>Дополнительные сведения см. в статье [RFC 7231: запрос директив управления Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2.1). |
| `Pragma` | Заголовок `Pragma: no-cache` в запросе дает тот же результат, что и `Cache-Control: no-cache`. Этот заголовок переопределяется соответствующими директивами в заголовке `Cache-Control`, если он есть. Рассматривается для обеспечения обратной совместимости с HTTP/1.0. |
| `Set-Cookie` | Ответ не кэшируется, если заголовок существует. Любое по промежуточного слоя в конвейере обработки запросов, которое задает один или несколько файлов cookie, предотвращает кэширование ответа по промежуточного слоя (например, [поставщик TempData на основе файлов cookie](xref:fundamentals/app-state#tempdata)).  |
| `Vary` | Заголовок `Vary` используется для изменения кэшированного ответа другим заголовком. Например, ответы кэшируются по кодировке, включая заголовок `Vary: Accept-Encoding`, который кэширует ответы для запросов с заголовками `Accept-Encoding: gzip` и `Accept-Encoding: text/plain` отдельно. Ответ со значением заголовка `*` никогда не сохраняется. |
| `Expires` | Ответ, который считается устаревшим по этому заголовку, не сохраняется или не извлекается, если он не переопределен другими `Cache-Control` заголовками. |
| `If-None-Match` | Полный ответ обрабатывается из кэша, если значение не `*` и `ETag` ответа не соответствует ни одному из указанных значений. В противном случае выдается ответ 304 (не изменено). |
| `If-Modified-Since` | Если заголовок `If-None-Match` отсутствует, полный ответ передается из кэша, если кэшированная Дата ответа новее заданного значения. В противном случае обслуживается ответ *304 — не изменено* . |
| `Date` | При обслуживании из кэша заголовок `Date` задается по промежуточного слоя, если оно не было указано в исходном ответе. |
| `Content-Length` | При обслуживании из кэша заголовок `Content-Length` задается по промежуточного слоя, если оно не было указано в исходном ответе. |
| `Age` | Заголовок `Age`, отправленный в исходном ответе, игнорируется. По промежуточного слоя выполняет вычисление нового значения при обработке кэшированного ответа. |

## <a name="caching-respects-request-cache-control-directives"></a>Кэширование директив запроса Cache-Control

По промежуточного слоя учитывает правила [спецификации кэширования HTTP 1,1](https://tools.ietf.org/html/rfc7234#section-5.2). Для правил требуется, чтобы кэш учитывал допустимый заголовок `Cache-Control`, отправленный клиентом. В соответствии со спецификацией клиент может выполнять запросы со значением заголовка `no-cache` и заставить сервер создавать новый ответ для каждого запроса. В настоящее время разработчик не управляет этим поведением кэширования при использовании по промежуточного слоя, поскольку по промежуточного слоя соответствует официальной спецификации кэширования.

Чтобы получить более полный контроль над поведением кэширования, изучите другие функции кэширования ASP.NET Core. См. следующие разделы:

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a>Диагностика

Если поведение кэширования не так, как ожидалось, убедитесь, что ответы кэшируются и могут обрабатываться из кэша. Изучите Входящие заголовки запроса и исходящие заголовки ответа. Включите [ведение журнала](xref:fundamentals/logging/index) для помощи при отладке.

При тестировании и устранении неполадок кэширования браузер может задавать заголовки запросов, которые влияют на кэширование нежелательным образом. Например, в браузере можно задать заголовок `Cache-Control` `no-cache` или `max-age=0` при обновлении страницы. Следующие средства могут явно задавать заголовки запроса и являются предпочтительными для тестирования кэширования.

* [Fiddler](https://www.telerik.com/fiddler)
* [Postman](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a>Условия для кэширования

* Запрос должен привести к отклику сервера с кодом состояния 200 (ОК).
* Метод запроса должен быть GET или HEAD.
* В `Startup.Configure`промежуточного слоя кэширования ответа необходимо разместить перед по промежуточного слоя, требующего кэширования. Для получения дополнительной информации см. <xref:fundamentals/middleware/index>.
* Заголовок `Authorization` не должен присутствовать.
* параметры заголовка `Cache-Control` должны быть допустимыми, и ответ должен быть помечен как `public` и не помечен как `private`.
* Заголовок `Pragma: no-cache` не должен присутствовать, если заголовок `Cache-Control` отсутствует, так как заголовок `Cache-Control` переопределяет заголовок `Pragma` при его наличии.
* Заголовок `Set-Cookie` не должен присутствовать.
* параметры заголовка `Vary` должны быть допустимыми и не равны `*`.
* Значение заголовка `Content-Length` (если оно задано) должно соответствовать размеру текста ответа.
* <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> не используется.
* Ответ не должен быть устаревшим, как указано в заголовке `Expires` и директивах кэша `max-age` и `s-maxage`.
* Буферизация ответов должна быть успешной. Размер ответа должен быть меньше, чем заданный <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>по умолчанию или. Размер текста ответа должен быть меньше заданного <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>по умолчанию или.
* Ответ должен быть кэширован согласно спецификациям [RFC 7234](https://tools.ietf.org/html/rfc7234) . Например, директива `no-store` не должна существовать в полях заголовка запроса или ответа. Дополнительные сведения см. *в разделе 3. хранение ответов в кэшах* [RFC 7234](https://tools.ietf.org/html/rfc7234) .

> [!NOTE]
> Система защиты от подделки для создания безопасных маркеров для предотвращения подделки межсайтовых запросов (CSRF) задает `Cache-Control` и `Pragma` заголовки для `no-cache`, чтобы ответы не были кэшированы. Сведения об отключении маркеров подделки для элементов HTML-форм см. в разделе <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
