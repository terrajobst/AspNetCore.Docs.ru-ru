---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/06/2018
uid: web-api/index
ms.openlocfilehash: 010c437afc494fa4426f6922421afac46bbf6b39
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225438"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="3a8c2-103">Сборка веб-API с использованием ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3a8c2-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="3a8c2-104">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="3a8c2-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="3a8c2-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3a8c2-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="3a8c2-106">Этот документ содержит сведения о сборке веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из функций.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="3a8c2-107">Создание класса, производного от ControllerBase</span><span class="sxs-lookup"><span data-stu-id="3a8c2-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="3a8c2-108">Вы можете наследовать от класса <xref:Microsoft.AspNetCore.Mvc.ControllerBase> в контроллере, который предназначен для работы в качестве веб-API.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-108">Inherit from the <xref:Microsoft.AspNetCore.Mvc.ControllerBase> class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="3a8c2-109">Пример:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="3a8c2-110">Класс `ControllerBase` предоставляет доступ к нескольким свойствам и методам.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="3a8c2-111">В приведенном выше коде примеры включают <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-111">In the preceding code, examples include <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>.</span></span> <span data-ttu-id="3a8c2-112">Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201 соответственно.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="3a8c2-113">Обращение к свойству <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState>, также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-113">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotation-with-apicontrollerattribute"></a><span data-ttu-id="3a8c2-114">Аннотирование атрибутом ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="3a8c2-114">Annotation with ApiControllerAttribute</span></span>

<span data-ttu-id="3a8c2-115">В ASP.NET Core 2.1 появился атрибут [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute), обозначающий класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-115">ASP.NET Core 2.1 introduces the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="3a8c2-116">Пример:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-116">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="3a8c2-117">Для использования этого атрибута на уровне контроллера требуется версия совместимости 2.1 или более поздняя, заданная с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-117">A compatibility version of 2.1 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the controller level.</span></span> <span data-ttu-id="3a8c2-118">Например, выделенный код в `Startup.ConfigureServices` задает флаг совместимости 2.1:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-118">For example, the highlighted code in `Startup.ConfigureServices` sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="3a8c2-119">Дополнительные сведения см. в разделе <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-119">For more information, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3a8c2-120">В ASP.NET Core 2.2 или более поздней версии атрибут `[ApiController]` применим к сборке.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-120">In ASP.NET Core 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="3a8c2-121">Аннотирование этим способом применяет поведение веб-API ко всем контроллерам в сборке.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-121">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="3a8c2-122">Обратите внимание, что его невозможно отменить для отдельных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-122">Beware that there's no way to opt out for individual controllers.</span></span> <span data-ttu-id="3a8c2-123">Атрибуты уровня сборки рекомендуется применять к классу `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-123">As a recommendation, assembly-level attributes should be applied to the `Startup` class:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ApiControllerAttributeOnAssembly&highlight=1)]

<span data-ttu-id="3a8c2-124">Для использования этого атрибута на уровне сборки требуется версия совместимости 2.2 или более поздняя, заданная с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-124">A compatibility version of 2.2 or later, set via <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>, is required to use this attribute at the assembly level.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="3a8c2-125">Атрибут `[ApiController]` обычно используется вместе с `ControllerBase` для включения связанного с REST поведения для контроллеров.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-125">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="3a8c2-126">`ControllerBase` предоставляет доступ к таким методам, как <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> и <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-126">`ControllerBase` provides access to methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*>.</span></span>

<span data-ttu-id="3a8c2-127">Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-127">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="3a8c2-128">В следующих разделах описаны удобные функции, добавляемые этим атрибутом.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-128">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="3a8c2-129">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="3a8c2-129">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="3a8c2-130">Ошибки проверки модели автоматически активируют отклик HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-130">Model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="3a8c2-131">В результате следующий код становится ненужным в ваших действиях:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-131">Consequently, the following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="3a8c2-132">Используйте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> для настройки выходных данных итогового ответа.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-132">Use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory> to customize the output of the resulting response.</span></span>

<span data-ttu-id="3a8c2-133">Отключение поведения по умолчанию полезно, если ваше действие может восстановиться после ошибки проверки модели.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-133">Disabling the default behavior is useful when your action can recover from a model validation error.</span></span> <span data-ttu-id="3a8c2-134">Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-134">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property is set to `true`.</span></span> <span data-ttu-id="3a8c2-135">Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-135">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=7)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3a8c2-136">Если установлен флаг совместимости 2.2 или более поздней версии, для ответов HTTP 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-136">With a compatibility flag of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="3a8c2-137">Тип `ValidationProblemDetails` соответствует [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="3a8c2-137">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span> <span data-ttu-id="3a8c2-138">Задайте для свойства `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` значение `true`, чтобы ошибка ASP.NET Core 2.1 возвращалась в формате <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-138">Set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` to instead return the ASP.NET Core 2.1 error format of <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="3a8c2-139">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-139">Add the following code in `Startup.ConfigureServices`:</span></span>

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

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="3a8c2-140">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="3a8c2-140">Binding source parameter inference</span></span>

<span data-ttu-id="3a8c2-141">Атрибут источника привязки определяет расположение, в котором находится значение параметра действия.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-141">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="3a8c2-142">Существуют следующие атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-142">The following binding source attributes exist:</span></span>

|<span data-ttu-id="3a8c2-143">Атрибут</span><span class="sxs-lookup"><span data-stu-id="3a8c2-143">Attribute</span></span>|<span data-ttu-id="3a8c2-144">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="3a8c2-144">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="3a8c2-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3a8c2-145">**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**</span></span>     | <span data-ttu-id="3a8c2-146">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="3a8c2-146">Request body</span></span> |
|<span data-ttu-id="3a8c2-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3a8c2-147">**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**</span></span>     | <span data-ttu-id="3a8c2-148">Данные формы в тексте запроса</span><span class="sxs-lookup"><span data-stu-id="3a8c2-148">Form data in the request body</span></span> |
|<span data-ttu-id="3a8c2-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3a8c2-149">**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)**</span></span> | <span data-ttu-id="3a8c2-150">Заголовок запроса</span><span class="sxs-lookup"><span data-stu-id="3a8c2-150">Request header</span></span> |
|<span data-ttu-id="3a8c2-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3a8c2-151">**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**</span></span>   | <span data-ttu-id="3a8c2-152">Параметры строки запроса для запроса</span><span class="sxs-lookup"><span data-stu-id="3a8c2-152">Request query string parameter</span></span> |
|<span data-ttu-id="3a8c2-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span><span class="sxs-lookup"><span data-stu-id="3a8c2-153">**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**</span></span>   | <span data-ttu-id="3a8c2-154">Данные маршрута из текущего запроса</span><span class="sxs-lookup"><span data-stu-id="3a8c2-154">Route data from the current request</span></span> |
|<span data-ttu-id="3a8c2-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="3a8c2-155">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="3a8c2-156">Служба запросов, внедренная в качестве параметра действия</span><span class="sxs-lookup"><span data-stu-id="3a8c2-156">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="3a8c2-157">Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`).</span><span class="sxs-lookup"><span data-stu-id="3a8c2-157">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="3a8c2-158">Для `%2f` не будет применяться отмена экранирования `/`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-158">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="3a8c2-159">Используйте `[FromQuery]`, если значение может содержать `%2f`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-159">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="3a8c2-160">Без атрибута `[ApiController]` атрибуты источника привязки определяются явно.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-160">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="3a8c2-161">В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-161">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="3a8c2-162">Правила зависимости применяются к источникам данных по умолчанию для параметров действий.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-162">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="3a8c2-163">Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-163">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="3a8c2-164">Атрибуты источника привязки работают следующим образом.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-164">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="3a8c2-165">**[FromBody]**  выводится для параметров сложного типа.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-165">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="3a8c2-166">Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как <xref:Microsoft.AspNetCore.Http.IFormCollection> и <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-166">An exception to this rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="3a8c2-167">Код определения источника привязки игнорирует эти особые типы.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-167">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="3a8c2-168">`[FromBody]` не определен для простых типов, таких как `string` или `int`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-168">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="3a8c2-169">Таким образом, атрибут `[FromBody]` должен использоваться для простых типов, когда требуются эти функции.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-169">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span> <span data-ttu-id="3a8c2-170">Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-170">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="3a8c2-171">Например, следующие сигнатуры действия вызывают исключение:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-171">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="3a8c2-172">**[FromForm]** выводится для параметров действия с типом <xref:Microsoft.AspNetCore.Http.IFormFile> и <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-172">**[FromForm]** is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="3a8c2-173">Он не выводится ни для каких простых или определяемых пользователем типов.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-173">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="3a8c2-174">**[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-174">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="3a8c2-175">Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-175">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="3a8c2-176">**[FromQuery]**  выводится для любых других параметров действия.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-176">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="3a8c2-177">Правила зависимости по умолчанию отключаются, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-177">The default inference rules are disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> property is set to `true`.</span></span> <span data-ttu-id="3a8c2-178">Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-178">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_<version_number>);`:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=6)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="3a8c2-179">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="3a8c2-179">Multipart/form-data request inference</span></span>

<span data-ttu-id="3a8c2-180">Когда параметр действия аннотирован атрибутом [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute), выводится тип содержимого запроса `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-180">When an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="3a8c2-181">Поведение по умолчанию отключается, если свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-181">The default behavior is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property is set to `true`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3a8c2-182">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-182">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="3a8c2-183">Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-183">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="attribute-routing-requirement"></a><span data-ttu-id="3a8c2-184">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="3a8c2-184">Attribute routing requirement</span></span>

<span data-ttu-id="3a8c2-185">Маршрутизация атрибутов стала обязательным требованием.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-185">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="3a8c2-186">Пример:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-186">For example:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.21/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="3a8c2-187">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или с помощью <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-187">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="problem-details-responses-for-error-status-codes"></a><span data-ttu-id="3a8c2-188">Отклики со сведениями о проблемах для кодов состояния ошибки</span><span class="sxs-lookup"><span data-stu-id="3a8c2-188">Problem details responses for error status codes</span></span>

<span data-ttu-id="3a8c2-189">В ASP.NET Core 2.2 или более поздней версии MVC преобразовывает код ошибки (код состояния 400 или выше) в результат с <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-189">In ASP.NET Core 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="3a8c2-190">`ProblemDetails` является:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-190">`ProblemDetails` is:</span></span>

* <span data-ttu-id="3a8c2-191">типом на основе [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807);</span><span class="sxs-lookup"><span data-stu-id="3a8c2-191">A type based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>
* <span data-ttu-id="3a8c2-192">стандартным форматом ответа HTTP, в котором указываются сведения об ошибке в машиночитаемом виде.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-192">A standardized format for specifying machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="3a8c2-193">Рассмотрим следующий код в действии контроллера:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-193">Consider the following code in a controller action:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Controllers/ProductsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="3a8c2-194">Ответ HTTP для `NotFound` содержит код состояния 404 с текстом `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-194">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="3a8c2-195">Пример:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-195">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

<span data-ttu-id="3a8c2-196">Для функции сведений о проблеме необходим флаг совместимости 2.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-196">The problem details feature requires a compatibility flag of 2.2 or later.</span></span> <span data-ttu-id="3a8c2-197">Поведение по умолчанию отключается, если свойству `SuppressMapClientErrors` задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-197">The default behavior is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="3a8c2-198">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-198">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=8)]

<span data-ttu-id="3a8c2-199">Используйте свойство `ClientErrorMapping`, чтобы настроить содержимое ответа `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="3a8c2-199">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="3a8c2-200">Например, в следующем коде показано обновление свойства `type` для откликов 404:</span><span class="sxs-lookup"><span data-stu-id="3a8c2-200">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](define-controller/samples/WebApiSample.Api.22/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3a8c2-201">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3a8c2-201">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
