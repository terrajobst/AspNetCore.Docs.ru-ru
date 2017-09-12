---
title: "Данные форматирования ответа в ASP.NET Core MVC"
author: ardalis
description: "Инструкции по форматированию данные ответа в ASP.NET Core MVC."
keywords: "ASP.NET Core, данные ответа, IOutputFormatter, IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7fd1ecb61c7b1559f8bd304c30f7201864bd448
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="7ee94-104">Общие сведения о форматировании данных ответа в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="7ee94-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="7ee94-105">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="7ee94-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7ee94-106">ASP.NET Core MVC имеет встроенную поддержку для форматирования данных ответа, используя форматы фиксированной или в ответ на спецификации клиента.</span><span class="sxs-lookup"><span data-stu-id="7ee94-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="7ee94-107">[Просмотреть или загрузить пример из GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="7ee94-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="7ee94-108">Результаты действий конкретного формата</span><span class="sxs-lookup"><span data-stu-id="7ee94-108">Format-Specific Action Results</span></span>

<span data-ttu-id="7ee94-109">Некоторые типы результатов действий характерные для определенного формата, таких как `JsonResult` и `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="7ee94-110">Действия может возвращать определенные результаты, всегда форматируются определенным образом.</span><span class="sxs-lookup"><span data-stu-id="7ee94-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="7ee94-111">Например, возвращая `JsonResult` будет возвращать данные в формате JSON, независимо от параметров клиента.</span><span class="sxs-lookup"><span data-stu-id="7ee94-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="7ee94-112">Аналогично, возвращая `ContentResult` возвращают строковые данные в формате обычного текста (как будет возвратить строку).</span><span class="sxs-lookup"><span data-stu-id="7ee94-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="7ee94-113">Действие не требуется возвращать любой определенный тип; MVC поддерживает любое значение, возвращаемое объектом.</span><span class="sxs-lookup"><span data-stu-id="7ee94-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="7ee94-114">Если действие возвращает `IActionResult` реализацию и контроллер наследует от `Controller`, разработчики имеют много вспомогательные методы, соответствующие параметры.</span><span class="sxs-lookup"><span data-stu-id="7ee94-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="7ee94-115">Результаты из действий, которые возвращают объекты, которые не являются `IActionResult` типы будут сериализованы с помощью соответствующей `IOutputFormatter` реализации.</span><span class="sxs-lookup"><span data-stu-id="7ee94-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="7ee94-116">Для возврата данных в определенном формате из контроллера, который наследует от `Controller` базового класса, используйте встроенные вспомогательный метод `Json` для возврата JSON и `Content` для обычного текста.</span><span class="sxs-lookup"><span data-stu-id="7ee94-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="7ee94-117">Метод действия должен возвращать тип определенного результата (например, `JsonResult`) или `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="7ee94-118">Возврат данных в формате JSON:</span><span class="sxs-lookup"><span data-stu-id="7ee94-118">Returning JSON-formatted data:</span></span>

<span data-ttu-id="7ee94-119">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]</span><span class="sxs-lookup"><span data-stu-id="7ee94-119">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]</span></span>

<span data-ttu-id="7ee94-120">Образец ответа из этого действия:</span><span class="sxs-lookup"><span data-stu-id="7ee94-120">Sample response from this action:</span></span>

![Вкладка «сеть» средств разработчика в Microsoft Edge, показывающая тип содержимого ответа — application/json](formatting/_static/json-response.png)

<span data-ttu-id="7ee94-122">Обратите внимание, что тип содержимого ответа `application/json`, показанный в списке сетевых запросов и в разделе заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="7ee94-122">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="7ee94-123">Также Обратите внимание, в списке вариантов, представленных браузера в заголовке Accept в разделе заголовки запроса (в данном случае Microsoft Edge).</span><span class="sxs-lookup"><span data-stu-id="7ee94-123">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="7ee94-124">Текущий метод игнорирует этот заголовок; obeying он описывается ниже.</span><span class="sxs-lookup"><span data-stu-id="7ee94-124">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="7ee94-125">Чтобы вернуть данные в формате обычного текста, используйте `ContentResult` и `Content` поддержки:</span><span class="sxs-lookup"><span data-stu-id="7ee94-125">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

<span data-ttu-id="7ee94-126">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]</span><span class="sxs-lookup"><span data-stu-id="7ee94-126">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]</span></span>

<span data-ttu-id="7ee94-127">Ответ от этого действия:</span><span class="sxs-lookup"><span data-stu-id="7ee94-127">A response from this action:</span></span>

![Вкладка «сеть» средств разработчика в Microsoft Edge, показывающая тип содержимого ответа — text/plain](formatting/_static/text-response.png)

<span data-ttu-id="7ee94-129">Обратите внимание, в этом случае `Content-Type` возвращаемое `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-129">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="7ee94-130">Можно также выполнить таким же образом, с помощью только строковый тип ответа:</span><span class="sxs-lookup"><span data-stu-id="7ee94-130">You can also achieve this same behavior using just a string response type:</span></span>

<span data-ttu-id="7ee94-131">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]</span><span class="sxs-lookup"><span data-stu-id="7ee94-131">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]</span></span>

>[!TIP]
> <span data-ttu-id="7ee94-132">Для нетривиального действия с несколькими возвращаемого типа или параметры (например, на основе результатов операций, выполняемых различные коды состояния HTTP), предпочтительно `IActionResult` как тип возвращаемого значения.</span><span class="sxs-lookup"><span data-stu-id="7ee94-132">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="7ee94-133">Согласование содержимого</span><span class="sxs-lookup"><span data-stu-id="7ee94-133">Content Negotiation</span></span>

<span data-ttu-id="7ee94-134">Согласование содержимого (*conneg* сокращенно) происходит, когда клиент задает [заголовок Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="7ee94-134">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="7ee94-135">JSON используется формат по умолчанию для ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="7ee94-135">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="7ee94-136">Согласование содержимого реализуется `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-136">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="7ee94-137">Он также встроена в код состояния, определенного действия результаты, возвращенные из вспомогательных методов (все основанные на `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="7ee94-137">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="7ee94-138">Также можно возвратить тип модели (класс был определен в качестве типа передачи данных) и платформа будет автоматически включать его в `ObjectResult` для вас.</span><span class="sxs-lookup"><span data-stu-id="7ee94-138">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="7ee94-139">Следующий метод действия использует `Ok` и `NotFound` вспомогательные методы:</span><span class="sxs-lookup"><span data-stu-id="7ee94-139">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

<span data-ttu-id="7ee94-140">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]</span><span class="sxs-lookup"><span data-stu-id="7ee94-140">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]</span></span>

<span data-ttu-id="7ee94-141">Если не был запрошен в другом формате и сервер может вернуть запрошенный формат ответа формата JSON будут возвращены.</span><span class="sxs-lookup"><span data-stu-id="7ee94-141">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="7ee94-142">Можно использовать средства, подобного [Fiddler](http://www.telerik.com/fiddler) создать запрос, содержащий заголовок Accept и указать другой формат.</span><span class="sxs-lookup"><span data-stu-id="7ee94-142">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="7ee94-143">В этом случае, если на сервере *форматирования* дающую ответа в запрошенном пользователем формате, будет возвращен результат в формате предпочитаемый клиента.</span><span class="sxs-lookup"><span data-stu-id="7ee94-143">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler консоль, показывающая, вручную создать запрос со значением заголовка Accept application/XML GET](formatting/_static/fiddler-composer.png)

<span data-ttu-id="7ee94-145">В приведенном выше снимке экрана Fiddler редактор используется для создания запроса, указав `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-145">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="7ee94-146">По умолчанию ASP.NET Core MVC поддерживает только JSON, так что даже если указан другой формат, возвращаемый результат будет по-прежнему формата JSON.</span><span class="sxs-lookup"><span data-stu-id="7ee94-146">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="7ee94-147">Вы увидите, как добавить дополнительные модули форматирования в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="7ee94-147">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="7ee94-148">Действия контроллера может возвращать POCO (обычные старые объекты CLR), в этом случае ASP.NET MVC автоматически создаст `ObjectResult` , создает оболочку этого объекта.</span><span class="sxs-lookup"><span data-stu-id="7ee94-148">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="7ee94-149">Клиент получит форматированные сериализованного объекта (по умолчанию используется формат JSON, XML или в других форматах можно настроить).</span><span class="sxs-lookup"><span data-stu-id="7ee94-149">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="7ee94-150">Если объекта возвращается `null`, платформа возвратит `204 No Content` ответа.</span><span class="sxs-lookup"><span data-stu-id="7ee94-150">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="7ee94-151">Возвращает тип объекта:</span><span class="sxs-lookup"><span data-stu-id="7ee94-151">Returning an object type:</span></span>

<span data-ttu-id="7ee94-152">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]</span><span class="sxs-lookup"><span data-stu-id="7ee94-152">[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]</span></span>

<span data-ttu-id="7ee94-153">В этом образце запроса для псевдонима допустимым автор получит ответ 200 ОК с данными автора.</span><span class="sxs-lookup"><span data-stu-id="7ee94-153">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="7ee94-154">Недопустимый псевдоним запрос получит 204 Нет содержимого ответа.</span><span class="sxs-lookup"><span data-stu-id="7ee94-154">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="7ee94-155">Ниже приведены снимки экрана, показывающий ответа в форматах XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="7ee94-155">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="7ee94-156">Процесс согласования содержимого</span><span class="sxs-lookup"><span data-stu-id="7ee94-156">Content Negotiation Process</span></span>

<span data-ttu-id="7ee94-157">Содержимого *согласование* поместите происходит только в случае, если `Accept` заголовок отображается в запросе.</span><span class="sxs-lookup"><span data-stu-id="7ee94-157">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="7ee94-158">Если запрос содержит заголовок accept, платформа перечисляет типы носителей в заголовке accept в порядке предпочтения и попытается найти средство форматирования, который может производить ответа в одном из форматов, указанных в заголовке accept.</span><span class="sxs-lookup"><span data-stu-id="7ee94-158">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="7ee94-159">В случае, если модуль форматирования не найден, можно удовлетворить запрос клиента, платформа будет пытаться найти первый модуль форматирования, который может производить ответ (если разработчик настроил параметр `MvcOptions` для возврата 406 неприемлемо вместо).</span><span class="sxs-lookup"><span data-stu-id="7ee94-159">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="7ee94-160">Если в запросе указан XML, но не был настроен модуль форматирования XML, будет использовать модуль форматирования JSON.</span><span class="sxs-lookup"><span data-stu-id="7ee94-160">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="7ee94-161">В общем случае если модуль форматирования не настроен, можно указать требуемый формат, то используется первый модуль форматирования не может отформатировать объект.</span><span class="sxs-lookup"><span data-stu-id="7ee94-161">More generally, if no formatter is configured that can provide the requested format, then the first formatter than can format the object is used.</span></span> <span data-ttu-id="7ee94-162">Если заголовок не указан, первый модуль форматирования, который может обрабатывать объекта, возвращаемого будет использоваться для сериализации ответа.</span><span class="sxs-lookup"><span data-stu-id="7ee94-162">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="7ee94-163">В этом случае не все согласования происходящей - сервер определяет, какой формат, который будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="7ee94-163">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="7ee94-164">Если заголовок Accept содержит `*/*`, заголовок будет игнорироваться, если `RespectBrowserAcceptHeader` имеет значение true для `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-164">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="7ee94-165">Браузеры и согласования содержимого</span><span class="sxs-lookup"><span data-stu-id="7ee94-165">Browsers and Content Negotiation</span></span>

<span data-ttu-id="7ee94-166">В отличие от типичные клиенты API веб-браузеры, как правило, для предоставления `Accept` заголовки, которые включают широкий набор форматов, включая подстановочные знаки.</span><span class="sxs-lookup"><span data-stu-id="7ee94-166">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="7ee94-167">По умолчанию при обнаружении платформу, запрос поступает из браузера, он игнорирует `Accept` заголовок и вместо него возвращаемое содержимое в приложении настройки по умолчанию формат (JSON пока иначе настроены).</span><span class="sxs-lookup"><span data-stu-id="7ee94-167">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="7ee94-168">Это обеспечивает более согласованной работы при использовании различных браузеров для использовать интерфейсы API.</span><span class="sxs-lookup"><span data-stu-id="7ee94-168">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="7ee94-169">Если вы предпочитаете заголовки accept браузере честь приложения, это можно настроить как часть конфигурации MVC, задав `RespectBrowserAcceptHeader` для `true` в `ConfigureServices` метод в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7ee94-169">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
}
```

## <a name="configuring-formatters"></a><span data-ttu-id="7ee94-170">Настройка модулей форматирования</span><span class="sxs-lookup"><span data-stu-id="7ee94-170">Configuring Formatters</span></span>

<span data-ttu-id="7ee94-171">Если приложению для поддержки дополнительных форматов, помимо стандартных JSON, можно добавить пакеты NuGet и настроить MVC для их поддержки.</span><span class="sxs-lookup"><span data-stu-id="7ee94-171">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="7ee94-172">Существуют отдельные модули форматирования для ввода и вывода.</span><span class="sxs-lookup"><span data-stu-id="7ee94-172">There are separate formatters for input and output.</span></span> <span data-ttu-id="7ee94-173">Входной модули форматирования, используемые [привязки модели](model-binding.md); модули форматирования выходных данных, используемые для форматирования ответов.</span><span class="sxs-lookup"><span data-stu-id="7ee94-173">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="7ee94-174">Можно также настроить [пользовательские модули форматирования](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="7ee94-174">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="7ee94-175">Добавление поддержки формата XML</span><span class="sxs-lookup"><span data-stu-id="7ee94-175">Adding XML Format Support</span></span>

<span data-ttu-id="7ee94-176">Чтобы добавить поддержку форматирования XML-кода, установите `Microsoft.AspNetCore.Mvc.Formatters.Xml` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="7ee94-176">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="7ee94-177">Добавить XmlSerializerFormatters конфигурацию MVC в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ee94-177">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

<span data-ttu-id="7ee94-178">[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="7ee94-178">[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]</span></span>

<span data-ttu-id="7ee94-179">Кроме того можно добавить только форматирования выходных данных:</span><span class="sxs-lookup"><span data-stu-id="7ee94-179">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="7ee94-180">Эти два подхода выполняет сериализацию результатов с помощью `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-180">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="7ee94-181">При желании можно использовать `System.Runtime.Serialization.DataContractSerializer` путем добавления его связанный форматирования:</span><span class="sxs-lookup"><span data-stu-id="7ee94-181">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="7ee94-182">После добавления поддержки форматирования XML на контроллер методы должны возвращать формат в запросе `Accept` заголовок как Fiddler, этот пример:</span><span class="sxs-lookup"><span data-stu-id="7ee94-182">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler консоли: необработанные вкладке для запроса отображается значение заголовка Accept приложения/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="7ee94-185">Можно увидеть на вкладке инспекторах, был сделан запрос на получение необработанных `Accept: application/xml` задан заголовок.</span><span class="sxs-lookup"><span data-stu-id="7ee94-185">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="7ee94-186">В области отображается ответ `Content-Type: application/xml` заголовок и `Author` сериализованный объект в формат XML.</span><span class="sxs-lookup"><span data-stu-id="7ee94-186">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="7ee94-187">Вкладка редактор для изменения запроса для указания `application/json` в `Accept` заголовок.</span><span class="sxs-lookup"><span data-stu-id="7ee94-187">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="7ee94-188">Выполнение запроса и ответа будут отформатированы в качестве JSON:</span><span class="sxs-lookup"><span data-stu-id="7ee94-188">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler консоли: необработанные вкладке для запроса отображается значение заголовка Accept — application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="7ee94-191">На этом снимке экрана можно увидеть в запросе задан заголовок `Accept: application/json` и ответ указывает таким же, как его `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-191">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="7ee94-192">`Author` Объекта отображается в тексте ответа в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="7ee94-192">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="7ee94-193">Принудительное определенного формата</span><span class="sxs-lookup"><span data-stu-id="7ee94-193">Forcing a Particular Format</span></span>

<span data-ttu-id="7ee94-194">Если вы хотите ограничить форматы ответа для конкретного действия можно выполнить, можно применить `[Produces]` фильтра.</span><span class="sxs-lookup"><span data-stu-id="7ee94-194">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="7ee94-195">`[Produces]` Фильтр указывает форматы ответа для определенных действий (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="7ee94-195">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="7ee94-196">Как и большинство [фильтры](../controllers/filters.md), могут применяться в действия, контроллера или глобальной области.</span><span class="sxs-lookup"><span data-stu-id="7ee94-196">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="7ee94-197">`[Produces]` Фильтр будет принудительно всем действиям в `AuthorsController` для возврата ответов формата JSON, даже если были настроены другие модули форматирования для приложения и предоставляются клиентом `Accept` заголовок запроса другой, доступной формат.</span><span class="sxs-lookup"><span data-stu-id="7ee94-197">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="7ee94-198">В разделе [фильтры](../controllers/filters.md) для получения дополнительных сведений, включая способы применения фильтров глобально.</span><span class="sxs-lookup"><span data-stu-id="7ee94-198">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="7ee94-199">Специальные вариантов форматирования данных</span><span class="sxs-lookup"><span data-stu-id="7ee94-199">Special Case Formatters</span></span>

<span data-ttu-id="7ee94-200">Некоторые особые случаи реализуются с помощью встроенных модулей форматирования.</span><span class="sxs-lookup"><span data-stu-id="7ee94-200">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="7ee94-201">По умолчанию `string` возвращаемые типы будут отформатированы в качестве *text/plain* (*текст или HTML-* по запросу через `Accept` заголовок).</span><span class="sxs-lookup"><span data-stu-id="7ee94-201">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="7ee94-202">Это поведение можно удалить, удалив `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-202">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="7ee94-203">Удаление модулей форматирования в `Configure` метод в *файла Startup.cs* (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="7ee94-203">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="7ee94-204">Действия, которые имеют объекта модели возвращают тип вернет 204 Нет содержимого ответа при возврате `null`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-204">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="7ee94-205">Это поведение можно удалить, удалив `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-205">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="7ee94-206">В следующем примере кода удаляет `TextOutputFormatter` и `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="7ee94-206">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="7ee94-207">Без `TextOutputFormatter`, `string` возврата типов возврата 406 неприемлемо, например.</span><span class="sxs-lookup"><span data-stu-id="7ee94-207">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="7ee94-208">Обратите внимание, если модуль форматирования XML существует, он будет формата `string` возвращаемые типы, если `TextOutputFormatter` удаляется.</span><span class="sxs-lookup"><span data-stu-id="7ee94-208">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="7ee94-209">Без `HttpNoContentOutputFormatter`, объектов со значением null форматируются с помощью настроенных модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="7ee94-209">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="7ee94-210">Например, модуль форматирования JSON просто возвращает ответ с текстом `null`, а модуль форматирования XML вернет пустым элементом XML с атрибутом `xsi:nil="true"` значение.</span><span class="sxs-lookup"><span data-stu-id="7ee94-210">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="7ee94-211">Сопоставления URL-адрес ответа формат</span><span class="sxs-lookup"><span data-stu-id="7ee94-211">Response Format URL Mappings</span></span>

<span data-ttu-id="7ee94-212">Клиенты могут запрашивать определенного формата как часть URL-адрес, например, в строке запроса или часть пути, либо используя расширение файла определенного формата, например XML или .json.</span><span class="sxs-lookup"><span data-stu-id="7ee94-212">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="7ee94-213">Сопоставление пути запроса должно быть указано в маршрут, который использует API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="7ee94-213">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="7ee94-214">Пример:</span><span class="sxs-lookup"><span data-stu-id="7ee94-214">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="7ee94-215">Запрошенный формат должен задаваться в качестве необязательного файла расширения и использовать этот маршрут.</span><span class="sxs-lookup"><span data-stu-id="7ee94-215">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="7ee94-216">`[FormatFilter]` Атрибут проверяет наличие значения в формате `RouteData` и назначит формат ответа соответствующий модуль форматирования при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="7ee94-216">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="7ee94-217">Маршрут</span><span class="sxs-lookup"><span data-stu-id="7ee94-217">Route</span></span>                      | <span data-ttu-id="7ee94-218">Formatter</span><span class="sxs-lookup"><span data-stu-id="7ee94-218">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="7ee94-219">Модуль форматирования выходных данных по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7ee94-219">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="7ee94-220">Модуль форматирования JSON (если настроено)</span><span class="sxs-lookup"><span data-stu-id="7ee94-220">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="7ee94-221">Модуль форматирования XML (если настроено)</span><span class="sxs-lookup"><span data-stu-id="7ee94-221">The XML formatter (if configured)</span></span>  |
