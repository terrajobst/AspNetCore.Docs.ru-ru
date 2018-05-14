## <a name="implement-the-other-crud-operations"></a>Реализация других операций CRUD

В следующих разделах к контроллеру добавляются методы `Create`, `Update` и `Delete`.

### <a name="create"></a>Создать

Добавьте приведенный ниже метод `Create`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Предыдущий код является методом HTTP POST, обозначенным атрибутом [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute). Атрибут [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.

Метод `CreatedAtRoute`:

* возвращает ответ 201. HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.
* Добавляет в ответ заголовок расположения. Заголовок расположения указывает URI вновь созданной задачи. См. раздел [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Использует для создания URL-адреса маршрут с именем GetTodo. Маршрут с именем GetTodo определяется в `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

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

Универсальный код ресурса (URI) заголовка расположения можно использовать для получения доступа к новому элементу.

### <a name="update"></a>Обновление

Добавьте приведенный ниже метод `Update`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT. Ответ — [204 (Нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только сведения о ней. Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Удаление

Добавьте приведенный ниже метод `Delete`.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete`Ответ[ — 204 (нет содержимого)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Проверьте `Delete`: 

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmd.png)
