---
title: Форматирование данных отклика в веб-API ASP.NET Core
author: ardalis
description: Сведения о форматировании данных отклика в веб-API ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 05/29/2019
uid: web-api/advanced/formatting
ms.openlocfilehash: e3417c9bfd3824133b86de2fe74f5f71367e1560
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886532"
---
<!-- DO NOT EDIT BEFORE https://github.com/aspnet/AspNetCore.Docs/pull/12077 MERGES -->
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="d8b47-103">Форматирование данных отклика в веб-API ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8b47-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="d8b47-104">Автор: [Стив Смит](https://ardalis.com/) (Steve Smith)</span><span class="sxs-lookup"><span data-stu-id="d8b47-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d8b47-105">ASP.NET Core MVC имеет встроенную поддержку для форматирования данных отклика с использованием фиксированных форматов или с учетом спецификаций клиента.</span><span class="sxs-lookup"><span data-stu-id="d8b47-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="d8b47-106">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d8b47-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="d8b47-107">Результаты действий для конкретного формата</span><span class="sxs-lookup"><span data-stu-id="d8b47-107">Format-Specific Action Results</span></span>

<span data-ttu-id="d8b47-108">Некоторые типы результатов действий характерны для определенного формата, например `JsonResult` и `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="d8b47-109">Действия могут возвращать определенные результаты, которые всегда форматируются определенным образом.</span><span class="sxs-lookup"><span data-stu-id="d8b47-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="d8b47-110">Например, при возврате `JsonResult` независимо от параметров клиента возвращаются данные в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="d8b47-111">Аналогичным образом, при возврате `ContentResult` возвращаются строковые данные в формате обычного текста (как и в случае возврата простой строки).</span><span class="sxs-lookup"><span data-stu-id="d8b47-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="d8b47-112">Действию не требуется возвращать какой-либо определенный тип; MVC поддерживает любое значение, возвращаемое объектом.</span><span class="sxs-lookup"><span data-stu-id="d8b47-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="d8b47-113">Если действие возвращает реализацию `IActionResult` и контроллер наследует от `Controller`, разработчики имеют множество вспомогательных методов, соответствующих различным вариантам.</span><span class="sxs-lookup"><span data-stu-id="d8b47-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="d8b47-114">Результаты из действий, возвращающих объекты, которые не являются типами `IActionResult`, будут сериализованы с помощью соответствующей реализации `IOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="d8b47-115">Чтобы возвратить данные в определенном формате из контроллера, наследующего от базового класса `Controller`, используйте встроенный вспомогательный метод `Json` для возврата JSON и `Content` для обычного текста.</span><span class="sxs-lookup"><span data-stu-id="d8b47-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="d8b47-116">Метод действия должен возвращать конкретный тип результата (например, `JsonResult`) или `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="d8b47-117">Возврат данных в формате JSON:</span><span class="sxs-lookup"><span data-stu-id="d8b47-117">Returning JSON-formatted data:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="d8b47-118">Пример отклика из этого действия:</span><span class="sxs-lookup"><span data-stu-id="d8b47-118">Sample response from this action:</span></span>

![Вкладка "Сеть" средств разработчика в Microsoft Edge, где в поле "Тип содержимого" для отклика указано значение application/json](formatting/_static/json-response.png)

<span data-ttu-id="d8b47-120">Обратите внимание, что как в списке сетевых запросов, так и в разделе "Заголовки ответа" для отклика указан тип содержимого `application/json`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="d8b47-121">Кроме того, обратите внимание на список параметров, предоставленный браузером (в данном случае Microsoft Edge) в заголовке Accept в разделе "Заголовки запроса".</span><span class="sxs-lookup"><span data-stu-id="d8b47-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="d8b47-122">Сейчас этот заголовок следует игнорировать, учитывая приведенные ниже данные о нем.</span><span class="sxs-lookup"><span data-stu-id="d8b47-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="d8b47-123">Чтобы возвратить данные в формате обычного текста, используйте `ContentResult` и вспомогательный метод `Content`:</span><span class="sxs-lookup"><span data-stu-id="d8b47-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="d8b47-124">Отклик из этого действия:</span><span class="sxs-lookup"><span data-stu-id="d8b47-124">A response from this action:</span></span>

![Вкладка "Сеть" средств разработчика в Microsoft Edge, где в поле "Тип содержимого" для отклика указано значение text/plain](formatting/_static/text-response.png)

<span data-ttu-id="d8b47-126">Обратите внимание, что в этом случае возвращаемый `Content-Type` имеет значение `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="d8b47-127">Аналогичного результата можно добиться с помощью одного только строкового типа отклика:</span><span class="sxs-lookup"><span data-stu-id="d8b47-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="d8b47-128">Для нетривиальных действий с несколькими типами возвращаемого значения или возвращаемыми параметрами (например, различными кодами состояния HTTP в зависимости от выполненных операций) рекомендуется использовать `IActionResult` в качестве типа возвращаемого значения.</span><span class="sxs-lookup"><span data-stu-id="d8b47-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="d8b47-129">Согласование содержимого</span><span class="sxs-lookup"><span data-stu-id="d8b47-129">Content Negotiation</span></span>

<span data-ttu-id="d8b47-130">Согласование *содержимого* происходит, когда клиент задает [заголовок Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="d8b47-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="d8b47-131">Для ASP.NET Core MVC по умолчанию используется формат JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="d8b47-132">Согласование содержимого реализуется `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="d8b47-133">Оно также встроено в результаты действия с определенным кодом состояния, возвращаемые из вспомогательных методов (которые все основаны на `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="d8b47-133">It's also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="d8b47-134">Вы также можете возвратить тип модели (класс, определенный в качестве типа передачи данных), при этом платформа автоматически использует для него оболочку `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="d8b47-135">Следующий метод действия использует вспомогательные методы `Ok` и `NotFound`:</span><span class="sxs-lookup"><span data-stu-id="d8b47-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="d8b47-136">Если только не был запрошен иной формат и сервер способен возвратить данные в нем, возвращается отклик в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="d8b47-137">Вы можете использовать такое средство, как [Fiddler](https://www.telerik.com/fiddler), чтобы создать запрос, содержащий заголовок Accept, и указать другой формат.</span><span class="sxs-lookup"><span data-stu-id="d8b47-137">You can use a tool like [Fiddler](https://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="d8b47-138">В этом случае, если на сервере есть *модуль форматирования*, способный создать отклик в запрошенном формате, результат возвращается в формате, предпочитаемом клиентом.</span><span class="sxs-lookup"><span data-stu-id="d8b47-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Консоль Fiddler, показывающая созданный вручную запрос GET, в котором заголовок Accept имеет значение application/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="d8b47-140">На предыдущем снимке экрана средство Fiddler Composer используется для создания запроса, для чего указано значение `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="d8b47-141">По умолчанию ASP.NET Core MVC поддерживает только JSON, поэтому даже при указании другого формата возвращаемый результат все равно будет иметь формат JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="d8b47-142">Сведения о добавлении дополнительных модулей форматирования приведены в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d8b47-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="d8b47-143">Действия контроллера могут возвращать объекты POCO, при этом ASP.NET Core MVC автоматически создает `ObjectResult`, используемый в качестве оболочки для такого объекта.</span><span class="sxs-lookup"><span data-stu-id="d8b47-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET Core MVC automatically creates an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="d8b47-144">Клиент получает отформатированный сериализованный объект (по умолчанию используется формат JSON, но можно настроить XML или другие форматы).</span><span class="sxs-lookup"><span data-stu-id="d8b47-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="d8b47-145">Если возвращается объект `null`, платформа возвращает отклик `204 No Content`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="d8b47-146">Возвращение типа объекта:</span><span class="sxs-lookup"><span data-stu-id="d8b47-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="d8b47-147">В этом примере запрос допустимого псевдонима автора получает отклик "200 OK" с данными об авторе.</span><span class="sxs-lookup"><span data-stu-id="d8b47-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="d8b47-148">Запрос недопустимого псевдонима получает отклик "204 Нет содержимого".</span><span class="sxs-lookup"><span data-stu-id="d8b47-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="d8b47-149">Ниже приведены снимки экрана, показывающие отклик в форматах XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="d8b47-150">Процесс согласования содержимого</span><span class="sxs-lookup"><span data-stu-id="d8b47-150">Content Negotiation Process</span></span>

<span data-ttu-id="d8b47-151">*Согласование* содержимого выполняется только при наличии заголовка `Accept` в запросе.</span><span class="sxs-lookup"><span data-stu-id="d8b47-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="d8b47-152">Если запрос содержит заголовок Accept, платформа перечисляет типы мультимедиа в заголовке Accept в порядке предпочтения и попытается найти модуль форматирования, способный создать отклик в одном из форматов, указанных в заголовке Accept.</span><span class="sxs-lookup"><span data-stu-id="d8b47-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="d8b47-153">Если модуль форматирования, способный удовлетворить запрос клиента, не найден, платформа пытается найти первый модуль форматирования, способный создать отклик (если только разработчик не настроил параметр `MvcOptions` для возврата отклика "406 Неприемлемо").</span><span class="sxs-lookup"><span data-stu-id="d8b47-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="d8b47-154">Если в запросе указан XML, однако модуль форматирования XML не настроен, используется модуль форматирования JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="d8b47-155">В общем случае если модуль форматирования, обеспечивающий требуемый формат, не настроен, используется первый модуль форматирования, способный отформатировать данный объект.</span><span class="sxs-lookup"><span data-stu-id="d8b47-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="d8b47-156">Если заголовок не указан, для сериализации отклика используется первый модуль форматирования, способный обработать возвращаемый объект.</span><span class="sxs-lookup"><span data-stu-id="d8b47-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="d8b47-157">В этом случае никакое согласование не выполняется — используемый формат определяется сервером.</span><span class="sxs-lookup"><span data-stu-id="d8b47-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="d8b47-158">Если заголовок Accept содержит `*/*`, он игнорируется, если только `RespectBrowserAcceptHeader` не имеет значение true в `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="d8b47-159">Браузеры и согласование содержимого</span><span class="sxs-lookup"><span data-stu-id="d8b47-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="d8b47-160">В отличие от типичных клиентов API веб-браузеры, как правило, предоставляют заголовки `Accept` с обширным набором форматов, включая подстановочные знаки.</span><span class="sxs-lookup"><span data-stu-id="d8b47-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="d8b47-161">По умолчанию, когда платформа обнаруживает поступающий из браузера запрос, она игнорирует заголовок `Accept` и возвращает содержимое в формате по умолчанию, настроенном в приложении (JSON, если не указано иное).</span><span class="sxs-lookup"><span data-stu-id="d8b47-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="d8b47-162">Это обеспечивает более согласованное взаимодействие при использовании различных браузеров для работы с API.</span><span class="sxs-lookup"><span data-stu-id="d8b47-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="d8b47-163">Если вы хотите, чтобы приложение учитывало заголовки Accept в браузере, это можно настроить в конфигурации MVC, задав для `RespectBrowserAcceptHeader` значение `true` в методе `ConfigureServices` в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="d8b47-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="d8b47-164">Настройка модулей форматирования</span><span class="sxs-lookup"><span data-stu-id="d8b47-164">Configuring Formatters</span></span>

<span data-ttu-id="d8b47-165">Если приложение должно поддерживать дополнительные форматы, кроме стандартного JSON, можно добавить пакеты NuGet и настроить MVC для их поддержки.</span><span class="sxs-lookup"><span data-stu-id="d8b47-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="d8b47-166">Существуют отдельные модули форматирования для ввода и вывода.</span><span class="sxs-lookup"><span data-stu-id="d8b47-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="d8b47-167">Модули форматирования ввода используются [привязкой модели](xref:mvc/models/model-binding); модули форматирования вывода используются для форматирования откликов.</span><span class="sxs-lookup"><span data-stu-id="d8b47-167">Input formatters are used by [Model Binding](xref:mvc/models/model-binding); output formatters are used to format responses.</span></span> <span data-ttu-id="d8b47-168">Можно также настроить [пользовательские модули форматирования](xref:web-api/advanced/custom-formatters).</span><span class="sxs-lookup"><span data-stu-id="d8b47-168">You can also configure [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="d8b47-169">Настройка форматировщиков на основе System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="d8b47-169">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="d8b47-170">Функции для форматировщиков на основе `System.Text.Json` можно настроить с помощью `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-170">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcOptions.SerializerOptions`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.SerializerOptions.WriterSettings.Indented = true;
});
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="d8b47-171">Добавление поддержки формата JSON на основе Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="d8b47-171">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="d8b47-172">До выпуска версии ASP.NET Core 3.0 в MVC по умолчанию использовались форматировщики JSON, реализованные с помощью пакета `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-172">Prior to ASP.NET Core 3.0, MVC defaulted to using JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="d8b47-173">В ASP.NET Core 3.0 или более поздней версии форматировщики JSON по умолчанию основаны на `System.Text.Json`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-173">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="d8b47-174">Чтобы добавить поддержку форматировщиков и функций на основе `Newtonsoft.Json`, установите пакет NuGet [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) и настройте его в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-174">Support for `Newtonsoft.Json`-based formatters and features is available by installing the [Microsoft.AspNetCore.Mvc.NewtonsoftJson](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

```csharp
services.AddMvc()
    .AddNewtonsoftJson();
```

<span data-ttu-id="d8b47-175">Возможно, некоторые функции не будут оптимально работать с форматировщиками на основе `System.Text.Json` и будут требовать ссылки на форматировщики на основе `Newtonsoft.Json` для выпуска ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d8b47-175">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters for the ASP.NET Core 3.0 release.</span></span> <span data-ttu-id="d8b47-176">Продолжайте работать с форматировщиками на основе `Newtonsoft.Json`, если при использовании приложения ASP.NET Core 3.0 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="d8b47-176">Continue using the `Newtonsoft.Json`-based formatters if your ASP.NET Core 3.0 or later app:</span></span>

* <span data-ttu-id="d8b47-177">Применяются атрибуты `Newtonsoft.Json` (например, `[JsonProperty]` или `[JsonIgnore]`), настраиваются параметры сериализации или работа приложения основана на функциях, которые предоставляет `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-177">Uses `Newtonsoft.Json` attributes (for example, `[JsonProperty]` or `[JsonIgnore]`), customizes the serialization settings, or relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="d8b47-178">Настраивается `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-178">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="d8b47-179">В ASP.NET Core более ранних версий, чем версия 3.0, `JsonResult.SerializerSettings` принимает экземпляр `JsonSerializerSettings`, который относится к `Newtonsoft.Json`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-179">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="d8b47-180">Создается документация [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>).</span><span class="sxs-lookup"><span data-stu-id="d8b47-180">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

::: moniker-end

### <a name="add-xml-format-support"></a><span data-ttu-id="d8b47-181">Добавление поддержки формата XML</span><span class="sxs-lookup"><span data-stu-id="d8b47-181">Add XML format support</span></span>

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d8b47-182">Чтобы включить поддержку форматирования XML в ASP.NET Core версии 2.2 или более ранней, установите пакет NuGet [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/).</span><span class="sxs-lookup"><span data-stu-id="d8b47-182">To add XML formatting support in ASP.NET Core 2.2 or earlier, install the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

::: moniker-end

<span data-ttu-id="d8b47-183">Форматировщики XML, реализованные с помощью `System.Xml.Serialization.XmlSerializer`, можно настроить путем вызова <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d8b47-183">XML formatters implemented using `System.Xml.Serialization.XmlSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="d8b47-184">А форматировщики XML, реализованные с помощью `System.Runtime.Serialization.DataContractSerializer`, можно настроить путем вызова <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d8b47-184">Alternatively, XML formatters implemented using `System.Runtime.Serialization.DataContractSerializer` can be configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlDataContractSerializerFormatters*> in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .AddXmlDataContractSerializerFormatters();
```

<span data-ttu-id="d8b47-185">После добавления поддержки для форматирования XML методы контроллера должны возвращать формат, соответствующий заголовку `Accept` запроса, как показано в следующем примере Fiddler:</span><span class="sxs-lookup"><span data-stu-id="d8b47-185">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Консоль Fiddler: на вкладке "Raw" (Необработанные) для запроса показано, что заголовок Accept имеет значение application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="d8b47-188">На вкладке "Inspectors" (Инспекторы) видно, что необработанный запрос GET был выполнен с использованием набора заголовков `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-188">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="d8b47-189">В области откликов отображается заголовок `Content-Type: application/xml`, а объект `Author` сериализуется в формат XML.</span><span class="sxs-lookup"><span data-stu-id="d8b47-189">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="d8b47-190">На вкладке "Composer" (Редактор) можно изменить запрос, указав `application/json` в заголовке `Accept`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-190">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="d8b47-191">Выполните запрос, при этом отклик будет иметь формат JSON:</span><span class="sxs-lookup"><span data-stu-id="d8b47-191">Execute the request, and the response will be formatted as JSON:</span></span>

![Консоль Fiddler: на вкладке "Raw" (Необработанные) для запроса показано, что заголовок Accept имеет значение application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="d8b47-194">На этом снимке экрана видно, что запрос задает заголовок `Accept: application/json`, а отклик указывает то же значение в `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-194">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="d8b47-195">Объект `Author` отображается в тексте ответа в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-195">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="d8b47-196">Принудительное использование определенного формата</span><span class="sxs-lookup"><span data-stu-id="d8b47-196">Forcing a Particular Format</span></span>

<span data-ttu-id="d8b47-197">Если вы хотите ограничить форматы отклика для конкретного действия, можно применить фильтр `[Produces]`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-197">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="d8b47-198">Фильтр `[Produces]` указывает форматы отклика для определенного действия (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="d8b47-198">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="d8b47-199">Как и большинство [фильтров](xref:mvc/controllers/filters), его можно применить к действию, контроллеру или глобальной области.</span><span class="sxs-lookup"><span data-stu-id="d8b47-199">Like most [Filters](xref:mvc/controllers/filters), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="d8b47-200">Фильтр `[Produces]` принуждает все действия в `AuthorsController` возвращать отклики в формате JSON, даже если для приложения настроены другие модули форматирования, а клиент предоставляет заголовок `Accept`, запрашивающий другой доступный формат.</span><span class="sxs-lookup"><span data-stu-id="d8b47-200">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="d8b47-201">Раздел [Фильтры](xref:mvc/controllers/filters) содержит дополнительные сведения, включая способы глобального применения фильтров.</span><span class="sxs-lookup"><span data-stu-id="d8b47-201">See [Filters](xref:mvc/controllers/filters) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="d8b47-202">Специальные модули форматирования</span><span class="sxs-lookup"><span data-stu-id="d8b47-202">Special Case Formatters</span></span>

<span data-ttu-id="d8b47-203">Встроенные модули форматирования реализуют некоторые специальные возможности.</span><span class="sxs-lookup"><span data-stu-id="d8b47-203">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="d8b47-204">По умолчанию типы возвращаемых значений `string` форматируются как *text/plain* (*text/html*, если того требует заголовок `Accept`).</span><span class="sxs-lookup"><span data-stu-id="d8b47-204">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="d8b47-205">Это поведение можно отключить, удалив `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-205">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="d8b47-206">Удалить модули форматирования можно в методе `Configure` в файле *Startup.cs* (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="d8b47-206">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="d8b47-207">Действия, у которых типом возвращаемого объекта является модель, возвращают отклик "204 Нет содержимого" при возврате `null`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-207">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="d8b47-208">Это поведение можно отключить, удалив `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-208">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="d8b47-209">Приведенный ниже код удаляет `TextOutputFormatter` и `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-209">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="d8b47-210">Без `TextOutputFormatter` типы возвращаемых значений `string` возвращают, например, отклик "406 Неприемлемо".</span><span class="sxs-lookup"><span data-stu-id="d8b47-210">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="d8b47-211">Обратите внимание, что если модуль форматирования XML существует, он форматирует типы возвращаемых значений `string`, когда `TextOutputFormatter` удален.</span><span class="sxs-lookup"><span data-stu-id="d8b47-211">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="d8b47-212">Без `HttpNoContentOutputFormatter` объекты со значением null форматируются с помощью настроенного модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="d8b47-212">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="d8b47-213">Например, модуль форматирования JSON просто возвращает отклик с текстом `null`, а модуль форматирования XML — пустой элемент XML с заданным атрибутом `xsi:nil="true"`.</span><span class="sxs-lookup"><span data-stu-id="d8b47-213">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="d8b47-214">Сопоставления URL-адреса для формата отклика</span><span class="sxs-lookup"><span data-stu-id="d8b47-214">Response Format URL Mappings</span></span>

<span data-ttu-id="d8b47-215">Клиенты могут запрашивать определенный формат в URL-адресе, например в строке запроса или части пути, либо используя расширение файла определенного формата, такое как XML или JSON.</span><span class="sxs-lookup"><span data-stu-id="d8b47-215">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="d8b47-216">Сопоставление из пути запроса должно быть указано в маршруте, используемом API.</span><span class="sxs-lookup"><span data-stu-id="d8b47-216">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="d8b47-217">Например:</span><span class="sxs-lookup"><span data-stu-id="d8b47-217">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="d8b47-218">Этот маршрут позволяет задать запрошенный формат в качестве дополнительного расширения файла.</span><span class="sxs-lookup"><span data-stu-id="d8b47-218">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="d8b47-219">Атрибут `[FormatFilter]` проверяет наличие значения формата в `RouteData` и сопоставляет этот формат отклика с соответствующим модулем форматирования при создании отклика.</span><span class="sxs-lookup"><span data-stu-id="d8b47-219">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="d8b47-220">Маршрут</span><span class="sxs-lookup"><span data-stu-id="d8b47-220">Route</span></span>            |             <span data-ttu-id="d8b47-221">Formatter</span><span class="sxs-lookup"><span data-stu-id="d8b47-221">Formatter</span></span>              |
|----------------------------|------------------------------------|
|   `/products/GetById/5`    |    <span data-ttu-id="d8b47-222">Модуль форматирования вывода по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d8b47-222">The default output formatter</span></span>    |
| `/products/GetById/5.json` | <span data-ttu-id="d8b47-223">Модуль форматирования JSON (если настроен)</span><span class="sxs-lookup"><span data-stu-id="d8b47-223">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="d8b47-224">Модуль форматирования XML (если настроен)</span><span class="sxs-lookup"><span data-stu-id="d8b47-224">The XML formatter (if configured)</span></span>  |
