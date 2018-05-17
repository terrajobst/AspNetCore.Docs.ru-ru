::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="db43a-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="db43a-101">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="db43a-102">В предыдущем коде определяется класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="db43a-102">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="db43a-103">В следующих разделах добавляются методы для реализации API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="db43a-103">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="db43a-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="db43a-104">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="db43a-105">В предыдущем коде определяется класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="db43a-105">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="db43a-106">В следующих разделах добавляются методы для реализации API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="db43a-106">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="db43a-107">Класс помечается атрибутом `[ApiController]` для включения некоторых удобных функций.</span><span class="sxs-lookup"><span data-stu-id="db43a-107">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="db43a-108">Сведения о функциях, включаемых этим атрибутом, см. в разделе [Аннотирование класса атрибутом ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="db43a-108">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="db43a-109">Конструктор контроллера применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="db43a-109">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="db43a-110">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="db43a-110">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="db43a-111">Конструктор добавляет элемент в базу данных в памяти, если он не существует.</span><span class="sxs-lookup"><span data-stu-id="db43a-111">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="db43a-112">Получение элементов задачи</span><span class="sxs-lookup"><span data-stu-id="db43a-112">Get to-do items</span></span>

<span data-ttu-id="db43a-113">Чтобы получить элементы задачи, добавьте к классу `TodoController` следующие методы:</span><span class="sxs-lookup"><span data-stu-id="db43a-113">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="db43a-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="db43a-114">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="db43a-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="db43a-115">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>
::: moniker-end

<span data-ttu-id="db43a-116">Эти методы реализуют два метода GET:</span><span class="sxs-lookup"><span data-stu-id="db43a-116">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="db43a-117">Ниже приведен пример ответа HTTP для метода `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="db43a-117">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="db43a-118">Далее в этом руководстве мы покажем, как можно просмотреть ответ HTTP с использованием [Postman](https://www.getpostman.com/) или [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="db43a-118">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="db43a-119">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="db43a-119">Routing and URL paths</span></span>

<span data-ttu-id="db43a-120">Атрибут `[HttpGet]` обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="db43a-120">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="db43a-121">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="db43a-121">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="db43a-122">Принимается строка шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="db43a-122">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="db43a-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="db43a-123">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="db43a-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="db43a-124">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>
::: moniker-end

* <span data-ttu-id="db43a-125">Замените `[controller]` именем контроллера (имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="db43a-125">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="db43a-126">В этом примере класс контроллера носит имя **Todo**Controller, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="db43a-126">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="db43a-127">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="db43a-127">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="db43a-128">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("/products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="db43a-128">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="db43a-129">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="db43a-129">This sample doesn't use a template.</span></span> <span data-ttu-id="db43a-130">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="db43a-130">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="db43a-131">В следующем методе `GetById` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="db43a-131">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="db43a-132">При вызове `GetById` параметру метода `id` присваивается значение `"{id}"` в URL.</span><span class="sxs-lookup"><span data-stu-id="db43a-132">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="db43a-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="db43a-133">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="db43a-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="db43a-134">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

<span data-ttu-id="db43a-135">`Name = "GetTodo"` создает именованный маршрут.</span><span class="sxs-lookup"><span data-stu-id="db43a-135">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="db43a-136">Именованные маршруты:</span><span class="sxs-lookup"><span data-stu-id="db43a-136">Named routes:</span></span>

* <span data-ttu-id="db43a-137">Предоставляют приложению возможность создать ссылку HTTP, используя имя маршрута.</span><span class="sxs-lookup"><span data-stu-id="db43a-137">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="db43a-138">Описываются далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="db43a-138">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="db43a-139">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="db43a-139">Return values</span></span>

<span data-ttu-id="db43a-140">Метод `GetAll` возвращает коллекцию объектов `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="db43a-140">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="db43a-141">MVC автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="db43a-141">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="db43a-142">Код ответа для этого метода равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="db43a-142">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="db43a-143">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="db43a-143">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="db43a-144">Напротив, метод `GetById` возвращает более общий тип [IActionResult type](xref:web-api/action-return-types#iactionresult-type), который представляет широкий диапазон типов возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="db43a-144">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="db43a-145">`GetById` имеет два разных типа возвращаемых значений:</span><span class="sxs-lookup"><span data-stu-id="db43a-145">`GetById` has two different return types:</span></span>

* <span data-ttu-id="db43a-146">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="db43a-146">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="db43a-147">При возвращении [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) возвращается ответ HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="db43a-147">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="db43a-148">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="db43a-148">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="db43a-149">При возвращении [ОК](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="db43a-149">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="db43a-150">Напротив, метод `GetById` возвращает тип [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type), который представляет широкий диапазон типов возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="db43a-150">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="db43a-151">`GetById` имеет два разных типа возвращаемых значений:</span><span class="sxs-lookup"><span data-stu-id="db43a-151">`GetById` has two different return types:</span></span>

* <span data-ttu-id="db43a-152">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="db43a-152">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="db43a-153">При возвращении [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) возвращается ответ HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="db43a-153">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="db43a-154">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="db43a-154">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="db43a-155">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="db43a-155">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end