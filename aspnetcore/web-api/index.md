---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 07/06/2018
uid: web-api/index
ms.openlocfilehash: ccee4f7bae0abe1b36088d58e5c1e1362d8de9f0
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243101"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="78ef8-103">Сборка веб-API с использованием ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78ef8-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="78ef8-104">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="78ef8-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="78ef8-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78ef8-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="78ef8-106">Этот документ содержит сведения о сборке веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из функций.</span><span class="sxs-lookup"><span data-stu-id="78ef8-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="78ef8-107">Создание класса, производного от ControllerBase</span><span class="sxs-lookup"><span data-stu-id="78ef8-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="78ef8-108">Вы можете наследовать от класса [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) в контроллере, который предназначен для работы в качестве веб-API.</span><span class="sxs-lookup"><span data-stu-id="78ef8-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="78ef8-109">Пример:</span><span class="sxs-lookup"><span data-stu-id="78ef8-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

<span data-ttu-id="78ef8-110">Класс `ControllerBase` предоставляет доступ к нескольким свойствам и методам.</span><span class="sxs-lookup"><span data-stu-id="78ef8-110">The `ControllerBase` class provides access to several properties and methods.</span></span> <span data-ttu-id="78ef8-111">В предыдущем коде примерами методов являются [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) и [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="78ef8-111">In the preceding code, examples include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="78ef8-112">Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201 соответственно.</span><span class="sxs-lookup"><span data-stu-id="78ef8-112">These methods are called within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="78ef8-113">Обращение к свойству [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.</span><span class="sxs-lookup"><span data-stu-id="78ef8-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to handle request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="78ef8-114">Аннотирование класса атрибутом ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="78ef8-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="78ef8-115">В ASP.NET Core 2.1 появился атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), обозначающий класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="78ef8-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="78ef8-116">Пример:</span><span class="sxs-lookup"><span data-stu-id="78ef8-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="78ef8-117">Для использования этого атрибута требуется версия совместимости 2.1 или более поздняя, заданная с помощью [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion).</span><span class="sxs-lookup"><span data-stu-id="78ef8-117">A compatibility version of 2.1 or later, set via [SetCompatibilityVersion](/dotnet/api/microsoft.extensions.dependencyinjection.mvccoremvcbuilderextensions.setcompatibilityversion), is required to use this attribute.</span></span> <span data-ttu-id="78ef8-118">Например, выделенный код в *Startup.ConfigureServices* задает флаг совместимости 2.1:</span><span class="sxs-lookup"><span data-stu-id="78ef8-118">For example, the highlighted code in *Startup.ConfigureServices* sets the 2.1 compatibility flag:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

<span data-ttu-id="78ef8-119">Атрибут `[ApiController]` обычно используется вместе с `ControllerBase` для включения связанного с REST поведения для контроллеров.</span><span class="sxs-lookup"><span data-stu-id="78ef8-119">The `[ApiController]` attribute is commonly coupled with `ControllerBase` to enable REST-specific behavior for controllers.</span></span> <span data-ttu-id="78ef8-120">`ControllerBase` предоставляет доступ к таким методам, как [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) и [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="78ef8-120">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="78ef8-121">Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="78ef8-121">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="78ef8-122">В следующих разделах описаны удобные функции, добавляемые этим атрибутом.</span><span class="sxs-lookup"><span data-stu-id="78ef8-122">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="78ef8-123">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="78ef8-123">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="78ef8-124">Ошибки проверки автоматически активируют отклик HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="78ef8-124">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="78ef8-125">Следующий код становится ненужным в ваших действиях:</span><span class="sxs-lookup"><span data-stu-id="78ef8-125">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<span data-ttu-id="78ef8-126">Поведение по умолчанию отключается, если свойству [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="78ef8-126">The default behavior is disabled when the [SuppressModelStateInvalidFilter](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressmodelstateinvalidfilter) property is set to `true`.</span></span> <span data-ttu-id="78ef8-127">Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="78ef8-127">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="78ef8-128">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="78ef8-128">Binding source parameter inference</span></span>

<span data-ttu-id="78ef8-129">Атрибут источника привязки определяет расположение, в котором находится значение параметра действия.</span><span class="sxs-lookup"><span data-stu-id="78ef8-129">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="78ef8-130">Существуют следующие атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="78ef8-130">The following binding source attributes exist:</span></span>

|<span data-ttu-id="78ef8-131">Атрибут</span><span class="sxs-lookup"><span data-stu-id="78ef8-131">Attribute</span></span>|<span data-ttu-id="78ef8-132">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="78ef8-132">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="78ef8-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="78ef8-133">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="78ef8-134">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="78ef8-134">Request body</span></span> |
|<span data-ttu-id="78ef8-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="78ef8-135">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="78ef8-136">Данные формы в тексте запроса</span><span class="sxs-lookup"><span data-stu-id="78ef8-136">Form data in the request body</span></span> |
|<span data-ttu-id="78ef8-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="78ef8-137">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="78ef8-138">Заголовок запроса</span><span class="sxs-lookup"><span data-stu-id="78ef8-138">Request header</span></span> |
|<span data-ttu-id="78ef8-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="78ef8-139">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="78ef8-140">Параметры строки запроса для запроса</span><span class="sxs-lookup"><span data-stu-id="78ef8-140">Request query string parameter</span></span> |
|<span data-ttu-id="78ef8-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="78ef8-141">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="78ef8-142">Данные маршрута из текущего запроса</span><span class="sxs-lookup"><span data-stu-id="78ef8-142">Route data from the current request</span></span> |
|<span data-ttu-id="78ef8-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="78ef8-143">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="78ef8-144">Служба запросов, внедренная в качестве параметра действия</span><span class="sxs-lookup"><span data-stu-id="78ef8-144">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="78ef8-145">Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`).</span><span class="sxs-lookup"><span data-stu-id="78ef8-145">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="78ef8-146">Для `%2f` не будет применяться отмена экранирования `/`.</span><span class="sxs-lookup"><span data-stu-id="78ef8-146">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="78ef8-147">Используйте `[FromQuery]`, если значение может содержать `%2f`.</span><span class="sxs-lookup"><span data-stu-id="78ef8-147">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="78ef8-148">Без атрибута `[ApiController]` атрибуты источника привязки определяются явно.</span><span class="sxs-lookup"><span data-stu-id="78ef8-148">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="78ef8-149">В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:</span><span class="sxs-lookup"><span data-stu-id="78ef8-149">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="78ef8-150">Правила зависимости применяются к источникам данных по умолчанию для параметров действий.</span><span class="sxs-lookup"><span data-stu-id="78ef8-150">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="78ef8-151">Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия.</span><span class="sxs-lookup"><span data-stu-id="78ef8-151">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="78ef8-152">Атрибуты источника привязки работают следующим образом.</span><span class="sxs-lookup"><span data-stu-id="78ef8-152">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="78ef8-153">**[FromBody]**  выводится для параметров сложного типа.</span><span class="sxs-lookup"><span data-stu-id="78ef8-153">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="78ef8-154">Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) и [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="78ef8-154">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="78ef8-155">Код определения источника привязки игнорирует эти особые типы.</span><span class="sxs-lookup"><span data-stu-id="78ef8-155">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="78ef8-156">Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="78ef8-156">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="78ef8-157">Например, следующие сигнатуры действия вызывают исключение:</span><span class="sxs-lookup"><span data-stu-id="78ef8-157">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="78ef8-158">**[FromForm]** выводится для параметров действия с типом [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="78ef8-158">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="78ef8-159">Он не выводится ни для каких простых или определяемых пользователем типов.</span><span class="sxs-lookup"><span data-stu-id="78ef8-159">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="78ef8-160">**[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="78ef8-160">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="78ef8-161">Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="78ef8-161">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="78ef8-162">**[FromQuery]**  выводится для любых других параметров действия.</span><span class="sxs-lookup"><span data-stu-id="78ef8-162">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="78ef8-163">Правила зависимости по умолчанию отключаются, если свойству [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="78ef8-163">The default inference rules are disabled when the [SuppressInferBindingSourcesForParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressinferbindingsourcesforparameters) property is set to `true`.</span></span> <span data-ttu-id="78ef8-164">Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="78ef8-164">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="78ef8-165">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="78ef8-165">Multipart/form-data request inference</span></span>

<span data-ttu-id="78ef8-166">Когда параметр действия аннотирован атрибутом [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), выводится тип содержимого запроса `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="78ef8-166">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="78ef8-167">Поведение по умолчанию отключается, если свойству [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="78ef8-167">The default behavior is disabled when the [SuppressConsumesConstraintForFormFileParameters](/dotnet/api/microsoft.aspnetcore.mvc.apibehavioroptions.suppressconsumesconstraintforformfileparameters) property is set to `true`.</span></span> <span data-ttu-id="78ef8-168">Добавьте следующий код в *Startup.ConfigureServices* после `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span><span class="sxs-lookup"><span data-stu-id="78ef8-168">Add the following code in *Startup.ConfigureServices* after `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);`:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="78ef8-169">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="78ef8-169">Attribute routing requirement</span></span>

<span data-ttu-id="78ef8-170">Маршрутизация атрибутов стала обязательным требованием.</span><span class="sxs-lookup"><span data-stu-id="78ef8-170">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="78ef8-171">Пример:</span><span class="sxs-lookup"><span data-stu-id="78ef8-171">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="78ef8-172">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) или с помощью [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) в *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="78ef8-172">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="78ef8-173">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78ef8-173">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
