[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="3fd61-101">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="3fd61-101">The preceding code:</span></span>

* <span data-ttu-id="3fd61-102">определяет пустой класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="3fd61-102">Defines an empty controller class.</span></span> <span data-ttu-id="3fd61-103">В следующих разделах добавляются методы для реализации API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="3fd61-103">In the next sections, methods are added to implement the API.</span></span>
* <span data-ttu-id="3fd61-104">Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`TodoContext `) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="3fd61-104">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="3fd61-105">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="3fd61-105">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="3fd61-106">Конструктор добавляет элемент в базу данных в памяти, если он не существует.</span><span class="sxs-lookup"><span data-stu-id="3fd61-106">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="3fd61-107">Получение элементов задачи</span><span class="sxs-lookup"><span data-stu-id="3fd61-107">Getting to-do items</span></span>

<span data-ttu-id="3fd61-108">Чтобы получить элементы задачи, добавьте к классу `TodoController` следующие методы.</span><span class="sxs-lookup"><span data-stu-id="3fd61-108">To get to-do items, add the following methods to the `TodoController` class.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="3fd61-109">Эти методы реализуют два метода GET:</span><span class="sxs-lookup"><span data-stu-id="3fd61-109">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="3fd61-110">Ниже приведен пример ответа HTTP для метода `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="3fd61-110">Here is an example HTTP response for the `GetAll` method:</span></span>

```
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
   ```

<span data-ttu-id="3fd61-111">Далее в этом руководстве мы покажем, как можно просмотреть ответ HTTP с использованием [Postman](https://www.getpostman.com/) или [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="3fd61-111">Later in the tutorial I'll show how the HTTP response can be viewed with [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="3fd61-112">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="3fd61-112">Routing and URL paths</span></span>

<span data-ttu-id="3fd61-113">Атрибут `[HttpGet]` задает метод HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3fd61-113">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="3fd61-114">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3fd61-114">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="3fd61-115">Принимают строку шаблона в атрибуте `Route` контроллера:</span><span class="sxs-lookup"><span data-stu-id="3fd61-115">Take the template string in the controller’s `Route` attribute:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="3fd61-116">Замените `[controller]` именем контроллера (имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="3fd61-116">Replace `[controller]` with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="3fd61-117">В этом примере класс контроллера носит имя **Todo**Controller, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="3fd61-117">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="3fd61-118">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="3fd61-118">ASP.NET Core [routing](xref:mvc/controllers/routing) isn't case sensitive.</span></span>
* <span data-ttu-id="3fd61-119">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("/products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="3fd61-119">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="3fd61-120">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="3fd61-120">This sample doesn't use a template.</span></span> <span data-ttu-id="3fd61-121">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="3fd61-121">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="3fd61-122">В методе `GetById`:</span><span class="sxs-lookup"><span data-stu-id="3fd61-122">In the `GetById` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="3fd61-123">`"{id}"` — это переменная-заполнитель для идентификатора элемента `todo`.</span><span class="sxs-lookup"><span data-stu-id="3fd61-123">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="3fd61-124">При вызове `GetById` параметру метода `id` присваивается значение "{id}" в URL.</span><span class="sxs-lookup"><span data-stu-id="3fd61-124">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="3fd61-125">`Name = "GetTodo"` создает именованный маршрут.</span><span class="sxs-lookup"><span data-stu-id="3fd61-125">`Name = "GetTodo"` creates a named route.</span></span> <span data-ttu-id="3fd61-126">Именованные маршруты:</span><span class="sxs-lookup"><span data-stu-id="3fd61-126">Named routes:</span></span>

* <span data-ttu-id="3fd61-127">Предоставляют приложению возможность создать ссылку HTTP, используя имя маршрута.</span><span class="sxs-lookup"><span data-stu-id="3fd61-127">Enable the app to create an HTTP link using the route name.</span></span>
* <span data-ttu-id="3fd61-128">Описываются далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="3fd61-128">Are explained later in the tutorial.</span></span>

### <a name="return-values"></a><span data-ttu-id="3fd61-129">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="3fd61-129">Return values</span></span>

<span data-ttu-id="3fd61-130">Метод `GetAll` возвращает значение `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="3fd61-130">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="3fd61-131">MVC автоматически сериализует объект в формат [JSON](http://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="3fd61-131">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="3fd61-132">Код ответа для этого метода равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="3fd61-132">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="3fd61-133">(Необработанные исключения преобразуются в ошибки 5xx.)</span><span class="sxs-lookup"><span data-stu-id="3fd61-133">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="3fd61-134">Напротив, метод `GetById` возвращает более общий тип `IActionResult`, который представляет широкий диапазон типов возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="3fd61-134">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="3fd61-135">`GetById` имеет два разных типа возвращаемых значений:</span><span class="sxs-lookup"><span data-stu-id="3fd61-135">`GetById` has two different return types:</span></span>

* <span data-ttu-id="3fd61-136">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="3fd61-136">If no item matches the requested ID, the method returns a 404 error.</span></span> <span data-ttu-id="3fd61-137">При возвращении `NotFound` возвращается ответ HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="3fd61-137">Returning `NotFound` returns an HTTP 404 response.</span></span>

* <span data-ttu-id="3fd61-138">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="3fd61-138">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="3fd61-139">При возвращении `ObjectResult` возвращается ответ HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="3fd61-139">Returning `ObjectResult` returns an HTTP 200 response.</span></span>
