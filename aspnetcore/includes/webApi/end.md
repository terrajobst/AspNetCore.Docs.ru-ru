## <a name="implement-the-other-crud-operations"></a>Реализация других операций CRUD

Добавим к контроллеру методы `Create`, `Update` и `Delete`. Это вариации темы, поэтому мы просто покажем код и выделим основные различия. После добавления или изменения кода выполните сборку проекта.

### <a name="create"></a>Создать

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Это метод HTTP POST, обозначенный атрибутом [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). Атрибут [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.

Метод `CreatedAtRoute` возвращает ответ 201, который представляет собой стандартный ответ для метода HTTP POST, создающий новый ресурс на сервере. `CreatedAtRoute` также добавляет в ответ заголовок расположения. Заголовок расположения указывает URI вновь созданной задачи. См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Отправка запроса Create с помощью Postman

![Консоль Postman](../../tutorials/first-web-api/_static/pmc.png)

* Укажите HTTP-метод `POST`
* Установите переключатель **Текст запроса**
* Установите переключатель **без обработки**
* В качестве типа выберите JSON
* В редакторе пар ключей и значений введите элемент Todo, например 

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Нажмите кнопку **Отправить**

* Откройте вкладку "Заголовки" в нижней области и скопируйте заголовок **расположения**:

![Вкладка "Заголовки" в консоли Postman](../../tutorials/first-web-api/_static/pmget.png)

URI заголовка расположения позволяет получить доступ к только что созданному ресурсу. Снова вызовите метод `GetById` для создания маршрута с именем `"GetTodo"`:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(long id)
```

### <a name="update"></a>Обновление

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT. Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней. Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Удаление

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmd.png)
