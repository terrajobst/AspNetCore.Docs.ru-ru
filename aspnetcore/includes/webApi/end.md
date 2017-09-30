## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="08f42-101">Реализация других операций CRUD</span><span class="sxs-lookup"><span data-stu-id="08f42-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="08f42-102">Добавим к контроллеру методы `Create`, `Update` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="08f42-102">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="08f42-103">Это вариации темы, поэтому мы просто покажем код и выделим основные различия.</span><span class="sxs-lookup"><span data-stu-id="08f42-103">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="08f42-104">После добавления или изменения кода выполните сборку проекта.</span><span class="sxs-lookup"><span data-stu-id="08f42-104">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="08f42-105">Создать</span><span class="sxs-lookup"><span data-stu-id="08f42-105">Create</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="08f42-106">Это метод HTTP POST, обозначенный атрибутом [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="08f42-106">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="08f42-107">Атрибут [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="08f42-107">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="08f42-108">Метод `CreatedAtRoute` возвращает ответ 201, который представляет собой стандартный ответ для метода HTTP POST, создающий новый ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="08f42-108">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="08f42-109">`CreatedAtRoute` также добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="08f42-109">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="08f42-110">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="08f42-110">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="08f42-111">См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="08f42-111">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="08f42-112">Отправка запроса Create с помощью Postman</span><span class="sxs-lookup"><span data-stu-id="08f42-112">Use Postman to send a Create request</span></span>

![Консоль Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="08f42-114">Укажите HTTP-метод `POST`</span><span class="sxs-lookup"><span data-stu-id="08f42-114">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="08f42-115">Установите переключатель **Текст запроса**</span><span class="sxs-lookup"><span data-stu-id="08f42-115">Select the **Body** radio button</span></span>
* <span data-ttu-id="08f42-116">Установите переключатель **без обработки**</span><span class="sxs-lookup"><span data-stu-id="08f42-116">Select the **raw** radio button</span></span>
* <span data-ttu-id="08f42-117">В качестве типа выберите JSON</span><span class="sxs-lookup"><span data-stu-id="08f42-117">Set the type to JSON</span></span>
* <span data-ttu-id="08f42-118">В редакторе пар ключей и значений введите элемент Todo, например</span><span class="sxs-lookup"><span data-stu-id="08f42-118">In the key-value editor, enter a Todo item such as</span></span> 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="08f42-119">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="08f42-119">Select **Send**</span></span>

* <span data-ttu-id="08f42-120">Откройте вкладку "Заголовки" в нижней области и скопируйте заголовок **расположения**:</span><span class="sxs-lookup"><span data-stu-id="08f42-120">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Вкладка "Заголовки" в консоли Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="08f42-122">URI заголовка расположения позволяет получить доступ к только что созданному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="08f42-122">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="08f42-123">Снова вызовите метод `GetById` для создания маршрута с именем `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="08f42-123">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a><span data-ttu-id="08f42-124">Обновление</span><span class="sxs-lookup"><span data-stu-id="08f42-124">Update</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="08f42-125">Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="08f42-125">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="08f42-126">Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="08f42-126">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="08f42-127">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней.</span><span class="sxs-lookup"><span data-stu-id="08f42-127">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="08f42-128">Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="08f42-128">To support partial updates, use HTTP PATCH.</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="08f42-130">Удаление</span><span class="sxs-lookup"><span data-stu-id="08f42-130">Delete</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="08f42-131">Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="08f42-131">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmd.png)
