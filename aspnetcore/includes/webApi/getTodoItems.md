<span data-ttu-id="43cf1-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span><span class="sxs-lookup"><span data-stu-id="43cf1-101">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]</span></span>

<span data-ttu-id="43cf1-102">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="43cf1-102">The preceding code:</span></span>

* <span data-ttu-id="43cf1-103">определяет пустой класс контроллера.</span><span class="sxs-lookup"><span data-stu-id="43cf1-103">Defines an empty controller class.</span></span> <span data-ttu-id="43cf1-104">В следующих разделах мы добавим методы для реализации API-интерфейса.</span><span class="sxs-lookup"><span data-stu-id="43cf1-104">In the next sections, we'll add methods to implement the API.</span></span>
* <span data-ttu-id="43cf1-105">Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`TodoContext `) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="43cf1-105">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`TodoContext `) into the controller.</span></span> <span data-ttu-id="43cf1-106">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="43cf1-106">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="43cf1-107">Конструктор добавляет элемент в базу данных в памяти, если он не существует.</span><span class="sxs-lookup"><span data-stu-id="43cf1-107">The constructor adds an item to the in-memory database if one doesn't exist.</span></span>

## <a name="getting-to-do-items"></a><span data-ttu-id="43cf1-108">Получение элементов задачи</span><span class="sxs-lookup"><span data-stu-id="43cf1-108">Getting to-do items</span></span>

<span data-ttu-id="43cf1-109">Чтобы получить элементы задачи, добавьте к классу `TodoController` следующие методы.</span><span class="sxs-lookup"><span data-stu-id="43cf1-109">To get to-do items, add the following methods to the `TodoController` class.</span></span>

<span data-ttu-id="43cf1-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span><span class="sxs-lookup"><span data-stu-id="43cf1-110">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]</span></span>

<span data-ttu-id="43cf1-111">Эти методы реализуют два метода GET:</span><span class="sxs-lookup"><span data-stu-id="43cf1-111">These methods implement the two GET methods:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="43cf1-112">Ниже приведен пример ответа HTTP для метода `GetAll`:</span><span class="sxs-lookup"><span data-stu-id="43cf1-112">Here is an example HTTP response for the `GetAll` method:</span></span>

```
HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8
   Server: Microsoft-IIS/10.0
   Date: Thu, 18 Jun 2015 20:51:10 GMT
   Content-Length: 82

   [{"Key":"1", "Name":"Item1","IsComplete":false}]
   ```

<span data-ttu-id="43cf1-113">Далее в этом руководстве мы покажем, как можно просмотреть ответ HTTP с использованием [Postman](https://www.getpostman.com/) или [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span><span class="sxs-lookup"><span data-stu-id="43cf1-113">Later in the tutorial I'll show how you can view the HTTP response using [Postman](https://www.getpostman.com/) or [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).</span></span>

### <a name="routing-and-url-paths"></a><span data-ttu-id="43cf1-114">Маршрутизация и пути URL</span><span class="sxs-lookup"><span data-stu-id="43cf1-114">Routing and URL paths</span></span>

<span data-ttu-id="43cf1-115">Атрибут `[HttpGet]` задает метод HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="43cf1-115">The `[HttpGet]` attribute specifies an HTTP GET method.</span></span> <span data-ttu-id="43cf1-116">Путь URL для каждого метода формируется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="43cf1-116">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="43cf1-117">Принимают строку шаблона в атрибуте маршрута контроллера:</span><span class="sxs-lookup"><span data-stu-id="43cf1-117">Take the template string in the controller’s route attribute:</span></span>

<span data-ttu-id="43cf1-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="43cf1-118">[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]</span></span>

* <span data-ttu-id="43cf1-119">Замените "[Controller]" именем контроллера (имя класса контроллера без суффикса "Controller").</span><span class="sxs-lookup"><span data-stu-id="43cf1-119">Replace "[Controller]" with the name of the controller, which is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="43cf1-120">В этом примере класс контроллера носит имя **Todo**Controller, а сам контроллер, соответственно, — "todo".</span><span class="sxs-lookup"><span data-stu-id="43cf1-120">For this sample, the controller class name is **Todo**Controller and the root name is "todo".</span></span> <span data-ttu-id="43cf1-121">В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.</span><span class="sxs-lookup"><span data-stu-id="43cf1-121">ASP.NET Core [routing](xref:mvc/controllers/routing) is not case sensitive.</span></span>
* <span data-ttu-id="43cf1-122">Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("/products")]`), добавьте его к пути.</span><span class="sxs-lookup"><span data-stu-id="43cf1-122">If the `[HttpGet]` attribute has a route template (such as `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="43cf1-123">В этом примере шаблон не используется.</span><span class="sxs-lookup"><span data-stu-id="43cf1-123">This sample doesn't use a template.</span></span> <span data-ttu-id="43cf1-124">Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="43cf1-124">See [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes) for more information.</span></span>

<span data-ttu-id="43cf1-125">В методе `GetById`:</span><span class="sxs-lookup"><span data-stu-id="43cf1-125">In the `GetById` method:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

<span data-ttu-id="43cf1-126">`"{id}"` — это переменная-заполнитель для идентификатора элемента `todo`.</span><span class="sxs-lookup"><span data-stu-id="43cf1-126">`"{id}"` is a placeholder variable for the ID of the `todo` item.</span></span> <span data-ttu-id="43cf1-127">При вызове `GetById` параметру метода `id` присваивается значение "{id}" в URL.</span><span class="sxs-lookup"><span data-stu-id="43cf1-127">When `GetById` is invoked, it assigns the value of "{id}" in the URL to the method's `id` parameter.</span></span>

<span data-ttu-id="43cf1-128">`Name = "GetTodo"` создает именованный маршрут и позволяет связать его с ответом HTTP.</span><span class="sxs-lookup"><span data-stu-id="43cf1-128">`Name = "GetTodo"` creates a named route and allows you to link to this route in an HTTP Response.</span></span> <span data-ttu-id="43cf1-129">Это будет продемонстрировано на примере позднее.</span><span class="sxs-lookup"><span data-stu-id="43cf1-129">I'll explain it with an example later.</span></span> <span data-ttu-id="43cf1-130">Дополнительные сведения см. в разделе [Маршрутизация к действиям контроллера](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="43cf1-130">See [Routing to Controller Actions](xref:mvc/controllers/routing) for detailed information.</span></span>

### <a name="return-values"></a><span data-ttu-id="43cf1-131">Возвращаемые значения</span><span class="sxs-lookup"><span data-stu-id="43cf1-131">Return values</span></span>

<span data-ttu-id="43cf1-132">Метод `GetAll` возвращает значение `IEnumerable`.</span><span class="sxs-lookup"><span data-stu-id="43cf1-132">The `GetAll` method returns an `IEnumerable`.</span></span> <span data-ttu-id="43cf1-133">MVC автоматически сериализует объект в формат [JSON](http://www.json.org/) и записывает данные JSON в тело сообщения ответа.</span><span class="sxs-lookup"><span data-stu-id="43cf1-133">MVC automatically serializes the object to [JSON](http://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="43cf1-134">Код ответа для этого метода равен 200, что свидетельствует об отсутствии необработанных исключений.</span><span class="sxs-lookup"><span data-stu-id="43cf1-134">The response code for this method is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="43cf1-135">(Необработанные исключения преобразуются в ошибки 5xx.)</span><span class="sxs-lookup"><span data-stu-id="43cf1-135">(Unhandled exceptions are translated into 5xx errors.)</span></span>

<span data-ttu-id="43cf1-136">Напротив, метод `GetById` возвращает более общий тип `IActionResult`, который представляет широкий диапазон типов возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="43cf1-136">In contrast, the `GetById` method returns the more general `IActionResult` type, which represents a wide range of return types.</span></span> <span data-ttu-id="43cf1-137">`GetById` имеет два разных типа возвращаемых значений:</span><span class="sxs-lookup"><span data-stu-id="43cf1-137">`GetById` has two different return types:</span></span>

* <span data-ttu-id="43cf1-138">Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="43cf1-138">If no item matches the requested ID, the method returns a 404 error.</span></span>  <span data-ttu-id="43cf1-139">Для этого возвращается значение `NotFound`.</span><span class="sxs-lookup"><span data-stu-id="43cf1-139">This is done by returning `NotFound`.</span></span>

* <span data-ttu-id="43cf1-140">В противном случае метод возвращает код 200 с телом ответа JSON.</span><span class="sxs-lookup"><span data-stu-id="43cf1-140">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="43cf1-141">Для этого возвращается значение `ObjectResult`</span><span class="sxs-lookup"><span data-stu-id="43cf1-141">This is done by returning an `ObjectResult`</span></span>
