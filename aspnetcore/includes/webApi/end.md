## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="69370-101">Реализация других операций CRUD</span><span class="sxs-lookup"><span data-stu-id="69370-101">Implement the other CRUD operations</span></span>

<span data-ttu-id="69370-102">В следующих разделах к контроллеру добавляются методы `Create`, `Update` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="69370-102">In the following sections, `Create`, `Update`, and `Delete` methods are added to the controller.</span></span>

### <a name="create"></a><span data-ttu-id="69370-103">Создать</span><span class="sxs-lookup"><span data-stu-id="69370-103">Create</span></span>

<span data-ttu-id="69370-104">Добавьте приведенный ниже метод `Create`.</span><span class="sxs-lookup"><span data-stu-id="69370-104">Add the following `Create` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="69370-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="69370-105">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="69370-106">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="69370-106">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="69370-107">Атрибут [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="69370-107">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="69370-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="69370-108">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="69370-109">Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="69370-109">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="69370-110">MVC получает значение элемента задачи из текста HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="69370-110">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="69370-111">Метод `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="69370-111">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="69370-112">возвращает ответ 201.</span><span class="sxs-lookup"><span data-stu-id="69370-112">Returns a 201 response.</span></span> <span data-ttu-id="69370-113">HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.</span><span class="sxs-lookup"><span data-stu-id="69370-113">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="69370-114">Добавляет в ответ заголовок расположения.</span><span class="sxs-lookup"><span data-stu-id="69370-114">Adds a Location header to the response.</span></span> <span data-ttu-id="69370-115">Заголовок расположения указывает URI вновь созданной задачи.</span><span class="sxs-lookup"><span data-stu-id="69370-115">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="69370-116">См. раздел [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="69370-116">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="69370-117">Использует для создания URL-адреса маршрут с именем GetTodo.</span><span class="sxs-lookup"><span data-stu-id="69370-117">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="69370-118">Маршрут с именем GetTodo определяется в `GetById`:</span><span class="sxs-lookup"><span data-stu-id="69370-118">The "GetTodo" named route is defined in `GetById`:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="69370-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="69370-119">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="69370-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span><span class="sxs-lookup"><span data-stu-id="69370-120">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]</span></span>
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="69370-121">Отправка запроса Create с помощью Postman</span><span class="sxs-lookup"><span data-stu-id="69370-121">Use Postman to send a Create request</span></span>

* <span data-ttu-id="69370-122">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="69370-122">Start the app.</span></span>
* <span data-ttu-id="69370-123">Откройте Postman.</span><span class="sxs-lookup"><span data-stu-id="69370-123">Open Postman.</span></span>

![Консоль Postman](../../tutorials/first-web-api/_static/pmc.png)

* <span data-ttu-id="69370-125">Измените номера порта в localhost URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="69370-125">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="69370-126">Укажите метод HTTP *POST*.</span><span class="sxs-lookup"><span data-stu-id="69370-126">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="69370-127">Откройте вкладку **Текст**.</span><span class="sxs-lookup"><span data-stu-id="69370-127">Click the **Body** tab.</span></span>
* <span data-ttu-id="69370-128">Установите переключатель **без обработки**.</span><span class="sxs-lookup"><span data-stu-id="69370-128">Select the **raw** radio button.</span></span>
* <span data-ttu-id="69370-129">Задайте тип *JSON (приложение/json)*.</span><span class="sxs-lookup"><span data-stu-id="69370-129">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="69370-130">Введите текст запроса с элементом задачи наподобие следующего JSON:</span><span class="sxs-lookup"><span data-stu-id="69370-130">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="69370-131">Нажмите кнопку **Отправить**.</span><span class="sxs-lookup"><span data-stu-id="69370-131">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="69370-132">Если ответ не отображается после нажатия кнопки **Отправить**, отключите параметр **Проверка сертификации SSL**.</span><span class="sxs-lookup"><span data-stu-id="69370-132">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="69370-133">Он находится в разделе **Файл** > **Параметры**.</span><span class="sxs-lookup"><span data-stu-id="69370-133">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="69370-134">Нажмите кнопку **Отправить** еще раз после отключения параметра.</span><span class="sxs-lookup"><span data-stu-id="69370-134">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="69370-135">Откройте вкладку **Заголовки** на панели **Ответ** и скопируйте значение заголовка **Расположение**:</span><span class="sxs-lookup"><span data-stu-id="69370-135">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Вкладка "Заголовки" в консоли Postman](../../tutorials/first-web-api/_static/pmc2.png)

<span data-ttu-id="69370-137">Универсальный код ресурса (URI) заголовка расположения можно использовать для получения доступа к новому элементу.</span><span class="sxs-lookup"><span data-stu-id="69370-137">The Location header URI can be used to access the new item.</span></span>

### <a name="update"></a><span data-ttu-id="69370-138">Обновление</span><span class="sxs-lookup"><span data-stu-id="69370-138">Update</span></span>

<span data-ttu-id="69370-139">Добавьте приведенный ниже метод `Update`.</span><span class="sxs-lookup"><span data-stu-id="69370-139">Add the following `Update` method:</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="69370-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="69370-140">[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="69370-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="69370-141">[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="69370-142">Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="69370-142">`Update` is similar to `Create`, except it uses HTTP PUT.</span></span> <span data-ttu-id="69370-143">Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="69370-143">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="69370-144">Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только отличия.</span><span class="sxs-lookup"><span data-stu-id="69370-144">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="69370-145">Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="69370-145">To support partial updates, use HTTP PATCH.</span></span>

<span data-ttu-id="69370-146">Используйте Postman, чтобы изменить имя элемента задачи на "walk cat":</span><span class="sxs-lookup"><span data-stu-id="69370-146">Use Postman to update the to-do item's name to "walk cat":</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="69370-148">Удаление</span><span class="sxs-lookup"><span data-stu-id="69370-148">Delete</span></span>

<span data-ttu-id="69370-149">Добавьте приведенный ниже метод `Delete`.</span><span class="sxs-lookup"><span data-stu-id="69370-149">Add the following `Delete` method:</span></span>

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="69370-150">`Delete`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="69370-150">The `Delete` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

<span data-ttu-id="69370-151">Используйте Postman для удаления элемента задачи:</span><span class="sxs-lookup"><span data-stu-id="69370-151">Use Postman to delete the to-do item:</span></span>

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmd.png)
