---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/30/2018
uid: web-api/index
ms.openlocfilehash: b3e26bee5e4dc8937e810bc5db300a486437f568
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244766"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="05ebe-103">Сборка веб-API с использованием ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05ebe-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="05ebe-104">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="05ebe-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="05ebe-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="05ebe-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="05ebe-106">Этот документ содержит сведения о сборке веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из функций.</span><span class="sxs-lookup"><span data-stu-id="05ebe-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="05ebe-107">Создание класса, производного от ControllerBase</span><span class="sxs-lookup"><span data-stu-id="05ebe-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="05ebe-108">Вы можете наследовать от класса <xref:Microsoft.AspNetCore.Mvc.ControllerBase> в контроллере, который предназначен для работы в качестве веб-API.</span><span class="sxs-lookup"><span data-stu-id="05ebe-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="05ebe-109">Пример:</span><span class="sxs-lookup"><span data-stu-id="05ebe-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="05ebe-110">Класс `ControllerBase` предоставляет доступ к нескольким свойствам и методам.</span><span class="sxs-lookup"><span data-stu-id="05ebe-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="05ebe-111">В приведенном выше коде примеры включают <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="05ebe-112">Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201 соответственно.</span><span class="sxs-lookup"><span data-stu-id="05ebe-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="05ebe-113">Обращение к свойству <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.</span><span class="sxs-lookup"><span data-stu-id="05ebe-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontrollerattribute"></a><span data-ttu-id="05ebe-114">Аннотирование атрибутом ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="05ebe-114">Annotation with ApiControllerAttribute</span></span>

<span data-ttu-id="05ebe-115">В ASP.NET Core 2.1 появился атрибут [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), обозначающий класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="05ebe-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="05ebe-116">Пример:</span><span class="sxs-lookup"><span data-stu-id="05ebe-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="05ebe-117">Для использования этого атрибута на уровне контроллера требуется версия совместимости 2.1 или более поздняя, заданная с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="05ebe-118">Например, выделенный код в `Startup.ConfigureServices` задает флаг совместимости 2.1:</span><span class="sxs-lookup"><span data-stu-id="05ebe-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="05ebe-119">Дополнительные сведения см. в разделе <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="05ebe-120">В ASP.NET Core 2.2 или более поздней версии атрибут `[ApiController]` применим к сборке.</span><span class="sxs-lookup"><span data-stu-id="05ebe-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="05ebe-121">Аннотирование этим способом применяет поведение веб-API ко всем контроллерам в сборке.</span><span class="sxs-lookup"><span data-stu-id="05ebe-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="05ebe-122">Обратите внимание, что его невозможно отменить для отдельных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="05ebe-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="05ebe-123">Атрибуты уровня сборки рекомендуется применять к классу `Startup`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="05ebe-124">Для использования этого атрибута на уровне сборки требуется версия совместимости 2.2 или более поздняя, заданная с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="05ebe-125">Атрибут `[ApiController]` обычно используется вместе с `ControllerBase` для включения связанного с REST поведения для контроллеров.</span><span class="sxs-lookup"><span data-stu-id="05ebe-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="05ebe-126">`ControllerBase` предоставляет доступ к таким методам, как <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="05ebe-127">Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="05ebe-128">В следующих разделах описаны удобные функции, добавляемые этим атрибутом.</span><span class="sxs-lookup"><span data-stu-id="05ebe-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="05ebe-129">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="05ebe-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="05ebe-130">Ошибки проверки автоматически активируют отклик HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="05ebe-130">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="05ebe-131">Следующий код становится ненужным в ваших действиях:</span><span class="sxs-lookup"><span data-stu-id="05ebe-131">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="05ebe-132">Используйте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> для настройки выходных данных итогового ответа.</span><span class="sxs-lookup"><span data-stu-id="05ebe-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="05ebe-133">Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-133">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="05ebe-134">Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-134">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="05ebe-135">Если установлен флаг совместимости 2.2 или более поздней версии, для ответов HTTP 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-135">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="05ebe-136">Тип `ValidationProblemDetails` соответствует [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="05ebe-136">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="05ebe-137">Задайте для свойства `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` значение `true`, чтобы ошибка ASP.NET Core 2.1 возвращалась в формате <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-137">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="05ebe-138">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-138">Add the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_2)
    .ConfigureApiBehaviorOptions(options =>
    {
        options
          .SuppressUseValidationProblemDetailsForInvalidModelStateResponses = true;
    });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="05ebe-139">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="05ebe-139">Binding source parameter inference</span></span>

<span data-ttu-id="05ebe-140">Атрибут источника привязки определяет расположение, в котором находится значение параметра действия.</span><span class="sxs-lookup"><span data-stu-id="05ebe-140">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="05ebe-141">Существуют следующие атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="05ebe-141">The following binding source attributes exist:</span></span>

|<span data-ttu-id="05ebe-142">Атрибут</span><span class="sxs-lookup"><span data-stu-id="05ebe-142">Attribute</span></span>|<span data-ttu-id="05ebe-143">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="05ebe-143">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="05ebe-144">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="05ebe-144">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="05ebe-145">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="05ebe-145">Request body</span></span> |
|<span data-ttu-id="05ebe-146">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="05ebe-146">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="05ebe-147">Данные формы в тексте запроса</span><span class="sxs-lookup"><span data-stu-id="05ebe-147">Form data in the request body</span></span> |
|<span data-ttu-id="05ebe-148">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="05ebe-148">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="05ebe-149">Заголовок запроса</span><span class="sxs-lookup"><span data-stu-id="05ebe-149">Request header</span></span> |
|<span data-ttu-id="05ebe-150">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="05ebe-150">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="05ebe-151">Параметры строки запроса для запроса</span><span class="sxs-lookup"><span data-stu-id="05ebe-151">Request query string parameter</span></span> |
|<span data-ttu-id="05ebe-152">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="05ebe-152">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="05ebe-153">Данные маршрута из текущего запроса</span><span class="sxs-lookup"><span data-stu-id="05ebe-153">Route data from the current request</span></span> |
|<span data-ttu-id="05ebe-154">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="05ebe-154">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="05ebe-155">Служба запросов, внедренная в качестве параметра действия</span><span class="sxs-lookup"><span data-stu-id="05ebe-155">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="05ebe-156">Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`).</span><span class="sxs-lookup"><span data-stu-id="05ebe-156">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="05ebe-157">Для `%2f` не будет применяться отмена экранирования `/`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-157">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="05ebe-158">Используйте `[FromQuery]`, если значение может содержать `%2f`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-158">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="05ebe-159">Без атрибута `[ApiController]` атрибуты источника привязки определяются явно.</span><span class="sxs-lookup"><span data-stu-id="05ebe-159">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="05ebe-160">В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:</span><span class="sxs-lookup"><span data-stu-id="05ebe-160">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="05ebe-161">Правила зависимости применяются к источникам данных по умолчанию для параметров действий.</span><span class="sxs-lookup"><span data-stu-id="05ebe-161">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="05ebe-162">Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия.</span><span class="sxs-lookup"><span data-stu-id="05ebe-162">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="05ebe-163">Атрибуты источника привязки работают следующим образом.</span><span class="sxs-lookup"><span data-stu-id="05ebe-163">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="05ebe-164">**[FromBody]**  выводится для параметров сложного типа.</span><span class="sxs-lookup"><span data-stu-id="05ebe-164">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="05ebe-165">Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как <xref:Microsoft.AspNetCore.Http.IFormCollection> и <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-165">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="05ebe-166">Код определения источника привязки игнорирует эти особые типы.</span><span class="sxs-lookup"><span data-stu-id="05ebe-166">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="05ebe-167">`[FromBody]` не определен для простых типов, таких как `string` или `int`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-167">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="05ebe-168">Таким образом, атрибут `[FromBody]` должен использоваться для простых типов, когда требуются эти функции.</span><span class="sxs-lookup"><span data-stu-id="05ebe-168">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="05ebe-169">Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="05ebe-169">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="05ebe-170">Например, следующие сигнатуры действия вызывают исключение:</span><span class="sxs-lookup"><span data-stu-id="05ebe-170">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="05ebe-171">**[FromForm]** выводится для параметров действия с типом <xref:Microsoft.AspNetCore.Http.IFormFile> и <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-171">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="05ebe-172">Он не выводится ни для каких простых или определяемых пользователем типов.</span><span class="sxs-lookup"><span data-stu-id="05ebe-172">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="05ebe-173">**[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="05ebe-173">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="05ebe-174">Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-174">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="05ebe-175">**[FromQuery]**  выводится для любых других параметров действия.</span><span class="sxs-lookup"><span data-stu-id="05ebe-175">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="05ebe-176">Правила зависимости по умолчанию отключаются, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-176">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="05ebe-177">Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-177">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="05ebe-178">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="05ebe-178">Multipart/form-data request inference</span></span>

<span data-ttu-id="05ebe-179">Когда параметр действия аннотирован атрибутом [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), выводится тип содержимого запроса `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-179">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="05ebe-180">Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-180">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="05ebe-181">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-181">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="05ebe-182">Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-182">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="05ebe-183">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="05ebe-183">Attribute routing requirement</span></span>

<span data-ttu-id="05ebe-184">Маршрутизация атрибутов стала обязательным требованием.</span><span class="sxs-lookup"><span data-stu-id="05ebe-184">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="05ebe-185">Пример:</span><span class="sxs-lookup"><span data-stu-id="05ebe-185">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="05ebe-186">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или с помощью <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-186">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="05ebe-187">Отклики со сведениями о проблемах для кодов состояния ошибки</span><span class="sxs-lookup"><span data-stu-id="05ebe-187">Problem details responses for error status codes</span></span>

<span data-ttu-id="05ebe-188">В ASP.NET Core 2.2 или более поздней версии MVC преобразовывает код ошибки (код состояния 400 или выше) в результат с <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="05ebe-188">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="05ebe-189">`ProblemDetails` является:</span><span class="sxs-lookup"><span data-stu-id="05ebe-189">`ProblemDetails` is:</span></span>

* <span data-ttu-id="05ebe-190">типом на основе [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807);</span><span class="sxs-lookup"><span data-stu-id="05ebe-190">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="05ebe-191">стандартным форматом ответа HTTP, в котором указываются сведения об ошибке в машиночитаемом виде.</span><span class="sxs-lookup"><span data-stu-id="05ebe-191">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="05ebe-192">Рассмотрим следующий код в действии контроллера:</span><span class="sxs-lookup"><span data-stu-id="05ebe-192">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="05ebe-193">Ответ HTTP для `NotFound` содержит код состояния 404 с текстом `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-193">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="05ebe-194">Пример:</span><span class="sxs-lookup"><span data-stu-id="05ebe-194">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="05ebe-195">Для функции сведений о проблеме необходим флаг совместимости 2.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="05ebe-195">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="05ebe-196">Поведение по умолчанию отключается, если свойству `SuppressMapClientErrors` задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-196">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="05ebe-197">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05ebe-197">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="05ebe-198">Используйте свойство `ClientErrorMapping`, чтобы настроить содержимое ответа `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="05ebe-198">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="05ebe-199">Например, в следующем коде показано обновление свойства `type` для откликов 404:</span><span class="sxs-lookup"><span data-stu-id="05ebe-199">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="05ebe-200">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="05ebe-200">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
