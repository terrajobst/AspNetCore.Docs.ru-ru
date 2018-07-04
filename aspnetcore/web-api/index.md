---
title: Сборка веб-API с использованием ASP.NET Core
author: scottaddie
description: Сведения о функциях, доступных для сборки веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из них.
ms.author: scaddie
ms.custom: mvc
ms.date: 04/24/2018
uid: web-api/index
ms.openlocfilehash: ab672667d1ca349d80c4ca80f8d1f32f4871c7e6
ms.sourcegitcommit: 18339e3cb5a891a3ca36d8146fa83cf91c32e707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/04/2018
ms.locfileid: "36274970"
---
# <a name="build-web-apis-with-aspnet-core"></a><span data-ttu-id="9de4d-103">Сборка веб-API с использованием ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9de4d-103">Build web APIs with ASP.NET Core</span></span>

<span data-ttu-id="9de4d-104">Автор: [Скотт Адди](https://github.com/scottaddie) (Scott Addie)</span><span class="sxs-lookup"><span data-stu-id="9de4d-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="9de4d-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9de4d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9de4d-106">Этот документ содержит сведения о сборке веб-API в ASP.NET Core, и о ситуациях, в которых уместно использовать каждую из функций.</span><span class="sxs-lookup"><span data-stu-id="9de4d-106">This document explains how to build a web API in ASP.NET Core and when it's most appropriate to use each feature.</span></span>

## <a name="derive-class-from-controllerbase"></a><span data-ttu-id="9de4d-107">Создание класса, производного от ControllerBase</span><span class="sxs-lookup"><span data-stu-id="9de4d-107">Derive class from ControllerBase</span></span>

<span data-ttu-id="9de4d-108">Вы можете наследовать от класса [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) в контроллере, который предназначен для работы в качестве веб-API.</span><span class="sxs-lookup"><span data-stu-id="9de4d-108">Inherit from the [ControllerBase](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase) class in a controller that's intended to serve as a web API.</span></span> <span data-ttu-id="9de4d-109">Пример:</span><span class="sxs-lookup"><span data-stu-id="9de4d-109">For example:</span></span>

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]
::: moniker-end

<span data-ttu-id="9de4d-110">Класс `ControllerBase` предоставляет доступ к множеству свойств и методов.</span><span class="sxs-lookup"><span data-stu-id="9de4d-110">The `ControllerBase` class provides access to numerous properties and methods.</span></span> <span data-ttu-id="9de4d-111">В предыдущем примере некоторые такие методы включают [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) и [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span><span class="sxs-lookup"><span data-stu-id="9de4d-111">In the preceding example, some such methods include [BadRequest](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.badrequest) and [CreatedAtAction](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdataction).</span></span> <span data-ttu-id="9de4d-112">Эти методы вызываются в методах действия для возврата кодов состояния HTTP 400 и 201, соответственно.</span><span class="sxs-lookup"><span data-stu-id="9de4d-112">These methods are invoked within action methods to return HTTP 400 and 201 status codes, respectively.</span></span> <span data-ttu-id="9de4d-113">Обращение к свойству [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate), также предоставляемому `ControllerBase`, выполняется для проверки модели запросов.</span><span class="sxs-lookup"><span data-stu-id="9de4d-113">The [ModelState](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.modelstate) property, also provided by `ControllerBase`, is accessed to perform request model validation.</span></span>

::: moniker range=">= aspnetcore-2.1"
## <a name="annotate-class-with-apicontrollerattribute"></a><span data-ttu-id="9de4d-114">Аннотирование класса атрибутом ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="9de4d-114">Annotate class with ApiControllerAttribute</span></span>

<span data-ttu-id="9de4d-115">В ASP.NET Core 2.1 появился атрибут [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute), обозначающий класс контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="9de4d-115">ASP.NET Core 2.1 introduces the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute to denote a web API controller class.</span></span> <span data-ttu-id="9de4d-116">Пример:</span><span class="sxs-lookup"><span data-stu-id="9de4d-116">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="9de4d-117">Этот атрибут обычно используется вместе с `ControllerBase` для получения доступа к полезным методам и свойствам.</span><span class="sxs-lookup"><span data-stu-id="9de4d-117">This attribute is commonly coupled with `ControllerBase` to gain access to useful methods and properties.</span></span> <span data-ttu-id="9de4d-118">`ControllerBase` предоставляет доступ к таким методам, как [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) и [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span><span class="sxs-lookup"><span data-stu-id="9de4d-118">`ControllerBase` provides access to methods such as [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) and [File](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.file).</span></span>

<span data-ttu-id="9de4d-119">Другой подход заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`:</span><span class="sxs-lookup"><span data-stu-id="9de4d-119">Another approach is to create a custom base controller class annotated with the `[ApiController]` attribute:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

<span data-ttu-id="9de4d-120">В следующих разделах описаны удобные функции, добавляемые этим атрибутом.</span><span class="sxs-lookup"><span data-stu-id="9de4d-120">The following sections describe convenience features added by the attribute.</span></span>

### <a name="automatic-http-400-responses"></a><span data-ttu-id="9de4d-121">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="9de4d-121">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="9de4d-122">Ошибки проверки автоматически активируют отклик HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="9de4d-122">Validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="9de4d-123">Следующий код становится ненужным в ваших действиях:</span><span class="sxs-lookup"><span data-stu-id="9de4d-123">The following code becomes unnecessary in your actions:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?range=46-49)]

<span data-ttu-id="9de4d-124">Это поведение по умолчанию отключено с помощью следующего кода в *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="9de4d-124">This default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a><span data-ttu-id="9de4d-125">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="9de4d-125">Binding source parameter inference</span></span>

<span data-ttu-id="9de4d-126">Атрибут источника привязки определяет расположение, в котором находится значение параметра действия.</span><span class="sxs-lookup"><span data-stu-id="9de4d-126">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="9de4d-127">Существуют следующие атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="9de4d-127">The following binding source attributes exist:</span></span>

|<span data-ttu-id="9de4d-128">Атрибут</span><span class="sxs-lookup"><span data-stu-id="9de4d-128">Attribute</span></span>|<span data-ttu-id="9de4d-129">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="9de4d-129">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="9de4d-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span><span class="sxs-lookup"><span data-stu-id="9de4d-130">**[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)**</span></span>     | <span data-ttu-id="9de4d-131">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="9de4d-131">Request body</span></span> |
|<span data-ttu-id="9de4d-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span><span class="sxs-lookup"><span data-stu-id="9de4d-132">**[[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute)**</span></span>     | <span data-ttu-id="9de4d-133">Данные формы в тексте запроса</span><span class="sxs-lookup"><span data-stu-id="9de4d-133">Form data in the request body</span></span> |
|<span data-ttu-id="9de4d-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span><span class="sxs-lookup"><span data-stu-id="9de4d-134">**[[FromHeader]](/dotnet/api/microsoft.aspnetcore.mvc.fromheaderattribute)**</span></span> | <span data-ttu-id="9de4d-135">Заголовок запроса</span><span class="sxs-lookup"><span data-stu-id="9de4d-135">Request header</span></span> |
|<span data-ttu-id="9de4d-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span><span class="sxs-lookup"><span data-stu-id="9de4d-136">**[[FromQuery]](/dotnet/api/microsoft.aspnetcore.mvc.fromqueryattribute)**</span></span>   | <span data-ttu-id="9de4d-137">Параметры строки запроса для запроса</span><span class="sxs-lookup"><span data-stu-id="9de4d-137">Request query string parameter</span></span> |
|<span data-ttu-id="9de4d-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span><span class="sxs-lookup"><span data-stu-id="9de4d-138">**[[FromRoute]](/dotnet/api/microsoft.aspnetcore.mvc.fromrouteattribute)**</span></span>   | <span data-ttu-id="9de4d-139">Данные маршрута из текущего запроса</span><span class="sxs-lookup"><span data-stu-id="9de4d-139">Route data from the current request</span></span> |
|<span data-ttu-id="9de4d-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span><span class="sxs-lookup"><span data-stu-id="9de4d-140">**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)**</span></span> | <span data-ttu-id="9de4d-141">Служба запросов, внедренная в качестве параметра действия</span><span class="sxs-lookup"><span data-stu-id="9de4d-141">The request service injected as an action parameter</span></span> |

> [!NOTE]
> <span data-ttu-id="9de4d-142">**Не** используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`), так как для `%2f` не будет применяться отмена экранирования (`/`).</span><span class="sxs-lookup"><span data-stu-id="9de4d-142">Do **not** use `[FromRoute]` when values might contain `%2f` (that is `/`) because `%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="9de4d-143">Используйте `[FromQuery]`, если значение может содержать `%2f`.</span><span class="sxs-lookup"><span data-stu-id="9de4d-143">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="9de4d-144">Без атрибута `[ApiController]` атрибуты источника привязки определяются явно.</span><span class="sxs-lookup"><span data-stu-id="9de4d-144">Without the `[ApiController]` attribute, binding source attributes are explicitly defined.</span></span> <span data-ttu-id="9de4d-145">В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:</span><span class="sxs-lookup"><span data-stu-id="9de4d-145">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=2)]

<span data-ttu-id="9de4d-146">Правила зависимости применяются к источникам данных по умолчанию для параметров действий.</span><span class="sxs-lookup"><span data-stu-id="9de4d-146">Inference rules are applied for the default data sources of action parameters.</span></span> <span data-ttu-id="9de4d-147">Эти правила настраивают те источники привязки, которые в противном случае вы, скорее всего, вручную применили бы к параметрам действия.</span><span class="sxs-lookup"><span data-stu-id="9de4d-147">These rules configure the binding sources you're otherwise likely to manually apply to the action parameters.</span></span> <span data-ttu-id="9de4d-148">Атрибуты источника привязки работают следующим образом.</span><span class="sxs-lookup"><span data-stu-id="9de4d-148">The binding source attributes behave as follows:</span></span>

* <span data-ttu-id="9de4d-149">**[FromBody]**  выводится для параметров сложного типа.</span><span class="sxs-lookup"><span data-stu-id="9de4d-149">**[FromBody]** is inferred for complex type parameters.</span></span> <span data-ttu-id="9de4d-150">Исключением из этого правила является любой сложный встроенный тип со специальным значением, такой как [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) и [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span><span class="sxs-lookup"><span data-stu-id="9de4d-150">An exception to this rule is any complex, built-in type with a special meaning, such as [IFormCollection](/dotnet/api/microsoft.aspnetcore.http.iformcollection) and [CancellationToken](/dotnet/api/system.threading.cancellationtoken).</span></span> <span data-ttu-id="9de4d-151">Код определения источника привязки игнорирует эти особые типы.</span><span class="sxs-lookup"><span data-stu-id="9de4d-151">The binding source inference code ignores those special types.</span></span> <span data-ttu-id="9de4d-152">Когда для действия явно задано более одного параметра (через `[FromBody]`) или оно выводится как привязанное из текста запроса, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="9de4d-152">When an action has more than one parameter explicitly specified (via `[FromBody]`) or inferred as bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="9de4d-153">Например, следующие сигнатуры действия вызывают исключение:</span><span class="sxs-lookup"><span data-stu-id="9de4d-153">For example, the following action signatures cause an exception:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* <span data-ttu-id="9de4d-154">**[FromForm]** выводится для параметров действия с типом [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) и [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span><span class="sxs-lookup"><span data-stu-id="9de4d-154">**[FromForm]** is inferred for action parameters of type [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [IFormFileCollection](/dotnet/api/microsoft.aspnetcore.http.iformfilecollection).</span></span> <span data-ttu-id="9de4d-155">Он не выводится ни для каких простых или определяемых пользователем типов.</span><span class="sxs-lookup"><span data-stu-id="9de4d-155">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="9de4d-156">**[FromRoute]**  выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="9de4d-156">**[FromRoute]** is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="9de4d-157">Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="9de4d-157">When multiple routes match an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="9de4d-158">**[FromQuery]**  выводится для любых других параметров действия.</span><span class="sxs-lookup"><span data-stu-id="9de4d-158">**[FromQuery]** is inferred for any other action parameters.</span></span>

<span data-ttu-id="9de4d-159">Правила зависимости по умолчанию отключены с помощью следующего кода в *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="9de4d-159">The default inference rules are disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a><span data-ttu-id="9de4d-160">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="9de4d-160">Multipart/form-data request inference</span></span>

<span data-ttu-id="9de4d-161">Когда параметр действия аннотирован атрибутом [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute), выводится тип содержимого запроса `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="9de4d-161">When an action parameter is annotated with the [[FromForm]](/dotnet/api/microsoft.aspnetcore.mvc.fromformattribute) attribute, the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="9de4d-162">Это поведение по умолчанию отключено с помощью следующего кода в *Startup.ConfigureServices*:</span><span class="sxs-lookup"><span data-stu-id="9de4d-162">The default behavior is disabled with the following code in *Startup.ConfigureServices*:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a><span data-ttu-id="9de4d-163">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="9de4d-163">Attribute routing requirement</span></span>

<span data-ttu-id="9de4d-164">Маршрутизация атрибутов стала обязательным требованием.</span><span class="sxs-lookup"><span data-stu-id="9de4d-164">Attribute routing becomes a requirement.</span></span> <span data-ttu-id="9de4d-165">Пример:</span><span class="sxs-lookup"><span data-stu-id="9de4d-165">For example:</span></span>

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="9de4d-166">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определенные в [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) или с помощью [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) в *Startup.Configure*.</span><span class="sxs-lookup"><span data-stu-id="9de4d-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined in [UseMvc](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvc#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvc_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Action_Microsoft_AspNetCore_Routing_IRouteBuilder__) or by [UseMvcWithDefaultRoute](/dotnet/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions.usemvcwithdefaultroute#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) in *Startup.Configure*.</span></span>
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9de4d-167">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9de4d-167">Additional resources</span></span>

* [<span data-ttu-id="9de4d-168">Типы возвращаемых значений действий контроллера</span><span class="sxs-lookup"><span data-stu-id="9de4d-168">Controller action return types</span></span>](xref:web-api/action-return-types)
* [<span data-ttu-id="9de4d-169">Пользовательские модули форматирования</span><span class="sxs-lookup"><span data-stu-id="9de4d-169">Custom formatters</span></span>](xref:web-api/advanced/custom-formatters)
* [<span data-ttu-id="9de4d-170">Форматирование данных ответа</span><span class="sxs-lookup"><span data-stu-id="9de4d-170">Format response data</span></span>](xref:web-api/advanced/formatting)
* [<span data-ttu-id="9de4d-171">Страницы справки с использованием Swagger</span><span class="sxs-lookup"><span data-stu-id="9de4d-171">Help pages using Swagger</span></span>](xref:tutorials/web-api-help-pages-using-swagger)
* [<span data-ttu-id="9de4d-172">Маршрутизация к действиям контроллера</span><span class="sxs-lookup"><span data-stu-id="9de4d-172">Routing to controller actions</span></span>](xref:mvc/controllers/routing)
