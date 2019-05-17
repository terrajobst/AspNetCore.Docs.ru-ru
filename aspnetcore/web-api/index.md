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
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="575ff-103">Создание веб-API с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="575ff-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="575ff-104">Авторы: [Скотт Адди](https://github.com/scottaddie) (Scott Addie) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="575ff-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="575ff-105">ASP.NET Core поддерживает создание служб RESTful, также известных как веб-API, с помощью C#.</span><span class="sxs-lookup"><span data-stu-id="575ff-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="575ff-106">Для обработки запросов веб-API использует контроллеры.</span><span class="sxs-lookup"><span data-stu-id="575ff-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="575ff-107">В веб-API *контроллеры* — это классы, производные от `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="575ff-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="575ff-108">В этой статье показано, как использовать контроллеры для обработки запросов API.</span><span class="sxs-lookup"><span data-stu-id="575ff-108">This article shows how to use controllers for handling API requests.</span></span>

<span data-ttu-id="575ff-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="575ff-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="575ff-110">([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)</span><span class="sxs-lookup"><span data-stu-id="575ff-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="575ff-111">Класс ControllerBase</span><span class="sxs-lookup"><span data-stu-id="575ff-111">ControllerBase class</span></span>

<span data-ttu-id="575ff-112">Веб-API имеет один или несколько классов контроллера, которые являются производными от <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="575ff-112">A web API has one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="575ff-113">Например, шаблон проекта веб-API создает контроллер значений:</span><span class="sxs-lookup"><span data-stu-id="575ff-113">For example, the web API project template creates a Values controller:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=3)]

<span data-ttu-id="575ff-114">Не создавайте контроллер веб-API путем наследования от базового класса <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="575ff-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class.</span></span> <span data-ttu-id="575ff-115">`Controller` является производным от `ControllerBase` и добавляет поддержку для представлений, обеспечивая обработку веб-страниц, а не запросов веб-API.</span><span class="sxs-lookup"><span data-stu-id="575ff-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span>  <span data-ttu-id="575ff-116">Существует одно исключение из этого правила: если вы планируете использовать один и тот же контроллер для представлений и API, сделайте его производным от `Controller`.</span><span class="sxs-lookup"><span data-stu-id="575ff-116">There's an exception to this rule: if you plan to use the same controller for both views and APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="575ff-117">Класс `ControllerBase` предоставляет множество свойств и методов, которые удобны для обработки HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="575ff-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="575ff-118">Например, `ControllerBase.CreatedAtAction` возвращает код состояния 201.</span><span class="sxs-lookup"><span data-stu-id="575ff-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=8-9)]

 <span data-ttu-id="575ff-119">Вот еще примеры методов, предоставляющих `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="575ff-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="575ff-120">Метод</span><span class="sxs-lookup"><span data-stu-id="575ff-120">Method</span></span>  |<span data-ttu-id="575ff-121">Примечания</span><span class="sxs-lookup"><span data-stu-id="575ff-121">Notes</span></span>  |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="575ff-122">Возвращает код состояния 400.</span><span class="sxs-lookup"><span data-stu-id="575ff-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> |<span data-ttu-id="575ff-123">Возвращает код состояния 404.</span><span class="sxs-lookup"><span data-stu-id="575ff-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="575ff-124">Возвращает файл.</span><span class="sxs-lookup"><span data-stu-id="575ff-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="575ff-125">Вызывает [привязку модели](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="575ff-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="575ff-126">Вызывает [проверку модели](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="575ff-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="575ff-127">Список всех доступных методов и свойств см. здесь: <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="575ff-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="575ff-128">Атрибуты</span><span class="sxs-lookup"><span data-stu-id="575ff-128">Attributes</span></span>

<span data-ttu-id="575ff-129">Пространство имен <xref:Microsoft.AspNetCore.Mvc> предоставляет атрибуты, которые можно использовать для настройки поведения контроллеров и методов действия веб-API.</span><span class="sxs-lookup"><span data-stu-id="575ff-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="575ff-130">В следующем примере с помощью атрибутов указан принятый метод HTTP и возвращаемые коды состояния.</span><span class="sxs-lookup"><span data-stu-id="575ff-130">The following example uses attributes to specify the HTTP method accepted and the status codes returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="575ff-131">Вот еще примеры доступных атрибутов.</span><span class="sxs-lookup"><span data-stu-id="575ff-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="575ff-132">Атрибут</span><span class="sxs-lookup"><span data-stu-id="575ff-132">Attribute</span></span>|<span data-ttu-id="575ff-133">Примечания</span><span class="sxs-lookup"><span data-stu-id="575ff-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="575ff-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="575ff-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="575ff-135">Определяет шаблон URL-адреса для контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="575ff-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="575ff-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="575ff-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="575ff-137">Задает префикс и свойства, которые добавляются для привязки модели.</span><span class="sxs-lookup"><span data-stu-id="575ff-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="575ff-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="575ff-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="575ff-139">Определяет действие, которое поддерживает метод HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="575ff-139">Identifies an action that supports the HTTP GET method.</span></span>|
|<span data-ttu-id="575ff-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="575ff-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="575ff-141">Указывает типы данных, которые принимает действие.</span><span class="sxs-lookup"><span data-stu-id="575ff-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="575ff-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="575ff-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="575ff-143">Указывает типы данных, которые возвращает действие.</span><span class="sxs-lookup"><span data-stu-id="575ff-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="575ff-144">Список доступных атрибутов см. в пространстве имен <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="575ff-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="575ff-145">Атрибут ApiController</span><span class="sxs-lookup"><span data-stu-id="575ff-145">ApiController attribute</span></span>

<span data-ttu-id="575ff-146">Атрибут [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) можно применить к классу контроллера для включения определенных схем поведения API:</span><span class="sxs-lookup"><span data-stu-id="575ff-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable API-specific behaviors:</span></span>

* [<span data-ttu-id="575ff-147">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="575ff-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="575ff-148">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="575ff-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="575ff-149">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="575ff-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="575ff-150">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="575ff-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="575ff-151">Сведения о проблемах для кодов состояния ошибки</span><span class="sxs-lookup"><span data-stu-id="575ff-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="575ff-152">Для реализации этих функций необходима [версия совместимости](<xref:mvc/compatibility-version>), начиная с 2.1.</span><span class="sxs-lookup"><span data-stu-id="575ff-152">These features require a [compatibility version](<xref:mvc/compatibility-version>) of 2.1 or later.</span></span>

### <a name="apicontroller-on-specific-controllers"></a><span data-ttu-id="575ff-153">Использование ApiController на определенных контроллерах</span><span class="sxs-lookup"><span data-stu-id="575ff-153">ApiController on specific controllers</span></span>

<span data-ttu-id="575ff-154">Атрибут `[ApiController]` может применяться к определенным контроллерам, как показано в следующем примере из шаблона проекта:</span><span class="sxs-lookup"><span data-stu-id="575ff-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=2)]

### <a name="apicontroller-on-multiple-controllers"></a><span data-ttu-id="575ff-155">Использование ApiController на нескольких контроллерах</span><span class="sxs-lookup"><span data-stu-id="575ff-155">ApiController on multiple controllers</span></span>

<span data-ttu-id="575ff-156">Один из подходов к использованию атрибута на более чем одном контроллере заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="575ff-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="575ff-157">Вот пример, демонстрирующий пользовательский базовый класс и производный от него контроллер:</span><span class="sxs-lookup"><span data-stu-id="575ff-157">Here's an example showing a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

### <a name="apicontroller-on-an-assembly"></a><span data-ttu-id="575ff-158">Использование ApiController в сборке</span><span class="sxs-lookup"><span data-stu-id="575ff-158">ApiController on an assembly</span></span>

<span data-ttu-id="575ff-159">Если задана [версия совместимости](<xref:mvc/compatibility-version>) 2.2 или последующая, атрибут `[ApiController]` можно применить к сборке.</span><span class="sxs-lookup"><span data-stu-id="575ff-159">If [compatibility version](<xref:mvc/compatibility-version>) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="575ff-160">Аннотирование этим способом применяет поведение веб-API ко всем контроллерам в сборке.</span><span class="sxs-lookup"><span data-stu-id="575ff-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="575ff-161">Его нельзя отменить для отдельных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="575ff-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="575ff-162">Примените атрибут уровня сборки `Startup` к классу, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="575ff-162">Apply the assembly-level attribute to the `Startup` class as shown in the following example:</span></span>

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

## <a name="attribute-routing-requirement"></a><span data-ttu-id="575ff-163">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="575ff-163">Attribute routing requirement</span></span>

<span data-ttu-id="575ff-164">Атрибут `ApiController` требует обязательной маршрутизации атрибутов.</span><span class="sxs-lookup"><span data-stu-id="575ff-164">The `ApiController` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="575ff-165">Например:</span><span class="sxs-lookup"><span data-stu-id="575ff-165">For example:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_Signature&highlight=1)]

<span data-ttu-id="575ff-166">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определяемые <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="575ff-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

## <a name="automatic-http-400-responses"></a><span data-ttu-id="575ff-167">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="575ff-167">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="575ff-168">Благодаря атрибуту `ApiController` ошибки проверки модели автоматически активируют отклик HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="575ff-168">The `ApiController` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="575ff-169">В результате следующий код ненужен в методе действия:</span><span class="sxs-lookup"><span data-stu-id="575ff-169">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

### <a name="default-badrequest-response"></a><span data-ttu-id="575ff-170">Отклик BadRequest по умолчанию</span><span class="sxs-lookup"><span data-stu-id="575ff-170">Default BadRequest response</span></span> 

<span data-ttu-id="575ff-171">Если задана версия совместимости, начиная с 2.2, для ответов HTTP 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="575ff-171">With a compatibility version of 2.2 or later, the default response type for HTTP 400 responses is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="575ff-172">Тип `ValidationProblemDetails` соответствует [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="575ff-172">The `ValidationProblemDetails` type complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

<span data-ttu-id="575ff-173">Чтобы изменить отклик по умолчанию на <xref:Microsoft.AspNetCore.Mvc.SerializableError>, задайте свойству `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` значение `true` в `Startup.ConfigureServices`, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="575ff-173">To change the default response to <xref:Microsoft.AspNetCore.Mvc.SerializableError>, set the `SuppressUseValidationProblemDetailsForInvalidModelStateResponses` property to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,9)]

### <a name="customize-badrequest-response"></a><span data-ttu-id="575ff-174">Настройка отклика BadRequest</span><span class="sxs-lookup"><span data-stu-id="575ff-174">Customize BadRequest response</span></span>

<span data-ttu-id="575ff-175">Чтобы настроить ответ, полученный в результате ошибки проверки, используйте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span><span class="sxs-lookup"><span data-stu-id="575ff-175">To customize the response that results from a validation error, use <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.InvalidModelStateResponseFactory>.</span></span> <span data-ttu-id="575ff-176">Добавьте выделенный ниже код после `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="575ff-176">Add the following highlighted code after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureBadRequestResponse&highlight=3-20)]

### <a name="log-automatic-400-responses"></a><span data-ttu-id="575ff-177">Запись в журнал автоматических откликов HTTP 400</span><span class="sxs-lookup"><span data-stu-id="575ff-177">Log automatic 400 responses</span></span>

<span data-ttu-id="575ff-178">См. статью об[записи в журнал автоматических откликов HTTP 400 на ошибки проверки модели (aspnet/AspNetCore.Docs № 12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="575ff-178">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400"></a><span data-ttu-id="575ff-179">Отключение автоматической активации отклика HTTP 400</span><span class="sxs-lookup"><span data-stu-id="575ff-179">Disable automatic 400</span></span>

<span data-ttu-id="575ff-180">Чтобы отключить автоматическую активацию отклика HTTP 400, задайте свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="575ff-180">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="575ff-181">Добавьте выделенный ниже код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion`.</span><span class="sxs-lookup"><span data-stu-id="575ff-181">Add the following highlighted code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="575ff-182">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="575ff-182">Binding source parameter inference</span></span>

<span data-ttu-id="575ff-183">Атрибут источника привязки определяет расположение, в котором находится значение параметра действия.</span><span class="sxs-lookup"><span data-stu-id="575ff-183">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="575ff-184">Существуют следующие атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="575ff-184">The following binding source attributes exist:</span></span>

|<span data-ttu-id="575ff-185">Атрибут</span><span class="sxs-lookup"><span data-stu-id="575ff-185">Attribute</span></span>|<span data-ttu-id="575ff-186">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="575ff-186">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="575ff-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="575ff-187">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="575ff-188">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="575ff-188">Request body</span></span> |
|<span data-ttu-id="575ff-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="575ff-189">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="575ff-190">Данные формы в тексте запроса</span><span class="sxs-lookup"><span data-stu-id="575ff-190">Form data in the request body</span></span> |
|<span data-ttu-id="575ff-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="575ff-191">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="575ff-192">Заголовок запроса</span><span class="sxs-lookup"><span data-stu-id="575ff-192">Request header</span></span> |
|<span data-ttu-id="575ff-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="575ff-193">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="575ff-194">Параметры строки запроса для запроса</span><span class="sxs-lookup"><span data-stu-id="575ff-194">Request query string parameter</span></span> |
|<span data-ttu-id="575ff-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="575ff-195">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="575ff-196">Данные маршрута из текущего запроса</span><span class="sxs-lookup"><span data-stu-id="575ff-196">Route data from the current request</span></span> |
|<span data-ttu-id="575ff-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="575ff-197">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="575ff-198">Служба запросов, внедренная в качестве параметра действия</span><span class="sxs-lookup"><span data-stu-id="575ff-198">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="575ff-199">Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`).</span><span class="sxs-lookup"><span data-stu-id="575ff-199">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="575ff-200">Для `%2f` не будет применяться отмена экранирования `/`.</span><span class="sxs-lookup"><span data-stu-id="575ff-200">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="575ff-201">Используйте `[FromQuery]`, если значение может содержать `%2f`.</span><span class="sxs-lookup"><span data-stu-id="575ff-201">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="575ff-202">Без атрибута `[ApiController]` или атрибутов источника привязки, таких как `[FromQuery]`, среда выполнения ASP.NET Core попытается использовать связыватель модели для составного объекта.</span><span class="sxs-lookup"><span data-stu-id="575ff-202">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="575ff-203">Связыватель модели для составного объекта извлекает данные из поставщиков значений в определенном порядке.</span><span class="sxs-lookup"><span data-stu-id="575ff-203">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="575ff-204">В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:</span><span class="sxs-lookup"><span data-stu-id="575ff-204">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="575ff-205">Атрибут `[ApiController]` применяет правила зависимости к источникам данных по умолчанию для параметров действий.</span><span class="sxs-lookup"><span data-stu-id="575ff-205">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="575ff-206">Эти правила избавляют от необходимости вручную определять источники привязки путем применения атрибутов к параметрам действий.</span><span class="sxs-lookup"><span data-stu-id="575ff-206">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="575ff-207">Правила зависимости источника привязки работают следующим образом:</span><span class="sxs-lookup"><span data-stu-id="575ff-207">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="575ff-208">`[FromBody]` выводится для параметров сложного типа.</span><span class="sxs-lookup"><span data-stu-id="575ff-208">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="575ff-209">Исключением из правила зависимости `[FromBody]` является любой сложный встроенный тип со специальным значением, такой как <xref:Microsoft.AspNetCore.Http.IFormCollection> и <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="575ff-209">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="575ff-210">Код определения источника привязки игнорирует эти особые типы.</span><span class="sxs-lookup"><span data-stu-id="575ff-210">The binding source inference code ignores those special types.</span></span> 
* <span data-ttu-id="575ff-211">`[FromForm]` выводится для параметров действия с типом <xref:Microsoft.AspNetCore.Http.IFormFile> и <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="575ff-211">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="575ff-212">Он не выводится ни для каких простых или определяемых пользователем типов.</span><span class="sxs-lookup"><span data-stu-id="575ff-212">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="575ff-213">`[FromRoute]` выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="575ff-213">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="575ff-214">Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="575ff-214">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="575ff-215">`[FromQuery]` выводится для любых других параметров действия.</span><span class="sxs-lookup"><span data-stu-id="575ff-215">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="575ff-216">Заметки о выводе FromBody</span><span class="sxs-lookup"><span data-stu-id="575ff-216">FromBody inference notes</span></span>

<span data-ttu-id="575ff-217">`[FromBody]` не определен для простых типов, таких как `string` или `int`.</span><span class="sxs-lookup"><span data-stu-id="575ff-217">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="575ff-218">Таким образом, атрибут `[FromBody]` должен использоваться для простых типов, когда требуются эти функции.</span><span class="sxs-lookup"><span data-stu-id="575ff-218">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="575ff-219">Если у действия более одного параметра для привязки из текста запроса, выдается исключение.</span><span class="sxs-lookup"><span data-stu-id="575ff-219">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="575ff-220">Например, все следующие сигнатуры метода действия вызывают исключение:</span><span class="sxs-lookup"><span data-stu-id="575ff-220">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="575ff-221">`[FromBody]` выводится для обеих параметров, так как они являются сложными типами.</span><span class="sxs-lookup"><span data-stu-id="575ff-221">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="575ff-222">Атрибут `[FromBody]`, заданный одному параметру, выводится для другого, так как это сложный тип.</span><span class="sxs-lookup"><span data-stu-id="575ff-222">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="575ff-223">Атрибут `[FromBody]` выводится для обоих параметров.</span><span class="sxs-lookup"><span data-stu-id="575ff-223">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

> [!NOTE]
> <span data-ttu-id="575ff-224">В ASP.NET Core 2.1 параметры типа коллекции, такие как списки и массивы, ошибочно выводятся как `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="575ff-224">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="575ff-225">Для этих параметров следует использовать атрибут `[FromBody]`, если они должны быть привязаны из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="575ff-225">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="575ff-226">Это поведение, при котором параметры типа коллекции выводятся для привязки из текста по умолчанию, исправлено в версии ASP.NET Core, начиная с 2.2.</span><span class="sxs-lookup"><span data-stu-id="575ff-226">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

### <a name="disable-inference-rules"></a><span data-ttu-id="575ff-227">Отключение правил зависимости</span><span class="sxs-lookup"><span data-stu-id="575ff-227">Disable inference rules</span></span>

<span data-ttu-id="575ff-228">Чтобы отключить вывод источника привязки, задайте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="575ff-228">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="575ff-229">Добавьте следующий код в `Startup.ConfigureServices` после `services.AddMvc().SetCompatibilityVersion`:</span><span class="sxs-lookup"><span data-stu-id="575ff-229">Add the following code in `Startup.ConfigureServices` after `services.AddMvc().SetCompatibilityVersion`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="575ff-230">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="575ff-230">Multipart/form-data request inference</span></span>

<span data-ttu-id="575ff-231">Атрибут `[ApiController]` применяет правило зависимости, когда параметр действия аннотирован атрибутом [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute). При этом выводится тип содержимого запроса `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="575ff-231">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute: the `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="575ff-232">Чтобы отключить поведение по умолчанию, задайте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> значение `true` в `Startup.ConfigureServices`, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="575ff-232">To disable the default behavior, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters>  to `true` in `Startup.ConfigureServices`, as shown in the following example:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="575ff-233">Сведения о проблемах для кодов состояния ошибки</span><span class="sxs-lookup"><span data-stu-id="575ff-233">Problem details for error status codes</span></span>

<span data-ttu-id="575ff-234">Если задана версия совместимости, начиная с 2.2, MVC преобразовывает код ошибки (код состояния 400 и далее) в результат с <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="575ff-234">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="575ff-235">Тип `ProblemDetails` основан на [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807) и предоставляет считываемые компьютером сведения об ошибке в HTTP-ответе.</span><span class="sxs-lookup"><span data-stu-id="575ff-235">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="575ff-236">Рассмотрим следующий код в действии контроллера:</span><span class="sxs-lookup"><span data-stu-id="575ff-236">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="575ff-237">Ответ HTTP для `NotFound` содержит код состояния 404 с текстом `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="575ff-237">The HTTP response for `NotFound` has a 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="575ff-238">Например:</span><span class="sxs-lookup"><span data-stu-id="575ff-238">For example:</span></span>

```json
{
    type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
    title: "Not Found",
    status: 404,
    traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="customize-problemdetails-response"></a><span data-ttu-id="575ff-239">Настройка ответа ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="575ff-239">Customize ProblemDetails response</span></span>

<span data-ttu-id="575ff-240">Используйте свойство `ClientErrorMapping`, чтобы настроить содержимое ответа `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="575ff-240">Use the `ClientErrorMapping` property to configure the contents of the `ProblemDetails` response.</span></span> <span data-ttu-id="575ff-241">Например, в следующем коде показано обновление свойства `type` для откликов 404:</span><span class="sxs-lookup"><span data-stu-id="575ff-241">For example, the following code updates the `type` property for 404 responses:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=10-11)]

### <a name="disable-problemdetails-response"></a><span data-ttu-id="575ff-242">Отключение ответа ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="575ff-242">Disable ProblemDetails response</span></span>

<span data-ttu-id="575ff-243">Отключить автоматическое создание `ProblemDetails` можно, задав свойству `SuppressMapClientErrors` значение `true`.</span><span class="sxs-lookup"><span data-stu-id="575ff-243">The automatic creation of `ProblemDetails` is disabled when the `SuppressMapClientErrors` property is set to `true`.</span></span> <span data-ttu-id="575ff-244">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="575ff-244">Add the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

## <a name="additional-resources"></a><span data-ttu-id="575ff-245">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="575ff-245">Additional resources</span></span> 

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
