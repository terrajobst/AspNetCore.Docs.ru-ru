---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: 84e4a51a8a8ab031752ef054cba834bd202a4927
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894219"
---
# <a name="build-web-apis-with-aspnet-core"></a>Сборка веб-API с использованием ASP.NET Core

Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Этот документ содержит сведения о сборке веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из функций.

## <a name="derive-class-from-controllerbase"></a>Создание класса, производного от ControllerBase

Вы можете наследовать от класса [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) в контроллере, который предназначен для работы в качестве веб-API. Пример:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

Класс `ControllerBase` предоставляет доступ к нескольким свойствам и методам. В предыдущем коде примерами методов являются [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) и [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201 соответственно. Обращение к свойству [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>Аннотирование класса атрибутом ApiControllerAttribute

В ASP.NET Core 2.1 появился атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), обозначающий класс контроллера веб-API. Пример:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Для использования этого атрибута требуется версия совместимости 2.1 или более поздняя, заданная с помощью [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion). Например, выделенный код в *Startup.ConfigureServices* задает флаг совместимости 2.1:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

Атрибут `[ApiController]` обычно используется вместе с `ControllerBase` для включения связанного с REST поведения для контроллеров. `ControllerBase` предоставляет доступ к таким методам, как [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) и [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

В следующих разделах описаны удобные функции, добавляемые этим атрибутом.

### <a name="automatic-http-400-responses"></a>Автоматические отклики HTTP 400

Ошибки проверки автоматически активируют отклик HTTP 400. Следующий код становится ненужным в ваших действиях:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Поведение по умолчанию отключается, если свойству [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) задано значение `true`. Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>Вывод параметров источника привязки

Атрибут источника привязки определяет расположение, в котором находится значение параметра действия. Существуют следующие атрибуты источника привязки.

|Атрибут|Источник привязки |
|---------|---------|
|**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**     | Текст запроса |
|**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**     | Данные формы в тексте запроса |
|**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)** | Заголовок запроса |
|**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**   | Параметры строки запроса для запроса |
|**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**   | Данные маршрута из текущего запроса |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | Служба запросов, внедренная в качестве параметра действия |

> [!WARNING]
> Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`). Для `%2f` не будет применяться отмена экранирования `/`. Используйте `[FromQuery]`, если значение может содержать `%2f`.

Без атрибута `[ApiController]` атрибуты источника привязки определяются явно. В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Правила зависимости применяются к источникам данных по умолчанию для параметров действий. Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия. Атрибуты источника привязки работают следующим образом.

* **[FromBody]**  выводится для параметров сложного типа. Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) и [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Код определения источника привязки игнорирует эти особые типы. Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение. Например, следующие сигнатуры действия вызывают исключение:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** выводится для параметров действия с типом [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Он не выводится ни для каких простых или определяемых пользователем типов.
* **[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута. Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.
* **[FromQuery]**  выводится для любых других параметров действия.

Правила зависимости по умолчанию отключаются, если свойству [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) задано значение `true`. Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Вывод многокомпонентных запросов и запросов данных форм

Когда параметр действия аннотирован атрибутом [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), выводится тип содержимого запроса `multipart/form-data`.

Поведение по умолчанию отключается, если свойству [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) задано значение `true`. Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Обязательная маршрутизация атрибутов

Маршрутизация атрибутов стала обязательным требованием. Пример:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) или с помощью [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) в *Startup.Configure*.

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
