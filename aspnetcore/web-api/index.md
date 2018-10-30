---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 950f4e8afa13bf297ea8658ef1c1bea0c9b62936
ms.sourcegitcommit: 2ef32676c16f76282f7c23154d13affce8c8bf35
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50234596"
---
# <a name="build-web-apis-with-aspnet-core"></a>Сборка веб-API с использованием ASP.NET Core

Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Этот документ содержит сведения о сборке веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из функций.

## <a name="derive-class-from-controllerbase"></a>Создание класса, производного от ControllerBase

Вы можете наследовать от класса <xref:Microsoft.AspNetCore.Mvc.ControllerBase> в контроллере, который предназначен для работы в качестве веб-API. Пример:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

Класс `ControllerBase` предоставляет доступ к нескольким свойствам и методам. В приведенном выше коде примеры включают <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>. Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201 соответственно. Обращение к свойству <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Аннотирование класса атрибутом ApiControllerAttribute

В ASP.NET Core 2.1 появился атрибут [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), обозначающий класс контроллера веб-API. Пример:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Для использования этого атрибута требуется версия совместимости 2.1 или более поздняя, заданная с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>. Например, выделенный код в *Startup.ConfigureServices* задает флаг совместимости 2.2:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Дополнительные сведения см. в разделе <xref:mvc/compatibility-version>.

Атрибут `[ApiController]` обычно используется вместе с `ControllerBase` для включения связанного с REST поведения для контроллеров. `ControllerBase` предоставляет доступ к таким методам, как <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.

Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

В следующих разделах описаны удобные функции, добавляемые этим атрибутом.

### <a name="problem-details-responses-for-error-status-codes"></a>Отклики со сведениями о проблемах для кодов состояния ошибки

ASP.NET Core 2.1 и более поздние версии содержат [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails) — тип на основе [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807). Тип `ProblemDetails` предоставляет стандартизированный формат для передачи распознаваемых компьютером сведений об ошибках в HTTP-отклике.

В ASP.NET Core 2.2 и более поздних версиях MVC преобразует результаты кода состояния ошибки (код состояния 400 и выше) в результат с помощью `ProblemDetails`. Рассмотрим следующий код.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

HTTP-отклик для результата `NotFound`содержит код состояния 404 с текстом `ProblemDetails`, подобным следующему:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

Для функции сведений о проблеме необходим флаг совместимости 2.2 или более поздней версии. Поведение по умолчанию отключается, если свойству [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> задано значение `true`. Следующий выделенный код из `Startup.ConfigureServices` отключает сведения о проблеме:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

Используйте свойство [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> -->, чтобы настроить содержимое отклика `ProblemDetails`. Например, в следующем коде показано обновление свойства `type` для откликов 404:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a>Автоматические отклики HTTP 400

Ошибки проверки автоматически активируют отклик HTTP 400. Следующий код становится ненужным в ваших действиях:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

Используйте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> для настройки выходных данных итогового отклика.

Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> задано значение `true`. Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

Если установлен флаг совместимости 2.2 или более поздней версии, для откликов 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Воспользуйтесь свойством [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> -->, чтобы использовать формат ошибок ASP.NET Core 2.1.

### <a name="binding-source-parameter-inference"></a>Вывод параметров источника привязки

Атрибут источника привязки определяет расположение, в котором находится значение параметра действия. Существуют следующие атрибуты источника привязки.

|Атрибут|Источник привязки |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | Текст запроса |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | Данные формы в тексте запроса |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | Заголовок запроса |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | Параметры строки запроса для запроса |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | Данные маршрута из текущего запроса |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Служба запросов, внедренная в качестве параметра действия |

> [!WARNING]
> Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`). Для `%2f` не будет применяться отмена экранирования `/`. Используйте `[FromQuery]`, если значение может содержать `%2f`.

Без атрибута `[ApiController]` атрибуты источника привязки определяются явно. В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Правила зависимости применяются к источникам данных по умолчанию для параметров действий. Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия. Атрибуты источника привязки работают следующим образом.

* **[FromBody]**  выводится для параметров сложного типа. Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как <xref:Microsoft.AspNetCore.Http.IFormCollection> и <xref:System.Threading.CancellationToken>. Код определения источника привязки игнорирует эти особые типы. `[FromBody]` не определен для простых типов, таких как `string` или `int`. Таким образом, атрибут `[FromBody]` должен использоваться для простых типов, когда требуются эти функции. Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение. Например, следующие сигнатуры действия вызывают исключение:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** выводится для параметров действия с типом <xref:Microsoft.AspNetCore.Http.IFormFile> и <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Он не выводится ни для каких простых или определяемых пользователем типов.
* **[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута. Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.
* **[FromQuery]**  выводится для любых других параметров действия.

Правила зависимости по умолчанию отключаются, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> задано значение `true`. Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Вывод многокомпонентных запросов и запросов данных форм

Когда параметр действия аннотирован атрибутом [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), выводится тип содержимого запроса `multipart/form-data`.

Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> задано значение `true`. Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Обязательная маршрутизация атрибутов

Маршрутизация атрибутов стала обязательным требованием. Пример:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или с помощью <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
