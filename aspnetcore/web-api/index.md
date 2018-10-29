---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: 763b95fb8ed3806bc67b7ad199153ea1027efa57
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090424"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="8c900-103">Сборка веб-API с использованием ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c900-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="8c900-104">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="8c900-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="8c900-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8c900-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8c900-106">Этот документ содержит сведения о сборке веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из функций.</span><span class="sxs-lookup"><span data-stu-id="8c900-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="8c900-107">Создание класса, производного от ControllerBase</span><span class="sxs-lookup"><span data-stu-id="8c900-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="8c900-108">Вы можете наследовать от класса <xref:Microsoft.AspNetCore.Mvc.ControllerBase> в контроллере, который предназначен для работы в качестве веб-API.</span><span class="sxs-lookup"><span data-stu-id="8c900-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="8c900-109">Пример:</span><span class="sxs-lookup"><span data-stu-id="8c900-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="8c900-110">Класс `ControllerBase` предоставляет доступ к нескольким свойствам и методам.</span><span class="sxs-lookup"><span data-stu-id="8c900-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="8c900-111">В приведенном выше коде примеры включают <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="8c900-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="8c900-112">Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201 соответственно.</span><span class="sxs-lookup"><span data-stu-id="8c900-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="8c900-113">Обращение к свойству <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.</span><span class="sxs-lookup"><span data-stu-id="8c900-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="8c900-114">Аннотирование класса атрибутом ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="8c900-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="8c900-115">В ASP.NET Core 2.1 появился атрибут [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), обозначающий класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="8c900-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="8c900-116">Пример:</span><span class="sxs-lookup"><span data-stu-id="8c900-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="8c900-117">Для использования этого атрибута требуется версия совместимости 2.1 или более поздняя, заданная с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="8c900-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute.</span></span> <span data-ttu-id="8c900-118">Например, выделенный код в *Startup.ConfigureServices* задает флаг совместимости 2.2:</span><span class="sxs-lookup"><span data-stu-id="8c900-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.2 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="8c900-119">Дополнительные сведения см. в разделе <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="8c900-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

<span data-ttu-id="8c900-120">Атрибут `[ApiController]` обычно используется вместе с `ControllerBase` для включения связанного с REST поведения для контроллеров.</span><span class="sxs-lookup"><span data-stu-id="8c900-120">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="8c900-121">`ControllerBase` предоставляет доступ к таким методам, как <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="8c900-121">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="8c900-122">Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="8c900-122">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="8c900-123">В следующих разделах описаны удобные функции, добавляемые этим атрибутом.</span><span class="sxs-lookup"><span data-stu-id="8c900-123">The following sections describe convenience features added by the attribute.</span></span>

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="8c900-124">Отклики со сведениями о проблемах для кодов состояния ошибки</span><span class="sxs-lookup"><span data-stu-id="8c900-124">Problem details responses for error status codes</span></span>

<span data-ttu-id="8c900-125">ASP.NET Core 2.1 и более поздние версии содержат [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails) — тип на основе [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="8c900-125">ASP.NET Core 2.1 and later includes [ProblemDetails](xref:Microsoft.AspNetCore.Mvc.ProblemDetails), a type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="8c900-126">Тип `ProblemDetails` предоставляет стандартизированный формат для передачи распознаваемых компьютером сведений об ошибках в HTTP-отклике.</span><span class="sxs-lookup"><span data-stu-id="8c900-126">The `ProblemDetails` type provides a standardized format for conveying machine readable details of errors in a HTTP response.</span></span>

<span data-ttu-id="8c900-127">В ASP.NET Core 2.2 и более поздних версиях MVC преобразует результаты кода состояния ошибки (код состояния 400 и выше) в результат с помощью `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="8c900-127">In ASP.NET Core 2.2 and later, MVC transforms error status code results (status code 400 and higher) to a result with `ProblemDetails`.</span></span> <span data-ttu-id="8c900-128">Рассмотрим следующий код.</span><span class="sxs-lookup"><span data-stu-id="8c900-128">Consider the following code:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_ProblemDetails_StatusCode&highlight=4)]

<span data-ttu-id="8c900-129">HTTP-отклик для результата `NotFound`содержит код состояния 404 с текстом `ProblemDetails`, подобным следующему:</span><span class="sxs-lookup"><span data-stu-id="8c900-129">The HTTP response for the `NotFound` result has a 404 status code with a `ProblemDetails` body similar to the following:</span></span>

```js
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="8c900-130">Для функции сведений о проблеме необходим флаг совместимости 2.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="8c900-130">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="8c900-131">Поведение по умолчанию отключается, если свойству [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="8c900-131">The default behavior is disabled when the [SuppressMapClientErrors](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors> --> property is set to `true`.</span></span> <span data-ttu-id="8c900-132">Следующий выделенный код из `Startup.ConfigureServices` отключает сведения о проблеме:</span><span class="sxs-lookup"><span data-stu-id="8c900-132">The following highlighted code from `Startup.ConfigureServices` disables problem details:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=8)]

<span data-ttu-id="8c900-133">Используйте свойство [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> -->, чтобы настроить содержимое отклика `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="8c900-133">Use the [ClientErrorMapping](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  Until these resolve, link to the parent class <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.ClientErrorMapping> --> property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="8c900-134">Например, в следующем коде показано обновление свойства `type` для откликов 404:</span><span class="sxs-lookup"><span data-stu-id="8c900-134">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=10)]

### <a name="automatic-http-400-responses"></a><span data-ttu-id="8c900-135">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="8c900-135">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="8c900-136">Ошибки проверки автоматически активируют отклик HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="8c900-136">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="8c900-137">Следующий код становится ненужным в ваших действиях:</span><span class="sxs-lookup"><span data-stu-id="8c900-137">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="8c900-138">Используйте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> для настройки выходных данных итогового отклика.</span><span class="sxs-lookup"><span data-stu-id="8c900-138">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the the resulting response.</span></span>

<span data-ttu-id="8c900-139">Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="8c900-139">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="8c900-140">Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="8c900-140">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

<span data-ttu-id="8c900-141">Если установлен флаг совместимости 2.2 или более поздней версии, для откликов 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="8c900-141">With a compatibility flag of 2.2 or later, the default response type returned for 400 responses is a <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="8c900-142">Воспользуйтесь свойством [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> -->, чтобы использовать формат ошибок ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="8c900-142">Use the Use the [SuppressUseValidationProblemDetailsForInvalidModelStateResponses](/dotnet/api/microsoft.aspnetcore.Mvc.ApiBehaviorOptions) <!--  <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressUseValidationProblemDetailsForInvalidModelStateResponses> --> property to use the ASP.NET Core 2.1 error format.</span></span>

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="8c900-143">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="8c900-143">Binding source parameter inference</span></span>

<span data-ttu-id="8c900-144">Атрибут источника привязки определяет расположение, в котором находится значение параметра действия.</span><span class="sxs-lookup"><span data-stu-id="8c900-144">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="8c900-145">Существуют следующие атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="8c900-145">The following binding source attributes exist:</span></span>

|<span data-ttu-id="8c900-146">Атрибут</span><span class="sxs-lookup"><span data-stu-id="8c900-146">Attribute</span></span>|<span data-ttu-id="8c900-147">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="8c900-147">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="8c900-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8c900-148">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="8c900-149">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="8c900-149">Request body</span></span> |
|<span data-ttu-id="8c900-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8c900-150">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="8c900-151">Данные формы в тексте запроса</span><span class="sxs-lookup"><span data-stu-id="8c900-151">Form data in the request body</span></span> |
|<span data-ttu-id="8c900-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8c900-152">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="8c900-153">Заголовок запроса</span><span class="sxs-lookup"><span data-stu-id="8c900-153">Request header</span></span> |
|<span data-ttu-id="8c900-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8c900-154">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="8c900-155">Параметры строки запроса для запроса</span><span class="sxs-lookup"><span data-stu-id="8c900-155">Request query string parameter</span></span> |
|<span data-ttu-id="8c900-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="8c900-156">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="8c900-157">Данные маршрута из текущего запроса</span><span class="sxs-lookup"><span data-stu-id="8c900-157">Route data from the current request</span></span> |
|<span data-ttu-id="8c900-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="8c900-158">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="8c900-159">Служба запросов, внедренная в качестве параметра действия</span><span class="sxs-lookup"><span data-stu-id="8c900-159">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="8c900-160">Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`).</span><span class="sxs-lookup"><span data-stu-id="8c900-160">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="8c900-161">Для `%2f` не будет применяться отмена экранирования `/`.</span><span class="sxs-lookup"><span data-stu-id="8c900-161">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="8c900-162">Используйте `[FromQuery]`, если значение может содержать `%2f`.</span><span class="sxs-lookup"><span data-stu-id="8c900-162">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="8c900-163">Без атрибута `[ApiController]` атрибуты источника привязки определяются явно.</span><span class="sxs-lookup"><span data-stu-id="8c900-163">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="8c900-164">В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:</span><span class="sxs-lookup"><span data-stu-id="8c900-164">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="8c900-165">Правила зависимости применяются к источникам данных по умолчанию для параметров действий.</span><span class="sxs-lookup"><span data-stu-id="8c900-165">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="8c900-166">Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия.</span><span class="sxs-lookup"><span data-stu-id="8c900-166">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="8c900-167">Атрибуты источника привязки работают следующим образом.</span><span class="sxs-lookup"><span data-stu-id="8c900-167">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="8c900-168">**[FromBody]**  выводится для параметров сложного типа.</span><span class="sxs-lookup"><span data-stu-id="8c900-168">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="8c900-169">Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как <xref:Microsoft.AspNetCore.Http.IFormCollection> и <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="8c900-169">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="8c900-170">Код определения источника привязки игнорирует эти особые типы.</span><span class="sxs-lookup"><span data-stu-id="8c900-170">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="8c900-171">`[FromBody]` не определен для простых типов, таких как `string` или `int`.</span><span class="sxs-lookup"><span data-stu-id="8c900-171">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="8c900-172">Таким образом, атрибут `[FromBody]` должен использоваться для простых типов, когда требуются эти функции.</span><span class="sxs-lookup"><span data-stu-id="8c900-172">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is desired.</span></span> <span data-ttu-id="8c900-173">Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="8c900-173">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="8c900-174">Например, следующие сигнатуры действия вызывают исключение:</span><span class="sxs-lookup"><span data-stu-id="8c900-174">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="8c900-175">**[FromForm]** выводится для параметров действия с типом <xref:Microsoft.AspNetCore.Http.IFormFile> и <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="8c900-175">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="8c900-176">Он не выводится ни для каких простых или определяемых пользователем типов.</span><span class="sxs-lookup"><span data-stu-id="8c900-176">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="8c900-177">**[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="8c900-177">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="8c900-178">Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="8c900-178">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="8c900-179">**[FromQuery]**  выводится для любых других параметров действия.</span><span class="sxs-lookup"><span data-stu-id="8c900-179">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="8c900-180">Правила зависимости по умолчанию отключаются, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="8c900-180">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="8c900-181">Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="8c900-181">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="8c900-182">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="8c900-182">Multipart/form-data request inference</span></span>

<span data-ttu-id="8c900-183">Когда параметр действия аннотирован атрибутом [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), выводится тип содержимого запроса `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="8c900-183">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="8c900-184">Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="8c900-184">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span> <span data-ttu-id="8c900-185">Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="8c900-185">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="8c900-186">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="8c900-186">Attribute routing requirement</span></span>

<span data-ttu-id="8c900-187">Маршрутизация атрибутов стала обязательным требованием.</span><span class="sxs-lookup"><span data-stu-id="8c900-187">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="8c900-188">Пример:</span><span class="sxs-lookup"><span data-stu-id="8c900-188">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="8c900-189">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или с помощью <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="8c900-189">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="8c900-190">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8c900-190">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>