---
title: Привязка модели в ASP.NET Core
author: rick-anderson
description: Узнайте, как работает привязка модели в ASP.NET Core и как настроить ее поведение.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 11/21/2019
uid: mvc/models/model-binding
ms.openlocfilehash: da6cc25e0bbb1b2301529b34eab4c91f9ccb46eb
ms.sourcegitcommit: 851b921080fe8d719f54871770ccf6f78052584e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2019
ms.locfileid: "74944299"
---
# <a name="model-binding-in-aspnet-core"></a><span data-ttu-id="fbb06-103">Привязка модели в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbb06-103">Model Binding in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fbb06-104">В этой статье объясняется, что такое привязка модели, как это работает и как настроить ее поведение.</span><span class="sxs-lookup"><span data-stu-id="fbb06-104">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="fbb06-105">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fbb06-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="fbb06-106">Что такое привязка модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-106">What is Model binding</span></span>

<span data-ttu-id="fbb06-107">Контроллеры и Razor Pages работают с данными, поступающими из HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-107">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="fbb06-108">Например, данные о маршруте могут предоставлять ключ записи, а опубликованные поля формы могут предоставлять значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-108">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="fbb06-109">Написание кода для получения этих значений и их преобразования из строк в типы .NET будет утомительной задачей с высоким риском ошибок.</span><span class="sxs-lookup"><span data-stu-id="fbb06-109">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="fbb06-110">Привязка модели позволяет автоматизировать этот процесс.</span><span class="sxs-lookup"><span data-stu-id="fbb06-110">Model binding automates this process.</span></span> <span data-ttu-id="fbb06-111">Система привязки модели:</span><span class="sxs-lookup"><span data-stu-id="fbb06-111">The model binding system:</span></span>

* <span data-ttu-id="fbb06-112">Извлекает данные из различных источников, таких как данные о маршруте, поля формы и строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-112">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="fbb06-113">Предоставляет данные для контроллеров и страниц Razor в параметрах метода и открытых свойствах.</span><span class="sxs-lookup"><span data-stu-id="fbb06-113">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="fbb06-114">Преобразует строковые данные в типы .NET.</span><span class="sxs-lookup"><span data-stu-id="fbb06-114">Converts string data to .NET types.</span></span>
* <span data-ttu-id="fbb06-115">Обновляет свойства сложных типов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-115">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="fbb06-116">Пример</span><span class="sxs-lookup"><span data-stu-id="fbb06-116">Example</span></span>

<span data-ttu-id="fbb06-117">Предположим, у вас есть следующий метод действия:</span><span class="sxs-lookup"><span data-stu-id="fbb06-117">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="fbb06-118">И приложение получает запрос с этим URL-адресом:</span><span class="sxs-lookup"><span data-stu-id="fbb06-118">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="fbb06-119">Привязка модели выполняет следующие действия, после того, как система маршрутизации выберет метод действия:</span><span class="sxs-lookup"><span data-stu-id="fbb06-119">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="fbb06-120">Находит первый параметр `GetByID`, целое число с именем `id`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-120">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="fbb06-121">Просматривает доступные источники в HTTP-запросе и находит `id` = "2" в данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="fbb06-121">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="fbb06-122">Преобразует строку "2" в целое число 2.</span><span class="sxs-lookup"><span data-stu-id="fbb06-122">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="fbb06-123">Находит следующий параметр `GetByID`, логическое значение с именем `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-123">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="fbb06-124">Просматривает источники и находит "DogsOnly=true" в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-124">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="fbb06-125">Сопоставление имен не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="fbb06-125">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="fbb06-126">Преобразует строку "true" в логическое значение `true`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-126">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="fbb06-127">Затем платформа вызывает метод `GetById`, передавая 2 для параметра `id` и `true` для параметра `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-127">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="fbb06-128">В приведенном выше примере целевые объекты привязки модели — это параметры методов, которые являются примитивными типами.</span><span class="sxs-lookup"><span data-stu-id="fbb06-128">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="fbb06-129">Целевые объекты также могут быть свойствами сложного типа.</span><span class="sxs-lookup"><span data-stu-id="fbb06-129">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="fbb06-130">После успешной привязки каждого свойства осуществляется [проверка модели](xref:mvc/models/validation) для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="fbb06-130">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="fbb06-131">Записи о данных, привязанных к модели, а также об ошибках привязки или проверки хранятся в [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) или [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="fbb06-131">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="fbb06-132">Чтобы узнать об успешном выполнении этого процесса, приложение проверяет наличие флага [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="fbb06-132">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="fbb06-133">Целевые объекты</span><span class="sxs-lookup"><span data-stu-id="fbb06-133">Targets</span></span>

<span data-ttu-id="fbb06-134">Привязка модели попытается найти значения для следующих типов целевых объектов:</span><span class="sxs-lookup"><span data-stu-id="fbb06-134">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="fbb06-135">Параметры метода действия контроллера, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="fbb06-135">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="fbb06-136">Параметры метода обработчика Razor Pages, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="fbb06-136">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="fbb06-137">Открытые свойства контроллера или класса `PageModel`, если задано атрибутами.</span><span class="sxs-lookup"><span data-stu-id="fbb06-137">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="fbb06-138">Атрибут [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="fbb06-138">[BindProperty] attribute</span></span>

<span data-ttu-id="fbb06-139">Может применяться к открытому свойству контроллера или класса `PageModel` для привязки модели для этого свойства:</span><span class="sxs-lookup"><span data-stu-id="fbb06-139">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="fbb06-140">Атрибут [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="fbb06-140">[BindProperties] attribute</span></span>

<span data-ttu-id="fbb06-141">Доступно в ASP.NET Core 2.1 и более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="fbb06-141">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="fbb06-142">Может применяться к контроллеру или классу `PageModel`, чтобы привязка модели была направлена на все открытые свойства этого класса:</span><span class="sxs-lookup"><span data-stu-id="fbb06-142">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="fbb06-143">Привязка модели для HTTP-запросов GET</span><span class="sxs-lookup"><span data-stu-id="fbb06-143">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="fbb06-144">По умолчанию свойства не привязываются к HTTP-запросам GET.</span><span class="sxs-lookup"><span data-stu-id="fbb06-144">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="fbb06-145">Как правило, для запроса GET вам нужен только параметр идентификатора записи.</span><span class="sxs-lookup"><span data-stu-id="fbb06-145">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="fbb06-146">Идентификатор записи используется для поиска элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-146">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="fbb06-147">Поэтому не нужно привязывать свойство, которое содержит экземпляр модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-147">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="fbb06-148">Если вы хотите привязать свойства к данным от запросов GET, задайте для свойства `SupportsGet` значение `true`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-148">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="fbb06-149">Источники</span><span class="sxs-lookup"><span data-stu-id="fbb06-149">Sources</span></span>

<span data-ttu-id="fbb06-150">По умолчанию привязка модели получает данные в виде пар "ключ-значение" из следующих источников в HTTP-запросе:</span><span class="sxs-lookup"><span data-stu-id="fbb06-150">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="fbb06-151">Поля формы</span><span class="sxs-lookup"><span data-stu-id="fbb06-151">Form fields</span></span>
1. <span data-ttu-id="fbb06-152">Текст запроса (для [контроллеров, имеющих атрибут [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="fbb06-152">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="fbb06-153">Данные маршрута</span><span class="sxs-lookup"><span data-stu-id="fbb06-153">Route data</span></span>
1. <span data-ttu-id="fbb06-154">Параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="fbb06-154">Query string parameters</span></span>
1. <span data-ttu-id="fbb06-155">Отправленные файлы</span><span class="sxs-lookup"><span data-stu-id="fbb06-155">Uploaded files</span></span>

<span data-ttu-id="fbb06-156">Для каждого целевого параметра или свойства источники проверяются в порядке, указанном в предыдущем списке.</span><span class="sxs-lookup"><span data-stu-id="fbb06-156">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="fbb06-157">Существует несколько исключений:</span><span class="sxs-lookup"><span data-stu-id="fbb06-157">There are a few exceptions:</span></span>

* <span data-ttu-id="fbb06-158">Данные маршрутизации и значения строк запросов используются только для примитивных типов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-158">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="fbb06-159">Отправленные файлы привязаны только к типам целевых объектов, которые реализуют `IFormFile` или `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-159">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="fbb06-160">Если источник по умолчанию неверен, используйте один из следующих атрибутов, чтобы указать источник:</span><span class="sxs-lookup"><span data-stu-id="fbb06-160">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="fbb06-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): возвращает значения из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-161">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="fbb06-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): возвращает значения из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="fbb06-162">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="fbb06-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): возвращает значения из опубликованных полей формы.</span><span class="sxs-lookup"><span data-stu-id="fbb06-163">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="fbb06-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): возвращает значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-164">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="fbb06-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): возвращает значения из заголовков HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbb06-165">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="fbb06-166">Эти атрибуты:</span><span class="sxs-lookup"><span data-stu-id="fbb06-166">These attributes:</span></span>

* <span data-ttu-id="fbb06-167">Добавляются к свойствам модели по отдельности (не к классу модели), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="fbb06-167">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="fbb06-168">При необходимости принимают значение имени модели в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="fbb06-168">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="fbb06-169">Этот параметр предоставляется в том случае, если имя свойства не соответствует значению в запросе.</span><span class="sxs-lookup"><span data-stu-id="fbb06-169">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="fbb06-170">Например, значение в запросе может быть заголовком с дефисом в имени, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="fbb06-170">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="fbb06-171">Атрибут [FromBody]</span><span class="sxs-lookup"><span data-stu-id="fbb06-171">[FromBody] attribute</span></span>

<span data-ttu-id="fbb06-172">Примените атрибут `[FromBody]` к параметру, чтобы заполнить его свойства из тела HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-172">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="fbb06-173">Среда выполнения ASP.NET Core делегирует ответственность за считывание тела форматировщику входных данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-173">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="fbb06-174">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="fbb06-174">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="fbb06-175">При применении `[FromBody]` к параметру сложного типа все атрибуты источника привязки, применяемые к его свойствам, игнорируются.</span><span class="sxs-lookup"><span data-stu-id="fbb06-175">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="fbb06-176">Например, следующее действие `Create` указывает, что параметр `pet` заполняется из тела:</span><span class="sxs-lookup"><span data-stu-id="fbb06-176">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="fbb06-177">Класс `Pet` указывает, что свойство `Breed` заполняется из параметра строки запроса:</span><span class="sxs-lookup"><span data-stu-id="fbb06-177">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="fbb06-178">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="fbb06-178">In the preceding example:</span></span>

* <span data-ttu-id="fbb06-179">Атрибут `[FromQuery]` не учитывается.</span><span class="sxs-lookup"><span data-stu-id="fbb06-179">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="fbb06-180">Свойство `Breed` не заполняется из параметра строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-180">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="fbb06-181">Форматировщики входных данных считывают только тело и не распознают атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="fbb06-181">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="fbb06-182">Если подходящее значение найдено в теле, оно используется для заполнения свойства `Breed`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-182">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="fbb06-183">Не применяют `[FromBody]` к нескольким параметрам в методе действия.</span><span class="sxs-lookup"><span data-stu-id="fbb06-183">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="fbb06-184">После считывания потока запроса форматировщиком входных данных он больше не доступен для повторного чтения для привязки других параметров `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-184">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="fbb06-185">Дополнительные источники</span><span class="sxs-lookup"><span data-stu-id="fbb06-185">Additional sources</span></span>

<span data-ttu-id="fbb06-186">Исходные данные предоставляются системой привязки модели *поставщиками значений*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-186">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="fbb06-187">Вы можете записать и зарегистрировать пользовательские поставщики значений, которые получают данные для привязки модели из других источников.</span><span class="sxs-lookup"><span data-stu-id="fbb06-187">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="fbb06-188">Например, вам могут потребоваться данные из файлов cookie или состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-188">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="fbb06-189">Для получения данных из нового источника:</span><span class="sxs-lookup"><span data-stu-id="fbb06-189">To get data from a new source:</span></span>

* <span data-ttu-id="fbb06-190">Создайте класс, реализующий `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-190">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="fbb06-191">Создайте класс, реализующий `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-191">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="fbb06-192">Зарегистрируйте класс фабрики в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-192">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="fbb06-193">Пример приложения включает пример [поставщика значений](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) и [фабрики](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs), которая получает значения из файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="fbb06-193">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="fbb06-194">Ниже приведен код регистрации в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-194">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="fbb06-195">Этот код помещает поставщик пользовательских значений после всех встроенных поставщиков значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-195">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="fbb06-196">Чтобы сделать его первым в списке, вызовите `Insert(0, new CookieValueProviderFactory())` вместо `Add`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-196">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="fbb06-197">Отсутствие источника для свойства модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-197">No source for a model property</span></span>

<span data-ttu-id="fbb06-198">По умолчанию ошибка состояния модели не создается, если не найдено значение для свойства модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-198">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="fbb06-199">Свойство получает значение NULL или значение по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="fbb06-199">The property is set to null or a default value:</span></span>

* <span data-ttu-id="fbb06-200">Примитивным типам, допускающим значение NULL, задается значение `null`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-200">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="fbb06-201">Типам значений, не допускающим значение NULL, задается значение `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-201">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="fbb06-202">Например, параметр `int id` получает значение 0.</span><span class="sxs-lookup"><span data-stu-id="fbb06-202">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="fbb06-203">Для сложных типов привязка модели создает экземпляр с помощью конструктора по умолчанию без задания свойств.</span><span class="sxs-lookup"><span data-stu-id="fbb06-203">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="fbb06-204">Массивы имеют значение `Array.Empty<T>()`, за исключением массивов `byte[]`, которые имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-204">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="fbb06-205">Если состояние модели должно быть признано недействительным, когда в полях формы не найдены данные для свойства модели, используйте атрибут [`[BindRequired]`](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="fbb06-205">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="fbb06-206">Обратите внимание, это поведение `[BindRequired]` применяется к привязке модели из опубликованных данных формы, а не к данным JSON или XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-206">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="fbb06-207">Данные основного текста запроса обрабатываются [форматировщиками входных данных](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="fbb06-207">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="fbb06-208">Ошибки преобразования типа</span><span class="sxs-lookup"><span data-stu-id="fbb06-208">Type conversion errors</span></span>

<span data-ttu-id="fbb06-209">Если источник найден, но его нельзя преобразовать в тип целевого объекта, состояние модели помечается как недопустимое.</span><span class="sxs-lookup"><span data-stu-id="fbb06-209">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="fbb06-210">Параметр или свойство целевого объекта получает значение NULL или значение по умолчанию, как отмечалось в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="fbb06-210">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="fbb06-211">В контроллере API с атрибутом `[ApiController]` недопустимое состояние модели приводит к автоматическому ответу HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="fbb06-211">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="fbb06-212">На странице Razor повторно отображается страница с сообщением об ошибке:</span><span class="sxs-lookup"><span data-stu-id="fbb06-212">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="fbb06-213">Проверка на стороне клиента перехватывает большинство неверных данных, которые в противном случае были бы отправлены в форму Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fbb06-213">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="fbb06-214">Эта проверка затрудняет срабатывание выделенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-214">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="fbb06-215">Пример приложения включает кнопку **Отправить с неверной датой**, которая помещает недопустимые данные в поле **Дата приема на работу** и отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="fbb06-215">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="fbb06-216">Эта кнопка показывает, как работает код для повторного отображения страницы, если возникла ошибка преобразования данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-216">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="fbb06-217">Когда страница отображается повторно приведенным выше кодом, недопустимые входные данные не отображаются в поле формы.</span><span class="sxs-lookup"><span data-stu-id="fbb06-217">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="fbb06-218">Это связано с тем, что свойству модели задано значение NULL или значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fbb06-218">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="fbb06-219">Недопустимые входные данные отображаются в сообщении об ошибке.</span><span class="sxs-lookup"><span data-stu-id="fbb06-219">The invalid input does appear in an error message.</span></span> <span data-ttu-id="fbb06-220">Но если требуется повторно отобразить неправильные данные в поле формы, возможно, следует сделать свойство модели строкой и выполнить преобразование данных вручную.</span><span class="sxs-lookup"><span data-stu-id="fbb06-220">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="fbb06-221">Та же стратегия рекомендуется в том случае, если вы не хотите, чтобы ошибки преобразования типов приводили к ошибкам состояния модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-221">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="fbb06-222">В этом случае следует сделать свойство модели строкой.</span><span class="sxs-lookup"><span data-stu-id="fbb06-222">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="fbb06-223">Простые типы</span><span class="sxs-lookup"><span data-stu-id="fbb06-223">Simple types</span></span>

<span data-ttu-id="fbb06-224">Связыватель модели может преобразовать исходные строки в следующие примитивные типы:</span><span class="sxs-lookup"><span data-stu-id="fbb06-224">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="fbb06-225">Boolean</span><span class="sxs-lookup"><span data-stu-id="fbb06-225">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="fbb06-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="fbb06-226">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="fbb06-227">Char</span><span class="sxs-lookup"><span data-stu-id="fbb06-227">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="fbb06-228">DateTime</span><span class="sxs-lookup"><span data-stu-id="fbb06-228">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="fbb06-229">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="fbb06-229">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="fbb06-230">Decimal</span><span class="sxs-lookup"><span data-stu-id="fbb06-230">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="fbb06-231">Double</span><span class="sxs-lookup"><span data-stu-id="fbb06-231">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="fbb06-232">Enum</span><span class="sxs-lookup"><span data-stu-id="fbb06-232">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="fbb06-233">Guid</span><span class="sxs-lookup"><span data-stu-id="fbb06-233">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="fbb06-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="fbb06-234">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="fbb06-235">Single</span><span class="sxs-lookup"><span data-stu-id="fbb06-235">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="fbb06-236">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fbb06-236">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="fbb06-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="fbb06-237">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="fbb06-238">Uri</span><span class="sxs-lookup"><span data-stu-id="fbb06-238">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="fbb06-239">Version</span><span class="sxs-lookup"><span data-stu-id="fbb06-239">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="fbb06-240">Сложные типы</span><span class="sxs-lookup"><span data-stu-id="fbb06-240">Complex types</span></span>

<span data-ttu-id="fbb06-241">Сложный тип должен иметь открытый конструктор по умолчанию, а также открытые и доступные для записи свойства, подлежащие привязке.</span><span class="sxs-lookup"><span data-stu-id="fbb06-241">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="fbb06-242">Когда происходит привязка модели, класс создается с помощью открытого конструктора по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fbb06-242">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="fbb06-243">Для каждого свойства сложного типа привязка модели ищет в источниках шаблон имени *prefix.property_name*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-243">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="fbb06-244">Если ничего не найдено, он ищет только *property_name* без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-244">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="fbb06-245">Для привязки к параметру префикс является именем параметра.</span><span class="sxs-lookup"><span data-stu-id="fbb06-245">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="fbb06-246">Для привязки к открытому свойству `PageModel` префикс является именем открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="fbb06-246">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="fbb06-247">Некоторые атрибуты имеют свойство `Prefix`, которое позволяет переопределить использование по умолчанию для имени параметра или свойства.</span><span class="sxs-lookup"><span data-stu-id="fbb06-247">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="fbb06-248">Предположим, например, что сложный тип принадлежит к следующему классу `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-248">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="fbb06-249">Префикс — это имя параметра</span><span class="sxs-lookup"><span data-stu-id="fbb06-249">Prefix = parameter name</span></span>

<span data-ttu-id="fbb06-250">Если модель, которую нужно привязать, является параметром с именем `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-250">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="fbb06-251">Привязка модели начинается с поиска источников ключа `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-251">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="fbb06-252">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-252">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="fbb06-253">Префикс — это имя свойства</span><span class="sxs-lookup"><span data-stu-id="fbb06-253">Prefix = property name</span></span>

<span data-ttu-id="fbb06-254">Если модель для привязки является свойством с именем `Instructor` контроллера или класса `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-254">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="fbb06-255">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-255">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="fbb06-256">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-256">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="fbb06-257">Пользовательский префикс</span><span class="sxs-lookup"><span data-stu-id="fbb06-257">Custom prefix</span></span>

<span data-ttu-id="fbb06-258">Если модель для привязки — это параметр с именем `instructorToUpdate`, а атрибут `Bind` задает `Instructor` как префикс:</span><span class="sxs-lookup"><span data-stu-id="fbb06-258">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="fbb06-259">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-259">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="fbb06-260">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-260">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="fbb06-261">Атрибуты для целевых объектов сложного типа</span><span class="sxs-lookup"><span data-stu-id="fbb06-261">Attributes for complex type targets</span></span>

<span data-ttu-id="fbb06-262">Несколько встроенных атрибутов доступны для управления привязкой моделей сложных типов:</span><span class="sxs-lookup"><span data-stu-id="fbb06-262">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="fbb06-263">Эти атрибуты влияют на привязку модели, если опубликованные данные формы являются источником значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-263">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="fbb06-264">Они не влияют на форматировщики входных данных, которые обрабатывают опубликованные тексты запросов JSON и XML.</span><span class="sxs-lookup"><span data-stu-id="fbb06-264">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="fbb06-265">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="fbb06-265">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="fbb06-266">Также см. обсуждение атрибута `[Required]` в разделе [Проверка модели](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="fbb06-266">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="fbb06-267">Атрибут [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="fbb06-267">[BindRequired] attribute</span></span>

<span data-ttu-id="fbb06-268">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-268">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="fbb06-269">Приводит к тому, что привязка модели добавляет ошибку состояния модели, если привязка для свойства модели невозможна.</span><span class="sxs-lookup"><span data-stu-id="fbb06-269">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="fbb06-270">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="fbb06-270">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="fbb06-271">Атрибут [BindNever]</span><span class="sxs-lookup"><span data-stu-id="fbb06-271">[BindNever] attribute</span></span>

<span data-ttu-id="fbb06-272">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-272">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="fbb06-273">Запрещает привязке модели задавать свойство модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-273">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="fbb06-274">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="fbb06-274">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="fbb06-275">Атрибут [Bind]</span><span class="sxs-lookup"><span data-stu-id="fbb06-275">[Bind] attribute</span></span>

<span data-ttu-id="fbb06-276">Может быть применен к классу или параметру метода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-276">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="fbb06-277">Указывает, какие свойства модели должны быть включены в привязку модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-277">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="fbb06-278">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается любой метод действия или обработчик:</span><span class="sxs-lookup"><span data-stu-id="fbb06-278">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="fbb06-279">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается метод `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-279">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="fbb06-280">Атрибут `[Bind]` может использоваться для защиты от чрезмерной передачи данных при *создании*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-280">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="fbb06-281">Он не работает в сценариях редактирования, поскольку исключенным свойствам задается значение NULL или значение по умолчанию, но не оставляется значение без изменений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-281">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="fbb06-282">Для защиты от чрезмерной передачи данных рекомендуется использовать модели представлений вместо атрибута `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-282">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="fbb06-283">Дополнительные сведения см. в разделе [Примечание по безопасности о чрезмерной передаче данных](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="fbb06-283">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="fbb06-284">Коллекции</span><span class="sxs-lookup"><span data-stu-id="fbb06-284">Collections</span></span>

<span data-ttu-id="fbb06-285">Для целевых объектов, которые являются коллекциями примитивных типов, привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-285">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="fbb06-286">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-286">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="fbb06-287">Например:</span><span class="sxs-lookup"><span data-stu-id="fbb06-287">For example:</span></span>

* <span data-ttu-id="fbb06-288">Предположим, что параметром для привязки является массив с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-288">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="fbb06-289">Строковые данные формы или запроса могут иметь один из следующих форматов:</span><span class="sxs-lookup"><span data-stu-id="fbb06-289">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="fbb06-290">Следующий формат поддерживается только в данных формы:</span><span class="sxs-lookup"><span data-stu-id="fbb06-290">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="fbb06-291">Для всех перечисленных ранее форматов привязка модели передает массив из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-291">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="fbb06-292">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="fbb06-292">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="fbb06-293">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="fbb06-293">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="fbb06-294">Форматы данных, которые используют номера нижних индексов (... [0] ... [1] ...), должны нумероваться последовательно, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="fbb06-294">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="fbb06-295">Если в нумерации есть пробелы, все элементы после пробела не учитываются.</span><span class="sxs-lookup"><span data-stu-id="fbb06-295">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="fbb06-296">Например, если указаны индексы 0 и 2, а не 0 и 1, второй элемент игнорируется.</span><span class="sxs-lookup"><span data-stu-id="fbb06-296">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="fbb06-297">Словари</span><span class="sxs-lookup"><span data-stu-id="fbb06-297">Dictionaries</span></span>

<span data-ttu-id="fbb06-298">Для целевых объектов `Dictionary` привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-298">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="fbb06-299">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-299">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="fbb06-300">Например:</span><span class="sxs-lookup"><span data-stu-id="fbb06-300">For example:</span></span>

* <span data-ttu-id="fbb06-301">Предположим, что целевой параметр является `Dictionary<int, string>` с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-301">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="fbb06-302">Опубликованные строковые данные формы или запроса могут выглядеть как один из следующих примеров:</span><span class="sxs-lookup"><span data-stu-id="fbb06-302">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="fbb06-303">Для всех перечисленных ранее форматов привязка модели передает словарь из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-303">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="fbb06-304">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="fbb06-304">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="fbb06-305">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="fbb06-305">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="fbb06-306">Поведение глобализации для данных маршрутов привязки модели и строк запросов</span><span class="sxs-lookup"><span data-stu-id="fbb06-306">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="fbb06-307">Поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fbb06-307">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="fbb06-308">обрабатывают значения как имеющие инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="fbb06-308">Treat values as invariant culture.</span></span>
* <span data-ttu-id="fbb06-309">Следует ожидать, что URL-адреса имеют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="fbb06-309">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="fbb06-310">Напротив, значения, поступающие из данных форм, подвергаются преобразованию с учетом языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="fbb06-310">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="fbb06-311">Это сделано намеренно, чтобы URL-адреса были общими в разных языковых стандартах.</span><span class="sxs-lookup"><span data-stu-id="fbb06-311">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="fbb06-312">Чтобы поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core производили преобразование с учетом языка и региональных параметров, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="fbb06-312">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="fbb06-313">наследуют от <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>.</span><span class="sxs-lookup"><span data-stu-id="fbb06-313">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="fbb06-314">Скопируйте код из [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) или [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="fbb06-314">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="fbb06-315">Замените [значение языка и региональных параметров](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30), передаваемое в конструктор поставщика значений, на [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="fbb06-315">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="fbb06-316">Замените метод производства поставщика значений по умолчанию в параметрах MVC на новый:</span><span class="sxs-lookup"><span data-stu-id="fbb06-316">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="fbb06-317">Специальные типы данных</span><span class="sxs-lookup"><span data-stu-id="fbb06-317">Special data types</span></span>

<span data-ttu-id="fbb06-318">Существуют некоторые особые типы данных, которые может обрабатывать привязка модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-318">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="fbb06-319">IFormFile и IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="fbb06-319">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="fbb06-320">Переданный файл, включенный в HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="fbb06-320">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="fbb06-321">Также поддерживается `IEnumerable<IFormFile>` для нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-321">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="fbb06-322">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="fbb06-322">CancellationToken</span></span>

<span data-ttu-id="fbb06-323">используется для отмены действия в асинхронных контроллерах.</span><span class="sxs-lookup"><span data-stu-id="fbb06-323">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="fbb06-324">FormCollection</span><span class="sxs-lookup"><span data-stu-id="fbb06-324">FormCollection</span></span>

<span data-ttu-id="fbb06-325">Используется для извлечения всех значений из опубликованных данных формы.</span><span class="sxs-lookup"><span data-stu-id="fbb06-325">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="fbb06-326">Форматировщики входных данных</span><span class="sxs-lookup"><span data-stu-id="fbb06-326">Input formatters</span></span>

<span data-ttu-id="fbb06-327">Данные в тексте запроса могут быть в формате JSON, XML или другом формате.</span><span class="sxs-lookup"><span data-stu-id="fbb06-327">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="fbb06-328">Для анализа этих данных модель привязки использует *форматировщик входных данных*, настроенный для обработки определенного типа содержимого.</span><span class="sxs-lookup"><span data-stu-id="fbb06-328">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="fbb06-329">По умолчанию ASP.NET Core включает форматировщики входных данных на основе JSON для обработки данных JSON.</span><span class="sxs-lookup"><span data-stu-id="fbb06-329">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="fbb06-330">Вы можете добавить другие форматировщики для других типов содержимого.</span><span class="sxs-lookup"><span data-stu-id="fbb06-330">You can add other formatters for other content types.</span></span>

<span data-ttu-id="fbb06-331">ASP.NET Core выбирает форматировщики входных данных на основе атрибута [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="fbb06-331">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="fbb06-332">Если атрибут отсутствует, используется [Заголовок Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="fbb06-332">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="fbb06-333">Чтобы использовать встроенные форматировщики входных данных XML:</span><span class="sxs-lookup"><span data-stu-id="fbb06-333">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="fbb06-334">Установите пакет NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-334">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="fbb06-335">В `Startup.ConfigureServices` вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> или <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="fbb06-335">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="fbb06-336">Примените атрибут `Consumes` к классам контроллера или методам действий, которые должны ожидать XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-336">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="fbb06-337">Дополнительные сведения см. в разделе [Введение в сериализацию XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="fbb06-337">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="fbb06-338">Исключение указанных типов из привязки модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-338">Exclude specified types from model binding</span></span>

<span data-ttu-id="fbb06-339">Поведение привязки модели и системы проверки определяется классом [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="fbb06-339">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="fbb06-340">Вы можете настроить `ModelMetadata`, добавив поставщик сведений в [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="fbb06-340">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="fbb06-341">Встроенные поставщики сведений доступны для отключения привязки модели или проверки для указанных типов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-341">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="fbb06-342">Чтобы отключить привязку модели для всех моделей указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-342">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fbb06-343">Например, для отключения привязки модели для всех моделей типа `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-343">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="fbb06-344">Чтобы отключить проверку свойств указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-344">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fbb06-345">Например, чтобы отключить проверку по свойствам типа `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-345">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="fbb06-346">Настраиваемые связыватели модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-346">Custom model binders</span></span>

<span data-ttu-id="fbb06-347">Привязку модели можно расширить путем написания пользовательского связывателя модели и с помощью атрибута `[ModelBinder]`, чтобы выбрать его для заданного целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="fbb06-347">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="fbb06-348">Узнайте больше о [пользовательской привязке модели](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="fbb06-348">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="fbb06-349">Привязка модели вручную</span><span class="sxs-lookup"><span data-stu-id="fbb06-349">Manual model binding</span></span>

<span data-ttu-id="fbb06-350">Привязка модели может вызываться вручную с помощью метода <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="fbb06-350">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="fbb06-351">Этот метод определен в классах `ControllerBase` и `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-351">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="fbb06-352">Перегрузки метода позволяют задать поставщик префиксов и значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-352">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="fbb06-353">Этот метод возвращает `false` при сбое привязки модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-353">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="fbb06-354">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="fbb06-354">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="fbb06-355">Атрибут [FromServices]</span><span class="sxs-lookup"><span data-stu-id="fbb06-355">[FromServices] attribute</span></span>

<span data-ttu-id="fbb06-356">Имя этого атрибута соответствует шаблону атрибутов привязки модели, которые указывают источник данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-356">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="fbb06-357">Но это не связано с привязкой данных от поставщика значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-357">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="fbb06-358">Он получает экземпляр типа из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fbb06-358">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="fbb06-359">Он предназначен для предоставления альтернативы внедрению через конструктор, когда вам нужна служба, только если вызывается конкретный метод.</span><span class="sxs-lookup"><span data-stu-id="fbb06-359">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbb06-360">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fbb06-360">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fbb06-361">В этой статье объясняется, что такое привязка модели, как это работает и как настроить ее поведение.</span><span class="sxs-lookup"><span data-stu-id="fbb06-361">This article explains what model binding is, how it works, and how to customize its behavior.</span></span>

<span data-ttu-id="fbb06-362">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fbb06-362">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/model-binding/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="what-is-model-binding"></a><span data-ttu-id="fbb06-363">Что такое привязка модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-363">What is Model binding</span></span>

<span data-ttu-id="fbb06-364">Контроллеры и Razor Pages работают с данными, поступающими из HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-364">Controllers and Razor pages work with data that comes from HTTP requests.</span></span> <span data-ttu-id="fbb06-365">Например, данные о маршруте могут предоставлять ключ записи, а опубликованные поля формы могут предоставлять значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-365">For example, route data may provide a record key, and posted form fields may provide values for the properties of the model.</span></span> <span data-ttu-id="fbb06-366">Написание кода для получения этих значений и их преобразования из строк в типы .NET будет утомительной задачей с высоким риском ошибок.</span><span class="sxs-lookup"><span data-stu-id="fbb06-366">Writing code to retrieve each of these values and convert them from strings to .NET types would be tedious and error-prone.</span></span> <span data-ttu-id="fbb06-367">Привязка модели позволяет автоматизировать этот процесс.</span><span class="sxs-lookup"><span data-stu-id="fbb06-367">Model binding automates this process.</span></span> <span data-ttu-id="fbb06-368">Система привязки модели:</span><span class="sxs-lookup"><span data-stu-id="fbb06-368">The model binding system:</span></span>

* <span data-ttu-id="fbb06-369">Извлекает данные из различных источников, таких как данные о маршруте, поля формы и строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-369">Retrieves data from various sources such as route data, form fields, and query strings.</span></span>
* <span data-ttu-id="fbb06-370">Предоставляет данные для контроллеров и страниц Razor в параметрах метода и открытых свойствах.</span><span class="sxs-lookup"><span data-stu-id="fbb06-370">Provides the data to controllers and Razor pages in method parameters and public properties.</span></span>
* <span data-ttu-id="fbb06-371">Преобразует строковые данные в типы .NET.</span><span class="sxs-lookup"><span data-stu-id="fbb06-371">Converts string data to .NET types.</span></span>
* <span data-ttu-id="fbb06-372">Обновляет свойства сложных типов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-372">Updates properties of complex types.</span></span>

## <a name="example"></a><span data-ttu-id="fbb06-373">Пример</span><span class="sxs-lookup"><span data-stu-id="fbb06-373">Example</span></span>

<span data-ttu-id="fbb06-374">Предположим, у вас есть следующий метод действия:</span><span class="sxs-lookup"><span data-stu-id="fbb06-374">Suppose you have the following action method:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

<span data-ttu-id="fbb06-375">И приложение получает запрос с этим URL-адресом:</span><span class="sxs-lookup"><span data-stu-id="fbb06-375">And the app receives a request with this URL:</span></span>

```
http://contoso.com/api/pets/2?DogsOnly=true
```

<span data-ttu-id="fbb06-376">Привязка модели выполняет следующие действия, после того, как система маршрутизации выберет метод действия:</span><span class="sxs-lookup"><span data-stu-id="fbb06-376">Model binding goes through the following steps after the routing system selects the action method:</span></span>

* <span data-ttu-id="fbb06-377">Находит первый параметр `GetByID`, целое число с именем `id`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-377">Finds the first parameter of `GetByID`, an integer named `id`.</span></span>
* <span data-ttu-id="fbb06-378">Просматривает доступные источники в HTTP-запросе и находит `id` = "2" в данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="fbb06-378">Looks through the available sources in the HTTP request and finds `id` = "2" in route data.</span></span>
* <span data-ttu-id="fbb06-379">Преобразует строку "2" в целое число 2.</span><span class="sxs-lookup"><span data-stu-id="fbb06-379">Converts the string "2" into integer 2.</span></span>
* <span data-ttu-id="fbb06-380">Находит следующий параметр `GetByID`, логическое значение с именем `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-380">Finds the next parameter of `GetByID`, a boolean named `dogsOnly`.</span></span>
* <span data-ttu-id="fbb06-381">Просматривает источники и находит "DogsOnly=true" в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-381">Looks through the sources and finds "DogsOnly=true" in the query string.</span></span> <span data-ttu-id="fbb06-382">Сопоставление имен не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="fbb06-382">Name matching is not case-sensitive.</span></span>
* <span data-ttu-id="fbb06-383">Преобразует строку "true" в логическое значение `true`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-383">Converts the string "true" into boolean `true`.</span></span>

<span data-ttu-id="fbb06-384">Затем платформа вызывает метод `GetById`, передавая 2 для параметра `id` и `true` для параметра `dogsOnly`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-384">The framework then calls the `GetById` method, passing in 2 for the `id` parameter, and `true` for the `dogsOnly` parameter.</span></span>

<span data-ttu-id="fbb06-385">В приведенном выше примере целевые объекты привязки модели — это параметры методов, которые являются примитивными типами.</span><span class="sxs-lookup"><span data-stu-id="fbb06-385">In the preceding example, the model binding targets are method parameters that are simple types.</span></span> <span data-ttu-id="fbb06-386">Целевые объекты также могут быть свойствами сложного типа.</span><span class="sxs-lookup"><span data-stu-id="fbb06-386">Targets may also be the properties of a complex type.</span></span> <span data-ttu-id="fbb06-387">После успешной привязки каждого свойства осуществляется [проверка модели](xref:mvc/models/validation) для этого свойства.</span><span class="sxs-lookup"><span data-stu-id="fbb06-387">After each property is successfully bound, [model validation](xref:mvc/models/validation) occurs for that property.</span></span> <span data-ttu-id="fbb06-388">Записи о данных, привязанных к модели, а также об ошибках привязки или проверки хранятся в [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) или [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span><span class="sxs-lookup"><span data-stu-id="fbb06-388">The record of what data is bound to the model, and any binding or validation errors, is stored in [ControllerBase.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) or [PageModel.ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState).</span></span> <span data-ttu-id="fbb06-389">Чтобы узнать об успешном выполнении этого процесса, приложение проверяет наличие флага [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid).</span><span class="sxs-lookup"><span data-stu-id="fbb06-389">To find out if this process was successful, the app checks the [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) flag.</span></span>

## <a name="targets"></a><span data-ttu-id="fbb06-390">Целевые объекты</span><span class="sxs-lookup"><span data-stu-id="fbb06-390">Targets</span></span>

<span data-ttu-id="fbb06-391">Привязка модели попытается найти значения для следующих типов целевых объектов:</span><span class="sxs-lookup"><span data-stu-id="fbb06-391">Model binding tries to find values for the following kinds of targets:</span></span>

* <span data-ttu-id="fbb06-392">Параметры метода действия контроллера, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="fbb06-392">Parameters of the controller action method that a request is routed to.</span></span>
* <span data-ttu-id="fbb06-393">Параметры метода обработчика Razor Pages, к которому направлен запрос.</span><span class="sxs-lookup"><span data-stu-id="fbb06-393">Parameters of the Razor Pages handler method that a request is routed to.</span></span> 
* <span data-ttu-id="fbb06-394">Открытые свойства контроллера или класса `PageModel`, если задано атрибутами.</span><span class="sxs-lookup"><span data-stu-id="fbb06-394">Public properties of a controller or `PageModel` class, if specified by attributes.</span></span>

### <a name="bindproperty-attribute"></a><span data-ttu-id="fbb06-395">Атрибут [BindProperty]</span><span class="sxs-lookup"><span data-stu-id="fbb06-395">[BindProperty] attribute</span></span>

<span data-ttu-id="fbb06-396">Может применяться к открытому свойству контроллера или класса `PageModel` для привязки модели для этого свойства:</span><span class="sxs-lookup"><span data-stu-id="fbb06-396">Can be applied to a public property of a controller or `PageModel` class to cause model binding to target that property:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindpropertiesattribute"></a><span data-ttu-id="fbb06-397">Атрибут [BindProperties]</span><span class="sxs-lookup"><span data-stu-id="fbb06-397">[BindProperties] attribute</span></span>

<span data-ttu-id="fbb06-398">Доступно в ASP.NET Core 2.1 и более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="fbb06-398">Available in ASP.NET Core 2.1 and later.</span></span>  <span data-ttu-id="fbb06-399">Может применяться к контроллеру или классу `PageModel`, чтобы привязка модели была направлена на все открытые свойства этого класса:</span><span class="sxs-lookup"><span data-stu-id="fbb06-399">Can be applied to a controller or `PageModel` class to tell model binding to target all public properties of the class:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a><span data-ttu-id="fbb06-400">Привязка модели для HTTP-запросов GET</span><span class="sxs-lookup"><span data-stu-id="fbb06-400">Model binding for HTTP GET requests</span></span>

<span data-ttu-id="fbb06-401">По умолчанию свойства не привязываются к HTTP-запросам GET.</span><span class="sxs-lookup"><span data-stu-id="fbb06-401">By default, properties are not bound for HTTP GET requests.</span></span> <span data-ttu-id="fbb06-402">Как правило, для запроса GET вам нужен только параметр идентификатора записи.</span><span class="sxs-lookup"><span data-stu-id="fbb06-402">Typically, all you need for a GET request is a record ID parameter.</span></span> <span data-ttu-id="fbb06-403">Идентификатор записи используется для поиска элемента в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-403">The record ID is used to look up the item in the database.</span></span> <span data-ttu-id="fbb06-404">Поэтому не нужно привязывать свойство, которое содержит экземпляр модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-404">Therefore, there is no need to bind a property that holds an instance of the model.</span></span> <span data-ttu-id="fbb06-405">Если вы хотите привязать свойства к данным от запросов GET, задайте для свойства `SupportsGet` значение `true`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-405">In scenarios where you do want properties bound to data from GET requests, set the `SupportsGet` property to `true`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a><span data-ttu-id="fbb06-406">Источники</span><span class="sxs-lookup"><span data-stu-id="fbb06-406">Sources</span></span>

<span data-ttu-id="fbb06-407">По умолчанию привязка модели получает данные в виде пар "ключ-значение" из следующих источников в HTTP-запросе:</span><span class="sxs-lookup"><span data-stu-id="fbb06-407">By default, model binding gets data in the form of key-value pairs from the following sources in an HTTP request:</span></span>

1. <span data-ttu-id="fbb06-408">Поля формы</span><span class="sxs-lookup"><span data-stu-id="fbb06-408">Form fields</span></span>
1. <span data-ttu-id="fbb06-409">Текст запроса (для [контроллеров, имеющих атрибут [ApiController]](xref:web-api/index#binding-source-parameter-inference).)</span><span class="sxs-lookup"><span data-stu-id="fbb06-409">The request body (For [controllers that have the [ApiController] attribute](xref:web-api/index#binding-source-parameter-inference).)</span></span>
1. <span data-ttu-id="fbb06-410">Данные маршрута</span><span class="sxs-lookup"><span data-stu-id="fbb06-410">Route data</span></span>
1. <span data-ttu-id="fbb06-411">Параметры строки запроса</span><span class="sxs-lookup"><span data-stu-id="fbb06-411">Query string parameters</span></span>
1. <span data-ttu-id="fbb06-412">Отправленные файлы</span><span class="sxs-lookup"><span data-stu-id="fbb06-412">Uploaded files</span></span>

<span data-ttu-id="fbb06-413">Для каждого целевого параметра или свойства источники проверяются в порядке, указанном в предыдущем списке.</span><span class="sxs-lookup"><span data-stu-id="fbb06-413">For each target parameter or property, the sources are scanned in the order indicated in the preceding list.</span></span> <span data-ttu-id="fbb06-414">Существует несколько исключений:</span><span class="sxs-lookup"><span data-stu-id="fbb06-414">There are a few exceptions:</span></span>

* <span data-ttu-id="fbb06-415">Данные маршрутизации и значения строк запросов используются только для примитивных типов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-415">Route data and query string values are used only for simple types.</span></span>
* <span data-ttu-id="fbb06-416">Отправленные файлы привязаны только к типам целевых объектов, которые реализуют `IFormFile` или `IEnumerable<IFormFile>`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-416">Uploaded files are bound only to target types that implement `IFormFile` or `IEnumerable<IFormFile>`.</span></span>

<span data-ttu-id="fbb06-417">Если источник по умолчанию неверен, используйте один из следующих атрибутов, чтобы указать источник:</span><span class="sxs-lookup"><span data-stu-id="fbb06-417">If the default source is not correct, use one of the following attributes to specify the source:</span></span>

* <span data-ttu-id="fbb06-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute): возвращает значения из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-418">[`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) - Gets values from the query string.</span></span> 
* <span data-ttu-id="fbb06-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute): возвращает значения из данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="fbb06-419">[`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) - Gets values from route data.</span></span>
* <span data-ttu-id="fbb06-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute): возвращает значения из опубликованных полей формы.</span><span class="sxs-lookup"><span data-stu-id="fbb06-420">[`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) - Gets values from posted form fields.</span></span>
* <span data-ttu-id="fbb06-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute): возвращает значения из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-421">[`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) - Gets values from the request body.</span></span>
* <span data-ttu-id="fbb06-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute): возвращает значения из заголовков HTTP.</span><span class="sxs-lookup"><span data-stu-id="fbb06-422">[`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) - Gets values from HTTP headers.</span></span>

<span data-ttu-id="fbb06-423">Эти атрибуты:</span><span class="sxs-lookup"><span data-stu-id="fbb06-423">These attributes:</span></span>

* <span data-ttu-id="fbb06-424">Добавляются к свойствам модели по отдельности (не к классу модели), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="fbb06-424">Are added to model properties individually (not to the model class), as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* <span data-ttu-id="fbb06-425">При необходимости принимают значение имени модели в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="fbb06-425">Optionally accept a model name value in the constructor.</span></span> <span data-ttu-id="fbb06-426">Этот параметр предоставляется в том случае, если имя свойства не соответствует значению в запросе.</span><span class="sxs-lookup"><span data-stu-id="fbb06-426">This option is provided in case the property name doesn't match the value in the request.</span></span> <span data-ttu-id="fbb06-427">Например, значение в запросе может быть заголовком с дефисом в имени, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="fbb06-427">For instance, the value in the request might be a header with a hyphen in its name, as in the following example:</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a><span data-ttu-id="fbb06-428">Атрибут [FromBody]</span><span class="sxs-lookup"><span data-stu-id="fbb06-428">[FromBody] attribute</span></span>

<span data-ttu-id="fbb06-429">Примените атрибут `[FromBody]` к параметру, чтобы заполнить его свойства из тела HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-429">Apply the `[FromBody]` attribute to a parameter to populate its properties from the body of an HTTP request.</span></span> <span data-ttu-id="fbb06-430">Среда выполнения ASP.NET Core делегирует ответственность за считывание тела форматировщику входных данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-430">The ASP.NET Core runtime delegates the responsibility of reading the body to an input formatter.</span></span> <span data-ttu-id="fbb06-431">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="fbb06-431">Input formatters are explained [later in this article](#input-formatters).</span></span>

<span data-ttu-id="fbb06-432">При применении `[FromBody]` к параметру сложного типа все атрибуты источника привязки, применяемые к его свойствам, игнорируются.</span><span class="sxs-lookup"><span data-stu-id="fbb06-432">When `[FromBody]` is applied to a complex type parameter, any binding source attributes applied to its properties are ignored.</span></span> <span data-ttu-id="fbb06-433">Например, следующее действие `Create` указывает, что параметр `pet` заполняется из тела:</span><span class="sxs-lookup"><span data-stu-id="fbb06-433">For example, the following `Create` action specifies that its `pet` parameter is populated from the body:</span></span>

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

<span data-ttu-id="fbb06-434">Класс `Pet` указывает, что свойство `Breed` заполняется из параметра строки запроса:</span><span class="sxs-lookup"><span data-stu-id="fbb06-434">The `Pet` class specifies that its `Breed` property is populated from a query string parameter:</span></span>

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

<span data-ttu-id="fbb06-435">В предшествующем примере:</span><span class="sxs-lookup"><span data-stu-id="fbb06-435">In the preceding example:</span></span>

* <span data-ttu-id="fbb06-436">Атрибут `[FromQuery]` не учитывается.</span><span class="sxs-lookup"><span data-stu-id="fbb06-436">The `[FromQuery]` attribute is ignored.</span></span>
* <span data-ttu-id="fbb06-437">Свойство `Breed` не заполняется из параметра строки запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-437">The `Breed` property is not populated from a query string parameter.</span></span> 

<span data-ttu-id="fbb06-438">Форматировщики входных данных считывают только тело и не распознают атрибуты источника привязки.</span><span class="sxs-lookup"><span data-stu-id="fbb06-438">Input formatters read only the body and don't understand binding source attributes.</span></span> <span data-ttu-id="fbb06-439">Если подходящее значение найдено в теле, оно используется для заполнения свойства `Breed`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-439">If a suitable value is found in the body, that value is used to populate the `Breed` property.</span></span>

<span data-ttu-id="fbb06-440">Не применяют `[FromBody]` к нескольким параметрам в методе действия.</span><span class="sxs-lookup"><span data-stu-id="fbb06-440">Don't apply `[FromBody]` to more than one parameter per action method.</span></span> <span data-ttu-id="fbb06-441">После считывания потока запроса форматировщиком входных данных он больше не доступен для повторного чтения для привязки других параметров `[FromBody]`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-441">Once the request stream is read by an input formatter, it's no longer available to be read again for binding other `[FromBody]` parameters.</span></span>

### <a name="additional-sources"></a><span data-ttu-id="fbb06-442">Дополнительные источники</span><span class="sxs-lookup"><span data-stu-id="fbb06-442">Additional sources</span></span>

<span data-ttu-id="fbb06-443">Исходные данные предоставляются системой привязки модели *поставщиками значений*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-443">Source data is provided to the model binding system by *value providers*.</span></span> <span data-ttu-id="fbb06-444">Вы можете записать и зарегистрировать пользовательские поставщики значений, которые получают данные для привязки модели из других источников.</span><span class="sxs-lookup"><span data-stu-id="fbb06-444">You can write and register custom value providers that get data for model binding from other sources.</span></span> <span data-ttu-id="fbb06-445">Например, вам могут потребоваться данные из файлов cookie или состояния сеанса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-445">For example, you might want data from cookies or session state.</span></span> <span data-ttu-id="fbb06-446">Для получения данных из нового источника:</span><span class="sxs-lookup"><span data-stu-id="fbb06-446">To get data from a new source:</span></span>

* <span data-ttu-id="fbb06-447">Создайте класс, реализующий `IValueProvider`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-447">Create a class that implements `IValueProvider`.</span></span>
* <span data-ttu-id="fbb06-448">Создайте класс, реализующий `IValueProviderFactory`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-448">Create a class that implements `IValueProviderFactory`.</span></span>
* <span data-ttu-id="fbb06-449">Зарегистрируйте класс фабрики в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-449">Register the factory class in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="fbb06-450">Пример приложения включает пример [поставщика значений](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) и [фабрики](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs), которая получает значения из файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="fbb06-450">The sample app includes a [value provider](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) and [factory](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) example that gets values from cookies.</span></span> <span data-ttu-id="fbb06-451">Ниже приведен код регистрации в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-451">Here's the registration code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

<span data-ttu-id="fbb06-452">Этот код помещает поставщик пользовательских значений после всех встроенных поставщиков значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-452">The code shown puts the custom value provider after all the built-in value providers.</span></span>  <span data-ttu-id="fbb06-453">Чтобы сделать его первым в списке, вызовите `Insert(0, new CookieValueProviderFactory())` вместо `Add`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-453">To make it the first in the list, call `Insert(0, new CookieValueProviderFactory())` instead of `Add`.</span></span>

## <a name="no-source-for-a-model-property"></a><span data-ttu-id="fbb06-454">Отсутствие источника для свойства модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-454">No source for a model property</span></span>

<span data-ttu-id="fbb06-455">По умолчанию ошибка состояния модели не создается, если не найдено значение для свойства модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-455">By default, a model state error isn't created if no value is found for a model property.</span></span> <span data-ttu-id="fbb06-456">Свойство получает значение NULL или значение по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="fbb06-456">The property is set to null or a default value:</span></span>

* <span data-ttu-id="fbb06-457">Примитивным типам, допускающим значение NULL, задается значение `null`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-457">Nullable simple types are set to `null`.</span></span>
* <span data-ttu-id="fbb06-458">Типам значений, не допускающим значение NULL, задается значение `default(T)`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-458">Non-nullable value types are set to `default(T)`.</span></span> <span data-ttu-id="fbb06-459">Например, параметр `int id` получает значение 0.</span><span class="sxs-lookup"><span data-stu-id="fbb06-459">For example, a parameter `int id` is set to 0.</span></span>
* <span data-ttu-id="fbb06-460">Для сложных типов привязка модели создает экземпляр с помощью конструктора по умолчанию без задания свойств.</span><span class="sxs-lookup"><span data-stu-id="fbb06-460">For complex Types, model binding creates an instance by using the default constructor, without setting properties.</span></span>
* <span data-ttu-id="fbb06-461">Массивы имеют значение `Array.Empty<T>()`, за исключением массивов `byte[]`, которые имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-461">Arrays are set to `Array.Empty<T>()`, except that `byte[]` arrays are set to `null`.</span></span>

<span data-ttu-id="fbb06-462">Если состояние модели должно быть признано недействительным, когда в полях формы не найдены данные для свойства модели, используйте атрибут [`[BindRequired]`](#bindrequired-attribute).</span><span class="sxs-lookup"><span data-stu-id="fbb06-462">If model state should be invalidated when nothing is found in form fields for a model property, use the [`[BindRequired]`](#bindrequired-attribute) attribute.</span></span>

<span data-ttu-id="fbb06-463">Обратите внимание, это поведение `[BindRequired]` применяется к привязке модели из опубликованных данных формы, а не к данным JSON или XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-463">Note that this `[BindRequired]` behavior applies to model binding from posted form data, not to JSON or XML data in a request body.</span></span> <span data-ttu-id="fbb06-464">Данные основного текста запроса обрабатываются [форматировщиками входных данных](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="fbb06-464">Request body data is handled by [input formatters](#input-formatters).</span></span>

## <a name="type-conversion-errors"></a><span data-ttu-id="fbb06-465">Ошибки преобразования типа</span><span class="sxs-lookup"><span data-stu-id="fbb06-465">Type conversion errors</span></span>

<span data-ttu-id="fbb06-466">Если источник найден, но его нельзя преобразовать в тип целевого объекта, состояние модели помечается как недопустимое.</span><span class="sxs-lookup"><span data-stu-id="fbb06-466">If a source is found but can't be converted into the target type, model state is flagged as invalid.</span></span> <span data-ttu-id="fbb06-467">Параметр или свойство целевого объекта получает значение NULL или значение по умолчанию, как отмечалось в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="fbb06-467">The target parameter or property is set to null or a default value, as noted in the previous section.</span></span>

<span data-ttu-id="fbb06-468">В контроллере API с атрибутом `[ApiController]` недопустимое состояние модели приводит к автоматическому ответу HTTP 400.</span><span class="sxs-lookup"><span data-stu-id="fbb06-468">In an API controller that has the `[ApiController]` attribute, invalid model state results in an automatic HTTP 400 response.</span></span>

<span data-ttu-id="fbb06-469">На странице Razor повторно отображается страница с сообщением об ошибке:</span><span class="sxs-lookup"><span data-stu-id="fbb06-469">In a Razor page, redisplay the page with an error message:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

<span data-ttu-id="fbb06-470">Проверка на стороне клиента перехватывает большинство неверных данных, которые в противном случае были бы отправлены в форму Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fbb06-470">Client-side validation catches most bad data that would otherwise be submitted to a Razor Pages form.</span></span> <span data-ttu-id="fbb06-471">Эта проверка затрудняет срабатывание выделенного выше кода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-471">This validation makes it hard to trigger the preceding highlighted code.</span></span> <span data-ttu-id="fbb06-472">Пример приложения включает кнопку **Отправить с неверной датой**, которая помещает недопустимые данные в поле **Дата приема на работу** и отправляет форму.</span><span class="sxs-lookup"><span data-stu-id="fbb06-472">The sample app includes a **Submit with Invalid Date** button that puts bad data in the **Hire Date** field and submits the form.</span></span> <span data-ttu-id="fbb06-473">Эта кнопка показывает, как работает код для повторного отображения страницы, если возникла ошибка преобразования данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-473">This button shows how the code for redisplaying the page works when data conversion errors occur.</span></span>

<span data-ttu-id="fbb06-474">Когда страница отображается повторно приведенным выше кодом, недопустимые входные данные не отображаются в поле формы.</span><span class="sxs-lookup"><span data-stu-id="fbb06-474">When the page is redisplayed by the preceding code, the invalid input is not shown in the form field.</span></span> <span data-ttu-id="fbb06-475">Это связано с тем, что свойству модели задано значение NULL или значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fbb06-475">This is because the model property has been set to null or a default value.</span></span> <span data-ttu-id="fbb06-476">Недопустимые входные данные отображаются в сообщении об ошибке.</span><span class="sxs-lookup"><span data-stu-id="fbb06-476">The invalid input does appear in an error message.</span></span> <span data-ttu-id="fbb06-477">Но если требуется повторно отобразить неправильные данные в поле формы, возможно, следует сделать свойство модели строкой и выполнить преобразование данных вручную.</span><span class="sxs-lookup"><span data-stu-id="fbb06-477">But if you want to redisplay the bad data in the form field, consider making the model property a string and doing the data conversion manually.</span></span>

<span data-ttu-id="fbb06-478">Та же стратегия рекомендуется в том случае, если вы не хотите, чтобы ошибки преобразования типов приводили к ошибкам состояния модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-478">The same strategy is recommended if you don't want type conversion errors to result in model state errors.</span></span> <span data-ttu-id="fbb06-479">В этом случае следует сделать свойство модели строкой.</span><span class="sxs-lookup"><span data-stu-id="fbb06-479">In that case, make the model property a string.</span></span>

## <a name="simple-types"></a><span data-ttu-id="fbb06-480">Простые типы</span><span class="sxs-lookup"><span data-stu-id="fbb06-480">Simple types</span></span>

<span data-ttu-id="fbb06-481">Связыватель модели может преобразовать исходные строки в следующие примитивные типы:</span><span class="sxs-lookup"><span data-stu-id="fbb06-481">The simple types that the model binder can convert source strings into include the following:</span></span>

* [<span data-ttu-id="fbb06-482">Boolean</span><span class="sxs-lookup"><span data-stu-id="fbb06-482">Boolean</span></span>](xref:System.ComponentModel.BooleanConverter)
* <span data-ttu-id="fbb06-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span><span class="sxs-lookup"><span data-stu-id="fbb06-483">[Byte](xref:System.ComponentModel.ByteConverter), [SByte](xref:System.ComponentModel.SByteConverter)</span></span>
* [<span data-ttu-id="fbb06-484">Char</span><span class="sxs-lookup"><span data-stu-id="fbb06-484">Char</span></span>](xref:System.ComponentModel.CharConverter)
* [<span data-ttu-id="fbb06-485">DateTime</span><span class="sxs-lookup"><span data-stu-id="fbb06-485">DateTime</span></span>](xref:System.ComponentModel.DateTimeConverter)
* [<span data-ttu-id="fbb06-486">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="fbb06-486">DateTimeOffset</span></span>](xref:System.ComponentModel.DateTimeOffsetConverter)
* [<span data-ttu-id="fbb06-487">Decimal</span><span class="sxs-lookup"><span data-stu-id="fbb06-487">Decimal</span></span>](xref:System.ComponentModel.DecimalConverter)
* [<span data-ttu-id="fbb06-488">Double</span><span class="sxs-lookup"><span data-stu-id="fbb06-488">Double</span></span>](xref:System.ComponentModel.DoubleConverter)
* [<span data-ttu-id="fbb06-489">Enum</span><span class="sxs-lookup"><span data-stu-id="fbb06-489">Enum</span></span>](xref:System.ComponentModel.EnumConverter)
* [<span data-ttu-id="fbb06-490">Guid</span><span class="sxs-lookup"><span data-stu-id="fbb06-490">Guid</span></span>](xref:System.ComponentModel.GuidConverter)
* <span data-ttu-id="fbb06-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span><span class="sxs-lookup"><span data-stu-id="fbb06-491">[Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)</span></span>
* [<span data-ttu-id="fbb06-492">Single</span><span class="sxs-lookup"><span data-stu-id="fbb06-492">Single</span></span>](xref:System.ComponentModel.SingleConverter)
* [<span data-ttu-id="fbb06-493">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="fbb06-493">TimeSpan</span></span>](xref:System.ComponentModel.TimeSpanConverter)
* <span data-ttu-id="fbb06-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span><span class="sxs-lookup"><span data-stu-id="fbb06-494">[UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)</span></span>
* [<span data-ttu-id="fbb06-495">Uri</span><span class="sxs-lookup"><span data-stu-id="fbb06-495">Uri</span></span>](xref:System.UriTypeConverter)
* [<span data-ttu-id="fbb06-496">Version</span><span class="sxs-lookup"><span data-stu-id="fbb06-496">Version</span></span>](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a><span data-ttu-id="fbb06-497">Сложные типы</span><span class="sxs-lookup"><span data-stu-id="fbb06-497">Complex types</span></span>

<span data-ttu-id="fbb06-498">Сложный тип должен иметь открытый конструктор по умолчанию, а также открытые и доступные для записи свойства, подлежащие привязке.</span><span class="sxs-lookup"><span data-stu-id="fbb06-498">A complex type must have a public default constructor and public writable properties to bind.</span></span> <span data-ttu-id="fbb06-499">Когда происходит привязка модели, класс создается с помощью открытого конструктора по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fbb06-499">When model binding occurs, the class is instantiated using the public default constructor.</span></span> 

<span data-ttu-id="fbb06-500">Для каждого свойства сложного типа привязка модели ищет в источниках шаблон имени *prefix.property_name*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-500">For each property of the complex type, model binding looks through the sources for the name pattern *prefix.property_name*.</span></span> <span data-ttu-id="fbb06-501">Если ничего не найдено, он ищет только *property_name* без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-501">If nothing is found, it looks for just *property_name* without the prefix.</span></span>

<span data-ttu-id="fbb06-502">Для привязки к параметру префикс является именем параметра.</span><span class="sxs-lookup"><span data-stu-id="fbb06-502">For binding to a parameter, the prefix is the parameter name.</span></span> <span data-ttu-id="fbb06-503">Для привязки к открытому свойству `PageModel` префикс является именем открытого свойства.</span><span class="sxs-lookup"><span data-stu-id="fbb06-503">For binding to a `PageModel` public property, the prefix is the public property name.</span></span> <span data-ttu-id="fbb06-504">Некоторые атрибуты имеют свойство `Prefix`, которое позволяет переопределить использование по умолчанию для имени параметра или свойства.</span><span class="sxs-lookup"><span data-stu-id="fbb06-504">Some attributes have a `Prefix` property that lets you override the default usage of parameter or property name.</span></span>

<span data-ttu-id="fbb06-505">Предположим, например, что сложный тип принадлежит к следующему классу `Instructor`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-505">For example, suppose the complex type is the following `Instructor` class:</span></span>

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a><span data-ttu-id="fbb06-506">Префикс — это имя параметра</span><span class="sxs-lookup"><span data-stu-id="fbb06-506">Prefix = parameter name</span></span>

<span data-ttu-id="fbb06-507">Если модель, которую нужно привязать, является параметром с именем `instructorToUpdate`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-507">If the model to be bound is a parameter named `instructorToUpdate`:</span></span>

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

<span data-ttu-id="fbb06-508">Привязка модели начинается с поиска источников ключа `instructorToUpdate.ID`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-508">Model binding starts by looking through the sources for the key `instructorToUpdate.ID`.</span></span> <span data-ttu-id="fbb06-509">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-509">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="prefix--property-name"></a><span data-ttu-id="fbb06-510">Префикс — это имя свойства</span><span class="sxs-lookup"><span data-stu-id="fbb06-510">Prefix = property name</span></span>

<span data-ttu-id="fbb06-511">Если модель для привязки является свойством с именем `Instructor` контроллера или класса `PageModel`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-511">If the model to be bound is a property named `Instructor` of the controller or `PageModel` class:</span></span>

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="fbb06-512">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-512">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="fbb06-513">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-513">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="custom-prefix"></a><span data-ttu-id="fbb06-514">Пользовательский префикс</span><span class="sxs-lookup"><span data-stu-id="fbb06-514">Custom prefix</span></span>

<span data-ttu-id="fbb06-515">Если модель для привязки — это параметр с именем `instructorToUpdate`, а атрибут `Bind` задает `Instructor` как префикс:</span><span class="sxs-lookup"><span data-stu-id="fbb06-515">If the model to be bound is a parameter named `instructorToUpdate` and a `Bind` attribute specifies `Instructor` as the prefix:</span></span>

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

<span data-ttu-id="fbb06-516">Привязка модели начинается с поиска источников ключа `Instructor.ID`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-516">Model binding starts by looking through the sources for the key `Instructor.ID`.</span></span> <span data-ttu-id="fbb06-517">Если он не найден, ищется `ID` без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-517">If that isn't found, it looks for `ID` without a prefix.</span></span>

### <a name="attributes-for-complex-type-targets"></a><span data-ttu-id="fbb06-518">Атрибуты для целевых объектов сложного типа</span><span class="sxs-lookup"><span data-stu-id="fbb06-518">Attributes for complex type targets</span></span>

<span data-ttu-id="fbb06-519">Несколько встроенных атрибутов доступны для управления привязкой моделей сложных типов:</span><span class="sxs-lookup"><span data-stu-id="fbb06-519">Several built-in attributes are available for controlling model binding of complex types:</span></span>

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> <span data-ttu-id="fbb06-520">Эти атрибуты влияют на привязку модели, если опубликованные данные формы являются источником значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-520">These attributes affect model binding when posted form data is the source of values.</span></span> <span data-ttu-id="fbb06-521">Они не влияют на форматировщики входных данных, которые обрабатывают опубликованные тексты запросов JSON и XML.</span><span class="sxs-lookup"><span data-stu-id="fbb06-521">They do not affect input formatters, which process posted JSON and XML request bodies.</span></span> <span data-ttu-id="fbb06-522">Форматировщики входных данных описываются [далее в этой статье](#input-formatters).</span><span class="sxs-lookup"><span data-stu-id="fbb06-522">Input formatters are explained [later in this article](#input-formatters).</span></span>
>
> <span data-ttu-id="fbb06-523">Также см. обсуждение атрибута `[Required]` в разделе [Проверка модели](xref:mvc/models/validation#required-attribute).</span><span class="sxs-lookup"><span data-stu-id="fbb06-523">See also the discussion of the `[Required]` attribute in [Model validation](xref:mvc/models/validation#required-attribute).</span></span>

### <a name="bindrequired-attribute"></a><span data-ttu-id="fbb06-524">Атрибут [BindRequired]</span><span class="sxs-lookup"><span data-stu-id="fbb06-524">[BindRequired] attribute</span></span>

<span data-ttu-id="fbb06-525">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-525">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="fbb06-526">Приводит к тому, что привязка модели добавляет ошибку состояния модели, если привязка для свойства модели невозможна.</span><span class="sxs-lookup"><span data-stu-id="fbb06-526">Causes model binding to add a model state error if binding cannot occur for a model's property.</span></span> <span data-ttu-id="fbb06-527">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="fbb06-527">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a><span data-ttu-id="fbb06-528">Атрибут [BindNever]</span><span class="sxs-lookup"><span data-stu-id="fbb06-528">[BindNever] attribute</span></span>

<span data-ttu-id="fbb06-529">Может применяться только к свойствам модели, а не к параметрам метода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-529">Can only be applied to model properties, not to method parameters.</span></span> <span data-ttu-id="fbb06-530">Запрещает привязке модели задавать свойство модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-530">Prevents model binding from setting a model's property.</span></span> <span data-ttu-id="fbb06-531">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="fbb06-531">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a><span data-ttu-id="fbb06-532">Атрибут [Bind]</span><span class="sxs-lookup"><span data-stu-id="fbb06-532">[Bind] attribute</span></span>

<span data-ttu-id="fbb06-533">Может быть применен к классу или параметру метода.</span><span class="sxs-lookup"><span data-stu-id="fbb06-533">Can be applied to a class or a method parameter.</span></span> <span data-ttu-id="fbb06-534">Указывает, какие свойства модели должны быть включены в привязку модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-534">Specifies which properties of a model should be included in model binding.</span></span>

<span data-ttu-id="fbb06-535">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается любой метод действия или обработчик:</span><span class="sxs-lookup"><span data-stu-id="fbb06-535">In the following example, only the specified properties of the `Instructor` model are bound when any handler or action method is called:</span></span>

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

<span data-ttu-id="fbb06-536">В следующем примере только указанные свойства модели `Instructor` привязываются, когда вызывается метод `OnPost`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-536">In the following example, only the specified properties of the `Instructor` model are bound when the `OnPost` method is called:</span></span>

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

<span data-ttu-id="fbb06-537">Атрибут `[Bind]` может использоваться для защиты от чрезмерной передачи данных при *создании*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-537">The `[Bind]` attribute can be used to protect against overposting in *create* scenarios.</span></span> <span data-ttu-id="fbb06-538">Он не работает в сценариях редактирования, поскольку исключенным свойствам задается значение NULL или значение по умолчанию, но не оставляется значение без изменений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-538">It doesn't work well in edit scenarios because excluded properties are set to null or a default value instead of being left unchanged.</span></span> <span data-ttu-id="fbb06-539">Для защиты от чрезмерной передачи данных рекомендуется использовать модели представлений вместо атрибута `[Bind]`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-539">For defense against overposting, view models are recommended rather than the `[Bind]` attribute.</span></span> <span data-ttu-id="fbb06-540">Дополнительные сведения см. в разделе [Примечание по безопасности о чрезмерной передаче данных](xref:data/ef-mvc/crud#security-note-about-overposting).</span><span class="sxs-lookup"><span data-stu-id="fbb06-540">For more information, see [Security note about overposting](xref:data/ef-mvc/crud#security-note-about-overposting).</span></span>

## <a name="collections"></a><span data-ttu-id="fbb06-541">Коллекции</span><span class="sxs-lookup"><span data-stu-id="fbb06-541">Collections</span></span>

<span data-ttu-id="fbb06-542">Для целевых объектов, которые являются коллекциями примитивных типов, привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-542">For targets that are collections of simple types, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="fbb06-543">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-543">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="fbb06-544">Например:</span><span class="sxs-lookup"><span data-stu-id="fbb06-544">For example:</span></span>

* <span data-ttu-id="fbb06-545">Предположим, что параметром для привязки является массив с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-545">Suppose the parameter to be bound is an array named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* <span data-ttu-id="fbb06-546">Строковые данные формы или запроса могут иметь один из следующих форматов:</span><span class="sxs-lookup"><span data-stu-id="fbb06-546">Form or query string data can be in one of the following formats:</span></span>
   
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

* <span data-ttu-id="fbb06-547">Следующий формат поддерживается только в данных формы:</span><span class="sxs-lookup"><span data-stu-id="fbb06-547">The following format is supported only in form data:</span></span>

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* <span data-ttu-id="fbb06-548">Для всех перечисленных ранее форматов привязка модели передает массив из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-548">For all of the preceding example formats, model binding passes an array of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="fbb06-549">selectedCourses[0]=1050</span><span class="sxs-lookup"><span data-stu-id="fbb06-549">selectedCourses[0]=1050</span></span>
  * <span data-ttu-id="fbb06-550">selectedCourses[1]=2000</span><span class="sxs-lookup"><span data-stu-id="fbb06-550">selectedCourses[1]=2000</span></span>

  <span data-ttu-id="fbb06-551">Форматы данных, которые используют номера нижних индексов (... [0] ... [1] ...), должны нумероваться последовательно, начиная с нуля.</span><span class="sxs-lookup"><span data-stu-id="fbb06-551">Data formats that use subscript numbers (... [0] ... [1] ...) must ensure that they are numbered sequentially starting at zero.</span></span> <span data-ttu-id="fbb06-552">Если в нумерации есть пробелы, все элементы после пробела не учитываются.</span><span class="sxs-lookup"><span data-stu-id="fbb06-552">If there are any gaps in subscript numbering, all items after the gap are ignored.</span></span> <span data-ttu-id="fbb06-553">Например, если указаны индексы 0 и 2, а не 0 и 1, второй элемент игнорируется.</span><span class="sxs-lookup"><span data-stu-id="fbb06-553">For example, if the subscripts are 0 and 2 instead of 0 and 1, the second item is ignored.</span></span>

## <a name="dictionaries"></a><span data-ttu-id="fbb06-554">Словари</span><span class="sxs-lookup"><span data-stu-id="fbb06-554">Dictionaries</span></span>

<span data-ttu-id="fbb06-555">Для целевых объектов `Dictionary` привязка модели ищет совпадения с *parameter_name* или *property_name*.</span><span class="sxs-lookup"><span data-stu-id="fbb06-555">For `Dictionary` targets, model binding looks for matches to *parameter_name* or *property_name*.</span></span> <span data-ttu-id="fbb06-556">Если совпадений не найдено, она ищет один из поддерживаемых форматов без префикса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-556">If no match is found, it looks for one of the supported formats without the prefix.</span></span> <span data-ttu-id="fbb06-557">Например:</span><span class="sxs-lookup"><span data-stu-id="fbb06-557">For example:</span></span>

* <span data-ttu-id="fbb06-558">Предположим, что целевой параметр является `Dictionary<int, string>` с именем `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-558">Suppose the target parameter is a `Dictionary<int, string>` named `selectedCourses`:</span></span>

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* <span data-ttu-id="fbb06-559">Опубликованные строковые данные формы или запроса могут выглядеть как один из следующих примеров:</span><span class="sxs-lookup"><span data-stu-id="fbb06-559">The posted form or query string data can look like one of the following examples:</span></span>

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

* <span data-ttu-id="fbb06-560">Для всех перечисленных ранее форматов привязка модели передает словарь из двух элементов в параметр `selectedCourses`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-560">For all of the preceding example formats, model binding passes a dictionary of two items to the `selectedCourses` parameter:</span></span>

  * <span data-ttu-id="fbb06-561">selectedCourses["1050"]="Chemistry"</span><span class="sxs-lookup"><span data-stu-id="fbb06-561">selectedCourses["1050"]="Chemistry"</span></span>
  * <span data-ttu-id="fbb06-562">selectedCourses["2000"]="Economics"</span><span class="sxs-lookup"><span data-stu-id="fbb06-562">selectedCourses["2000"]="Economics"</span></span>

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a><span data-ttu-id="fbb06-563">Поведение глобализации для данных маршрутов привязки модели и строк запросов</span><span class="sxs-lookup"><span data-stu-id="fbb06-563">Globalization behavior of model binding route data and query strings</span></span>

<span data-ttu-id="fbb06-564">Поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fbb06-564">The ASP.NET Core route value provider and query string value provider:</span></span>

* <span data-ttu-id="fbb06-565">обрабатывают значения как имеющие инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="fbb06-565">Treat values as invariant culture.</span></span>
* <span data-ttu-id="fbb06-566">Следует ожидать, что URL-адреса имеют инвариантные язык и региональные параметры.</span><span class="sxs-lookup"><span data-stu-id="fbb06-566">Expect that URLs are culture-invariant.</span></span>

<span data-ttu-id="fbb06-567">Напротив, значения, поступающие из данных форм, подвергаются преобразованию с учетом языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="fbb06-567">In contrast, values coming from form data undergo a culture-sensitive conversion.</span></span> <span data-ttu-id="fbb06-568">Это сделано намеренно, чтобы URL-адреса были общими в разных языковых стандартах.</span><span class="sxs-lookup"><span data-stu-id="fbb06-568">This is by design so that URLs are shareable across locales.</span></span>

<span data-ttu-id="fbb06-569">Чтобы поставщик значений маршрутов и поставщик значений для строк запросов ASP.NET Core производили преобразование с учетом языка и региональных параметров, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="fbb06-569">To make the ASP.NET Core route value provider and query string value provider undergo a culture-sensitive conversion:</span></span>

* <span data-ttu-id="fbb06-570">наследуют от <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>.</span><span class="sxs-lookup"><span data-stu-id="fbb06-570">Inherit from <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory></span></span>
* <span data-ttu-id="fbb06-571">Скопируйте код из [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) или [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span><span class="sxs-lookup"><span data-stu-id="fbb06-571">Copy the code from [QueryStringValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) or [RouteValueValueProviderFactory](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)</span></span>
* <span data-ttu-id="fbb06-572">Замените [значение языка и региональных параметров](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30), передаваемое в конструктор поставщика значений, на [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span><span class="sxs-lookup"><span data-stu-id="fbb06-572">Replace the [culture value](https://github.com/aspnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) passed to the value provider constructor with [CultureInfo.CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)</span></span>
* <span data-ttu-id="fbb06-573">Замените метод производства поставщика значений по умолчанию в параметрах MVC на новый:</span><span class="sxs-lookup"><span data-stu-id="fbb06-573">Replace the default value provider factory in MVC options with your new one:</span></span>

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a><span data-ttu-id="fbb06-574">Специальные типы данных</span><span class="sxs-lookup"><span data-stu-id="fbb06-574">Special data types</span></span>

<span data-ttu-id="fbb06-575">Существуют некоторые особые типы данных, которые может обрабатывать привязка модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-575">There are some special data types that model binding can handle.</span></span>

### <a name="iformfile-and-iformfilecollection"></a><span data-ttu-id="fbb06-576">IFormFile и IFormFileCollection</span><span class="sxs-lookup"><span data-stu-id="fbb06-576">IFormFile and IFormFileCollection</span></span>

<span data-ttu-id="fbb06-577">Переданный файл, включенный в HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="fbb06-577">An uploaded file included in the HTTP request.</span></span>  <span data-ttu-id="fbb06-578">Также поддерживается `IEnumerable<IFormFile>` для нескольких файлов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-578">Also supported is `IEnumerable<IFormFile>` for multiple files.</span></span>

### <a name="cancellationtoken"></a><span data-ttu-id="fbb06-579">CancellationToken</span><span class="sxs-lookup"><span data-stu-id="fbb06-579">CancellationToken</span></span>

<span data-ttu-id="fbb06-580">используется для отмены действия в асинхронных контроллерах.</span><span class="sxs-lookup"><span data-stu-id="fbb06-580">Used to cancel activity in asynchronous controllers.</span></span>

### <a name="formcollection"></a><span data-ttu-id="fbb06-581">FormCollection</span><span class="sxs-lookup"><span data-stu-id="fbb06-581">FormCollection</span></span>

<span data-ttu-id="fbb06-582">Используется для извлечения всех значений из опубликованных данных формы.</span><span class="sxs-lookup"><span data-stu-id="fbb06-582">Used to retrieve all the values from posted form data.</span></span>

## <a name="input-formatters"></a><span data-ttu-id="fbb06-583">Форматировщики входных данных</span><span class="sxs-lookup"><span data-stu-id="fbb06-583">Input formatters</span></span>

<span data-ttu-id="fbb06-584">Данные в тексте запроса могут быть в формате JSON, XML или другом формате.</span><span class="sxs-lookup"><span data-stu-id="fbb06-584">Data in the request body can be in JSON, XML, or some other format.</span></span> <span data-ttu-id="fbb06-585">Для анализа этих данных модель привязки использует *форматировщик входных данных*, настроенный для обработки определенного типа содержимого.</span><span class="sxs-lookup"><span data-stu-id="fbb06-585">To parse this data, model binding uses an *input formatter* that is configured to handle a particular content type.</span></span> <span data-ttu-id="fbb06-586">По умолчанию ASP.NET Core включает форматировщики входных данных на основе JSON для обработки данных JSON.</span><span class="sxs-lookup"><span data-stu-id="fbb06-586">By default, ASP.NET Core includes JSON based input formatters for handling JSON data.</span></span> <span data-ttu-id="fbb06-587">Вы можете добавить другие форматировщики для других типов содержимого.</span><span class="sxs-lookup"><span data-stu-id="fbb06-587">You can add other formatters for other content types.</span></span>

<span data-ttu-id="fbb06-588">ASP.NET Core выбирает форматировщики входных данных на основе атрибута [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute).</span><span class="sxs-lookup"><span data-stu-id="fbb06-588">ASP.NET Core selects input formatters based on the [Consumes](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) attribute.</span></span> <span data-ttu-id="fbb06-589">Если атрибут отсутствует, используется [Заголовок Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span><span class="sxs-lookup"><span data-stu-id="fbb06-589">If no attribute is present, it uses the [Content-Type header](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).</span></span>

<span data-ttu-id="fbb06-590">Чтобы использовать встроенные форматировщики входных данных XML:</span><span class="sxs-lookup"><span data-stu-id="fbb06-590">To use the built-in XML input formatters:</span></span>

* <span data-ttu-id="fbb06-591">Установите пакет NuGet `Microsoft.AspNetCore.Mvc.Formatters.Xml`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-591">Install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

* <span data-ttu-id="fbb06-592">В `Startup.ConfigureServices` вызовите <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> или <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span><span class="sxs-lookup"><span data-stu-id="fbb06-592">In `Startup.ConfigureServices`, call <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> or <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*>.</span></span>

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* <span data-ttu-id="fbb06-593">Примените атрибут `Consumes` к классам контроллера или методам действий, которые должны ожидать XML в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="fbb06-593">Apply the `Consumes` attribute to controller classes or action methods that should expect XML in the request body.</span></span>

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  <span data-ttu-id="fbb06-594">Дополнительные сведения см. в разделе [Введение в сериализацию XML](/dotnet/standard/serialization/introducing-xml-serialization).</span><span class="sxs-lookup"><span data-stu-id="fbb06-594">For more information, see [Introducing XML Serialization](/dotnet/standard/serialization/introducing-xml-serialization).</span></span>

## <a name="exclude-specified-types-from-model-binding"></a><span data-ttu-id="fbb06-595">Исключение указанных типов из привязки модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-595">Exclude specified types from model binding</span></span>

<span data-ttu-id="fbb06-596">Поведение привязки модели и системы проверки определяется классом [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span><span class="sxs-lookup"><span data-stu-id="fbb06-596">The model binding and validation systems' behavior is driven by [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata).</span></span> <span data-ttu-id="fbb06-597">Вы можете настроить `ModelMetadata`, добавив поставщик сведений в [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span><span class="sxs-lookup"><span data-stu-id="fbb06-597">You can customize `ModelMetadata` by adding a details provider to [MvcOptions.ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders).</span></span> <span data-ttu-id="fbb06-598">Встроенные поставщики сведений доступны для отключения привязки модели или проверки для указанных типов.</span><span class="sxs-lookup"><span data-stu-id="fbb06-598">Built-in details providers are available for disabling model binding or validation for specified types.</span></span>

<span data-ttu-id="fbb06-599">Чтобы отключить привязку модели для всех моделей указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-599">To disable model binding on all models of a specified type, add an <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fbb06-600">Например, для отключения привязки модели для всех моделей типа `System.Version`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-600">For example, to disable model binding on all models of type `System.Version`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

<span data-ttu-id="fbb06-601">Чтобы отключить проверку свойств указанного типа, добавьте <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-601">To disable validation on properties of a specified type, add a <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fbb06-602">Например, чтобы отключить проверку по свойствам типа `System.Guid`:</span><span class="sxs-lookup"><span data-stu-id="fbb06-602">For example, to disable validation on properties of type `System.Guid`:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a><span data-ttu-id="fbb06-603">Настраиваемые связыватели модели</span><span class="sxs-lookup"><span data-stu-id="fbb06-603">Custom model binders</span></span>

<span data-ttu-id="fbb06-604">Привязку модели можно расширить путем написания пользовательского связывателя модели и с помощью атрибута `[ModelBinder]`, чтобы выбрать его для заданного целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="fbb06-604">You can extend model binding by writing a custom model binder and using the `[ModelBinder]` attribute to select it for a given target.</span></span> <span data-ttu-id="fbb06-605">Узнайте больше о [пользовательской привязке модели](xref:mvc/advanced/custom-model-binding).</span><span class="sxs-lookup"><span data-stu-id="fbb06-605">Learn more about [custom model binding](xref:mvc/advanced/custom-model-binding).</span></span>

## <a name="manual-model-binding"></a><span data-ttu-id="fbb06-606">Привязка модели вручную</span><span class="sxs-lookup"><span data-stu-id="fbb06-606">Manual model binding</span></span>

<span data-ttu-id="fbb06-607">Привязка модели может вызываться вручную с помощью метода <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>.</span><span class="sxs-lookup"><span data-stu-id="fbb06-607">Model binding can be invoked manually by using the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> method.</span></span> <span data-ttu-id="fbb06-608">Этот метод определен в классах `ControllerBase` и `PageModel`.</span><span class="sxs-lookup"><span data-stu-id="fbb06-608">The method is defined on both `ControllerBase` and `PageModel` classes.</span></span> <span data-ttu-id="fbb06-609">Перегрузки метода позволяют задать поставщик префиксов и значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-609">Method overloads let you specify the prefix and value provider to use.</span></span> <span data-ttu-id="fbb06-610">Этот метод возвращает `false` при сбое привязки модели.</span><span class="sxs-lookup"><span data-stu-id="fbb06-610">The method returns `false` if model binding fails.</span></span> <span data-ttu-id="fbb06-611">Ниже приведен пример:</span><span class="sxs-lookup"><span data-stu-id="fbb06-611">Here's an example:</span></span>

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a><span data-ttu-id="fbb06-612">Атрибут [FromServices]</span><span class="sxs-lookup"><span data-stu-id="fbb06-612">[FromServices] attribute</span></span>

<span data-ttu-id="fbb06-613">Имя этого атрибута соответствует шаблону атрибутов привязки модели, которые указывают источник данных.</span><span class="sxs-lookup"><span data-stu-id="fbb06-613">This attribute's name follows the pattern of model binding attributes that specify a data source.</span></span> <span data-ttu-id="fbb06-614">Но это не связано с привязкой данных от поставщика значений.</span><span class="sxs-lookup"><span data-stu-id="fbb06-614">But it's not about binding data from a value provider.</span></span> <span data-ttu-id="fbb06-615">Он получает экземпляр типа из контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fbb06-615">It gets an instance of a type from the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="fbb06-616">Он предназначен для предоставления альтернативы внедрению через конструктор, когда вам нужна служба, только если вызывается конкретный метод.</span><span class="sxs-lookup"><span data-stu-id="fbb06-616">Its purpose is to provide an alternative to constructor injection for when you need a service only if a particular method is called.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbb06-617">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fbb06-617">Additional resources</span></span>

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
