---
title: Привязка модели в ASP.NET Core
author: tdykstra
description: Узнайте, как работает привязка модели в ASP.NET Core и как настроить ее поведение.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: tdykstra
ms.date: 05/31/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 10a9f8327bf16d11ec1e04ac3888d701f1ab1778
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724526"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="0d854-103">Привязка модели в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d854-103">Model Binding in ASP.NET Core</span></span>

<span data-ttu-id="0d854-104">В этой статье объясняется, что такое привязка модели, как это работает и как настроить ее поведение.</span><span class="sxs-lookup"><span data-stu-id="0d854-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="0d854-105">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0d854-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="0d854-106">Что такое привязка модели</span><span class="sxs-lookup"><span data-stu-id="0d854-106">What is Model binding</span></span>

<span data-ttu-id="0d854-107">Контроллеры и Razor Pages работают с данными, поступающими из HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="0d854-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="0d854-108">Например, данные о маршруте могут предоставлять ключ записи, а опубликованные поля формы могут предоставлять значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="0d854-109">Написание кода для получения этих значений и их преобразования из строк в типы .NET будет утомительной задачей с высоким риском ошибок.</span><span class="sxs-lookup"><span data-stu-id="0d854-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="0d854-110">Привязка модели позволяет автоматизировать этот процесс.</span><span class="sxs-lookup"><span data-stu-id="0d854-110">Model binding automates this process.</span></span> <span data-ttu-id="0d854-111">Система привязки модели:</span><span class="sxs-lookup"><span data-stu-id="0d854-111">The model binding system:</span></span>

* <span data-ttu-id="0d854-112">Извлекает данные из различных источников, таких как данные о маршруте, поля формы и строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0d854-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="0d854-113">Предоставляет данные для контроллеров и страниц Razor в параметрах метода и открытых свойствах.</span><span class="sxs-lookup"><span data-stu-id="0d854-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="0d854-114">Преобразует строковые данные в типы .NET.</span><span class="sxs-lookup"><span data-stu-id="0d854-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="0d854-115">Обновляет свойства сложных типов.</span><span class="sxs-lookup"><span data-stu-id="0d854-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="0d854-116">Пример</span><span class="sxs-lookup"><span data-stu-id="0d854-116">Example</span></span>

<span data-ttu-id="0d854-117">Предположим, у вас есть следующий метод действия:</span><span class="sxs-lookup"><span data-stu-id="0d854-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="0d854-118">И приложение получает запрос с этим URL-адресом:</span><span class="sxs-lookup"><span data-stu-id="0d854-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="0d854-119">Привязка модели выполняет следующие действия, после того, как система маршрутизации выберет метод действия:</span><span class="sxs-lookup"><span data-stu-id="0d854-119">Model binding goes though the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="0d854-120">Находит первый параметр `GetByID`, целое число с именем `id`.</span><span class="sxs-lookup"><span data-stu-id="0d854-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="0d854-121">Просматривает доступные источники в HTTP-запросе и находит `id` = "2" в данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="0d854-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="0d854-122">Преобразует строку "2" в целое число 2.</span><span class="sxs-lookup"><span data-stu-id="0d854-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="0d854-123">Находит следующий параметр `GetByID`, логическое значение с именем `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="0d854-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="0d854-124">Просматривает источники и находит "DogsOnly=true" в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="0d854-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="0d854-125">Сопоставление имен не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="0d854-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="0d854-126">Преобразует строку "true" в логическое значение `true`.</span><span class="sxs-lookup"><span data-stu-id="0d854-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="0d854-127">Затем платформа вызывает метод `GetById`, передавая 2 для параметра `id` и `true` для параметра `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="0d854-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="0d854-128">В приведенном выше примере целевые объекты привязки модели — это параметры методов, которые являются примитивными типами.</span><span class="sxs-lookup"><span data-stu-id="0d854-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="0d854-129">Целевые объекты также могут быть свойствами сложного типа.</span><span class="sxs-lookup"><span data-stu-id="0d854-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="0d854-130">После успешной привязки каждого свойства осуществляется [проверка модели](xref:mvc/models/validation) для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="0d854-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="0d854-131">Записи о данных, привязанных к модели, а также об ошибках привязки или проверки хранятся в [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) или [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="0d854-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="0d854-132">Чтобы узнать об успешном выполнении этого процесса, приложение проверяет наличие флага [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="0d854-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="0d854-133">Целевые объекты</span><span class="sxs-lookup"><span data-stu-id="0d854-133">Targets</span></span>

<span data-ttu-id="0d854-134">Привязка модели попытается найти значения для следующих типов целевых объектов:</span><span class="sxs-lookup"><span data-stu-id="0d854-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="0d854-135">Параметры метода действия контроллера, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="0d854-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="0d854-136">Параметры метода обработчика Razor Pages, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="0d854-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="0d854-137">Открытые свойства контроллера или класса `PageModel`, если задано атрибутами.</span><span class="sxs-lookup"><span data-stu-id="0d854-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="0d854-138">Атрибут [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="0d854-138">[BindProperty] attribute</span></span>

<span data-ttu-id="0d854-139">Может применяться к открытому свойству контроллера или класса `PageModel` для привязки модели для этого свойства:</span><span class="sxs-lookup"><span data-stu-id="0d854-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=7-8)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="0d854-140">Атрибут [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="0d854-140">[BindProperties] attribute</span></span>

<span data-ttu-id="0d854-141">Доступно в ASP.NET Core 2.1 и более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="0d854-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="0d854-142">Может применяться к контроллеру или классу `PageModel`, чтобы привязка модели была направлена на все открытые свойства этого класса:</span><span class="sxs-lookup"><span data-stu-id="0d854-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="0d854-143">Привязка модели для HTTP-запросов GET</span><span class="sxs-lookup"><span data-stu-id="0d854-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="0d854-144">По умолчанию свойства не привязываются к HTTP-запросам GET.</span><span class="sxs-lookup"><span data-stu-id="0d854-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="0d854-145">Как правило, для запроса GET вам нужен только параметр идентификатора записи.</span><span class="sxs-lookup"><span data-stu-id="0d854-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="0d854-146">Идентификатор записи используется для поиска элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0d854-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="0d854-147">Поэтому не нужно привязывать свойство, которое содержит экземпляр модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="0d854-148">Если вы хотите привязать свойства к данным от запросов GET, задайте для свойства `SupportsGet` значение `true`:</span><span class="sxs-lookup"><span data-stu-id="0d854-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="0d854-149">Источники</span><span class="sxs-lookup"><span data-stu-id="0d854-149">Sources</span></span>

<span data-ttu-id="0d854-150">По умолчанию привязка модели получает данные в виде пар "ключ-значение" из следующих источников в HTTP-запросе:</span><span class="sxs-lookup"><span data-stu-id="0d854-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="0d854-151">Поля формы</span><span class="sxs-lookup"><span data-stu-id="0d854-151">Form fields</span></span> 
1. <span data-ttu-id="0d854-152">Текст запроса (для [контроллеров, имеющих атрибут [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="0d854-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="0d854-153">Данные маршрута</span><span class="sxs-lookup"><span data-stu-id="0d854-153">Route data</span></span>
1. <span data-ttu-id="0d854-154">Параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="0d854-154">Query string parameters</span></span>
1. <span data-ttu-id="0d854-155">Отправленные файлы</span><span class="sxs-lookup"><span data-stu-id="0d854-155">Uploaded files</span></span> 

<span data-ttu-id="0d854-156">Для каждого целевого параметра или свойства источники проверяются в порядке, указанном в этом списке.</span><span class="sxs-lookup"><span data-stu-id="0d854-156">For each target parameter or property, the sources are scanned in the order indicated in this list.</span></span> <span data-ttu-id="0d854-157">Существует несколько исключений:</span><span class="sxs-lookup"><span data-stu-id="0d854-157">There are a few exceptions:</span></span>

* <span data-ttu-id="0d854-158">Данные маршрутизации и значения строк запросов используются только для примитивных типов.</span><span class="sxs-lookup"><span data-stu-id="0d854-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="0d854-159">Отправленные файлы привязаны только к типам целевых объектов, которые реализуют `IFormFile` или `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="0d854-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="0d854-160">Если поведение по умолчанию не дает правильные результаты, можно использовать один из следующих атрибутов для указания источника для любого заданного целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="0d854-160">If the default behavior doesn't give the right results, you can use one of the following attributes to specify the source to use for any given target.</span></span> 

* <span data-ttu-id="0d854-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): возвращает значения из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0d854-161">[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="0d854-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): возвращает значения из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="0d854-162">[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="0d854-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): возвращает значения из опубликованных полей формы.</span><span class="sxs-lookup"><span data-stu-id="0d854-163">[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="0d854-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): возвращает значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="0d854-164">[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="0d854-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): возвращает значения из заголовков HTTP.</span><span class="sxs-lookup"><span data-stu-id="0d854-165">[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="0d854-166">Эти атрибуты:</span><span class="sxs-lookup"><span data-stu-id="0d854-166">These attributes:</span></span>

* <span data-ttu-id="0d854-167">Добавляются к свойствам модели по отдельности (не к классу модели), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0d854-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="0d854-168">При необходимости принимают значение имени модели в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="0d854-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="0d854-169">Этот параметр предоставляется в том случае, если имя свойства не соответствует значению в запросе.</span><span class="sxs-lookup"><span data-stu-id="0d854-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="0d854-170">Например, значение в запросе может быть заголовком с дефисом в имени, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0d854-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="0d854-171">Атрибут [FromBody]</span><span class="sxs-lookup"><span data-stu-id="0d854-171">[FromBody] attribute</span></span>

<span data-ttu-id="0d854-172">Данные текста запроса анализируются с помощью форматировщиков входных данных для конкретного типа содержимого запроса.</span><span class="sxs-lookup"><span data-stu-id="0d854-172">The request body data is parsed by using input formatters specific to the content type of the request.</span></span> <span data-ttu-id="0d854-173">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="0d854-173">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="0d854-174">Не применяют `[FromBody]` к нескольким параметрам в методе действия.</span><span class="sxs-lookup"><span data-stu-id="0d854-174">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="0d854-175">Среда выполнения ASP.NET Core делегирует ответственность за считывание потока запроса форматировщику входных данных.</span><span class="sxs-lookup"><span data-stu-id="0d854-175">The ASP.NET Core runtime delegates the responsibility of reading the request stream to the input formatter.</span></span> <span data-ttu-id="0d854-176">После считывания потока запроса он больше не доступен для повторного чтения для привязки других параметров `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="0d854-176">Once the request stream is read, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="0d854-177">Дополнительные источники</span><span class="sxs-lookup"><span data-stu-id="0d854-177">Additional sources</span></span>

<span data-ttu-id="0d854-178">Исходные данные предоставляются системой привязки модели *поставщиками значений*.</span><span class="sxs-lookup"><span data-stu-id="0d854-178">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="0d854-179">Вы можете записать и зарегистрировать пользовательские поставщики значений, которые получают данные для привязки модели из других источников.</span><span class="sxs-lookup"><span data-stu-id="0d854-179">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="0d854-180">Например, вам могут потребоваться данные из файлов cookie или состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="0d854-180">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="0d854-181">Для получения данных из нового источника:</span><span class="sxs-lookup"><span data-stu-id="0d854-181">To get data from a new source:</span></span>

* <span data-ttu-id="0d854-182">Создайте класс, реализующий `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="0d854-182">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="0d854-183">Создайте класс, реализующий `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="0d854-183">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="0d854-184">Зарегистрируйте класс фабрики в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d854-184">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="0d854-185">Пример приложения включает пример [поставщика значений](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) и [фабрики](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs), которая получает значения из файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="0d854-185">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="0d854-186">Ниже приведен код регистрации в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0d854-186">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="0d854-187">Этот код помещает поставщик пользовательских значений после всех встроенных поставщиков значений.</span><span class="sxs-lookup"><span data-stu-id="0d854-187">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="0d854-188">Чтобы сделать его первым в списке, вызовите `Insert(0, new CookieValueProviderFactory())` вместо `Add`.</span><span class="sxs-lookup"><span data-stu-id="0d854-188">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="0d854-189">Отсутствие источника для свойства модели</span><span class="sxs-lookup"><span data-stu-id="0d854-189">No source for a model property</span></span>

<span data-ttu-id="0d854-190">По умолчанию ошибка состояния модели не создается, если не найдено значение для свойства модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-190">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="0d854-191">Свойство получает значение NULL или значение по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="0d854-191">The property is set to null or a default value:</span></span>

* <span data-ttu-id="0d854-192">Примитивным типам, допускающим значение NULL, задается значение `null`.</span><span class="sxs-lookup"><span data-stu-id="0d854-192">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="0d854-193">Типам значений, не допускающим значение NULL, задается значение `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="0d854-193">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="0d854-194">Например, параметр `int id` получает значение 0.</span><span class="sxs-lookup"><span data-stu-id="0d854-194">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="0d854-195">Для сложных типов привязка модели создает экземпляр с помощью конструктора по умолчанию без задания свойств.</span><span class="sxs-lookup"><span data-stu-id="0d854-195">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="0d854-196">Массивы имеют значение `Array.Empty<T>()`, за исключением массивов `byte[]`, которые имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="0d854-196">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="0d854-197">Если состояние модели должно быть признано недействительным, когда в полях формы не найдены данные для свойства модели, используйте [атрибут [BindRequired]](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="0d854-197">If model state should be invalidated when nothing is found in form fields for a model property, use the [[BindRequired] attribute](#bindrequired-attribute).</span></span>

<span data-ttu-id="0d854-198">Обратите внимание, это поведение `[BindRequired]` применяется к привязке модели из опубликованных данных формы, а не к данным JSON или XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="0d854-198">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="0d854-199">Данные основного текста запроса обрабатываются [форматировщиками входных данных](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="0d854-199">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="0d854-200">Ошибки преобразования типа</span><span class="sxs-lookup"><span data-stu-id="0d854-200">Type conversion errors</span></span>

<span data-ttu-id="0d854-201">Если источник найден, но его нельзя преобразовать в тип целевого объекта, состояние модели помечается как недопустимое.</span><span class="sxs-lookup"><span data-stu-id="0d854-201">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="0d854-202">Параметр или свойство целевого объекта получает значение NULL или значение по умолчанию, как отмечалось в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="0d854-202">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="0d854-203">В контроллере API с атрибутом `[ApiController]` недопустимое состояние модели приводит к автоматическому ответу HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="0d854-203">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="0d854-204">На странице Razor повторно отображается страница с сообщением об ошибке:</span><span class="sxs-lookup"><span data-stu-id="0d854-204">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="0d854-205">Проверка на стороне клиента перехватывает большинство неверных данных, которые в противном случае были бы отправлены в форму Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="0d854-205">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="0d854-206">Эта проверка затрудняет срабатывание выделенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="0d854-206">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="0d854-207">Пример приложения включает кнопку **Отправить с неверной датой**, которая помещает недопустимые данные в поле **Дата приема на работу** и отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="0d854-207">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="0d854-208">Эта кнопка показывает, как работает код для повторного отображения страницы, если возникла ошибка преобразования данных.</span><span class="sxs-lookup"><span data-stu-id="0d854-208">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="0d854-209">Когда страница отображается повторно приведенным выше кодом, недопустимые входные данные не отображаются в поле формы.</span><span class="sxs-lookup"><span data-stu-id="0d854-209">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="0d854-210">Это связано с тем, что свойству модели задано значение NULL или значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0d854-210">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="0d854-211">Недопустимые входные данные отображаются в сообщении об ошибке.</span><span class="sxs-lookup"><span data-stu-id="0d854-211">The invalid input does appear in an error message.</span></span> <span data-ttu-id="0d854-212">Но если требуется повторно отобразить неправильные данные в поле формы, возможно, следует сделать свойство модели строкой и выполнить преобразование данных вручную.</span><span class="sxs-lookup"><span data-stu-id="0d854-212">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="0d854-213">Та же стратегия рекомендуется в том случае, если вы не хотите, чтобы ошибки преобразования типов приводили к ошибкам состояния модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-213">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="0d854-214">В этом случае следует сделать свойство модели строкой.</span><span class="sxs-lookup"><span data-stu-id="0d854-214">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="0d854-215">Простые типы</span><span class="sxs-lookup"><span data-stu-id="0d854-215">Simple types</span></span>

<span data-ttu-id="0d854-216">Связыватель модели может преобразовать исходные строки в следующие примитивные типы:</span><span class="sxs-lookup"><span data-stu-id="0d854-216">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="0d854-217">Boolean</span><span class="sxs-lookup"><span data-stu-id="0d854-217">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="0d854-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="0d854-218">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="0d854-219">Char</span><span class="sxs-lookup"><span data-stu-id="0d854-219">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="0d854-220">DateTime</span><span class="sxs-lookup"><span data-stu-id="0d854-220">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="0d854-221">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="0d854-221">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="0d854-222">Decimal</span><span class="sxs-lookup"><span data-stu-id="0d854-222">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="0d854-223">Double</span><span class="sxs-lookup"><span data-stu-id="0d854-223">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="0d854-224">Enum</span><span class="sxs-lookup"><span data-stu-id="0d854-224">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="0d854-225">Guid</span><span class="sxs-lookup"><span data-stu-id="0d854-225">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="0d854-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="0d854-226">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="0d854-227">Single</span><span class="sxs-lookup"><span data-stu-id="0d854-227">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="0d854-228">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="0d854-228">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="0d854-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="0d854-229">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="0d854-230">Uri</span><span class="sxs-lookup"><span data-stu-id="0d854-230">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="0d854-231">Version</span><span class="sxs-lookup"><span data-stu-id="0d854-231">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="0d854-232">Сложные типы</span><span class="sxs-lookup"><span data-stu-id="0d854-232">Complex types</span></span>

<span data-ttu-id="0d854-233">Сложный тип должен иметь открытый конструктор по умолчанию, а также открытые и доступные для записи свойства, подлежащие привязке.</span><span class="sxs-lookup"><span data-stu-id="0d854-233">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="0d854-234">Когда происходит привязка модели, класс создается с помощью открытого конструктора по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0d854-234">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="0d854-235">Для каждого свойства сложного типа привязка модели ищет в источниках шаблон имени *prefix.property_name*.</span><span class="sxs-lookup"><span data-stu-id="0d854-235">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="0d854-236">Если ничего не найдено, он ищет только *property_name* без префикса.</span><span class="sxs-lookup"><span data-stu-id="0d854-236">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="0d854-237">Для привязки к параметру префикс является именем параметра.</span><span class="sxs-lookup"><span data-stu-id="0d854-237">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="0d854-238">Для привязки к открытому свойству `PageModel` префикс является именем открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="0d854-238">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="0d854-239">Некоторые атрибуты имеют свойство `Prefix`, которое позволяет переопределить использование по умолчанию для имени параметра или свойства.</span><span class="sxs-lookup"><span data-stu-id="0d854-239">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="0d854-240">Предположим, например, что сложный тип принадлежит к следующему классу `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="0d854-240">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="0d854-241">Префикс — это имя параметра</span><span class="sxs-lookup"><span data-stu-id="0d854-241">Prefix = parameter name</span></span>

<span data-ttu-id="0d854-242">Если модель, которую нужно привязать, является параметром с именем `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="0d854-242">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="0d854-243">Привязка модели начинается с поиска источников ключа `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="0d854-243">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="0d854-244">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="0d854-244">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="0d854-245">Префикс — это имя свойства</span><span class="sxs-lookup"><span data-stu-id="0d854-245">Prefix = property name</span></span>

<span data-ttu-id="0d854-246">Если модель для привязки является свойством с именем `Instructor` контроллера или класса `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="0d854-246">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="0d854-247">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="0d854-247">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="0d854-248">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="0d854-248">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="0d854-249">Пользовательский префикс</span><span class="sxs-lookup"><span data-stu-id="0d854-249">Custom prefix</span></span>

<span data-ttu-id="0d854-250">Если модель для привязки — это параметр с именем `instructorToUpdate`, а атрибут `Bind` задает `Instructor` как префикс:</span><span class="sxs-lookup"><span data-stu-id="0d854-250">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="0d854-251">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="0d854-251">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="0d854-252">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="0d854-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="0d854-253">Атрибуты для целевых объектов сложного типа</span><span class="sxs-lookup"><span data-stu-id="0d854-253">Attributes for complex type targets</span></span>

<span data-ttu-id="0d854-254">Несколько встроенных атрибутов доступны для управления привязкой моделей сложных типов:</span><span class="sxs-lookup"><span data-stu-id="0d854-254">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="0d854-255">Эти атрибуты влияют на привязку модели, если опубликованные данные формы являются источником значений.</span><span class="sxs-lookup"><span data-stu-id="0d854-255">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="0d854-256">Они не влияют на форматировщики входных данных, которые обрабатывают опубликованные тексты запросов JSON и XML.</span><span class="sxs-lookup"><span data-stu-id="0d854-256">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="0d854-257">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="0d854-257">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="0d854-258">Также см. обсуждение атрибута `[Required]` в разделе [Проверка модели](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="0d854-258">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="0d854-259">Атрибут [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="0d854-259">[BindRequired] attribute</span></span>

<span data-ttu-id="0d854-260">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="0d854-260">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="0d854-261">Приводит к тому, что привязка модели добавляет ошибку состояния модели, если привязка для свойства модели невозможна.</span><span class="sxs-lookup"><span data-stu-id="0d854-261">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="0d854-262">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="0d854-262">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="0d854-263">Атрибут [BindNever]</span><span class="sxs-lookup"><span data-stu-id="0d854-263">[BindNever] attribute</span></span>

<span data-ttu-id="0d854-264">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="0d854-264">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="0d854-265">Запрещает привязке модели задавать свойство модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-265">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="0d854-266">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="0d854-266">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="0d854-267">Атрибут [Bind]</span><span class="sxs-lookup"><span data-stu-id="0d854-267">[Bind] attribute</span></span>

<span data-ttu-id="0d854-268">Может быть применен к классу или параметру метода.</span><span class="sxs-lookup"><span data-stu-id="0d854-268">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="0d854-269">Указывает, какие свойства модели должны быть включены в привязку модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-269">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="0d854-270">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается любой метод действия или обработчик:</span><span class="sxs-lookup"><span data-stu-id="0d854-270">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="0d854-271">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается метод `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="0d854-271">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="0d854-272">Атрибут `[Bind]` может использоваться для защиты от чрезмерной передачи данных при *создании*.</span><span class="sxs-lookup"><span data-stu-id="0d854-272">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="0d854-273">Он не работает в сценариях редактирования, поскольку исключенным свойствам задается значение NULL или значение по умолчанию, но не оставляется значение без изменений.</span><span class="sxs-lookup"><span data-stu-id="0d854-273">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="0d854-274">Для защиты от чрезмерной передачи данных рекомендуется использовать модели представлений вместо атрибута `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="0d854-274">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="0d854-275">Дополнительные сведения см. в разделе [Примечание по безопасности о чрезмерной передаче данных](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="0d854-275">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="0d854-276">Коллекции</span><span class="sxs-lookup"><span data-stu-id="0d854-276">Collections</span></span>

<span data-ttu-id="0d854-277">Для целевых объектов, которые являются коллекциями примитивных типов, привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="0d854-277">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="0d854-278">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="0d854-278">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="0d854-279">Например:</span><span class="sxs-lookup"><span data-stu-id="0d854-279">For example:</span></span>

* <span data-ttu-id="0d854-280">Предположим, что параметром для привязки является массив с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0d854-280">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="0d854-281">Строковые данные формы или запроса могут иметь один из следующих форматов:</span><span class="sxs-lookup"><span data-stu-id="0d854-281">Form or query string data can be in one of the following formats:</span></span>
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* <span data-ttu-id="0d854-282">Следующий формат поддерживается только в данных формы:</span><span class="sxs-lookup"><span data-stu-id="0d854-282">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="0d854-283">Для всех перечисленных ранее форматов привязка модели передает массив из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0d854-283">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="0d854-284">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="0d854-284">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="0d854-285">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="0d854-285">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="0d854-286">Форматы данных, которые используют номера нижних индексов (... [0] ... [1] ...), должны нумероваться последовательно, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="0d854-286">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="0d854-287">Если в нумерации есть пробелы, все элементы после пробела не учитываются.</span><span class="sxs-lookup"><span data-stu-id="0d854-287">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="0d854-288">Например, если указаны индексы 0 и 2, а не 0 и 1, второй элемент игнорируется.</span><span class="sxs-lookup"><span data-stu-id="0d854-288">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="0d854-289">Словари</span><span class="sxs-lookup"><span data-stu-id="0d854-289">Dictionaries</span></span>

<span data-ttu-id="0d854-290">Для целевых объектов `Dictionary` привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="0d854-290">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="0d854-291">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="0d854-291">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="0d854-292">Например:</span><span class="sxs-lookup"><span data-stu-id="0d854-292">For example:</span></span>

* <span data-ttu-id="0d854-293">Предположим, что целевой параметр является `Dictionary<int, string>` с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0d854-293">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="0d854-294">Опубликованные строковые данные формы или запроса могут выглядеть как один из следующих примеров:</span><span class="sxs-lookup"><span data-stu-id="0d854-294">The posted form or query string data can look like one of the following examples:</span></span>

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* <span data-ttu-id="0d854-295">Для всех перечисленных ранее форматов привязка модели передает словарь из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="0d854-295">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="0d854-296">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="0d854-296">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="0d854-297">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="0d854-297">selectedCourses["2000"]="Economics"</span></span>

## <a name="special-data-types"></a><span data-ttu-id="0d854-298">Специальные типы данных</span><span class="sxs-lookup"><span data-stu-id="0d854-298">Special data types</span></span>

<span data-ttu-id="0d854-299">Существуют некоторые особые типы данных, которые может обрабатывать привязка модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-299">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="0d854-300">IFormFile и IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="0d854-300">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="0d854-301">Переданный файл, включенный в HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="0d854-301">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="0d854-302">Также поддерживается `IEnumerable<IFormFile>` для нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="0d854-302">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="0d854-303">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="0d854-303">CancellationToken</span></span>

<span data-ttu-id="0d854-304">используется для отмены действия в асинхронных контроллерах.</span><span class="sxs-lookup"><span data-stu-id="0d854-304">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="0d854-305">FormCollection</span><span class="sxs-lookup"><span data-stu-id="0d854-305">FormCollection</span></span>

<span data-ttu-id="0d854-306">Используется для извлечения всех значений из опубликованных данных формы.</span><span class="sxs-lookup"><span data-stu-id="0d854-306">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="0d854-307">Форматировщики входных данных</span><span class="sxs-lookup"><span data-stu-id="0d854-307">Input formatters</span></span>

<span data-ttu-id="0d854-308">Данные в тексте запроса могут быть в формате JSON, XML или другом формате.</span><span class="sxs-lookup"><span data-stu-id="0d854-308">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="0d854-309">Для анализа этих данных модель привязки использует *форматировщик входных данных*, настроенный для обработки определенного типа содержимого.</span><span class="sxs-lookup"><span data-stu-id="0d854-309">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="0d854-310">По умолчанию ASP.NET Core включает форматировщики входных данных на основе JSON для обработки данных JSON.</span><span class="sxs-lookup"><span data-stu-id="0d854-310">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="0d854-311">Вы можете добавить другие форматировщики для других типов содержимого.</span><span class="sxs-lookup"><span data-stu-id="0d854-311">You can add other formatters for other content types.</span></span>

<span data-ttu-id="0d854-312">ASP.NET Core выбирает форматировщики входных данных на основе атрибута [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="0d854-312">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="0d854-313">Если атрибут отсутствует, используется [Заголовок Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="0d854-313">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="0d854-314">Чтобы использовать встроенные форматировщики входных данных XML:</span><span class="sxs-lookup"><span data-stu-id="0d854-314">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="0d854-315">Установите пакет NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="0d854-315">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="0d854-316">В `Startup.ConfigureServices` вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> или <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="0d854-316">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="0d854-317">Примените атрибут `Consumes` к классам контроллера или методам действий, которые должны ожидать XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="0d854-317">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="0d854-318">Дополнительные сведения см. в разделе [Введение в сериализацию XML](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="0d854-318">For more information, see [Introducing XML Serialization](https://docs.microsoft.com/en-us/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="0d854-319">Исключение указанных типов из привязки модели</span><span class="sxs-lookup"><span data-stu-id="0d854-319">Exclude specified types from model binding</span></span>

<span data-ttu-id="0d854-320">Поведение привязки модели и системы проверки определяется классом [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="0d854-320">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="0d854-321">Вы можете настроить `ModelMetadata`, добавив поставщик сведений в [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="0d854-321">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="0d854-322">Встроенные поставщики сведений доступны для отключения привязки модели или проверки для указанных типов.</span><span class="sxs-lookup"><span data-stu-id="0d854-322">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="0d854-323">Чтобы отключить привязку модели для всех моделей указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d854-323">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0d854-324">Например, для отключения привязки модели для всех моделей типа `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="0d854-324">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="0d854-325">Чтобы отключить проверку свойств указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d854-325">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0d854-326">Например, чтобы отключить проверку по свойствам типа `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="0d854-326">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="0d854-327">Настраиваемые связыватели модели</span><span class="sxs-lookup"><span data-stu-id="0d854-327">Custom model binders</span></span>

<span data-ttu-id="0d854-328">Привязку модели можно расширить путем написания пользовательского связывателя модели и с помощью атрибута `[ModelBinder]`, чтобы выбрать его для заданного целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="0d854-328">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="0d854-329">Узнайте больше о [пользовательской привязке модели](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="0d854-329">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="0d854-330">Привязка модели вручную</span><span class="sxs-lookup"><span data-stu-id="0d854-330">Manual model binding</span></span>

<span data-ttu-id="0d854-331">Привязка модели может вызываться вручную с помощью метода <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0d854-331">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="0d854-332">Этот метод определен в классах `ControllerBase` и `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="0d854-332">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="0d854-333">Перегрузки метода позволяют задать поставщик префиксов и значений.</span><span class="sxs-lookup"><span data-stu-id="0d854-333">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="0d854-334">Этот метод возвращает `false` при сбое привязки модели.</span><span class="sxs-lookup"><span data-stu-id="0d854-334">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="0d854-335">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="0d854-335">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="0d854-336">Атрибут [FromServices]</span><span class="sxs-lookup"><span data-stu-id="0d854-336">[FromServices] attribute</span></span>

<span data-ttu-id="0d854-337">Имя этого атрибута соответствует шаблону атрибутов привязки модели, которые указывают источник данных.</span><span class="sxs-lookup"><span data-stu-id="0d854-337">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="0d854-338">Но это не связано с привязкой данных от поставщика значений.</span><span class="sxs-lookup"><span data-stu-id="0d854-338">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="0d854-339">Он получает экземпляр типа из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0d854-339">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0d854-340">Он предназначен для предоставления альтернативы внедрению через конструктор, когда вам нужна служба, только если вызывается конкретный метод.</span><span class="sxs-lookup"><span data-stu-id="0d854-340">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d854-341">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0d854-341">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>
