::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="71123-101">В предыдущем коде определяется класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="71123-101">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="71123-102">В следующих разделах добавляются методы для реализации API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="71123-102">In the next sections, methods are added to implement the API.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="71123-103">В предыдущем коде определяется класс контроллера API без методов.</span><span class="sxs-lookup"><span data-stu-id="71123-103">The preceding code defines an API controller class without methods.</span></span> <span data-ttu-id="71123-104">В следующих разделах добавляются методы для реализации API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="71123-104">In the next sections, methods are added to implement the API.</span></span> <span data-ttu-id="71123-105">Класс помечается атрибутом `[ApiController]` для включения некоторых удобных функций.</span><span class="sxs-lookup"><span data-stu-id="71123-105">The class is annotated with an `[ApiController]` attribute to enable some convenient features.</span></span> <span data-ttu-id="71123-106">Сведения о функциях, включаемых этим атрибутом, см. в разделе [Аннотирование класса атрибутом ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="71123-106">For information on features enabled by the attribute, see [Annotate class with ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).</span></span>
::: moniker-end

<span data-ttu-id="71123-107">Конструктор контроллера применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`TodoContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="71123-107">The controller's constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="71123-108">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="71123-108">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span> <span data-ttu-id="71123-109">Конструктор добавляет элемент в базу данных в памяти, если он не существует.</span><span class="sxs-lookup"><span data-stu-id="71123-109">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="get-to-do-items"></a><span data-ttu-id="71123-110">Получение элементов задачи</span><span class="sxs-lookup"><span data-stu-id="71123-110">Get to-do items</span></span>

<span data-ttu-id="71123-111">Чтобы получить элементы задачи, добавьте к классу `TodoController` следующие методы:</span><span class="sxs-lookup"><span data-stu-id="71123-111">To get to-do items, add the following methods to the `TodoController` class:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

<span data-ttu-id="71123-112">Эти методы реализуют два метода GET:</span><span class="sxs-lookup"><span data-stu-id="71123-112">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="71123-113">Ниже приведен пример ответа HTTP для метода `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="71123-113">Here's a sample HTTP response for the `GetAll` method:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

<span data-ttu-id="71123-114">Далее в этом руководстве мы покажем, как можно просмотреть ответ HTTP с использованием [Postman](https://www.getpostman.com/) или [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="71123-114">Later in the tutorial, I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="71123-115">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="71123-115">Routing and URL paths</span></span>

<span data-ttu-id="71123-116">Атрибут `[HttpGet]` обозначает метод, который отвечает на запрос HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="71123-116">The `[HttpGet]` attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="71123-117">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="71123-117">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="71123-118">Принимается строка шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="71123-118">Take the template string in the controller's `Route` attribute:</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* <span data-ttu-id="71123-119">Замените `[controller]` именем контроллера (имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="71123-119">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="71123-120">В этом примере класс контроллера носит имя **Todo**Controller, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="71123-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="71123-121">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="71123-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="71123-122">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("/products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="71123-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="71123-123">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="71123-123">This sample doesn't use a template.</span></span> <span data-ttu-id="71123-124">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="71123-124">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="71123-125">В следующем методе `GetById` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи.</span><span class="sxs-lookup"><span data-stu-id="71123-125">In the following `GetById` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="71123-126">При вызове `GetById` параметру метода `id` присваивается значение `"{id}"` в URL.</span><span class="sxs-lookup"><span data-stu-id="71123-126">When `GetById` is invoked, it assigns the value of `"{id}"` in the URL to the method's `id` parameter.</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

<span data-ttu-id="71123-127">`Name = "GetTodo"` создает именованный маршрут.</span><span class="sxs-lookup"><span data-stu-id="71123-127">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="71123-128">Именованные маршруты:</span><span class="sxs-lookup"><span data-stu-id="71123-128">Named routes:</span></span>

* <span data-ttu-id="71123-129">Предоставляют приложению возможность создать ссылку HTTP, используя имя маршрута.</span><span class="sxs-lookup"><span data-stu-id="71123-129">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="71123-130">Описываются далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="71123-130">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="71123-131">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="71123-131">Return values</span></span>

<span data-ttu-id="71123-132">Метод `GetAll` возвращает коллекцию объектов `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="71123-132">The `GetAll` method returns a collection of `TodoItem` objects.</span></span> <span data-ttu-id="71123-133">MVC автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="71123-133">MVC automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="71123-134">Код ответа для этого метода равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="71123-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="71123-135">Необработанные исключения преобразуются в ошибки 5xx.</span><span class="sxs-lookup"><span data-stu-id="71123-135">Unhandled exceptions are translated into 5xx errors.</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="71123-136">Напротив, метод `GetById` возвращает более общий тип [IActionResult type](xref:web-api/action-return-types#iactionresult-type), который представляет широкий диапазон типов возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="71123-136">In contrast, the `GetById` method returns the more general [IActionResult type](xref:web-api/action-return-types#iactionresult-type), which represents a wide range of return types.</span></span> <span data-ttu-id="71123-137">`GetById` имеет два разных типа возвращаемых значений:</span><span class="sxs-lookup"><span data-stu-id="71123-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="71123-138">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="71123-138">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="71123-139">При возвращении [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) возвращается ответ HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="71123-139">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="71123-140">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="71123-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="71123-141">При возвращении [ОК](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="71123-141">Returning [Ok](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) results in an HTTP 200 response.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="71123-142">Напротив, метод `GetById` возвращает тип [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type), который представляет широкий диапазон типов возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="71123-142">In contrast, the `GetById` method returns the [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type), which represents a wide range of return types.</span></span> <span data-ttu-id="71123-143">`GetById` имеет два разных типа возвращаемых значений:</span><span class="sxs-lookup"><span data-stu-id="71123-143">`GetById` has two different return types:</span></span>

* <span data-ttu-id="71123-144">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="71123-144">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="71123-145">При возвращении [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) возвращается ответ HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="71123-145">Returning [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) returns an HTTP 404 response.</span></span>
* <span data-ttu-id="71123-146">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="71123-146">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="71123-147">При возвращении `item` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="71123-147">Returning `item` results in an HTTP 200 response.</span></span>
::: moniker-end