---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274970"
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

Класс `ControllerBase` предоставляет доступ к множеству свойств и методов. В предыдущем примере некоторые такие методы включают [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) и [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction). Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201, соответственно. Обращение к свойству [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a>Аннотирование класса атрибутом ApiControllerAttribute

В ASP.NET Core 2.1 появился атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), обозначающий класс контроллера веб-API. Пример:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

Этот атрибут обычно используется вместе с `ControllerBase` для получения доступа к полезным методам и свойствам. `ControllerBase` предоставляет доступ к таким методам, как [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) и [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).

Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

В следующих разделах описаны удобные функции, добавляемые этим атрибутом.

### <a name="automatic-http-400-responses"></a>Автоматические отклики HTTP 400

Ошибки проверки автоматически активируют отклик HTTP 400. Следующий код становится ненужным в ваших действиях:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

Это поведение по умолчанию отключено с помощью следующего кода в *Startup.ConfigureServices*:

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

> [!NOTE]
> **Не** используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`), так как для `%2f` не будет применяться отмена экранирования (`/`). Используйте `[FromQuery]`, если значение может содержать `%2f`.

Без атрибута `[ApiController]` атрибуты источника привязки определяются явно. В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

Правила зависимости применяются к источникам данных по умолчанию для параметров действий. Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия. Атрибуты источника привязки работают следующим образом.

* **[FromBody]**  выводится для параметров сложного типа. Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) и [CancellationToken](/dotnet/api/system.threading.cancellationtoken). Код определения источника привязки игнорирует эти особые типы. Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение. Например, следующие сигнатуры действия вызывают исключение:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** выводится для параметров действия с типом [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection). Он не выводится ни для каких простых или определяемых пользователем типов.
* **[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута. Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.
* **[FromQuery]**  выводится для любых других параметров действия.

Правила зависимости по умолчанию отключены с помощью следующего кода в *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>Вывод многокомпонентных запросов и запросов данных форм

Когда параметр действия аннотирован атрибутом [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), выводится тип содержимого запроса `multipart/form-data`.

Это поведение по умолчанию отключено с помощью следующего кода в *Startup.ConfigureServices*:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>Обязательная маршрутизация атрибутов

Маршрутизация атрибутов стала обязательным требованием. Пример:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) или с помощью [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) в *Startup.Configure*.
::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Типы возвращаемых значений действий контроллера](xref:web-api/action-return-types)
* [Пользовательские модули форматирования](xref:web-api/advanced/custom-formatters)
* [Форматирование данных ответа](xref:web-api/advanced/formatting)
* [Страницы справки с использованием Swagger](xref:tutorials/web-api-help-pages-using-swagger)
* [Маршрутизация к действиям контроллера](xref:mvc/controllers/routing)
