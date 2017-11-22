## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="e658c-101">Реализация других операций CRUD</span><span class="sxs-lookup"><span data-stu-id="e658c-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="e658c-102">В следующих разделах к контроллеру добавляются методы `Create`, `Update` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="e658c-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="e658c-103">Создать</span><span class="sxs-lookup"><span data-stu-id="e658c-103">Create</span></span>

<span data-ttu-id="e658c-104">Добавьте приведенный ниже метод `Create`.</span><span class="sxs-lookup"><span data-stu-id="e658c-104">Add the following `Create` method.</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="e658c-105">Предыдущий код является методом HTTP POST, обозначенным атрибутом [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="e658c-105">The preceding code is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="e658c-106">Атрибут [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="e658c-106">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e658c-107">Метод `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="e658c-107">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="e658c-108">возвращает ответ 201.</span><span class="sxs-lookup"><span data-stu-id="e658c-108">Returns a 201 response.</span></span> <span data-ttu-id="e658c-109">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="e658c-109">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e658c-110">Добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="e658c-110">Adds a Location header to the response.</span></span> <span data-ttu-id="e658c-111">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="e658c-111">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e658c-112">См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="e658c-112">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e658c-113">Использует для создания URL-адреса маршрут с именем GetTodo.</span><span class="sxs-lookup"><span data-stu-id="e658c-113">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="e658c-114">Маршрут с именем GetTodo определяется в `GetById`:</span><span class="sxs-lookup"><span data-stu-id="e658c-114">The "GetTodo" named route is defined in `GetById`:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="e658c-115">Отправка запроса Create с помощью Postman</span><span class="sxs-lookup"><span data-stu-id="e658c-115">Use Postman to send a Create request</span></span>

![Консоль Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="e658c-117">Укажите HTTP-метод `POST`</span><span class="sxs-lookup"><span data-stu-id="e658c-117">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="e658c-118">Установите переключатель **Текст запроса**</span><span class="sxs-lookup"><span data-stu-id="e658c-118">Select the **Body** radio button</span></span>
* <span data-ttu-id="e658c-119">Установите переключатель **без обработки**</span><span class="sxs-lookup"><span data-stu-id="e658c-119">Select the **raw** radio button</span></span>
* <span data-ttu-id="e658c-120">В качестве типа выберите JSON</span><span class="sxs-lookup"><span data-stu-id="e658c-120">Set the type to JSON</span></span>
* <span data-ttu-id="e658c-121">В редакторе пар ключей и значений введите элемент Todo, например</span><span class="sxs-lookup"><span data-stu-id="e658c-121">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="e658c-122">Нажмите кнопку **Отправить**</span><span class="sxs-lookup"><span data-stu-id="e658c-122">Select **Send**</span></span>
* <span data-ttu-id="e658c-123">Откройте вкладку "Заголовки" в нижней области и скопируйте заголовок **расположения**:</span><span class="sxs-lookup"><span data-stu-id="e658c-123">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Вкладка "Заголовки" в консоли Postman](../../tutorials/first-web-api/_static/pmget.png)

<span data-ttu-id="e658c-125">Универсальный код ресурса (URI) заголовка расположения можно использовать для получения доступа к новому элементу.</span><span class="sxs-lookup"><span data-stu-id="e658c-125">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="e658c-126">Обновление</span><span class="sxs-lookup"><span data-stu-id="e658c-126">Update</span></span>

<span data-ttu-id="e658c-127">Добавьте приведенный ниже метод `Update`.</span><span class="sxs-lookup"><span data-stu-id="e658c-127">Add the following `Update` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="e658c-128">Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="e658c-128">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="e658c-129">Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e658c-129">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e658c-130">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней.</span><span class="sxs-lookup"><span data-stu-id="e658c-130">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="e658c-131">Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="e658c-131">To support partial updates, use HTTP PATCH.</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="e658c-133">Удаление</span><span class="sxs-lookup"><span data-stu-id="e658c-133">Delete</span></span>

<span data-ttu-id="e658c-134">Добавьте приведенный ниже метод `Delete`.</span><span class="sxs-lookup"><span data-stu-id="e658c-134">Add the following `Delete` method:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="e658c-135">`Delete`Ответ[ — 204 (нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="e658c-135">The `Delete` response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="e658c-136">Проверьте `Delete`:</span><span class="sxs-lookup"><span data-stu-id="e658c-136">Test `Delete`:</span></span> 

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmd.png)
