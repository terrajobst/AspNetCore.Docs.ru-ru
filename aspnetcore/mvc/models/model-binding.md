---
title: Привязка модели в ASP.NET Core
author: rick-anderson
description: Узнайте, как работает привязка модели в ASP.NET Core и как настроить ее поведение.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 12/18/2019
uid: mvc/models/model-binding
ms.openlocfilehash: 19580768679f30131683717792252c03aade68f9
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654472"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="8b2d9-103">Привязка модели в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b2d9-103">Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8b2d9-104">В этой статье объясняется, что такое привязка модели, как это работает и как настроить ее поведение.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="8b2d9-105">[Просмотреть или скачать пример кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="8b2d9-106">Что такое привязка модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-106">What is Model binding</span></span>

<span data-ttu-id="8b2d9-107">Контроллеры и Razor Pages работают с данными, поступающими из HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="8b2d9-108">Например, данные о маршруте могут предоставлять ключ записи, а опубликованные поля формы могут предоставлять значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="8b2d9-109">Написание кода для получения этих значений и их преобразования из строк в типы .NET будет утомительной задачей с высоким риском ошибок.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="8b2d9-110">Привязка модели позволяет автоматизировать этот процесс.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-110">Model binding automates this process.</span></span> <span data-ttu-id="8b2d9-111">Система привязки модели:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-111">The model binding system:</span></span>

* <span data-ttu-id="8b2d9-112">Извлекает данные из различных источников, таких как данные о маршруте, поля формы и строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="8b2d9-113">Предоставляет данные для контроллеров и страниц Razor в параметрах метода и открытых свойствах.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="8b2d9-114">Преобразует строковые данные в типы .NET.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="8b2d9-115">Обновляет свойства сложных типов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="8b2d9-116">Пример</span><span class="sxs-lookup"><span data-stu-id="8b2d9-116">Example</span></span>

<span data-ttu-id="8b2d9-117">Предположим, у вас есть следующий метод действия:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="8b2d9-118">И приложение получает запрос с этим URL-адресом:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="8b2d9-119">Привязка модели выполняет следующие действия, после того, как система маршрутизации выберет метод действия:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="8b2d9-120">Находит первый параметр `GetByID`, целое число с именем `id`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="8b2d9-121">Просматривает доступные источники в HTTP-запросе и находит `id` = "2" в данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="8b2d9-122">Преобразует строку "2" в целое число 2.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="8b2d9-123">Находит следующий параметр `GetByID`, логическое значение с именем `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="8b2d9-124">Просматривает источники и находит "DogsOnly=true" в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="8b2d9-125">Сопоставление имен не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="8b2d9-126">Преобразует строку "true" в логическое значение `true`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="8b2d9-127">Затем платформа вызывает метод `GetById`, передавая 2 для параметра `id` и `true` для параметра `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="8b2d9-128">В приведенном выше примере целевые объекты привязки модели — это параметры методов, которые являются примитивными типами.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="8b2d9-129">Целевые объекты также могут быть свойствами сложного типа.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="8b2d9-130">После успешной привязки каждого свойства осуществляется [проверка модели](xref:mvc/models/validation) для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="8b2d9-131">Записи о данных, привязанных к модели, а также об ошибках привязки или проверки хранятся в [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) или [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="8b2d9-132">Чтобы узнать об успешном выполнении этого процесса, приложение проверяет наличие флага [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="8b2d9-133">Цели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-133">Targets</span></span>

<span data-ttu-id="8b2d9-134">Привязка модели попытается найти значения для следующих типов целевых объектов:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="8b2d9-135">Параметры метода действия контроллера, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="8b2d9-136">Параметры метода обработчика Razor Pages, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="8b2d9-137">Открытые свойства контроллера или класса `PageModel`, если задано атрибутами.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="8b2d9-138">Атрибут [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-138">[BindProperty] attribute</span></span>

<span data-ttu-id="8b2d9-139">Может применяться к открытому свойству контроллера или класса `PageModel` для привязки модели для этого свойства:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="8b2d9-140">Атрибут [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-140">[BindProperties] attribute</span></span>

<span data-ttu-id="8b2d9-141">Доступно в ASP.NET Core 2.1 и более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="8b2d9-142">Может применяться к контроллеру или классу `PageModel`, чтобы привязка модели была направлена на все открытые свойства этого класса:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="8b2d9-143">Привязка модели для HTTP-запросов GET</span><span class="sxs-lookup"><span data-stu-id="8b2d9-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="8b2d9-144">По умолчанию свойства не привязываются к HTTP-запросам GET.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="8b2d9-145">Как правило, для запроса GET вам нужен только параметр идентификатора записи.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="8b2d9-146">Идентификатор записи используется для поиска элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="8b2d9-147">Поэтому не нужно привязывать свойство, которое содержит экземпляр модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="8b2d9-148">Если вы хотите привязать свойства к данным от запросов GET, задайте для свойства `SupportsGet` значение `true`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="8b2d9-149">Источники</span><span class="sxs-lookup"><span data-stu-id="8b2d9-149">Sources</span></span>

<span data-ttu-id="8b2d9-150">По умолчанию привязка модели получает данные в виде пар "ключ-значение" из следующих источников в HTTP-запросе:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="8b2d9-151">Поля формы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-151">Form fields</span></span>
1. <span data-ttu-id="8b2d9-152">Текст запроса (для [контроллеров, имеющих атрибут [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="8b2d9-153">Отправка данных</span><span class="sxs-lookup"><span data-stu-id="8b2d9-153">Route data</span></span>
1. <span data-ttu-id="8b2d9-154">Параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="8b2d9-154">Query string parameters</span></span>
1. <span data-ttu-id="8b2d9-155">Отправленные файлы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-155">Uploaded files</span></span>

<span data-ttu-id="8b2d9-156">Для каждого целевого параметра или свойства источники проверяются в порядке, указанном в предыдущем списке.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="8b2d9-157">Существует несколько исключений:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-157">There are a few exceptions:</span></span>

* <span data-ttu-id="8b2d9-158">Данные маршрутизации и значения строк запросов используются только для примитивных типов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="8b2d9-159">Отправленные файлы привязаны только к типам целевых объектов, которые реализуют `IFormFile` или `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="8b2d9-160">Если источник по умолчанию неверен, используйте один из следующих атрибутов, чтобы указать источник:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="8b2d9-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): возвращает значения из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="8b2d9-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): возвращает значения из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="8b2d9-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): возвращает значения из опубликованных полей формы.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="8b2d9-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): возвращает значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="8b2d9-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): возвращает значения из заголовков HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="8b2d9-166">Эти атрибуты:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-166">These attributes:</span></span>

* <span data-ttu-id="8b2d9-167">Добавляются к свойствам модели по отдельности (не к классу модели), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="8b2d9-168">При необходимости принимают значение имени модели в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="8b2d9-169">Этот параметр предоставляется в том случае, если имя свойства не соответствует значению в запросе.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="8b2d9-170">Например, значение в запросе может быть заголовком с дефисом в имени, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="8b2d9-171">Атрибут [FromBody]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-171">[FromBody] attribute</span></span>

<span data-ttu-id="8b2d9-172">Примените атрибут `[FromBody]` к параметру, чтобы заполнить его свойства из тела HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="8b2d9-173">Среда выполнения ASP.NET Core делегирует ответственность за считывание тела форматировщику входных данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="8b2d9-174">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="8b2d9-175">При применении `[FromBody]` к параметру сложного типа все атрибуты источника привязки, применяемые к его свойствам, игнорируются.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="8b2d9-176">Например, следующее действие `Create` указывает, что параметр `pet` заполняется из тела:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="8b2d9-177">Класс `Pet` указывает, что свойство `Breed` заполняется из параметра строки запроса:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="8b2d9-178">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-178">In the preceding example:</span></span>

* <span data-ttu-id="8b2d9-179">Атрибут `[FromQuery]` не учитывается.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="8b2d9-180">Свойство `Breed` не заполняется из параметра строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="8b2d9-181">Форматировщики входных данных считывают только тело и не распознают атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="8b2d9-182">Если подходящее значение найдено в теле, оно используется для заполнения свойства `Breed`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="8b2d9-183">Не применяют `[FromBody]` к нескольким параметрам в методе действия.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="8b2d9-184">После считывания потока запроса форматировщиком входных данных он больше не доступен для повторного чтения для привязки других параметров `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="8b2d9-185">Дополнительные источники</span><span class="sxs-lookup"><span data-stu-id="8b2d9-185">Additional sources</span></span>

<span data-ttu-id="8b2d9-186">Исходные данные предоставляются системой привязки модели *поставщиками значений*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="8b2d9-187">Вы можете записать и зарегистрировать пользовательские поставщики значений, которые получают данные для привязки модели из других источников.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="8b2d9-188">Например, вам могут потребоваться данные из файлов cookie или состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="8b2d9-189">Для получения данных из нового источника:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-189">To get data from a new source:</span></span>

* <span data-ttu-id="8b2d9-190">Создайте класс, реализующий перехватчик `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="8b2d9-191">Создайте класс, реализующий перехватчик `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="8b2d9-192">Зарегистрируйте класс фабрики в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="8b2d9-193">Пример приложения включает пример [поставщика значений](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs) и [фабрики](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs), которая получает значения из файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-193">The sample app includes a [value provider](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="8b2d9-194">Ниже приведен код регистрации в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4)]

<span data-ttu-id="8b2d9-195">Этот код помещает поставщик пользовательских значений после всех встроенных поставщиков значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="8b2d9-196">Чтобы сделать его первым в списке, вызовите `Insert(0, new CookieValueProviderFactory())` вместо `Add`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="8b2d9-197">Отсутствие источника для свойства модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-197">No source for a model property</span></span>

<span data-ttu-id="8b2d9-198">По умолчанию ошибка состояния модели не создается, если не найдено значение для свойства модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="8b2d9-199">Свойство получает значение NULL или значение по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="8b2d9-200">Примитивным типам, допускающим значение NULL, задается значение `null`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="8b2d9-201">Типам значений, не допускающим значение NULL, задается значение `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="8b2d9-202">Например, параметр `int id` получает значение 0.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="8b2d9-203">Для сложных типов привязка модели создает экземпляр с помощью конструктора по умолчанию без задания свойств.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="8b2d9-204">Массивы имеют значение `Array.Empty<T>()`, за исключением массивов `byte[]`, которые имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="8b2d9-205">Если состояние модели должно быть признано недействительным, когда в полях формы не найдены данные для свойства модели, используйте атрибут [`[BindRequired]`](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="8b2d9-206">Обратите внимание, это поведение `[BindRequired]` применяется к привязке модели из опубликованных данных формы, а не к данным JSON или XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="8b2d9-207">Данные основного текста запроса обрабатываются [форматировщиками входных данных](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="8b2d9-208">Ошибки преобразования типа</span><span class="sxs-lookup"><span data-stu-id="8b2d9-208">Type conversion errors</span></span>

<span data-ttu-id="8b2d9-209">Если источник найден, но его нельзя преобразовать в тип целевого объекта, состояние модели помечается как недопустимое.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="8b2d9-210">Параметр или свойство целевого объекта получает значение NULL или значение по умолчанию, как отмечалось в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="8b2d9-211">В контроллере API с атрибутом `[ApiController]` недопустимое состояние модели приводит к автоматическому ответу HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="8b2d9-212">На странице Razor повторно отображается страница с сообщением об ошибке:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="8b2d9-213">Проверка на стороне клиента перехватывает большинство неверных данных, которые в противном случае были бы отправлены в форму Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="8b2d9-214">Эта проверка затрудняет срабатывание выделенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="8b2d9-215">Пример приложения включает кнопку **Отправить с неверной датой**, которая помещает недопустимые данные в поле **Дата приема на работу** и отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="8b2d9-216">Эта кнопка показывает, как работает код для повторного отображения страницы, если возникла ошибка преобразования данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="8b2d9-217">Когда страница отображается повторно приведенным выше кодом, недопустимые входные данные не отображаются в поле формы.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="8b2d9-218">Это связано с тем, что свойству модели задано значение NULL или значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="8b2d9-219">Недопустимые входные данные отображаются в сообщении об ошибке.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="8b2d9-220">Но если требуется повторно отобразить неправильные данные в поле формы, возможно, следует сделать свойство модели строкой и выполнить преобразование данных вручную.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="8b2d9-221">Та же стратегия рекомендуется в том случае, если вы не хотите, чтобы ошибки преобразования типов приводили к ошибкам состояния модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="8b2d9-222">В этом случае следует сделать свойство модели строкой.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="8b2d9-223">Простые типы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-223">Simple types</span></span>

<span data-ttu-id="8b2d9-224">Связыватель модели может преобразовать исходные строки в следующие примитивные типы:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="8b2d9-225">Boolean</span><span class="sxs-lookup"><span data-stu-id="8b2d9-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="8b2d9-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="8b2d9-227">Char</span><span class="sxs-lookup"><span data-stu-id="8b2d9-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="8b2d9-228">DateTime</span><span class="sxs-lookup"><span data-stu-id="8b2d9-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="8b2d9-229">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="8b2d9-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="8b2d9-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="8b2d9-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="8b2d9-231">Double</span><span class="sxs-lookup"><span data-stu-id="8b2d9-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="8b2d9-232">Enum</span><span class="sxs-lookup"><span data-stu-id="8b2d9-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="8b2d9-233">Guid</span><span class="sxs-lookup"><span data-stu-id="8b2d9-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="8b2d9-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="8b2d9-235">Один</span><span class="sxs-lookup"><span data-stu-id="8b2d9-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="8b2d9-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8b2d9-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="8b2d9-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="8b2d9-238">URI</span><span class="sxs-lookup"><span data-stu-id="8b2d9-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="8b2d9-239">Версия</span><span class="sxs-lookup"><span data-stu-id="8b2d9-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="8b2d9-240">Сложные типы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-240">Complex types</span></span>

<span data-ttu-id="8b2d9-241">Сложный тип должен иметь открытый конструктор по умолчанию, а также открытые и доступные для записи свойства, подлежащие привязке.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="8b2d9-242">Когда происходит привязка модели, класс создается с помощью открытого конструктора по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="8b2d9-243">Для каждого свойства сложного типа привязка модели ищет в источниках шаблон имени *prefix.property_name*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="8b2d9-244">Если ничего не найдено, он ищет только *property_name* без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="8b2d9-245">Для привязки к параметру префикс является именем параметра.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="8b2d9-246">Для привязки к открытому свойству `PageModel` префикс является именем открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="8b2d9-247">Некоторые атрибуты имеют свойство `Prefix`, которое позволяет переопределить использование по умолчанию для имени параметра или свойства.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="8b2d9-248">Предположим, например, что сложный тип принадлежит к следующему классу `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="8b2d9-249">Префикс — это имя параметра</span><span class="sxs-lookup"><span data-stu-id="8b2d9-249">Prefix = parameter name</span></span>

<span data-ttu-id="8b2d9-250">Если модель, которую нужно привязать, является параметром с именем `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="8b2d9-251">Привязка модели начинается с поиска источников ключа `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="8b2d9-252">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="8b2d9-253">Префикс — это имя свойства</span><span class="sxs-lookup"><span data-stu-id="8b2d9-253">Prefix = property name</span></span>

<span data-ttu-id="8b2d9-254">Если модель для привязки является свойством с именем `Instructor` контроллера или класса `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="8b2d9-255">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="8b2d9-256">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="8b2d9-257">Пользовательский префикс</span><span class="sxs-lookup"><span data-stu-id="8b2d9-257">Custom prefix</span></span>

<span data-ttu-id="8b2d9-258">Если модель для привязки — это параметр с именем `instructorToUpdate`, а атрибут `Bind` задает `Instructor` как префикс:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="8b2d9-259">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="8b2d9-260">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="8b2d9-261">Атрибуты для целевых объектов сложного типа</span><span class="sxs-lookup"><span data-stu-id="8b2d9-261">Attributes for complex type targets</span></span>

<span data-ttu-id="8b2d9-262">Несколько встроенных атрибутов доступны для управления привязкой моделей сложных типов:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="8b2d9-263">Эти атрибуты влияют на привязку модели, если опубликованные данные формы являются источником значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="8b2d9-264">Они не влияют на форматировщики входных данных, которые обрабатывают опубликованные тексты запросов JSON и XML.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="8b2d9-265">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="8b2d9-266">Также см. обсуждение атрибута `[Required]` в разделе [Проверка модели](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="8b2d9-267">Атрибут [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-267">[BindRequired] attribute</span></span>

<span data-ttu-id="8b2d9-268">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="8b2d9-269">Приводит к тому, что привязка модели добавляет ошибку состояния модели, если привязка для свойства модели невозможна.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="8b2d9-270">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="8b2d9-271">Атрибут [BindNever]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-271">[BindNever] attribute</span></span>

<span data-ttu-id="8b2d9-272">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="8b2d9-273">Запрещает привязке модели задавать свойство модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="8b2d9-274">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="8b2d9-275">Атрибут [Bind]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-275">[Bind] attribute</span></span>

<span data-ttu-id="8b2d9-276">Может быть применен к классу или параметру метода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="8b2d9-277">Указывает, какие свойства модели должны быть включены в привязку модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="8b2d9-278">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается любой метод действия или обработчик:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="8b2d9-279">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается метод `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="8b2d9-280">Атрибут `[Bind]` может использоваться для защиты от чрезмерной передачи данных при *создании*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="8b2d9-281">Он не работает в сценариях редактирования, поскольку исключенным свойствам задается значение NULL или значение по умолчанию, но не оставляется значение без изменений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="8b2d9-282">Для защиты от чрезмерной передачи данных рекомендуется использовать модели представлений вместо атрибута `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="8b2d9-283">Дополнительные сведения см. в разделе [Примечание по безопасности о чрезмерной передаче данных](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="8b2d9-284">Коллекции</span><span class="sxs-lookup"><span data-stu-id="8b2d9-284">Collections</span></span>

<span data-ttu-id="8b2d9-285">Для целевых объектов, которые являются коллекциями примитивных типов, привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="8b2d9-286">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="8b2d9-287">Пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-287">For example:</span></span>

* <span data-ttu-id="8b2d9-288">Предположим, что параметром для привязки является массив с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="8b2d9-289">Строковые данные формы или запроса могут иметь один из следующих форматов:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-289">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="8b2d9-290">Следующий формат поддерживается только в данных формы:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="8b2d9-291">Для всех перечисленных ранее форматов привязка модели передает массив из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="8b2d9-292">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="8b2d9-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="8b2d9-293">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="8b2d9-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="8b2d9-294">Форматы данных, которые используют номера нижних индексов (... [0] ... [1] ...), должны нумероваться последовательно, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="8b2d9-295">Если в нумерации есть пробелы, все элементы после пробела не учитываются.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="8b2d9-296">Например, если указаны индексы 0 и 2, а не 0 и 1, второй элемент игнорируется.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="8b2d9-297">Словари</span><span class="sxs-lookup"><span data-stu-id="8b2d9-297">Dictionaries</span></span>

<span data-ttu-id="8b2d9-298">Для целевых объектов `Dictionary` привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="8b2d9-299">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="8b2d9-300">Пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-300">For example:</span></span>

* <span data-ttu-id="8b2d9-301">Предположим, что целевой параметр является `Dictionary<int, string>` с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="8b2d9-302">Опубликованные строковые данные формы или запроса могут выглядеть как один из следующих примеров:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-302">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="8b2d9-303">Для всех перечисленных ранее форматов привязка модели передает словарь из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="8b2d9-304">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="8b2d9-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="8b2d9-305">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="8b2d9-305">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="8b2d9-306">Поведение глобализации для данных маршрутов привязки модели и строк запросов</span><span class="sxs-lookup"><span data-stu-id="8b2d9-306">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="8b2d9-307">Поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-307">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="8b2d9-308">обрабатывают значения как имеющие инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-308">Treat values as invariant culture.</span></span>
* <span data-ttu-id="8b2d9-309">Следует ожидать, что URL-адреса имеют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-309">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="8b2d9-310">Напротив, значения, поступающие из данных форм, подвергаются преобразованию с учетом языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-310">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="8b2d9-311">Это сделано намеренно, чтобы URL-адреса были общими в разных языковых стандартах.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-311">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="8b2d9-312">Чтобы поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core производили преобразование с учетом языка и региональных параметров, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-312">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="8b2d9-313">наследуют от <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-313">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="8b2d9-314">Скопируйте код из [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) или [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-314">Copy the code from [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="8b2d9-315">Замените [значение языка и региональных параметров](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30), передаваемое в конструктор поставщика значений, на [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-315">Replace the [culture value](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="8b2d9-316">Замените метод производства поставщика значений по умолчанию в параметрах MVC на новый:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-316">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="8b2d9-317">Специальные типы данных</span><span class="sxs-lookup"><span data-stu-id="8b2d9-317">Special data types</span></span>

<span data-ttu-id="8b2d9-318">Существуют некоторые особые типы данных, которые может обрабатывать привязка модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-318">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="8b2d9-319">IFormFile и IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="8b2d9-319">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="8b2d9-320">Переданный файл, включенный в HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-320">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="8b2d9-321">Также поддерживается `IEnumerable<IFormFile>` для нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-321">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="8b2d9-322">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="8b2d9-322">CancellationToken</span></span>

<span data-ttu-id="8b2d9-323">используется для отмены действия в асинхронных контроллерах.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-323">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="8b2d9-324">FormCollection</span><span class="sxs-lookup"><span data-stu-id="8b2d9-324">FormCollection</span></span>

<span data-ttu-id="8b2d9-325">Используется для извлечения всех значений из опубликованных данных формы.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-325">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="8b2d9-326">Форматировщики входных данных</span><span class="sxs-lookup"><span data-stu-id="8b2d9-326">Input formatters</span></span>

<span data-ttu-id="8b2d9-327">Данные в тексте запроса могут быть в формате JSON, XML или другом формате.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-327">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="8b2d9-328">Для анализа этих данных модель привязки использует *форматировщик входных данных*, настроенный для обработки определенного типа содержимого.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-328">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="8b2d9-329">По умолчанию ASP.NET Core включает форматировщики входных данных на основе JSON для обработки данных JSON.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-329">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="8b2d9-330">Вы можете добавить другие форматировщики для других типов содержимого.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-330">You can add other formatters for other content types.</span></span>

<span data-ttu-id="8b2d9-331">ASP.NET Core выбирает форматировщики входных данных на основе атрибута [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-331">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="8b2d9-332">Если атрибут отсутствует, используется [Заголовок Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-332">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="8b2d9-333">Чтобы использовать встроенные форматировщики входных данных XML:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-333">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="8b2d9-334">Установите пакет NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-334">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="8b2d9-335">В `Startup.ConfigureServices` вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> или <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-335">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=10)]

* <span data-ttu-id="8b2d9-336">Примените атрибут `Consumes` к классам контроллера или методам действий, которые должны ожидать XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-336">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="8b2d9-337">Дополнительные сведения см. в разделе [Введение в сериализацию XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-337">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

### <a name="customize-model-binding-with-input-formatters"></a><span data-ttu-id="8b2d9-338">Настройка привязки модели с помощью форматировщиков входных данных</span><span class="sxs-lookup"><span data-stu-id="8b2d9-338">Customize model binding with input formatters</span></span>

<span data-ttu-id="8b2d9-339">Форматировщик входных данных полностью отвечает за чтение данных из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-339">An input formatter takes full responsibility for reading data from the request body.</span></span> <span data-ttu-id="8b2d9-340">Чтобы настроить этот процесс, настройте API-интерфейсы, используемые форматировщиками входных данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-340">To customize this process, configure the APIs used by the input formatter.</span></span> <span data-ttu-id="8b2d9-341">В этом разделе описывается, как настроить форматировщик входных данных на основе `System.Text.Json` так, чтобы он понимал настраиваемый тип с именем `ObjectId`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-341">This section describes how to customize the `System.Text.Json`-based input formatter to understand a custom type named `ObjectId`.</span></span> 

<span data-ttu-id="8b2d9-342">Рассмотрим следующую модель, которая содержит настраиваемое свойство `ObjectId` с именем `Id`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-342">Consider the following model, which contains a custom `ObjectId` property named `Id`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ModelWithObjectId.cs?name=snippet_Class&highlight=3)]

<span data-ttu-id="8b2d9-343">Чтобы настроить процесс привязки модели при использовании `System.Text.Json`, создайте производный класс на основе класса <xref:System.Text.Json.Serialization.JsonConverter%601>:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-343">To customize the model binding process when using `System.Text.Json`, create a class derived from <xref:System.Text.Json.Serialization.JsonConverter%601>:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/JsonConverters/ObjectIdConverter.cs?name=snippet_Class)]

<span data-ttu-id="8b2d9-344">Чтобы использовать пользовательский преобразователь, примените к типу атрибут <xref:System.Text.Json.Serialization.JsonConverterAttribute>.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-344">To use a custom converter, apply the <xref:System.Text.Json.Serialization.JsonConverterAttribute> attribute to the type.</span></span> <span data-ttu-id="8b2d9-345">В следующем примере тип `ObjectId` настраивается с `ObjectIdConverter` в качестве пользовательского преобразователя:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-345">In the following example, the `ObjectId` type is configured with `ObjectIdConverter` as its custom converter:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ObjectId.cs?name=snippet_Class&highlight=1)]

<span data-ttu-id="8b2d9-346">Дополнительные сведения см. в статье [How to write custom converters for JSON serialization (marshalling) in .NET](/dotnet/standard/serialization/system-text-json-converters-how-to) (Создание пользовательских преобразователей для сериализации JSON (маршалинг) в .NET).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-346">For more information, see [How to write custom converters](/dotnet/standard/serialization/system-text-json-converters-how-to).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="8b2d9-347">Исключение указанных типов из привязки модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-347">Exclude specified types from model binding</span></span>

<span data-ttu-id="8b2d9-348">Поведение привязки модели и системы проверки определяется классом [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-348">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="8b2d9-349">Вы можете настроить `ModelMetadata`, добавив поставщик сведений в [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-349">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="8b2d9-350">Встроенные поставщики сведений доступны для отключения привязки модели или проверки для указанных типов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-350">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="8b2d9-351">Чтобы отключить привязку модели для всех моделей указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-351">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8b2d9-352">Например, для отключения привязки модели для всех моделей типа `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-352">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=5-6)]

<span data-ttu-id="8b2d9-353">Чтобы отключить проверку свойств указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-353">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8b2d9-354">Например, чтобы отключить проверку по свойствам типа `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-354">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=7-8)]

## <a name="custom-model-binders"></a><span data-ttu-id="8b2d9-355">Настраиваемые связыватели модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-355">Custom model binders</span></span>

<span data-ttu-id="8b2d9-356">Привязку модели можно расширить путем написания пользовательского связывателя модели и с помощью атрибута `[ModelBinder]`, чтобы выбрать его для заданного целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-356">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="8b2d9-357">Узнайте больше о [пользовательской привязке модели](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-357">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="8b2d9-358">Привязка модели вручную</span><span class="sxs-lookup"><span data-stu-id="8b2d9-358">Manual model binding</span></span> 

<span data-ttu-id="8b2d9-359">Привязка модели может вызываться вручную с помощью метода <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-359">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="8b2d9-360">Этот метод определен в классах `ControllerBase` и `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-360">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="8b2d9-361">Перегрузки метода позволяют задать поставщик префиксов и значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-361">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="8b2d9-362">Этот метод возвращает `false` при сбое привязки модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-362">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="8b2d9-363">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-363">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

<span data-ttu-id="8b2d9-364"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> использует поставщиков значений для получения данных из текста формы, строки запроса и данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-364"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>  uses value providers to get data from the form body, query string, and route data.</span></span> <span data-ttu-id="8b2d9-365">`TryUpdateModelAsync` как правило:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-365">`TryUpdateModelAsync` is typically:</span></span> 

* <span data-ttu-id="8b2d9-366">используется с приложениями Razor Pages и MVC, применяющими контроллеры и представления для предотвращения чрезмерной передачи данных;</span><span class="sxs-lookup"><span data-stu-id="8b2d9-366">Used with Razor Pages and MVC apps using controllers and views to prevent over-posting.</span></span>
* <span data-ttu-id="8b2d9-367">не используется с веб-API, если только не используется из данных формы, строк запроса и данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-367">Not used with a web API unless consumed from form data, query strings, and route data.</span></span> <span data-ttu-id="8b2d9-368">Конечные точки веб-API, которые используют JSON, применяют [форматировщики входных данных](#input-formatters) для десериализации текста запроса в объект.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-368">Web API endpoints that consume JSON use [Input formatters](#input-formatters) to deserialize the request body into an object.</span></span>

<span data-ttu-id="8b2d9-369">Дополнительные сведения см. в разделе [TryUpdateModelAsync](xref:data/ef-rp/crud#TryUpdateModelAsync).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-369">For more information, see [TryUpdateModelAsync](xref:data/ef-rp/crud#TryUpdateModelAsync).</span></span>

## <a name="fromservices-attribute"></a><span data-ttu-id="8b2d9-370">Атрибут [FromServices]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-370">[FromServices] attribute</span></span>

<span data-ttu-id="8b2d9-371">Имя этого атрибута соответствует шаблону атрибутов привязки модели, которые указывают источник данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-371">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="8b2d9-372">Но это не связано с привязкой данных от поставщика значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-372">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="8b2d9-373">Он получает экземпляр типа из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-373">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8b2d9-374">Он предназначен для предоставления альтернативы внедрению через конструктор, когда вам нужна служба, только если вызывается конкретный метод.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-374">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b2d9-375">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-375">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8b2d9-376">В этой статье объясняется, что такое привязка модели, как это работает и как настроить ее поведение.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-376">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="8b2d9-377">[Просмотреть или скачать пример кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-377">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="8b2d9-378">Что такое привязка модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-378">What is Model binding</span></span>

<span data-ttu-id="8b2d9-379">Контроллеры и Razor Pages работают с данными, поступающими из HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-379">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="8b2d9-380">Например, данные о маршруте могут предоставлять ключ записи, а опубликованные поля формы могут предоставлять значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-380">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="8b2d9-381">Написание кода для получения этих значений и их преобразования из строк в типы .NET будет утомительной задачей с высоким риском ошибок.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-381">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="8b2d9-382">Привязка модели позволяет автоматизировать этот процесс.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-382">Model binding automates this process.</span></span> <span data-ttu-id="8b2d9-383">Система привязки модели:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-383">The model binding system:</span></span>

* <span data-ttu-id="8b2d9-384">Извлекает данные из различных источников, таких как данные о маршруте, поля формы и строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-384">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="8b2d9-385">Предоставляет данные для контроллеров и страниц Razor в параметрах метода и открытых свойствах.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-385">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="8b2d9-386">Преобразует строковые данные в типы .NET.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-386">Converts string data to .NET types.</span></span>
* <span data-ttu-id="8b2d9-387">Обновляет свойства сложных типов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-387">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="8b2d9-388">Пример</span><span class="sxs-lookup"><span data-stu-id="8b2d9-388">Example</span></span>

<span data-ttu-id="8b2d9-389">Предположим, у вас есть следующий метод действия:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-389">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="8b2d9-390">И приложение получает запрос с этим URL-адресом:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-390">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="8b2d9-391">Привязка модели выполняет следующие действия, после того, как система маршрутизации выберет метод действия:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-391">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="8b2d9-392">Находит первый параметр `GetByID`, целое число с именем `id`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-392">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="8b2d9-393">Просматривает доступные источники в HTTP-запросе и находит `id` = "2" в данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-393">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="8b2d9-394">Преобразует строку "2" в целое число 2.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-394">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="8b2d9-395">Находит следующий параметр `GetByID`, логическое значение с именем `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-395">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="8b2d9-396">Просматривает источники и находит "DogsOnly=true" в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-396">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="8b2d9-397">Сопоставление имен не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-397">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="8b2d9-398">Преобразует строку "true" в логическое значение `true`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-398">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="8b2d9-399">Затем платформа вызывает метод `GetById`, передавая 2 для параметра `id` и `true` для параметра `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-399">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="8b2d9-400">В приведенном выше примере целевые объекты привязки модели — это параметры методов, которые являются примитивными типами.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-400">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="8b2d9-401">Целевые объекты также могут быть свойствами сложного типа.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-401">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="8b2d9-402">После успешной привязки каждого свойства осуществляется [проверка модели](xref:mvc/models/validation) для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-402">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="8b2d9-403">Записи о данных, привязанных к модели, а также об ошибках привязки или проверки хранятся в [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) или [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-403">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="8b2d9-404">Чтобы узнать об успешном выполнении этого процесса, приложение проверяет наличие флага [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-404">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="8b2d9-405">Цели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-405">Targets</span></span>

<span data-ttu-id="8b2d9-406">Привязка модели попытается найти значения для следующих типов целевых объектов:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-406">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="8b2d9-407">Параметры метода действия контроллера, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-407">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="8b2d9-408">Параметры метода обработчика Razor Pages, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-408">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="8b2d9-409">Открытые свойства контроллера или класса `PageModel`, если задано атрибутами.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-409">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="8b2d9-410">Атрибут [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-410">[BindProperty] attribute</span></span>

<span data-ttu-id="8b2d9-411">Может применяться к открытому свойству контроллера или класса `PageModel` для привязки модели для этого свойства:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-411">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="8b2d9-412">Атрибут [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-412">[BindProperties] attribute</span></span>

<span data-ttu-id="8b2d9-413">Доступно в ASP.NET Core 2.1 и более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-413">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="8b2d9-414">Может применяться к контроллеру или классу `PageModel`, чтобы привязка модели была направлена на все открытые свойства этого класса:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-414">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="8b2d9-415">Привязка модели для HTTP-запросов GET</span><span class="sxs-lookup"><span data-stu-id="8b2d9-415">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="8b2d9-416">По умолчанию свойства не привязываются к HTTP-запросам GET.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-416">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="8b2d9-417">Как правило, для запроса GET вам нужен только параметр идентификатора записи.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-417">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="8b2d9-418">Идентификатор записи используется для поиска элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-418">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="8b2d9-419">Поэтому не нужно привязывать свойство, которое содержит экземпляр модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-419">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="8b2d9-420">Если вы хотите привязать свойства к данным от запросов GET, задайте для свойства `SupportsGet` значение `true`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-420">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="8b2d9-421">Источники</span><span class="sxs-lookup"><span data-stu-id="8b2d9-421">Sources</span></span>

<span data-ttu-id="8b2d9-422">По умолчанию привязка модели получает данные в виде пар "ключ-значение" из следующих источников в HTTP-запросе:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-422">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="8b2d9-423">Поля формы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-423">Form fields</span></span>
1. <span data-ttu-id="8b2d9-424">Текст запроса (для [контроллеров, имеющих атрибут [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-424">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="8b2d9-425">Отправка данных</span><span class="sxs-lookup"><span data-stu-id="8b2d9-425">Route data</span></span>
1. <span data-ttu-id="8b2d9-426">Параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="8b2d9-426">Query string parameters</span></span>
1. <span data-ttu-id="8b2d9-427">Отправленные файлы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-427">Uploaded files</span></span>

<span data-ttu-id="8b2d9-428">Для каждого целевого параметра или свойства источники проверяются в порядке, указанном в предыдущем списке.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-428">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="8b2d9-429">Существует несколько исключений:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-429">There are a few exceptions:</span></span>

* <span data-ttu-id="8b2d9-430">Данные маршрутизации и значения строк запросов используются только для примитивных типов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-430">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="8b2d9-431">Отправленные файлы привязаны только к типам целевых объектов, которые реализуют `IFormFile` или `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-431">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="8b2d9-432">Если источник по умолчанию неверен, используйте один из следующих атрибутов, чтобы указать источник:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-432">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="8b2d9-433">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): возвращает значения из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-433">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="8b2d9-434">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): возвращает значения из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-434">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="8b2d9-435">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): возвращает значения из опубликованных полей формы.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-435">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="8b2d9-436">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): возвращает значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-436">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="8b2d9-437">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): возвращает значения из заголовков HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-437">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="8b2d9-438">Эти атрибуты:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-438">These attributes:</span></span>

* <span data-ttu-id="8b2d9-439">Добавляются к свойствам модели по отдельности (не к классу модели), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-439">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="8b2d9-440">При необходимости принимают значение имени модели в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-440">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="8b2d9-441">Этот параметр предоставляется в том случае, если имя свойства не соответствует значению в запросе.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-441">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="8b2d9-442">Например, значение в запросе может быть заголовком с дефисом в имени, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-442">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="8b2d9-443">Атрибут [FromBody]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-443">[FromBody] attribute</span></span>

<span data-ttu-id="8b2d9-444">Примените атрибут `[FromBody]` к параметру, чтобы заполнить его свойства из тела HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-444">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="8b2d9-445">Среда выполнения ASP.NET Core делегирует ответственность за считывание тела форматировщику входных данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-445">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="8b2d9-446">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-446">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="8b2d9-447">При применении `[FromBody]` к параметру сложного типа все атрибуты источника привязки, применяемые к его свойствам, игнорируются.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-447">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="8b2d9-448">Например, следующее действие `Create` указывает, что параметр `pet` заполняется из тела:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-448">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="8b2d9-449">Класс `Pet` указывает, что свойство `Breed` заполняется из параметра строки запроса:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-449">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="8b2d9-450">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-450">In the preceding example:</span></span>

* <span data-ttu-id="8b2d9-451">Атрибут `[FromQuery]` не учитывается.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-451">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="8b2d9-452">Свойство `Breed` не заполняется из параметра строки запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-452">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="8b2d9-453">Форматировщики входных данных считывают только тело и не распознают атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-453">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="8b2d9-454">Если подходящее значение найдено в теле, оно используется для заполнения свойства `Breed`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-454">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="8b2d9-455">Не применяют `[FromBody]` к нескольким параметрам в методе действия.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-455">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="8b2d9-456">После считывания потока запроса форматировщиком входных данных он больше не доступен для повторного чтения для привязки других параметров `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-456">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="8b2d9-457">Дополнительные источники</span><span class="sxs-lookup"><span data-stu-id="8b2d9-457">Additional sources</span></span>

<span data-ttu-id="8b2d9-458">Исходные данные предоставляются системой привязки модели *поставщиками значений*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-458">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="8b2d9-459">Вы можете записать и зарегистрировать пользовательские поставщики значений, которые получают данные для привязки модели из других источников.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-459">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="8b2d9-460">Например, вам могут потребоваться данные из файлов cookie или состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-460">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="8b2d9-461">Для получения данных из нового источника:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-461">To get data from a new source:</span></span>

* <span data-ttu-id="8b2d9-462">Создайте класс, реализующий перехватчик `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-462">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="8b2d9-463">Создайте класс, реализующий перехватчик `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-463">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="8b2d9-464">Зарегистрируйте класс фабрики в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-464">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="8b2d9-465">Пример приложения включает пример [поставщика значений](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) и [фабрики](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs), которая получает значения из файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-465">The sample app includes a [value provider](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="8b2d9-466">Ниже приведен код регистрации в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-466">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="8b2d9-467">Этот код помещает поставщик пользовательских значений после всех встроенных поставщиков значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-467">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="8b2d9-468">Чтобы сделать его первым в списке, вызовите `Insert(0, new CookieValueProviderFactory())` вместо `Add`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-468">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="8b2d9-469">Отсутствие источника для свойства модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-469">No source for a model property</span></span>

<span data-ttu-id="8b2d9-470">По умолчанию ошибка состояния модели не создается, если не найдено значение для свойства модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-470">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="8b2d9-471">Свойство получает значение NULL или значение по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-471">The property is set to null or a default value:</span></span>

* <span data-ttu-id="8b2d9-472">Примитивным типам, допускающим значение NULL, задается значение `null`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-472">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="8b2d9-473">Типам значений, не допускающим значение NULL, задается значение `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-473">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="8b2d9-474">Например, параметр `int id` получает значение 0.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-474">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="8b2d9-475">Для сложных типов привязка модели создает экземпляр с помощью конструктора по умолчанию без задания свойств.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-475">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="8b2d9-476">Массивы имеют значение `Array.Empty<T>()`, за исключением массивов `byte[]`, которые имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-476">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="8b2d9-477">Если состояние модели должно быть признано недействительным, когда в полях формы не найдены данные для свойства модели, используйте атрибут [`[BindRequired]`](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-477">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="8b2d9-478">Обратите внимание, это поведение `[BindRequired]` применяется к привязке модели из опубликованных данных формы, а не к данным JSON или XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-478">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="8b2d9-479">Данные основного текста запроса обрабатываются [форматировщиками входных данных](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-479">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="8b2d9-480">Ошибки преобразования типа</span><span class="sxs-lookup"><span data-stu-id="8b2d9-480">Type conversion errors</span></span>

<span data-ttu-id="8b2d9-481">Если источник найден, но его нельзя преобразовать в тип целевого объекта, состояние модели помечается как недопустимое.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-481">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="8b2d9-482">Параметр или свойство целевого объекта получает значение NULL или значение по умолчанию, как отмечалось в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-482">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="8b2d9-483">В контроллере API с атрибутом `[ApiController]` недопустимое состояние модели приводит к автоматическому ответу HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-483">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="8b2d9-484">На странице Razor повторно отображается страница с сообщением об ошибке:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-484">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="8b2d9-485">Проверка на стороне клиента перехватывает большинство неверных данных, которые в противном случае были бы отправлены в форму Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-485">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="8b2d9-486">Эта проверка затрудняет срабатывание выделенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-486">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="8b2d9-487">Пример приложения включает кнопку **Отправить с неверной датой**, которая помещает недопустимые данные в поле **Дата приема на работу** и отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-487">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="8b2d9-488">Эта кнопка показывает, как работает код для повторного отображения страницы, если возникла ошибка преобразования данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-488">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="8b2d9-489">Когда страница отображается повторно приведенным выше кодом, недопустимые входные данные не отображаются в поле формы.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-489">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="8b2d9-490">Это связано с тем, что свойству модели задано значение NULL или значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-490">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="8b2d9-491">Недопустимые входные данные отображаются в сообщении об ошибке.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-491">The invalid input does appear in an error message.</span></span> <span data-ttu-id="8b2d9-492">Но если требуется повторно отобразить неправильные данные в поле формы, возможно, следует сделать свойство модели строкой и выполнить преобразование данных вручную.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-492">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="8b2d9-493">Та же стратегия рекомендуется в том случае, если вы не хотите, чтобы ошибки преобразования типов приводили к ошибкам состояния модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-493">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="8b2d9-494">В этом случае следует сделать свойство модели строкой.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-494">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="8b2d9-495">Простые типы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-495">Simple types</span></span>

<span data-ttu-id="8b2d9-496">Связыватель модели может преобразовать исходные строки в следующие примитивные типы:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-496">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="8b2d9-497">Boolean</span><span class="sxs-lookup"><span data-stu-id="8b2d9-497">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="8b2d9-498">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-498">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="8b2d9-499">Char</span><span class="sxs-lookup"><span data-stu-id="8b2d9-499">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="8b2d9-500">DateTime</span><span class="sxs-lookup"><span data-stu-id="8b2d9-500">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="8b2d9-501">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="8b2d9-501">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="8b2d9-502">Decimal</span><span class="sxs-lookup"><span data-stu-id="8b2d9-502">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="8b2d9-503">Double</span><span class="sxs-lookup"><span data-stu-id="8b2d9-503">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="8b2d9-504">Enum</span><span class="sxs-lookup"><span data-stu-id="8b2d9-504">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="8b2d9-505">Guid</span><span class="sxs-lookup"><span data-stu-id="8b2d9-505">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="8b2d9-506">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-506">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="8b2d9-507">Один</span><span class="sxs-lookup"><span data-stu-id="8b2d9-507">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="8b2d9-508">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="8b2d9-508">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="8b2d9-509">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-509">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="8b2d9-510">URI</span><span class="sxs-lookup"><span data-stu-id="8b2d9-510">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="8b2d9-511">Версия</span><span class="sxs-lookup"><span data-stu-id="8b2d9-511">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="8b2d9-512">Сложные типы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-512">Complex types</span></span>

<span data-ttu-id="8b2d9-513">Сложный тип должен иметь открытый конструктор по умолчанию, а также открытые и доступные для записи свойства, подлежащие привязке.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-513">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="8b2d9-514">Когда происходит привязка модели, класс создается с помощью открытого конструктора по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-514">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="8b2d9-515">Для каждого свойства сложного типа привязка модели ищет в источниках шаблон имени *prefix.property_name*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-515">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="8b2d9-516">Если ничего не найдено, он ищет только *property_name* без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-516">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="8b2d9-517">Для привязки к параметру префикс является именем параметра.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-517">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="8b2d9-518">Для привязки к открытому свойству `PageModel` префикс является именем открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-518">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="8b2d9-519">Некоторые атрибуты имеют свойство `Prefix`, которое позволяет переопределить использование по умолчанию для имени параметра или свойства.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-519">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="8b2d9-520">Предположим, например, что сложный тип принадлежит к следующему классу `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-520">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="8b2d9-521">Префикс — это имя параметра</span><span class="sxs-lookup"><span data-stu-id="8b2d9-521">Prefix = parameter name</span></span>

<span data-ttu-id="8b2d9-522">Если модель, которую нужно привязать, является параметром с именем `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-522">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="8b2d9-523">Привязка модели начинается с поиска источников ключа `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-523">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="8b2d9-524">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-524">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="8b2d9-525">Префикс — это имя свойства</span><span class="sxs-lookup"><span data-stu-id="8b2d9-525">Prefix = property name</span></span>

<span data-ttu-id="8b2d9-526">Если модель для привязки является свойством с именем `Instructor` контроллера или класса `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-526">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="8b2d9-527">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-527">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="8b2d9-528">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-528">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="8b2d9-529">Пользовательский префикс</span><span class="sxs-lookup"><span data-stu-id="8b2d9-529">Custom prefix</span></span>

<span data-ttu-id="8b2d9-530">Если модель для привязки — это параметр с именем `instructorToUpdate`, а атрибут `Bind` задает `Instructor` как префикс:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-530">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="8b2d9-531">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-531">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="8b2d9-532">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-532">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="8b2d9-533">Атрибуты для целевых объектов сложного типа</span><span class="sxs-lookup"><span data-stu-id="8b2d9-533">Attributes for complex type targets</span></span>

<span data-ttu-id="8b2d9-534">Несколько встроенных атрибутов доступны для управления привязкой моделей сложных типов:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-534">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="8b2d9-535">Эти атрибуты влияют на привязку модели, если опубликованные данные формы являются источником значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-535">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="8b2d9-536">Они не влияют на форматировщики входных данных, которые обрабатывают опубликованные тексты запросов JSON и XML.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-536">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="8b2d9-537">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-537">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="8b2d9-538">Также см. обсуждение атрибута `[Required]` в разделе [Проверка модели](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-538">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="8b2d9-539">Атрибут [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-539">[BindRequired] attribute</span></span>

<span data-ttu-id="8b2d9-540">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-540">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="8b2d9-541">Приводит к тому, что привязка модели добавляет ошибку состояния модели, если привязка для свойства модели невозможна.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-541">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="8b2d9-542">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-542">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="8b2d9-543">Атрибут [BindNever]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-543">[BindNever] attribute</span></span>

<span data-ttu-id="8b2d9-544">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-544">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="8b2d9-545">Запрещает привязке модели задавать свойство модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-545">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="8b2d9-546">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-546">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="8b2d9-547">Атрибут [Bind]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-547">[Bind] attribute</span></span>

<span data-ttu-id="8b2d9-548">Может быть применен к классу или параметру метода.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-548">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="8b2d9-549">Указывает, какие свойства модели должны быть включены в привязку модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-549">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="8b2d9-550">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается любой метод действия или обработчик:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-550">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="8b2d9-551">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается метод `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-551">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="8b2d9-552">Атрибут `[Bind]` может использоваться для защиты от чрезмерной передачи данных при *создании*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-552">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="8b2d9-553">Он не работает в сценариях редактирования, поскольку исключенным свойствам задается значение NULL или значение по умолчанию, но не оставляется значение без изменений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-553">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="8b2d9-554">Для защиты от чрезмерной передачи данных рекомендуется использовать модели представлений вместо атрибута `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-554">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="8b2d9-555">Дополнительные сведения см. в разделе [Примечание по безопасности о чрезмерной передаче данных](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-555">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="8b2d9-556">Коллекции</span><span class="sxs-lookup"><span data-stu-id="8b2d9-556">Collections</span></span>

<span data-ttu-id="8b2d9-557">Для целевых объектов, которые являются коллекциями примитивных типов, привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-557">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="8b2d9-558">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-558">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="8b2d9-559">Пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-559">For example:</span></span>

* <span data-ttu-id="8b2d9-560">Предположим, что параметром для привязки является массив с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-560">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="8b2d9-561">Строковые данные формы или запроса могут иметь один из следующих форматов:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-561">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="8b2d9-562">Следующий формат поддерживается только в данных формы:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-562">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="8b2d9-563">Для всех перечисленных ранее форматов привязка модели передает массив из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-563">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="8b2d9-564">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="8b2d9-564">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="8b2d9-565">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="8b2d9-565">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="8b2d9-566">Форматы данных, которые используют номера нижних индексов (... [0] ... [1] ...), должны нумероваться последовательно, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-566">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="8b2d9-567">Если в нумерации есть пробелы, все элементы после пробела не учитываются.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-567">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="8b2d9-568">Например, если указаны индексы 0 и 2, а не 0 и 1, второй элемент игнорируется.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-568">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="8b2d9-569">Словари</span><span class="sxs-lookup"><span data-stu-id="8b2d9-569">Dictionaries</span></span>

<span data-ttu-id="8b2d9-570">Для целевых объектов `Dictionary` привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-570">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="8b2d9-571">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-571">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="8b2d9-572">Пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-572">For example:</span></span>

* <span data-ttu-id="8b2d9-573">Предположим, что целевой параметр является `Dictionary<int, string>` с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-573">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="8b2d9-574">Опубликованные строковые данные формы или запроса могут выглядеть как один из следующих примеров:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-574">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="8b2d9-575">Для всех перечисленных ранее форматов привязка модели передает словарь из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-575">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="8b2d9-576">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="8b2d9-576">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="8b2d9-577">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="8b2d9-577">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="8b2d9-578">Поведение глобализации для данных маршрутов привязки модели и строк запросов</span><span class="sxs-lookup"><span data-stu-id="8b2d9-578">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="8b2d9-579">Поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-579">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="8b2d9-580">обрабатывают значения как имеющие инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-580">Treat values as invariant culture.</span></span>
* <span data-ttu-id="8b2d9-581">Следует ожидать, что URL-адреса имеют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-581">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="8b2d9-582">Напротив, значения, поступающие из данных форм, подвергаются преобразованию с учетом языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-582">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="8b2d9-583">Это сделано намеренно, чтобы URL-адреса были общими в разных языковых стандартах.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-583">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="8b2d9-584">Чтобы поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core производили преобразование с учетом языка и региональных параметров, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-584">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="8b2d9-585">наследуют от <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-585">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="8b2d9-586">Скопируйте код из [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) или [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-586">Copy the code from [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="8b2d9-587">Замените [значение языка и региональных параметров](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30), передаваемое в конструктор поставщика значений, на [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="8b2d9-587">Replace the [culture value](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="8b2d9-588">Замените метод производства поставщика значений по умолчанию в параметрах MVC на новый:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-588">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="8b2d9-589">Специальные типы данных</span><span class="sxs-lookup"><span data-stu-id="8b2d9-589">Special data types</span></span>

<span data-ttu-id="8b2d9-590">Существуют некоторые особые типы данных, которые может обрабатывать привязка модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-590">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="8b2d9-591">IFormFile и IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="8b2d9-591">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="8b2d9-592">Переданный файл, включенный в HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-592">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="8b2d9-593">Также поддерживается `IEnumerable<IFormFile>` для нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-593">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="8b2d9-594">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="8b2d9-594">CancellationToken</span></span>

<span data-ttu-id="8b2d9-595">используется для отмены действия в асинхронных контроллерах.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-595">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="8b2d9-596">FormCollection</span><span class="sxs-lookup"><span data-stu-id="8b2d9-596">FormCollection</span></span>

<span data-ttu-id="8b2d9-597">Используется для извлечения всех значений из опубликованных данных формы.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-597">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="8b2d9-598">Форматировщики входных данных</span><span class="sxs-lookup"><span data-stu-id="8b2d9-598">Input formatters</span></span>

<span data-ttu-id="8b2d9-599">Данные в тексте запроса могут быть в формате JSON, XML или другом формате.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-599">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="8b2d9-600">Для анализа этих данных модель привязки использует *форматировщик входных данных*, настроенный для обработки определенного типа содержимого.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-600">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="8b2d9-601">По умолчанию ASP.NET Core включает форматировщики входных данных на основе JSON для обработки данных JSON.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-601">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="8b2d9-602">Вы можете добавить другие форматировщики для других типов содержимого.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-602">You can add other formatters for other content types.</span></span>

<span data-ttu-id="8b2d9-603">ASP.NET Core выбирает форматировщики входных данных на основе атрибута [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-603">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="8b2d9-604">Если атрибут отсутствует, используется [Заголовок Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-604">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="8b2d9-605">Чтобы использовать встроенные форматировщики входных данных XML:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-605">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="8b2d9-606">Установите пакет NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-606">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="8b2d9-607">В `Startup.ConfigureServices` вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> или <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-607">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="8b2d9-608">Примените атрибут `Consumes` к классам контроллера или методам действий, которые должны ожидать XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-608">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="8b2d9-609">Дополнительные сведения см. в разделе [Введение в сериализацию XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-609">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="8b2d9-610">Исключение указанных типов из привязки модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-610">Exclude specified types from model binding</span></span>

<span data-ttu-id="8b2d9-611">Поведение привязки модели и системы проверки определяется классом [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-611">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="8b2d9-612">Вы можете настроить `ModelMetadata`, добавив поставщик сведений в [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-612">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="8b2d9-613">Встроенные поставщики сведений доступны для отключения привязки модели или проверки для указанных типов.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-613">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="8b2d9-614">Чтобы отключить привязку модели для всех моделей указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-614">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8b2d9-615">Например, для отключения привязки модели для всех моделей типа `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-615">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="8b2d9-616">Чтобы отключить проверку свойств указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-616">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8b2d9-617">Например, чтобы отключить проверку по свойствам типа `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-617">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="8b2d9-618">Настраиваемые связыватели модели</span><span class="sxs-lookup"><span data-stu-id="8b2d9-618">Custom model binders</span></span>

<span data-ttu-id="8b2d9-619">Привязку модели можно расширить путем написания пользовательского связывателя модели и с помощью атрибута `[ModelBinder]`, чтобы выбрать его для заданного целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-619">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="8b2d9-620">Узнайте больше о [пользовательской привязке модели](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-620">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="8b2d9-621">Привязка модели вручную</span><span class="sxs-lookup"><span data-stu-id="8b2d9-621">Manual model binding</span></span>

<span data-ttu-id="8b2d9-622">Привязка модели может вызываться вручную с помощью метода <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-622">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="8b2d9-623">Этот метод определен в классах `ControllerBase` и `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-623">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="8b2d9-624">Перегрузки метода позволяют задать поставщик префиксов и значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-624">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="8b2d9-625">Этот метод возвращает `false` при сбое привязки модели.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-625">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="8b2d9-626">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="8b2d9-626">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="8b2d9-627">Атрибут [FromServices]</span><span class="sxs-lookup"><span data-stu-id="8b2d9-627">[FromServices] attribute</span></span>

<span data-ttu-id="8b2d9-628">Имя этого атрибута соответствует шаблону атрибутов привязки модели, которые указывают источник данных.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-628">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="8b2d9-629">Но это не связано с привязкой данных от поставщика значений.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-629">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="8b2d9-630">Он получает экземпляр типа из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="8b2d9-630">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="8b2d9-631">Он предназначен для предоставления альтернативы внедрению через конструктор, когда вам нужна служба, только если вызывается конкретный метод.</span><span class="sxs-lookup"><span data-stu-id="8b2d9-631">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b2d9-632">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8b2d9-632">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
