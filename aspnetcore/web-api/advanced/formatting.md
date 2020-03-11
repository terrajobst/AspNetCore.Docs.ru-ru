---
title: Форматирование данных отклика в веб-API ASP.NET Core
author: ardalis
description: Сведения о форматировании данных отклика в веб-API ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 12/05/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: 908016720ade67a02ebe30d1dcb7929ad7592270
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653044"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="44df8-103">Форматирование данных отклика в веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44df8-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="44df8-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="44df8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="44df8-105">Приложение MVC ASP.NET Core поддерживает форматирование данных ответа.</span><span class="sxs-lookup"><span data-stu-id="44df8-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="44df8-106">Данные ответа могут возвращаться в определенных форматах или в формате, запрошенном клиентом.</span><span class="sxs-lookup"><span data-stu-id="44df8-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="44df8-107">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="44df8-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="44df8-108">Результаты действий для конкретного формата</span><span class="sxs-lookup"><span data-stu-id="44df8-108">Format-specific Action Results</span></span>

<span data-ttu-id="44df8-109">Некоторые типы результатов действий характерны для определенного формата, например <xref:Microsoft.AspNetCore.Mvc.JsonResult> и <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span><span class="sxs-lookup"><span data-stu-id="44df8-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="44df8-110">Действия могут возвращать результаты в определенном формате независимо от настроек клиента.</span><span class="sxs-lookup"><span data-stu-id="44df8-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="44df8-111">Например, при возврате `JsonResult` возвращаются данные в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="44df8-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="44df8-112">При возврате `ContentResult` или строки возвращаются строковые данные в формате обычного текста.</span><span class="sxs-lookup"><span data-stu-id="44df8-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="44df8-113">Действие не должно возвращать данные конкретного типа.</span><span class="sxs-lookup"><span data-stu-id="44df8-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="44df8-114">ASP.NET Core поддерживает любое возвращаемое значение объекта.</span><span class="sxs-lookup"><span data-stu-id="44df8-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="44df8-115">Результаты из действий, возвращающих объекты, которые не являются типами <xref:Microsoft.AspNetCore.Mvc.IActionResult>, сериализуются с помощью соответствующей реализации <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="44df8-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="44df8-116">Дополнительные сведения см. в разделе <xref:web-api/action-return-types>.</span><span class="sxs-lookup"><span data-stu-id="44df8-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="44df8-117">Встроенный вспомогательный метод <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> возвращает данные в формате JSON: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="44df8-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="44df8-118">Скачанный пример возвращает список авторов.</span><span class="sxs-lookup"><span data-stu-id="44df8-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="44df8-119">При использовании средств разработчика в браузере (F12) или [Postman](https://www.getpostman.com/tools) с предыдущим кодом:</span><span class="sxs-lookup"><span data-stu-id="44df8-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="44df8-120">Отобразится заголовок ответа, содержащий **Content-Type:** `application/json; charset=utf-8`.</span><span class="sxs-lookup"><span data-stu-id="44df8-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="44df8-121">Отобразятся заголовки запросов.</span><span class="sxs-lookup"><span data-stu-id="44df8-121">The request headers are displayed.</span></span> <span data-ttu-id="44df8-122">Например, заголовок `Accept`.</span><span class="sxs-lookup"><span data-stu-id="44df8-122">For example, the `Accept` header.</span></span> <span data-ttu-id="44df8-123">Приведенный выше код игнорирует заголовок `Accept`.</span><span class="sxs-lookup"><span data-stu-id="44df8-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="44df8-124">Чтобы возвратить данные в формате обычного текста, используйте <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> и вспомогательный метод <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content>:</span><span class="sxs-lookup"><span data-stu-id="44df8-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> and the <xref:Microsoft.AspNetCore.Mvc.ContentResult.Content> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="44df8-125">В приведенном выше коде возвращаемым `Content-Type` является `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="44df8-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="44df8-126">При возврате строки возвращается `Content-Type` со значением `text/plain`:</span><span class="sxs-lookup"><span data-stu-id="44df8-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="44df8-127">Для действий с несколькими возвращаемыми типами возвращается значение `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="44df8-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="44df8-128">Например, возвращаются различные коды состояния HTTP на основе результатов выполненных операций.</span><span class="sxs-lookup"><span data-stu-id="44df8-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="44df8-129">Согласование содержимого</span><span class="sxs-lookup"><span data-stu-id="44df8-129">Content negotiation</span></span>

<span data-ttu-id="44df8-130">Согласование содержимого происходит, когда клиент задает [заголовок Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="44df8-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="44df8-131">Для ASP.NET Core по умолчанию используется формат [JSON](https://json.org/).</span><span class="sxs-lookup"><span data-stu-id="44df8-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="44df8-132">Согласование содержимого:</span><span class="sxs-lookup"><span data-stu-id="44df8-132">Content negotiation is:</span></span>

* <span data-ttu-id="44df8-133">реализуется с помощью <xref:Microsoft.AspNetCore.Mvc.ObjectResult>;</span><span class="sxs-lookup"><span data-stu-id="44df8-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="44df8-134">встроено в результаты действия с определенным кодом состояния, возвращаемые из вспомогательных методов;</span><span class="sxs-lookup"><span data-stu-id="44df8-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="44df8-135">вспомогательные методы результатов действия основаны на `ObjectResult`;</span><span class="sxs-lookup"><span data-stu-id="44df8-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="44df8-136">при возврате типа модели возвращаемым типом является `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="44df8-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="44df8-137">Следующий метод действия использует вспомогательные методы `Ok` и `NotFound`:</span><span class="sxs-lookup"><span data-stu-id="44df8-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="44df8-138">По умолчанию ASP.NET Core поддерживает типы мультимедиа `application/json`, `text/json` и `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="44df8-138">By default, ASP.NET Core supports `application/json`, `text/json`, and `text/plain` media types.</span></span> <span data-ttu-id="44df8-139">Средства, такие как [Fiddler](https://www.telerik.com/fiddler) или [Postman](https://www.getpostman.com/tools), могут установить заголовок запроса `Accept`, чтобы указать формат возвращаемого значения.</span><span class="sxs-lookup"><span data-stu-id="44df8-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` request header to specify the return format.</span></span> <span data-ttu-id="44df8-140">Если заголовок `Accept` содержит тип, который поддерживается сервером, возвращается этот тип.</span><span class="sxs-lookup"><span data-stu-id="44df8-140">When the `Accept` header contains a type the server supports, that type is returned.</span></span> <span data-ttu-id="44df8-141">Сведения о добавлении дополнительных форматировщиков приведены в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="44df8-141">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="44df8-142">Действия контроллера могут возвращать POCO (простые объекты CLR).</span><span class="sxs-lookup"><span data-stu-id="44df8-142">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="44df8-143">При возвращении POCO среда выполнения автоматически создает `ObjectResult`, который генерирует оболочку объекта.</span><span class="sxs-lookup"><span data-stu-id="44df8-143">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="44df8-144">Клиент получает отформатированный сериализованный объект.</span><span class="sxs-lookup"><span data-stu-id="44df8-144">The client gets the formatted serialized object.</span></span> <span data-ttu-id="44df8-145">Если возвращаемый объект имеет значение `null`, возвращается ответ `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="44df8-145">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="44df8-146">Возвращение типа объекта:</span><span class="sxs-lookup"><span data-stu-id="44df8-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="44df8-147">В приведенном выше коде запрос допустимого псевдонима автора получает ответ `200 OK` с данными об авторе.</span><span class="sxs-lookup"><span data-stu-id="44df8-147">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="44df8-148">Запрос недопустимого псевдонима возвращает ответ `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="44df8-148">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="44df8-149">Заголовок Accept.</span><span class="sxs-lookup"><span data-stu-id="44df8-149">The Accept header</span></span>

<span data-ttu-id="44df8-150">*Согласование* содержимого выполняется только при наличии в запросе заголовка `Accept`.</span><span class="sxs-lookup"><span data-stu-id="44df8-150">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="44df8-151">Если запрос содержит заголовок Accept, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="44df8-151">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="44df8-152">перечисляет типы мультимедиа в заголовке Accept в порядке предпочтения;</span><span class="sxs-lookup"><span data-stu-id="44df8-152">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="44df8-153">пытается найти форматировщик, который может создать ответ в одном из указанных форматов.</span><span class="sxs-lookup"><span data-stu-id="44df8-153">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="44df8-154">Если форматировщик, который может удовлетворить запрос клиента, не найден, ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="44df8-154">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="44df8-155">возвращает значение `406 Not Acceptable`, если <xref:Microsoft.AspNetCore.Mvc.MvcOptions> задано, или:</span><span class="sxs-lookup"><span data-stu-id="44df8-155">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="44df8-156">пытается найти первый форматировщик, который может создать ответ.</span><span class="sxs-lookup"><span data-stu-id="44df8-156">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="44df8-157">Если форматировщик, обеспечивающий требуемый формат, не настроен, используется первый форматировщик, способный отформатировать данный объект.</span><span class="sxs-lookup"><span data-stu-id="44df8-157">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="44df8-158">Если в запросе не отображается заголовок `Accept`:</span><span class="sxs-lookup"><span data-stu-id="44df8-158">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="44df8-159">Для сериализации ответа используется первый форматировщик, который может работать с объектом.</span><span class="sxs-lookup"><span data-stu-id="44df8-159">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="44df8-160">Согласование не выполняется.</span><span class="sxs-lookup"><span data-stu-id="44df8-160">There isn't any negotiation taking place.</span></span> <span data-ttu-id="44df8-161">Сервер определяет возвращаемый формат.</span><span class="sxs-lookup"><span data-stu-id="44df8-161">The server is determining what format to return.</span></span>

<span data-ttu-id="44df8-162">Если заголовок Accept содержит `*/*`, он игнорируется, если только `RespectBrowserAcceptHeader` не имеет значение true в <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span><span class="sxs-lookup"><span data-stu-id="44df8-162">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="44df8-163">Браузеры и согласование содержимого</span><span class="sxs-lookup"><span data-stu-id="44df8-163">Browsers and content negotiation</span></span>

<span data-ttu-id="44df8-164">В отличие от обычных клиентов API, веб-браузеры предоставляют заголовки `Accept`.</span><span class="sxs-lookup"><span data-stu-id="44df8-164">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="44df8-165">Веб-браузер определяет множество форматов, включая подстановочные знаки.</span><span class="sxs-lookup"><span data-stu-id="44df8-165">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="44df8-166">Если платформа обнаруживает, что запрос поступает из браузера, по умолчанию выполняется следующее:</span><span class="sxs-lookup"><span data-stu-id="44df8-166">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="44df8-167">Заголовок `Accept` игнорируется.</span><span class="sxs-lookup"><span data-stu-id="44df8-167">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="44df8-168">Содержимое возвращается в формате JSON, если не указано иначе.</span><span class="sxs-lookup"><span data-stu-id="44df8-168">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="44df8-169">Это обеспечивает более согласованное взаимодействие между браузерами при использовании API.</span><span class="sxs-lookup"><span data-stu-id="44df8-169">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="44df8-170">Чтобы настроить приложение для учета заголовков Accept в браузере, установите для параметра <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> значение `true`:</span><span class="sxs-lookup"><span data-stu-id="44df8-170">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="44df8-171">Настройка форматировщиков</span><span class="sxs-lookup"><span data-stu-id="44df8-171">Configure formatters</span></span>

<span data-ttu-id="44df8-172">В приложениях, которым требуется поддержка дополнительных форматов, можно добавлять соответствующие пакеты NuGet и настраивать поддержку.</span><span class="sxs-lookup"><span data-stu-id="44df8-172">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="44df8-173">Существуют отдельные модули форматирования для ввода и вывода.</span><span class="sxs-lookup"><span data-stu-id="44df8-173">There are separate formatters for input and output.</span></span> <span data-ttu-id="44df8-174">Форматировщики ввода используются [привязкой модели](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="44df8-174">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="44df8-175">Форматировщики вывода используются для форматирования ответов.</span><span class="sxs-lookup"><span data-stu-id="44df8-175">Output formatters are used to format responses.</span></span> <span data-ttu-id="44df8-176">Сведения о создании пользовательского форматировщика см. в статье [Пользовательские модули форматирования для веб-API в ASP.NET Core](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="44df8-176">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="44df8-177">Добавление поддержки формата XML</span><span class="sxs-lookup"><span data-stu-id="44df8-177">Add XML format support</span></span>

<span data-ttu-id="44df8-178">Форматировщики XML, реализованные с помощью <xref:System.Xml.Serialization.XmlSerializer>, можно настроить путем вызова <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span><span class="sxs-lookup"><span data-stu-id="44df8-178">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="44df8-179">Приведенный выше код сериализует результаты с помощью `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="44df8-179">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="44df8-180">При использовании приведенного выше кода методы контроллера возвращают соответствующий формат на основе заголовка `Accept` запроса.</span><span class="sxs-lookup"><span data-stu-id="44df8-180">When using the preceding code, controller methods return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="44df8-181">Настройка форматировщиков на основе System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="44df8-181">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="44df8-182">Функции для форматировщиков на основе `System.Text.Json` можно настроить с помощью `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="44df8-182">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.SerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="44df8-183">Параметры сериализации выходных данных для отдельных действий можно настроить с помощью `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="44df8-183">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="44df8-184">Пример:</span><span class="sxs-lookup"><span data-stu-id="44df8-184">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        options.WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="44df8-185">Добавление поддержки формата JSON на основе Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="44df8-185">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="44df8-186">До выпуска версии ASP.NET Core 3.0 по умолчанию использовались форматировщики JSON, реализованные с помощью пакета `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="44df8-186">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="44df8-187">В ASP.NET Core 3.0 или более поздней версии форматировщики JSON по умолчанию основаны на `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="44df8-187">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="44df8-188">Чтобы добавить поддержку форматировщиков и функций на основе `Newtonsoft.Json`, установите пакет NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) и настройте его в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="44df8-188">Support for `Newtonsoft.Json` based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="44df8-189">Возможно, некоторые функции не будут оптимально работать с форматировщиками на основе `System.Text.Json` и будут требовать ссылки на форматировщики на основе `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="44df8-189">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="44df8-190">Продолжайте использовать форматировщики на основе `Newtonsoft.Json`, если приложение:</span><span class="sxs-lookup"><span data-stu-id="44df8-190">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="44df8-191">Использует атрибут `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="44df8-191">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="44df8-192">Например, `[JsonProperty]` или `[JsonIgnore]`.</span><span class="sxs-lookup"><span data-stu-id="44df8-192">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="44df8-193">Настраивает параметры сериализации.</span><span class="sxs-lookup"><span data-stu-id="44df8-193">Customizes the serialization settings.</span></span>
* <span data-ttu-id="44df8-194">Зависит от функций, предоставляемых `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="44df8-194">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="44df8-195">Настраивается `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="44df8-195">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="44df8-196">В ASP.NET Core более ранних версий, чем версия 3.0, `JsonResult.SerializerSettings` принимает экземпляр `JsonSerializerSettings`, который относится к `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="44df8-196">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="44df8-197">Создается документация [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).</span><span class="sxs-lookup"><span data-stu-id="44df8-197">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="44df8-198">Функции для форматировщиков на основе `Newtonsoft.Json` можно настроить с помощью `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span><span class="sxs-lookup"><span data-stu-id="44df8-198">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="44df8-199">Параметры сериализации выходных данных для отдельных действий можно настроить с помощью `JsonResult`.</span><span class="sxs-lookup"><span data-stu-id="44df8-199">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="44df8-200">Пример:</span><span class="sxs-lookup"><span data-stu-id="44df8-200">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        options.Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="44df8-201">Добавление поддержки формата XML</span><span class="sxs-lookup"><span data-stu-id="44df8-201">Add XML format support</span></span>

<span data-ttu-id="44df8-202">Для форматирования XML требуется пакет NuGet [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/).</span><span class="sxs-lookup"><span data-stu-id="44df8-202">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="44df8-203">Форматировщики XML, реализованные с помощью <xref:System.Xml.Serialization.XmlSerializer>, можно настроить путем вызова <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span><span class="sxs-lookup"><span data-stu-id="44df8-203">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="44df8-204">Приведенный выше код сериализует результаты с помощью `XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="44df8-204">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="44df8-205">При использовании приведенного выше кода методы контроллера должны возвращать соответствующий формат на основе заголовка `Accept` запроса.</span><span class="sxs-lookup"><span data-stu-id="44df8-205">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="44df8-206">Указание формата</span><span class="sxs-lookup"><span data-stu-id="44df8-206">Specify a format</span></span>

<span data-ttu-id="44df8-207">Чтобы ограничить форматы ответа, примените фильтр [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute).</span><span class="sxs-lookup"><span data-stu-id="44df8-207">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="44df8-208">Как и большинство [фильтров](xref:mvc/controllers/filters), `[Produces]` можно применить к действию, контроллеру или глобальной области.</span><span class="sxs-lookup"><span data-stu-id="44df8-208">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="44df8-209">Предыдущий фильтр [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute):</span><span class="sxs-lookup"><span data-stu-id="44df8-209">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="44df8-210">Заставляет все действия в контроллере возвращать ответы в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="44df8-210">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="44df8-211">Если другие форматировщики настроены и клиент указывает другой формат, возвращается JSON.</span><span class="sxs-lookup"><span data-stu-id="44df8-211">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="44df8-212">Дополнительные сведения см. в статье [Фильтры в ASP.NET Core](xref:mvc/controllers/filters).</span><span class="sxs-lookup"><span data-stu-id="44df8-212">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="44df8-213">Специальные форматировщики</span><span class="sxs-lookup"><span data-stu-id="44df8-213">Special case formatters</span></span>

<span data-ttu-id="44df8-214">Встроенные модули форматирования реализуют некоторые специальные возможности.</span><span class="sxs-lookup"><span data-stu-id="44df8-214">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="44df8-215">По умолчанию типы возвращаемых значений `string` форматируются как *text/plain* (*text/html*, если того требует заголовок `Accept`).</span><span class="sxs-lookup"><span data-stu-id="44df8-215">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="44df8-216">Это поведение можно отключить, удалив <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="44df8-216">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span></span> <span data-ttu-id="44df8-217">Форматировщики удаляются в методе `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="44df8-217">Formatters are removed in the `ConfigureServices` method.</span></span> <span data-ttu-id="44df8-218">Действия, у которых типом возвращаемого объекта является модель, возвращают ответ `204 No Content` при возврате значения `null`.</span><span class="sxs-lookup"><span data-stu-id="44df8-218">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="44df8-219">Это поведение можно отключить, удалив <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span><span class="sxs-lookup"><span data-stu-id="44df8-219">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="44df8-220">Приведенный ниже код удаляет `StringOutputFormatter` и `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="44df8-220">The following code removes the `StringOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="44df8-221">Без `StringOutputFormatter`, встроенный модуль форматирования JSON форматирует типы возвращаемого значения `string`.</span><span class="sxs-lookup"><span data-stu-id="44df8-221">Without the `StringOutputFormatter`, the built-in JSON formatter formats `string` return types.</span></span> <span data-ttu-id="44df8-222">Если встроенный модуль форматирования JSON удален и доступен модуль форматирования XML, то типы возвращаемого значения `string` форматирует модуль форматирования XML.</span><span class="sxs-lookup"><span data-stu-id="44df8-222">If the built-in JSON formatter is removed and an XML formatter is available, the XML formatter formats `string` return types.</span></span> <span data-ttu-id="44df8-223">В противном случае, `string` типы возвращаемого значения возвращают `406 Not Acceptable`.</span><span class="sxs-lookup"><span data-stu-id="44df8-223">Otherwise, `string` return types return `406 Not Acceptable`.</span></span>

<span data-ttu-id="44df8-224">Без `HttpNoContentOutputFormatter` объекты со значением null форматируются с помощью настроенного модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="44df8-224">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="44df8-225">Пример:</span><span class="sxs-lookup"><span data-stu-id="44df8-225">For example:</span></span>

* <span data-ttu-id="44df8-226">Форматировщик JSON возвращает ответ с текстом `null`.</span><span class="sxs-lookup"><span data-stu-id="44df8-226">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="44df8-227">Форматировщик XML возвращает пустой XML-элемент с атрибутом `xsi:nil="true"`.</span><span class="sxs-lookup"><span data-stu-id="44df8-227">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="44df8-228">Сопоставления URL-адреса для формата ответа</span><span class="sxs-lookup"><span data-stu-id="44df8-228">Response format URL mappings</span></span>

<span data-ttu-id="44df8-229">Клиенты могут запрашивать определенный формат как часть URL-адреса, например:</span><span class="sxs-lookup"><span data-stu-id="44df8-229">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="44df8-230">В строке запроса или в части пути.</span><span class="sxs-lookup"><span data-stu-id="44df8-230">In the query string or part of the path.</span></span>
* <span data-ttu-id="44df8-231">С использованием расширения файла конкретного формата, такого как XML или JSON.</span><span class="sxs-lookup"><span data-stu-id="44df8-231">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="44df8-232">Сопоставление из пути запроса должно быть указано в маршруте, используемом API.</span><span class="sxs-lookup"><span data-stu-id="44df8-232">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="44df8-233">Пример:</span><span class="sxs-lookup"><span data-stu-id="44df8-233">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="44df8-234">Этот маршрут позволяет задать запрошенный формат в качестве дополнительного расширения файла.</span><span class="sxs-lookup"><span data-stu-id="44df8-234">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="44df8-235">Атрибут [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) проверяет наличие значения формата в `RouteData` и сопоставляет этот формат ответа с соответствующим форматировщиком при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="44df8-235">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="44df8-236">Маршрут</span><span class="sxs-lookup"><span data-stu-id="44df8-236">Route</span></span>        |             <span data-ttu-id="44df8-237">Formatter</span><span class="sxs-lookup"><span data-stu-id="44df8-237">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="44df8-238">Модуль форматирования вывода по умолчанию</span><span class="sxs-lookup"><span data-stu-id="44df8-238">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="44df8-239">Модуль форматирования JSON (если настроен)</span><span class="sxs-lookup"><span data-stu-id="44df8-239">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="44df8-240">Модуль форматирования XML (если настроен)</span><span class="sxs-lookup"><span data-stu-id="44df8-240">The XML formatter (if configured)</span></span>  |
