---
title: "Данные форматирования ответа в ASP.NET Core MVC"
author: ardalis
description: "Инструкции по форматированию данные ответа в ASP.NET Core MVC."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85398928164e75ec27c91870f03ee1c81725e080
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="8e49e-103">Общие сведения о форматировании данных ответа в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="8e49e-103">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="8e49e-104">По [Стив Смит](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="8e49e-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="8e49e-105">ASP.NET Core MVC имеет встроенную поддержку для форматирования данных ответа, используя форматы фиксированной или в ответ на спецификации клиента.</span><span class="sxs-lookup"><span data-stu-id="8e49e-105">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="8e49e-106">[Просмотреть или загрузить пример из GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="8e49e-106">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="8e49e-107">Результаты действий конкретного формата</span><span class="sxs-lookup"><span data-stu-id="8e49e-107">Format-Specific Action Results</span></span>

<span data-ttu-id="8e49e-108">Некоторые типы результатов действий характерные для определенного формата, таких как `JsonResult` и `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-108">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="8e49e-109">Действия может возвращать определенные результаты, всегда форматируются определенным образом.</span><span class="sxs-lookup"><span data-stu-id="8e49e-109">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="8e49e-110">Например, возвращая `JsonResult` будет возвращать данные в формате JSON, независимо от параметров клиента.</span><span class="sxs-lookup"><span data-stu-id="8e49e-110">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="8e49e-111">Аналогично, возвращая `ContentResult` возвращают строковые данные в формате обычного текста (как будет возвратить строку).</span><span class="sxs-lookup"><span data-stu-id="8e49e-111">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="8e49e-112">Действие не требуется возвращать любой определенный тип; MVC поддерживает любое значение, возвращаемое объектом.</span><span class="sxs-lookup"><span data-stu-id="8e49e-112">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="8e49e-113">Если действие возвращает `IActionResult` реализацию и контроллер наследует от `Controller`, разработчики имеют много вспомогательные методы, соответствующие параметры.</span><span class="sxs-lookup"><span data-stu-id="8e49e-113">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="8e49e-114">Результаты из действий, которые возвращают объекты, которые не являются `IActionResult` типы будут сериализованы с помощью соответствующей `IOutputFormatter` реализации.</span><span class="sxs-lookup"><span data-stu-id="8e49e-114">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="8e49e-115">Для возврата данных в определенном формате из контроллера, который наследует от `Controller` базового класса, используйте встроенные вспомогательный метод `Json` для возврата JSON и `Content` для обычного текста.</span><span class="sxs-lookup"><span data-stu-id="8e49e-115">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="8e49e-116">Метод действия должен возвращать тип определенного результата (например, `JsonResult`) или `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-116">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="8e49e-117">Возврат данных в формате JSON:</span><span class="sxs-lookup"><span data-stu-id="8e49e-117">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="8e49e-118">Образец ответа из этого действия:</span><span class="sxs-lookup"><span data-stu-id="8e49e-118">Sample response from this action:</span></span>

![Вкладка «сеть» средств разработчика в Microsoft Edge, показывающая тип содержимого ответа — application/json](formatting/_static/json-response.png)

<span data-ttu-id="8e49e-120">Обратите внимание, что тип содержимого ответа `application/json`, показанный в списке сетевых запросов и в разделе заголовки ответа.</span><span class="sxs-lookup"><span data-stu-id="8e49e-120">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="8e49e-121">Также Обратите внимание, в списке вариантов, представленных браузера в заголовке Accept в разделе заголовки запроса (в данном случае Microsoft Edge).</span><span class="sxs-lookup"><span data-stu-id="8e49e-121">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="8e49e-122">Текущий метод игнорирует этот заголовок; obeying он описывается ниже.</span><span class="sxs-lookup"><span data-stu-id="8e49e-122">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="8e49e-123">Чтобы вернуть данные в формате обычного текста, используйте `ContentResult` и `Content` поддержки:</span><span class="sxs-lookup"><span data-stu-id="8e49e-123">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="8e49e-124">Ответ от этого действия:</span><span class="sxs-lookup"><span data-stu-id="8e49e-124">A response from this action:</span></span>

![Вкладка «сеть» средств разработчика в Microsoft Edge, показывающая тип содержимого ответа — text/plain](formatting/_static/text-response.png)

<span data-ttu-id="8e49e-126">Обратите внимание, в этом случае `Content-Type` возвращаемое `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-126">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="8e49e-127">Можно также выполнить таким же образом, с помощью только строковый тип ответа:</span><span class="sxs-lookup"><span data-stu-id="8e49e-127">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="8e49e-128">Для нетривиального действия с несколькими возвращаемого типа или параметры (например, на основе результатов операций, выполняемых различные коды состояния HTTP), предпочтительно `IActionResult` как тип возвращаемого значения.</span><span class="sxs-lookup"><span data-stu-id="8e49e-128">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="8e49e-129">Согласование содержимого</span><span class="sxs-lookup"><span data-stu-id="8e49e-129">Content Negotiation</span></span>

<span data-ttu-id="8e49e-130">Согласование содержимого (*conneg* сокращенно) происходит, когда клиент задает [заголовок Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="8e49e-130">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="8e49e-131">JSON используется формат по умолчанию для ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="8e49e-131">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="8e49e-132">Согласование содержимого реализуется `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-132">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="8e49e-133">Он также встроена в код состояния, определенного действия результаты, возвращенные из вспомогательных методов (все основанные на `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="8e49e-133">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="8e49e-134">Также можно возвратить тип модели (класс был определен в качестве типа передачи данных) и платформа будет автоматически включать его в `ObjectResult` для вас.</span><span class="sxs-lookup"><span data-stu-id="8e49e-134">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="8e49e-135">Следующий метод действия использует `Ok` и `NotFound` вспомогательные методы:</span><span class="sxs-lookup"><span data-stu-id="8e49e-135">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="8e49e-136">Если не был запрошен в другом формате и сервер может вернуть запрошенный формат ответа формата JSON будут возвращены.</span><span class="sxs-lookup"><span data-stu-id="8e49e-136">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="8e49e-137">Можно использовать средства, подобного [Fiddler](http://www.telerik.com/fiddler) создать запрос, содержащий заголовок Accept и указать другой формат.</span><span class="sxs-lookup"><span data-stu-id="8e49e-137">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="8e49e-138">В этом случае, если на сервере *форматирования* дающую ответа в запрошенном пользователем формате, будет возвращен результат в формате предпочитаемый клиента.</span><span class="sxs-lookup"><span data-stu-id="8e49e-138">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Fiddler консоль, показывающая, вручную создать запрос со значением заголовка Accept application/XML GET](formatting/_static/fiddler-composer.png)

<span data-ttu-id="8e49e-140">В приведенном выше снимке экрана Fiddler редактор используется для создания запроса, указав `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-140">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="8e49e-141">По умолчанию ASP.NET Core MVC поддерживает только JSON, так что даже если указан другой формат, возвращаемый результат будет по-прежнему формата JSON.</span><span class="sxs-lookup"><span data-stu-id="8e49e-141">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="8e49e-142">Вы увидите, как добавить дополнительные модули форматирования в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="8e49e-142">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="8e49e-143">Действия контроллера может возвращать POCO (обычные старые объекты CLR), в этом случае ASP.NET MVC автоматически создаст `ObjectResult` , создает оболочку этого объекта.</span><span class="sxs-lookup"><span data-stu-id="8e49e-143">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="8e49e-144">Клиент получит форматированные сериализованного объекта (по умолчанию используется формат JSON, XML или в других форматах можно настроить).</span><span class="sxs-lookup"><span data-stu-id="8e49e-144">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="8e49e-145">Если объекта возвращается `null`, платформа возвратит `204 No Content` ответа.</span><span class="sxs-lookup"><span data-stu-id="8e49e-145">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="8e49e-146">Возвращает тип объекта:</span><span class="sxs-lookup"><span data-stu-id="8e49e-146">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="8e49e-147">В этом образце запроса для псевдонима допустимым автор получит ответ 200 ОК с данными автора.</span><span class="sxs-lookup"><span data-stu-id="8e49e-147">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="8e49e-148">Недопустимый псевдоним запрос получит 204 Нет содержимого ответа.</span><span class="sxs-lookup"><span data-stu-id="8e49e-148">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="8e49e-149">Ниже приведены снимки экрана, показывающий ответа в форматах XML и JSON.</span><span class="sxs-lookup"><span data-stu-id="8e49e-149">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="8e49e-150">Процесс согласования содержимого</span><span class="sxs-lookup"><span data-stu-id="8e49e-150">Content Negotiation Process</span></span>

<span data-ttu-id="8e49e-151">Содержимого *согласование* поместите происходит только в случае, если `Accept` заголовок отображается в запросе.</span><span class="sxs-lookup"><span data-stu-id="8e49e-151">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="8e49e-152">Если запрос содержит заголовок accept, платформа перечисляет типы носителей в заголовке accept в порядке предпочтения и попытается найти средство форматирования, который может производить ответа в одном из форматов, указанных в заголовке accept.</span><span class="sxs-lookup"><span data-stu-id="8e49e-152">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="8e49e-153">В случае, если модуль форматирования не найден, можно удовлетворить запрос клиента, платформа будет пытаться найти первый модуль форматирования, который может производить ответ (если разработчик настроил параметр `MvcOptions` для возврата 406 неприемлемо вместо).</span><span class="sxs-lookup"><span data-stu-id="8e49e-153">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="8e49e-154">Если в запросе указан XML, но не был настроен модуль форматирования XML, будет использовать модуль форматирования JSON.</span><span class="sxs-lookup"><span data-stu-id="8e49e-154">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="8e49e-155">В общем случае если модуль форматирования не настроен, можно указать требуемый формат, то используется первый модуль форматирования, который можно форматировать объект.</span><span class="sxs-lookup"><span data-stu-id="8e49e-155">More generally, if no formatter is configured that can provide the requested format, then the first formatter that can format the object is used.</span></span> <span data-ttu-id="8e49e-156">Если заголовок не указан, первый модуль форматирования, который может обрабатывать объекта, возвращаемого будет использоваться для сериализации ответа.</span><span class="sxs-lookup"><span data-stu-id="8e49e-156">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="8e49e-157">В этом случае не все согласования происходящей - сервер определяет, какой формат, который будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="8e49e-157">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="8e49e-158">Если заголовок Accept содержит `*/*`, заголовок будет игнорироваться, если `RespectBrowserAcceptHeader` имеет значение true для `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-158">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="8e49e-159">Браузеры и согласования содержимого</span><span class="sxs-lookup"><span data-stu-id="8e49e-159">Browsers and Content Negotiation</span></span>

<span data-ttu-id="8e49e-160">В отличие от типичные клиенты API веб-браузеры, как правило, для предоставления `Accept` заголовки, которые включают широкий набор форматов, включая подстановочные знаки.</span><span class="sxs-lookup"><span data-stu-id="8e49e-160">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="8e49e-161">По умолчанию при обнаружении платформу, запрос поступает из браузера, он игнорирует `Accept` заголовок и вместо него возвращаемое содержимое в приложении настройки по умолчанию формат (JSON пока иначе настроены).</span><span class="sxs-lookup"><span data-stu-id="8e49e-161">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="8e49e-162">Это обеспечивает более согласованной работы при использовании различных браузеров для использовать интерфейсы API.</span><span class="sxs-lookup"><span data-stu-id="8e49e-162">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="8e49e-163">Если вы предпочитаете заголовки accept браузере честь приложения, это можно настроить как часть конфигурации MVC, задав `RespectBrowserAcceptHeader` для `true` в `ConfigureServices` метод в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8e49e-163">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a><span data-ttu-id="8e49e-164">Настройка модулей форматирования</span><span class="sxs-lookup"><span data-stu-id="8e49e-164">Configuring Formatters</span></span>

<span data-ttu-id="8e49e-165">Если приложению для поддержки дополнительных форматов, помимо стандартных JSON, можно добавить пакеты NuGet и настроить MVC для их поддержки.</span><span class="sxs-lookup"><span data-stu-id="8e49e-165">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="8e49e-166">Существуют отдельные модули форматирования для ввода и вывода.</span><span class="sxs-lookup"><span data-stu-id="8e49e-166">There are separate formatters for input and output.</span></span> <span data-ttu-id="8e49e-167">Входной модули форматирования, используемые [привязки модели](model-binding.md); модули форматирования выходных данных, используемые для форматирования ответов.</span><span class="sxs-lookup"><span data-stu-id="8e49e-167">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="8e49e-168">Можно также настроить [пользовательские модули форматирования](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="8e49e-168">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="8e49e-169">Добавление поддержки формата XML</span><span class="sxs-lookup"><span data-stu-id="8e49e-169">Adding XML Format Support</span></span>

<span data-ttu-id="8e49e-170">Чтобы добавить поддержку форматирования XML-кода, установите `Microsoft.AspNetCore.Mvc.Formatters.Xml` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="8e49e-170">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="8e49e-171">Добавить XmlSerializerFormatters конфигурацию MVC в *файла Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="8e49e-171">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="8e49e-172">Кроме того можно добавить только форматирования выходных данных:</span><span class="sxs-lookup"><span data-stu-id="8e49e-172">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="8e49e-173">Эти два подхода выполняет сериализацию результатов с помощью `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-173">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="8e49e-174">При желании можно использовать `System.Runtime.Serialization.DataContractSerializer` путем добавления его связанный форматирования:</span><span class="sxs-lookup"><span data-stu-id="8e49e-174">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="8e49e-175">После добавления поддержки форматирования XML на контроллер методы должны возвращать формат в запросе `Accept` заголовок как Fiddler, этот пример:</span><span class="sxs-lookup"><span data-stu-id="8e49e-175">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Fiddler консоли: необработанные вкладке для запроса отображается значение заголовка Accept приложения/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="8e49e-178">Можно увидеть на вкладке инспекторах, был сделан запрос на получение необработанных `Accept: application/xml` задан заголовок.</span><span class="sxs-lookup"><span data-stu-id="8e49e-178">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="8e49e-179">В области отображается ответ `Content-Type: application/xml` заголовок и `Author` сериализованный объект в формат XML.</span><span class="sxs-lookup"><span data-stu-id="8e49e-179">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="8e49e-180">Вкладка редактор для изменения запроса для указания `application/json` в `Accept` заголовок.</span><span class="sxs-lookup"><span data-stu-id="8e49e-180">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="8e49e-181">Выполнение запроса и ответа будут отформатированы в качестве JSON:</span><span class="sxs-lookup"><span data-stu-id="8e49e-181">Execute the request, and the response will be formatted as JSON:</span></span>

![Fiddler консоли: необработанные вкладке для запроса отображается значение заголовка Accept — application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="8e49e-184">На этом снимке экрана можно увидеть в запросе задан заголовок `Accept: application/json` и ответ указывает таким же, как его `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-184">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="8e49e-185">`Author` Объекта отображается в тексте ответа в формате JSON.</span><span class="sxs-lookup"><span data-stu-id="8e49e-185">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="8e49e-186">Принудительное определенного формата</span><span class="sxs-lookup"><span data-stu-id="8e49e-186">Forcing a Particular Format</span></span>

<span data-ttu-id="8e49e-187">Если вы хотите ограничить форматы ответа для конкретного действия можно выполнить, можно применить `[Produces]` фильтра.</span><span class="sxs-lookup"><span data-stu-id="8e49e-187">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="8e49e-188">`[Produces]` Фильтр указывает форматы ответа для определенных действий (или контроллера).</span><span class="sxs-lookup"><span data-stu-id="8e49e-188">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="8e49e-189">Как и большинство [фильтры](../controllers/filters.md), могут применяться в действия, контроллера или глобальной области.</span><span class="sxs-lookup"><span data-stu-id="8e49e-189">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="8e49e-190">`[Produces]` Фильтр будет принудительно всем действиям в `AuthorsController` для возврата ответов формата JSON, даже если были настроены другие модули форматирования для приложения и предоставляются клиентом `Accept` заголовок запроса другой, доступной формат.</span><span class="sxs-lookup"><span data-stu-id="8e49e-190">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="8e49e-191">В разделе [фильтры](../controllers/filters.md) для получения дополнительных сведений, включая способы применения фильтров глобально.</span><span class="sxs-lookup"><span data-stu-id="8e49e-191">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="8e49e-192">Специальные вариантов форматирования данных</span><span class="sxs-lookup"><span data-stu-id="8e49e-192">Special Case Formatters</span></span>

<span data-ttu-id="8e49e-193">Некоторые особые случаи реализуются с помощью встроенных модулей форматирования.</span><span class="sxs-lookup"><span data-stu-id="8e49e-193">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="8e49e-194">По умолчанию `string` возвращаемые типы будут отформатированы в качестве *text/plain* (*текст или HTML-* по запросу через `Accept` заголовок).</span><span class="sxs-lookup"><span data-stu-id="8e49e-194">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="8e49e-195">Это поведение можно удалить, удалив `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-195">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="8e49e-196">Удаление модулей форматирования в `Configure` метод в *файла Startup.cs* (см. ниже).</span><span class="sxs-lookup"><span data-stu-id="8e49e-196">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="8e49e-197">Действия, которые имеют объекта модели возвращают тип вернет 204 Нет содержимого ответа при возврате `null`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-197">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="8e49e-198">Это поведение можно удалить, удалив `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-198">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="8e49e-199">В следующем примере кода удаляет `TextOutputFormatter` и `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="8e49e-199">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="8e49e-200">Без `TextOutputFormatter`, `string` возврата типов возврата 406 неприемлемо, например.</span><span class="sxs-lookup"><span data-stu-id="8e49e-200">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="8e49e-201">Обратите внимание, если модуль форматирования XML существует, он будет формата `string` возвращаемые типы, если `TextOutputFormatter` удаляется.</span><span class="sxs-lookup"><span data-stu-id="8e49e-201">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="8e49e-202">Без `HttpNoContentOutputFormatter`, объектов со значением null форматируются с помощью настроенных модуля форматирования.</span><span class="sxs-lookup"><span data-stu-id="8e49e-202">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="8e49e-203">Например, модуль форматирования JSON просто возвращает ответ с текстом `null`, а модуль форматирования XML вернет пустым элементом XML с атрибутом `xsi:nil="true"` значение.</span><span class="sxs-lookup"><span data-stu-id="8e49e-203">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="8e49e-204">Сопоставления URL-адрес ответа формат</span><span class="sxs-lookup"><span data-stu-id="8e49e-204">Response Format URL Mappings</span></span>

<span data-ttu-id="8e49e-205">Клиенты могут запрашивать определенного формата как часть URL-адрес, например, в строке запроса или часть пути, либо используя расширение файла определенного формата, например XML или .json.</span><span class="sxs-lookup"><span data-stu-id="8e49e-205">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="8e49e-206">Сопоставление пути запроса должно быть указано в маршрут, который использует API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="8e49e-206">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="8e49e-207">Пример:</span><span class="sxs-lookup"><span data-stu-id="8e49e-207">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="8e49e-208">Запрошенный формат должен задаваться в качестве необязательного файла расширения и использовать этот маршрут.</span><span class="sxs-lookup"><span data-stu-id="8e49e-208">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="8e49e-209">`[FormatFilter]` Атрибут проверяет наличие значения в формате `RouteData` и назначит формат ответа соответствующий модуль форматирования при создании ответа.</span><span class="sxs-lookup"><span data-stu-id="8e49e-209">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="8e49e-210">Маршрут</span><span class="sxs-lookup"><span data-stu-id="8e49e-210">Route</span></span>                      | <span data-ttu-id="8e49e-211">Formatter</span><span class="sxs-lookup"><span data-stu-id="8e49e-211">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="8e49e-212">Модуль форматирования выходных данных по умолчанию</span><span class="sxs-lookup"><span data-stu-id="8e49e-212">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="8e49e-213">Модуль форматирования JSON (если настроено)</span><span class="sxs-lookup"><span data-stu-id="8e49e-213">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="8e49e-214">Модуль форматирования XML (если настроено)</span><span class="sxs-lookup"><span data-stu-id="8e49e-214">The XML formatter (if configured)</span></span>  |
