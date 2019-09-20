---
title: Создание веб-API с помощью ASP.NET Core
author: scottaddie
description: Узнайте, как создать веб-API в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 09/12/2019
uid: web-api/index
ms.openlocfilehash: aab9b848eb6e69055b019c9253c716898e9847e2
ms.sourcegitcommit: a11f09c10ef3d4eeab7ae9ce993e7f30427741c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149340"
---
# <a name="create-web-apis-with-aspnet-core"></a><span data-ttu-id="ec6c6-103">Создание веб-API с помощью ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ec6c6-103">Create web APIs with ASP.NET Core</span></span>

<span data-ttu-id="ec6c6-104">Авторы: [Скотт Адди](https://github.com/scottaddie) (Scott Addie) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-104">By [Scott Addie](https://github.com/scottaddie) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ec6c6-105">ASP.NET Core поддерживает создание служб RESTful, также известных как веб-API, с помощью C#.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-105">ASP.NET Core supports creating RESTful services, also known as web APIs, using C#.</span></span> <span data-ttu-id="ec6c6-106">Для обработки запросов веб-API использует контроллеры.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-106">To handle requests, a web API uses controllers.</span></span> <span data-ttu-id="ec6c6-107">В веб-API *контроллеры* — это классы, производные от `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-107">*Controllers* in a web API are classes that derive from `ControllerBase`.</span></span> <span data-ttu-id="ec6c6-108">В этой статье показано, как использовать контроллеры для обработки веб-запросов API.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-108">This article shows how to use controllers for handling web API requests.</span></span>

<span data-ttu-id="ec6c6-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span><span class="sxs-lookup"><span data-stu-id="ec6c6-109">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/index/samples).</span></span> <span data-ttu-id="ec6c6-110">([Инструкция по скачиванию](xref:index#how-to-download-a-sample).)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-110">([How to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="controllerbase-class"></a><span data-ttu-id="ec6c6-111">Класс ControllerBase</span><span class="sxs-lookup"><span data-stu-id="ec6c6-111">ControllerBase class</span></span>

<span data-ttu-id="ec6c6-112">Веб-API состоит из одного или нескольких классов контроллера, которые являются производными от <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-112">A web API consists of one or more controller classes that derive from <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="ec6c6-113">Шаблон проекта веб-API предоставляет начальный контроллер:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-113">The web API project template provides a starter controller:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=3)]

::: moniker-end

<span data-ttu-id="ec6c6-114">Не создавайте контроллер веб-API путем наследования от класса <xref:Microsoft.AspNetCore.Mvc.Controller>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-114">Don't create a web API controller by deriving from the <xref:Microsoft.AspNetCore.Mvc.Controller> class.</span></span> <span data-ttu-id="ec6c6-115">`Controller` является производным от `ControllerBase` и добавляет поддержку для представлений, обеспечивая обработку веб-страниц, а не запросов веб-API.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-115">`Controller` derives from `ControllerBase` and adds support for views, so it's for handling web pages, not web API requests.</span></span> <span data-ttu-id="ec6c6-116">Существует одно исключение из этого правила: если вы планируете использовать один и тот же контроллер для представлений и веб-API, сделайте его производным от `Controller`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-116">There's an exception to this rule: if you plan to use the same controller for both views and web APIs, derive it from `Controller`.</span></span>

<span data-ttu-id="ec6c6-117">Класс `ControllerBase` предоставляет множество свойств и методов, которые удобны для обработки HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-117">The `ControllerBase` class provides many properties and methods that are useful for handling HTTP requests.</span></span> <span data-ttu-id="ec6c6-118">Например, `ControllerBase.CreatedAtAction` возвращает код состояния 201.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-118">For example, `ControllerBase.CreatedAtAction` returns a 201 status code:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=10)]

<span data-ttu-id="ec6c6-119">Вот еще примеры методов, предоставляющих `ControllerBase`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-119">Here are some more examples of methods that `ControllerBase` provides.</span></span>

|<span data-ttu-id="ec6c6-120">Метод</span><span class="sxs-lookup"><span data-stu-id="ec6c6-120">Method</span></span>   |<span data-ttu-id="ec6c6-121">Примечания</span><span class="sxs-lookup"><span data-stu-id="ec6c6-121">Notes</span></span>    |
|---------|---------|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest*>| <span data-ttu-id="ec6c6-122">Возвращает код состояния 400.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-122">Returns 400 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*>|<span data-ttu-id="ec6c6-123">Возвращает код состояния 404.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-123">Returns 404 status code.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.PhysicalFile*>|<span data-ttu-id="ec6c6-124">Возвращает файл.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-124">Returns a file.</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>|<span data-ttu-id="ec6c6-125">Вызывает [привязку модели](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="ec6c6-125">Invokes [model binding](xref:mvc/models/model-binding).</span></span>|
|<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryValidateModel*>|<span data-ttu-id="ec6c6-126">Вызывает [проверку модели](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="ec6c6-126">Invokes [model validation](xref:mvc/models/validation).</span></span>|

<span data-ttu-id="ec6c6-127">Список всех доступных методов и свойств см. здесь: <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-127">For a list of all available methods and properties, see <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span>

## <a name="attributes"></a><span data-ttu-id="ec6c6-128">Атрибуты</span><span class="sxs-lookup"><span data-stu-id="ec6c6-128">Attributes</span></span>

<span data-ttu-id="ec6c6-129">Пространство имен <xref:Microsoft.AspNetCore.Mvc> предоставляет атрибуты, которые можно использовать для настройки поведения контроллеров и методов действия веб-API.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-129">The <xref:Microsoft.AspNetCore.Mvc> namespace provides attributes that can be used to configure the behavior of web API controllers and action methods.</span></span> <span data-ttu-id="ec6c6-130">В следующем примере атрибуты используются для указания поддерживаемой команды действия HTTP и всех известных кодов состояния HTTP, которые могут быть возвращены:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-130">The following example uses attributes to specify the supported HTTP action verb and any known HTTP status codes that could be returned:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_400And201&highlight=1-3)]

<span data-ttu-id="ec6c6-131">Вот еще примеры доступных атрибутов.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-131">Here are some more examples of attributes that are available.</span></span>

|<span data-ttu-id="ec6c6-132">Атрибут</span><span class="sxs-lookup"><span data-stu-id="ec6c6-132">Attribute</span></span>|<span data-ttu-id="ec6c6-133">Примечания</span><span class="sxs-lookup"><span data-stu-id="ec6c6-133">Notes</span></span>|
|---------|-----|
|<span data-ttu-id="ec6c6-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-134">[[Route]](<xref:Microsoft.AspNetCore.Mvc.RouteAttribute>)</span></span>      |<span data-ttu-id="ec6c6-135">Определяет шаблон URL-адреса для контроллера или действия.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-135">Specifies URL pattern for a controller or action.</span></span>|
|<span data-ttu-id="ec6c6-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-136">[[Bind]](<xref:Microsoft.AspNetCore.Mvc.BindAttribute>)</span></span>        |<span data-ttu-id="ec6c6-137">Задает префикс и свойства, которые добавляются для привязки модели.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-137">Specifies prefix and properties to include for model binding.</span></span>|
|<span data-ttu-id="ec6c6-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-138">[[HttpGet]](<xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute>)</span></span>  |<span data-ttu-id="ec6c6-139">Определяет действие, которое поддерживает команду действия HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-139">Identifies an action that supports the HTTP GET action verb.</span></span>|
|<span data-ttu-id="ec6c6-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-140">[[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>)</span></span>|<span data-ttu-id="ec6c6-141">Указывает типы данных, которые принимает действие.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-141">Specifies data types that an action accepts.</span></span>|
|<span data-ttu-id="ec6c6-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-142">[[Produces]](<xref:Microsoft.AspNetCore.Mvc.ProducesAttribute>)</span></span>|<span data-ttu-id="ec6c6-143">Указывает типы данных, которые возвращает действие.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-143">Specifies data types that an action returns.</span></span>|

<span data-ttu-id="ec6c6-144">Список доступных атрибутов см. в пространстве имен <xref:Microsoft.AspNetCore.Mvc>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-144">For a list that includes the available attributes, see the <xref:Microsoft.AspNetCore.Mvc> namespace.</span></span>

## <a name="apicontroller-attribute"></a><span data-ttu-id="ec6c6-145">Атрибут ApiController</span><span class="sxs-lookup"><span data-stu-id="ec6c6-145">ApiController attribute</span></span>

<span data-ttu-id="ec6c6-146">Атрибут [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) можно применить к классу контроллера для включения следующих специализированных схем поведения API:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-146">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute can be applied to a controller class to enable the following opinionated, API-specific behaviors:</span></span>

* [<span data-ttu-id="ec6c6-147">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="ec6c6-147">Attribute routing requirement</span></span>](#attribute-routing-requirement)
* [<span data-ttu-id="ec6c6-148">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="ec6c6-148">Automatic HTTP 400 responses</span></span>](#automatic-http-400-responses)
* [<span data-ttu-id="ec6c6-149">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="ec6c6-149">Binding source parameter inference</span></span>](#binding-source-parameter-inference)
* [<span data-ttu-id="ec6c6-150">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="ec6c6-150">Multipart/form-data request inference</span></span>](#multipartform-data-request-inference)
* [<span data-ttu-id="ec6c6-151">Сведения о проблемах для кодов состояния ошибки</span><span class="sxs-lookup"><span data-stu-id="ec6c6-151">Problem details for error status codes</span></span>](#problem-details-for-error-status-codes)

<span data-ttu-id="ec6c6-152">Для реализации этих функций необходима [версия совместимости](xref:mvc/compatibility-version), начиная с 2.1.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-152">These features require a [compatibility version](xref:mvc/compatibility-version) of 2.1 or later.</span></span>

### <a name="attribute-on-specific-controllers"></a><span data-ttu-id="ec6c6-153">Атрибут в определенных контроллерах</span><span class="sxs-lookup"><span data-stu-id="ec6c6-153">Attribute on specific controllers</span></span>

<span data-ttu-id="ec6c6-154">Атрибут `[ApiController]` может применяться к определенным контроллерам, как показано в следующем примере из шаблона проекта:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-154">The `[ApiController]` attribute can be applied to specific controllers, as in the following example from the project template:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2])]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=2)]

::: moniker-end

### <a name="attribute-on-multiple-controllers"></a><span data-ttu-id="ec6c6-155">Атрибут в нескольких контроллерах</span><span class="sxs-lookup"><span data-stu-id="ec6c6-155">Attribute on multiple controllers</span></span>

<span data-ttu-id="ec6c6-156">Один из подходов к использованию атрибута на более чем одном контроллере заключается в создании пользовательского базового класса контроллера, аннотированного атрибутом `[ApiController]`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-156">One approach to using the attribute on more than one controller is to create a custom base controller class annotated with the `[ApiController]` attribute.</span></span> <span data-ttu-id="ec6c6-157">В следующем примере демонстрируется пользовательский базовый класс и производный от него контроллер:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-157">The following example shows a custom base class and a controller that derives from it:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/MyControllerBase.cs?name=snippet_MyControllerBase)]

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_Inherit)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="attribute-on-an-assembly"></a><span data-ttu-id="ec6c6-158">Атрибут в сборке</span><span class="sxs-lookup"><span data-stu-id="ec6c6-158">Attribute on an assembly</span></span>

<span data-ttu-id="ec6c6-159">Если задана [версия совместимости](xref:mvc/compatibility-version) 2.2 или последующая, атрибут `[ApiController]` можно применить к сборке.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-159">If [compatibility version](xref:mvc/compatibility-version) is set to 2.2 or later, the `[ApiController]` attribute can be applied to an assembly.</span></span> <span data-ttu-id="ec6c6-160">Аннотирование этим способом применяет поведение веб-API ко всем контроллерам в сборке.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-160">Annotation in this manner applies web API behavior to all controllers in the assembly.</span></span> <span data-ttu-id="ec6c6-161">Его нельзя отменить для отдельных контроллеров.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-161">There's no way to opt out for individual controllers.</span></span> <span data-ttu-id="ec6c6-162">Примените атрибут уровня сборки к объявлению пространства имен, окружающему класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-162">Apply the assembly-level attribute to the namespace declaration surrounding the `Startup` class:</span></span>

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

::: moniker-end

## <a name="attribute-routing-requirement"></a><span data-ttu-id="ec6c6-163">Обязательная маршрутизация атрибутов</span><span class="sxs-lookup"><span data-stu-id="ec6c6-163">Attribute routing requirement</span></span>

<span data-ttu-id="ec6c6-164">Атрибут `[ApiController]` требует обязательной маршрутизации атрибутов.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-164">The `[ApiController]` attribute makes attribute routing a requirement.</span></span> <span data-ttu-id="ec6c6-165">Например:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-165">For example:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Controllers/WeatherForecastController.cs?name=snippet_ControllerSignature&highlight=2)]

<span data-ttu-id="ec6c6-166">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определяемые `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-166">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by `UseEndpoints`, <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>, or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Controllers/ValuesController.cs?name=snippet_ControllerSignature&highlight=1)]

<span data-ttu-id="ec6c6-167">Действия недоступны через [обычные маршруты](xref:mvc/controllers/routing#conventional-routing), определяемые <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> или <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-167">Actions are inaccessible via [conventional routes](xref:mvc/controllers/routing#conventional-routing) defined by <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> or <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> in `Startup.Configure`.</span></span>

::: moniker-end

### <a name="automatic-http-400-responses"></a><span data-ttu-id="ec6c6-168">Автоматические отклики HTTP 400</span><span class="sxs-lookup"><span data-stu-id="ec6c6-168">Automatic HTTP 400 responses</span></span>

<span data-ttu-id="ec6c6-169">Благодаря атрибуту `[ApiController]` ошибки проверки модели автоматически активируют отклик HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-169">The `[ApiController]` attribute makes model validation errors automatically trigger an HTTP 400 response.</span></span> <span data-ttu-id="ec6c6-170">В результате следующий код ненужен в методе действия:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-170">Consequently, the following code is unnecessary in an action method:</span></span>

```csharp
if (!ModelState.IsValid)
{
    return BadRequest(ModelState);
}
```

<span data-ttu-id="ec6c6-171">Для выполнения предыдущей проверки ASP.NET Core MVC использует фильтр действий <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-171">ASP.NET Core MVC uses the <xref:Microsoft.AspNetCore.Mvc.Infrastructure.ModelStateInvalidFilter> action filter to do the preceding check.</span></span>

### <a name="default-badrequest-response"></a><span data-ttu-id="ec6c6-172">Отклик BadRequest по умолчанию</span><span class="sxs-lookup"><span data-stu-id="ec6c6-172">Default BadRequest response</span></span>

<span data-ttu-id="ec6c6-173">Если задана версия совместимости 2.1, для ответов HTTP 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-173">With a compatibility version of 2.1, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.SerializableError>.</span></span> <span data-ttu-id="ec6c6-174">Следующий текст запроса является примером сериализованного типа:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-174">The following request body is an example of the serialized type:</span></span>

```json
{
  "": [
    "A non-empty request body is required."
  ]
}
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ec6c6-175">Если задана версия совместимости 2.2 или более поздние версии, для ответов HTTP 400 по умолчанию возвращается тип отклика <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-175">With a compatibility version of 2.2 or later, the default response type for an HTTP 400 response is <xref:Microsoft.AspNetCore.Mvc.ValidationProblemDetails>.</span></span> <span data-ttu-id="ec6c6-176">Следующий текст запроса является примером сериализованного типа:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-176">The following request body is an example of the serialized type:</span></span>

```json
{
  "type": "https://tools.ietf.org/html/rfc7231#section-6.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "traceId": "|7fb5e16a-4c8f23bbfc974667.",
  "errors": {
    "": [
      "A non-empty request body is required."
    ]
  }
}
```

<span data-ttu-id="ec6c6-177">Тип `ValidationProblemDetails`:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-177">The `ValidationProblemDetails` type:</span></span>

* <span data-ttu-id="ec6c6-178">предоставляет распознаваемый компьютером формат для указания ошибок в откликах веб-API;</span><span class="sxs-lookup"><span data-stu-id="ec6c6-178">Provides a machine-readable format for specifying errors in web API responses.</span></span>
* <span data-ttu-id="ec6c6-179">соответствует [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807).</span><span class="sxs-lookup"><span data-stu-id="ec6c6-179">Complies with the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807).</span></span>

::: moniker-end

### <a name="log-automatic-400-responses"></a><span data-ttu-id="ec6c6-180">Запись в журнал автоматических откликов HTTP 400</span><span class="sxs-lookup"><span data-stu-id="ec6c6-180">Log automatic 400 responses</span></span>

<span data-ttu-id="ec6c6-181">См. статью об[записи в журнал автоматических откликов HTTP 400 на ошибки проверки модели (aspnet/AspNetCore.Docs № 12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span><span class="sxs-lookup"><span data-stu-id="ec6c6-181">See [How to log automatic 400 responses on model validation errors (aspnet/AspNetCore.Docs #12157)](https://github.com/aspnet/AspNetCore.Docs/issues/12157).</span></span>

### <a name="disable-automatic-400-response"></a><span data-ttu-id="ec6c6-182">Отключение автоматической активации отклика HTTP 400</span><span class="sxs-lookup"><span data-stu-id="ec6c6-182">Disable automatic 400 response</span></span>

<span data-ttu-id="ec6c6-183">Чтобы отключить автоматическую активацию отклика HTTP 400, задайте свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-183">To disable the automatic 400 behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> property to `true`.</span></span> <span data-ttu-id="ec6c6-184">Добавьте выделенный ниже код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-184">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,6)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,7)]

::: moniker-end

## <a name="binding-source-parameter-inference"></a><span data-ttu-id="ec6c6-185">Вывод параметров источника привязки</span><span class="sxs-lookup"><span data-stu-id="ec6c6-185">Binding source parameter inference</span></span>

<span data-ttu-id="ec6c6-186">Атрибут источника привязки определяет расположение, в котором находится значение параметра действия.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-186">A binding source attribute defines the location at which an action parameter's value is found.</span></span> <span data-ttu-id="ec6c6-187">Существуют следующие атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-187">The following binding source attributes exist:</span></span>

|<span data-ttu-id="ec6c6-188">Атрибут</span><span class="sxs-lookup"><span data-stu-id="ec6c6-188">Attribute</span></span>|<span data-ttu-id="ec6c6-189">Источник привязки</span><span class="sxs-lookup"><span data-stu-id="ec6c6-189">Binding source</span></span> |
|---------|---------|
|<span data-ttu-id="ec6c6-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-190">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)</span></span>     | <span data-ttu-id="ec6c6-191">Текст запроса</span><span class="sxs-lookup"><span data-stu-id="ec6c6-191">Request body</span></span> |
|<span data-ttu-id="ec6c6-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-192">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)</span></span>     | <span data-ttu-id="ec6c6-193">Данные формы в тексте запроса</span><span class="sxs-lookup"><span data-stu-id="ec6c6-193">Form data in the request body</span></span> |
|<span data-ttu-id="ec6c6-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-194">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)</span></span> | <span data-ttu-id="ec6c6-195">Заголовок запроса</span><span class="sxs-lookup"><span data-stu-id="ec6c6-195">Request header</span></span> |
|<span data-ttu-id="ec6c6-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-196">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)</span></span>   | <span data-ttu-id="ec6c6-197">Параметры строки запроса для запроса</span><span class="sxs-lookup"><span data-stu-id="ec6c6-197">Request query string parameter</span></span> |
|<span data-ttu-id="ec6c6-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-198">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)</span></span>   | <span data-ttu-id="ec6c6-199">Данные маршрута из текущего запроса</span><span class="sxs-lookup"><span data-stu-id="ec6c6-199">Route data from the current request</span></span> |
|<span data-ttu-id="ec6c6-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span><span class="sxs-lookup"><span data-stu-id="ec6c6-200">[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)</span></span> | <span data-ttu-id="ec6c6-201">Служба запросов, внедренная в качестве параметра действия</span><span class="sxs-lookup"><span data-stu-id="ec6c6-201">The request service injected as an action parameter</span></span> |

> [!WARNING]
> <span data-ttu-id="ec6c6-202">Не используйте `[FromRoute]`, если значения могут содержать `%2f` (то есть `/`).</span><span class="sxs-lookup"><span data-stu-id="ec6c6-202">Don't use `[FromRoute]` when values might contain `%2f` (that is `/`).</span></span> <span data-ttu-id="ec6c6-203">Для `%2f` не будет применяться отмена экранирования `/`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-203">`%2f` won't be unescaped to `/`.</span></span> <span data-ttu-id="ec6c6-204">Используйте `[FromQuery]`, если значение может содержать `%2f`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-204">Use `[FromQuery]` if the value might contain `%2f`.</span></span>

<span data-ttu-id="ec6c6-205">Без атрибута `[ApiController]` или атрибутов источника привязки, таких как `[FromQuery]`, среда выполнения ASP.NET Core попытается использовать связыватель модели для составного объекта.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-205">Without the `[ApiController]` attribute or binding source attributes like `[FromQuery]`, the ASP.NET Core runtime attempts to use the complex object model binder.</span></span> <span data-ttu-id="ec6c6-206">Связыватель модели для составного объекта извлекает данные из поставщиков значений в определенном порядке.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-206">The complex object model binder pulls data from value providers in a defined order.</span></span>

<span data-ttu-id="ec6c6-207">В следующем примере атрибут `[FromQuery]` указывает, что значение параметра `discontinuedOnly` задано в строке запроса URL-адреса для запроса:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-207">In the following example, the `[FromQuery]` attribute indicates that the `discontinuedOnly` parameter value is provided in the request URL's query string:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

<span data-ttu-id="ec6c6-208">Атрибут `[ApiController]` применяет правила зависимости к источникам данных по умолчанию для параметров действий.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-208">The `[ApiController]` attribute applies inference rules for the default data sources of action parameters.</span></span> <span data-ttu-id="ec6c6-209">Эти правила избавляют от необходимости вручную определять источники привязки путем применения атрибутов к параметрам действий.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-209">These rules save you from having to identify binding sources manually by applying attributes to the action parameters.</span></span> <span data-ttu-id="ec6c6-210">Правила зависимости источника привязки работают следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-210">The binding source inference rules behave as follows:</span></span>

* <span data-ttu-id="ec6c6-211">`[FromBody]` выводится для параметров сложного типа.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-211">`[FromBody]` is inferred for complex type parameters.</span></span> <span data-ttu-id="ec6c6-212">Исключением из правила зависимости `[FromBody]` является любой сложный встроенный тип со специальным значением, такой как <xref:Microsoft.AspNetCore.Http.IFormCollection> и <xref:System.Threading.CancellationToken>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-212">An exception to the `[FromBody]` inference rule is any complex, built-in type with a special meaning, such as <xref:Microsoft.AspNetCore.Http.IFormCollection> and <xref:System.Threading.CancellationToken>.</span></span> <span data-ttu-id="ec6c6-213">Код определения источника привязки игнорирует эти особые типы.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-213">The binding source inference code ignores those special types.</span></span>
* <span data-ttu-id="ec6c6-214">`[FromForm]` выводится для параметров действия с типом <xref:Microsoft.AspNetCore.Http.IFormFile> и <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-214">`[FromForm]` is inferred for action parameters of type <xref:Microsoft.AspNetCore.Http.IFormFile> and <xref:Microsoft.AspNetCore.Http.IFormFileCollection>.</span></span> <span data-ttu-id="ec6c6-215">Он не выводится ни для каких простых или определяемых пользователем типов.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-215">It's not inferred for any simple or user-defined types.</span></span>
* <span data-ttu-id="ec6c6-216">`[FromRoute]` выводится для любого имени параметра действия, соответствующего параметру в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-216">`[FromRoute]` is inferred for any action parameter name matching a parameter in the route template.</span></span> <span data-ttu-id="ec6c6-217">Если параметру действия соответствуют несколько маршрутов, любое значение маршрута рассматривается как `[FromRoute]`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-217">When more than one route matches an action parameter, any route value is considered `[FromRoute]`.</span></span>
* <span data-ttu-id="ec6c6-218">`[FromQuery]` выводится для любых других параметров действия.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-218">`[FromQuery]` is inferred for any other action parameters.</span></span>

### <a name="frombody-inference-notes"></a><span data-ttu-id="ec6c6-219">Заметки о выводе FromBody</span><span class="sxs-lookup"><span data-stu-id="ec6c6-219">FromBody inference notes</span></span>

<span data-ttu-id="ec6c6-220">`[FromBody]` не определен для простых типов, таких как `string` или `int`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-220">`[FromBody]` isn't inferred for simple types such as `string` or `int`.</span></span> <span data-ttu-id="ec6c6-221">Таким образом, атрибут `[FromBody]` должен использоваться для простых типов, когда требуются эти функции.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-221">Therefore, the `[FromBody]` attribute should be used for simple types when that functionality is needed.</span></span>

<span data-ttu-id="ec6c6-222">Если у действия более одного параметра для привязки из текста запроса, выдается исключение.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-222">When an action has more than one parameter bound from the request body, an exception is thrown.</span></span> <span data-ttu-id="ec6c6-223">Например, все следующие сигнатуры метода действия вызывают исключение:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-223">For example, all of the following action method signatures cause an exception:</span></span>

* <span data-ttu-id="ec6c6-224">`[FromBody]` выводится для обеих параметров, так как они являются сложными типами.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-224">`[FromBody]` inferred on both because they're complex types.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action1(Product product, Order order)
  ```

* <span data-ttu-id="ec6c6-225">Атрибут `[FromBody]`, заданный одному параметру, выводится для другого, так как это сложный тип.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-225">`[FromBody]` attribute on one, inferred on the other because it's a complex type.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action2(Product product, [FromBody] Order order)
  ```

* <span data-ttu-id="ec6c6-226">Атрибут `[FromBody]` выводится для обоих параметров.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-226">`[FromBody]` attribute on both.</span></span>

  ```csharp
  [HttpPost]
  public IActionResult Action3([FromBody] Product product, [FromBody] Order order)
  ```

::: moniker range="= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="ec6c6-227">В ASP.NET Core 2.1 параметры типа коллекции, такие как списки и массивы, ошибочно выводятся как `[FromQuery]`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-227">In ASP.NET Core 2.1, collection type parameters such as lists and arrays are incorrectly inferred as `[FromQuery]`.</span></span> <span data-ttu-id="ec6c6-228">Для этих параметров следует использовать атрибут `[FromBody]`, если они должны быть привязаны из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-228">The `[FromBody]` attribute should be used for these parameters if they are to be bound from the request body.</span></span> <span data-ttu-id="ec6c6-229">Это поведение, при котором параметры типа коллекции выводятся для привязки из текста по умолчанию, исправлено в версии ASP.NET Core, начиная с 2.2.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-229">This behavior is corrected in ASP.NET Core 2.2 or later, where collection type parameters are inferred to be bound from the body by default.</span></span>

::: moniker-end

### <a name="disable-inference-rules"></a><span data-ttu-id="ec6c6-230">Отключение правил зависимости</span><span class="sxs-lookup"><span data-stu-id="ec6c6-230">Disable inference rules</span></span>

<span data-ttu-id="ec6c6-231">Чтобы отключить вывод источника привязки, задайте <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-231">To disable binding source inference, set <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> to `true`.</span></span> <span data-ttu-id="ec6c6-232">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-232">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,5)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,6)]

::: moniker-end

## <a name="multipartform-data-request-inference"></a><span data-ttu-id="ec6c6-233">Вывод многокомпонентных запросов и запросов данных форм</span><span class="sxs-lookup"><span data-stu-id="ec6c6-233">Multipart/form-data request inference</span></span>

<span data-ttu-id="ec6c6-234">Атрибут `[ApiController]` применяет правило зависимости, когда параметр действия аннотирован атрибутом [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute).</span><span class="sxs-lookup"><span data-stu-id="ec6c6-234">The `[ApiController]` attribute applies an inference rule when an action parameter is annotated with the [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) attribute.</span></span> <span data-ttu-id="ec6c6-235">При этом выводится тип содержимого запроса `multipart/form-data`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-235">The `multipart/form-data` request content type is inferred.</span></span>

<span data-ttu-id="ec6c6-236">Чтобы отключить поведение по умолчанию, задайте свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> значение `true` в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-236">To disable the default behavior, set the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> property to `true` in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,5)]

::: moniker-end

## <a name="problem-details-for-error-status-codes"></a><span data-ttu-id="ec6c6-237">Сведения о проблемах для кодов состояния ошибки</span><span class="sxs-lookup"><span data-stu-id="ec6c6-237">Problem details for error status codes</span></span>

<span data-ttu-id="ec6c6-238">Если задана версия совместимости, начиная с 2.2, MVC преобразовывает код ошибки (код состояния 400 и далее) в результат с <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-238">When the compatibility version is 2.2 or later, MVC transforms an error result (a result with status code 400 or higher) to a result with <xref:Microsoft.AspNetCore.Mvc.ProblemDetails>.</span></span> <span data-ttu-id="ec6c6-239">Тип `ProblemDetails` основан на [спецификации RFC 7807](https://tools.ietf.org/html/rfc7807) и предоставляет считываемые компьютером сведения об ошибке в HTTP-ответе.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-239">The `ProblemDetails` type is based on the [RFC 7807 specification](https://tools.ietf.org/html/rfc7807) for providing machine-readable error details in an HTTP response.</span></span>

<span data-ttu-id="ec6c6-240">Рассмотрим следующий код в действии контроллера:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-240">Consider the following code in a controller action:</span></span>

[!code-csharp[](index/samples/2.x/Controllers/PetsController.cs?name=snippet_ProblemDetailsStatusCode)]

<span data-ttu-id="ec6c6-241">Метод `NotFound` создает код состояния HTTP 404 с текстом `ProblemDetails`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-241">The `NotFound` method produces an HTTP 404 status code with a `ProblemDetails` body.</span></span> <span data-ttu-id="ec6c6-242">Например:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-242">For example:</span></span>

```json
{
  type: "https://tools.ietf.org/html/rfc7231#section-6.5.4",
  title: "Not Found",
  status: 404,
  traceId: "0HLHLV31KRN83:00000001"
}
```

### <a name="disable-problemdetails-response"></a><span data-ttu-id="ec6c6-243">Отключение ответа ProblemDetails</span><span class="sxs-lookup"><span data-stu-id="ec6c6-243">Disable ProblemDetails response</span></span>

<span data-ttu-id="ec6c6-244">Отключить автоматическое создание экземпляра `ProblemDetails` можно, задав свойству <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> значение `true`.</span><span class="sxs-lookup"><span data-stu-id="ec6c6-244">The automatic creation of a `ProblemDetails` instance is disabled when the <xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressMapClientErrors*> property is set to `true`.</span></span> <span data-ttu-id="ec6c6-245">Добавьте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="ec6c6-245">Add the following code in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=2,7)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](index/samples/2.x/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3,8)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ec6c6-246">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ec6c6-246">Additional resources</span></span>

* <xref:web-api/action-return-types>
* <xref:web-api/handle-errors>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
