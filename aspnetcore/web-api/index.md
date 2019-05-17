---
title: Создание веб-API с помощью ASP.NET Core
author: scottaddie
description: Узнайте, как создать веб-API в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 05/07/2019
uid: web-api/index
ms.openlocfilehash: 593fd33babc81cddfc4db2150a37e5ec3bc1a0be
ms.sourcegitcommit: a3926eae3f687013027a2828830c12a89add701f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65450835"
---
# <a name="create-web-apis-with-aspnet-core"></a>Создание веб-API с помощью ASP.NET Core

Авторы: [Скотт Адди](https://github.com/scottaddie) (Scott Addie) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)

ASP.NET Core поддерживает создание служб RESTful, также известных как веб-API, с помощью C#. Для обработки запросов веб-API использует контроллеры. В веб-API *контроллеры* — это классы, производные от `ControllerBase`. В этой статье показано, как использовать контроллеры для обработки запросов API.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples). ([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)

## <a name="controllerbase-class"></a>Класс ControllerBase

Веб-API имеет один или несколько классов контроллера, которые являются производными от <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Например, шаблон проекта веб-API создает контроллер значений:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

Не создавайте контроллер веб-API путем наследования от базового класса <xref:Microsoft.AspNetCore.Mvc.Controller>. `Controller` является производным от `ControllerBase` и добавляет поддержку для представлений, обеспечивая обработку веб-страниц, а не запросов веб-API.  Существует одно исключение из этого правила: если вы планируете использовать один и тот же контроллер для представлений и API, сделайте его производным от `Controller`.

Класс `ControllerBase` предоставляет множество свойств и методов, которые удобны для обработки HTTP-запросов. Например, `ControllerBase.CreatedAtAction` возвращает код состояния 201.

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 Вот еще примеры методов, предоставляющих `ControllerBase`.

|Метод  |Примечания  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| Возвращает код состояния 400.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |Возвращает код состояния 404.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|Возвращает файл.|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|Вызывает [привязку модели](xref:mvc/models/model-binding).|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|Вызывает [проверку модели](xref:mvc/models/validation).|

Список всех доступных методов и свойств см. здесь: <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.

## <a name="attributes"></a>Атрибуты

Пространство имен <xref:Microsoft.AspNetCore.Mvc> предоставляет атрибуты, которые можно использовать для настройки поведения контроллеров и методов действия веб-API. В следующем примере с помощью атрибутов указан принятый метод HTTP и возвращаемые коды состояния.

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

Вот еще примеры доступных атрибутов.

|Атрибут|Примечания|
|---------|-----|
|[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)      |Определяет шаблон URL-адреса для контроллера или действия.|
|[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)        |Задает префикс и свойства, которые добавляются для привязки модели.|
|[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)  |Определяет действие, которое поддерживает метод HTTP GET.|
|[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)|Указывает типы данных, которые принимает действие.|
|[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)|Указывает типы данных, которые возвращает действие.|

Список доступных атрибутов см. в пространстве имен <xref:Microsoft.AspNetCore.Mvc>.

## <a name="apicontroller-attribute"></a>Атрибут ApiController

Атрибут [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) можно применить к классу контроллера для включения определенных схем поведения API:

* [Обязательная маршрутизация атрибутов](#attribute-routing-requirement)
* [Автоматические отклики HTTP 400](#automatic-http-400-responses)
* [Вывод параметров источника привязки](#binding-source-parameter-inference)
* [Вывод многокомпонентных запросов и запросов данных форм](#multipartform-data-request-inference)
* [Сведения о проблемах для кодов состояния ошибки](#problem-details-for-error-status-codes)

Для реализации этих функций необходима [версия совместимости](<xref:mvc/compatibility-version>), начиная с 2.1.

### <a name="apicontroller-on-specific-controllers"></a>Использование ApiController на определенных контроллерах

Атрибут `[ApiController]` может применяться к определенным контроллерам, как показано в следующем примере из шаблона проекта:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a>Использование ApiController на нескольких контроллерах

Один из подходов к использованию атрибута на более чем одном контроллере заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`. Вот пример, демонстрирующий пользовательский базовый класс и производный от него контроллер:

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a>Использование ApiController в сборке

Если задана [версия совместимости](<xref:mvc/compatibility-version>) 2.2 или последующая, атрибут `[ApiController]` можно применить к сборке. Аннотирование этим способом применяет поведение веб-API ко всем контроллерам в сборке. Его нельзя отменить для отдельных контроллеров. Примените атрибут уровня сборки `Startup` к классу, как показано в следующем примере.

```csharp
[assembly: ApiController]
namespace WebApiSample
{
    public class Startup
    {
        ...
    }
}
```

## <a name="attribute-routing-requirement"></a>Обязательная маршрутизация атрибутов

Атрибут `ApiController` требует обязательной маршрутизации атрибутов. Например:

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определяемые <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в `Startup.Configure`.

## <a name="automatic-http-400-responses"></a>Автоматические отклики HTTP 400

Благодаря атрибуту `ApiController` ошибки проверки модели автоматически активируют отклик HTTP 400. В результате следующий код ненужен в методе действия:

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a>Отклик BadRequest по умолчанию 

Если задана версия совместимости, начиная с 2.2, для ответов HTTP 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>. Тип `ValidationProblemDetails` соответствует [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807).

Чтобы изменить отклик по умолчанию на <xref:Microsoft.AspNetCore.Mvc.SerializableError>, задайте свойству `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` значение `true` в `Startup.ConfigureServices`, как показано в следующем примере:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a>Настройка отклика BadRequest

Чтобы настроить ответ, полученный в результате ошибки проверки, используйте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>. Добавьте выделенный ниже код после `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a>Запись в журнал автоматических откликов HTTP 400

См. статью об[записи в журнал автоматических откликов HTTP 400 на ошибки проверки модели (aspnet/AspNetCore.Docs № 12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).

### <a name="disable-automatic-400"></a>Отключение автоматической активации отклика HTTP 400

Чтобы отключить автоматическую активацию отклика HTTP 400, задайте свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> значение `true`. Добавьте выделенный ниже код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion`.

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a>Вывод параметров источника привязки

Атрибут источника привязки определяет расположение, в котором находится значение параметра действия. Существуют следующие атрибуты источника привязки.

|Атрибут|Источник привязки |
|---------|---------|
|[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)     | Текст запроса |
|[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)     | Данные формы в тексте запроса |
|[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) | Заголовок запроса |
|[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)   | Параметры строки запроса для запроса |
|[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)   | Данные маршрута из текущего запроса |
|[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) | Служба запросов, внедренная в качестве параметра действия |

> [!WARNING]
> Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`). Для `%2f` не будет применяться отмена экранирования `/`. Используйте `[FromQuery]`, если значение может содержать `%2f`.

Без атрибута `[ApiController]` или атрибутов источника привязки, таких как `[FromQuery]`, среда выполнения ASP.NET Core попытается использовать связыватель модели для составного объекта. Связыватель модели для составного объекта извлекает данные из поставщиков значений в определенном порядке.

В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

Атрибут `[ApiController]` применяет правила зависимости к источникам данных по умолчанию для параметров действий. Эти правила избавляют от необходимости вручную определять источники привязки путем применения атрибутов к параметрам действий. Правила зависимости источника привязки работают следующим образом:

* `[FromBody]` выводится для параметров сложного типа. Исключением из правила зависимости `[FromBody]` является любой сложный встроенный тип со специальным значением, такой как <xref:Microsoft.AspNetCore.Http.IFormCollection> и <xref:System.Threading.CancellationToken>. Код определения источника привязки игнорирует эти особые типы. 
* `[FromForm]` выводится для параметров действия с типом <xref:Microsoft.AspNetCore.Http.IFormFile> и <xref:Microsoft.AspNetCore.Http.IFormFileCollection>. Он не выводится ни для каких простых или определяемых пользователем типов.
* `[FromRoute]` выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута. Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.
* `[FromQuery]` выводится для любых других параметров действия.

### <a name="frombody-inference-notes"></a>Заметки о выводе FromBody

`[FromBody]` не определен для простых типов, таких как `string` или `int`. Таким образом, атрибут `[FromBody]` должен использоваться для простых типов, когда требуются эти функции.

Если у действия более одного параметра для привязки из текста запроса, выдается исключение. Например, все следующие сигнатуры метода действия вызывают исключение:

* `[FromBody]` выводится для обеих параметров, так как они являются сложными типами.

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* Атрибут `[FromBody]`, заданный одному параметру, выводится для другого, так как это сложный тип.

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* Атрибут `[FromBody]` выводится для обоих параметров.

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> В ASP.NET Core 2.1 параметры типа коллекции, такие как списки и массивы, ошибочно выводятся как `[FromQuery]`. Для этих параметров следует использовать атрибут `[FromBody]`, если они должны быть привязаны из текста запроса. Это поведение, при котором параметры типа коллекции выводятся для привязки из текста по умолчанию, исправлено в версии ASP.NET Core, начиная с 2.2.

### <a name="disable-inference-rules"></a>Отключение правил зависимости

Чтобы отключить вывод источника привязки, задайте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> значение `true`. Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a>Вывод многокомпонентных запросов и запросов данных форм

Атрибут `[ApiController]` применяет правило зависимости, когда параметр действия аннотирован атрибутом [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute). При этом выводится тип содержимого запроса `multipart/form-data`.

Чтобы отключить поведение по умолчанию, задайте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> значение `true` в `Startup.ConfigureServices`, как показано в следующем примере:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a>Сведения о проблемах для кодов состояния ошибки

Если задана версия совместимости, начиная с 2.2, MVC преобразовывает код ошибки (код состояния 400 и далее) в результат с <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>. Тип `ProblemDetails` основан на [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807) и предоставляет считываемые компьютером сведения об ошибке в HTTP-ответе.

Рассмотрим следующий код в действии контроллера:

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

Ответ HTTP для `NotFound` содержит код состояния 404 с текстом `ProblemDetails`. Например:

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a>Настройка ответа ProblemDetails

Используйте свойство `ClientErrorMapping`, чтобы настроить содержимое ответа `ProblemDetails`. Например, в следующем коде показано обновление свойства `type` для откликов 404:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a>Отключение ответа ProblemDetails

Отключить автоматическое создание `ProblemDetails` можно, задав свойству `SuppressMapClientErrors` значение `true`. Добавьте следующий код в `Startup.ConfigureServices`:

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a>Дополнительные ресурсы 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
