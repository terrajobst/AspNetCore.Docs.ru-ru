## <a name="implement-the-other-crud-operations"></a>Реализация других операций CRUD

В следующих разделах к контроллеру добавляются методы `Create`, `Update` и `Delete`.

### <a name="create"></a>Создать

Добавьте приведенный ниже метод `Create`.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). Атрибут [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) сообщает MVC, что необходимо получить значение элемента задачи из текста HTTP-запроса.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Предыдущий код является методом HTTP POST, как указывает атрибут [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute). MVC получает значение элемента задачи из текста HTTP-запроса.
::: moniker-end

Метод `CreatedAtRoute`:

* возвращает ответ 201. HTTP 201 представляет собой стандартный ответ для метода HTTP POST, создающий ресурс на сервере.
* Добавляет в ответ заголовок расположения. Заголовок расположения указывает URI вновь созданной задачи. См. раздел [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).
* Использует для создания URL-адреса маршрут с именем GetTodo. Маршрут с именем GetTodo определяется в `GetById`:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

### <a name="use-postman-to-send-a-create-request"></a>Отправка запроса Create с помощью Postman

* Запустите приложение.
* Откройте Postman.

![Консоль Postman](../../tutorials/first-web-api/_static/pmc.png)

* Измените номера порта в localhost URL-адреса.
* Укажите метод HTTP *POST*.
* Откройте вкладку **Текст**.
* Установите переключатель **без обработки**.
* Задайте тип *JSON (приложение/json)*.
* Введите текст запроса с элементом задачи наподобие следующего JSON:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Нажмите кнопку **Отправить**.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Если ответ не отображается после нажатия кнопки **Отправить**, отключите параметр **Проверка сертификации SSL**. Он находится в разделе **Файл** > **Параметры**. Нажмите кнопку **Отправить** еще раз после отключения параметра.
::: moniker-end

Откройте вкладку **Заголовки** на панели **Ответ** и скопируйте значение заголовка **Расположение**:

![Вкладка "Заголовки" в консоли Postman](../../tutorials/first-web-api/_static/pmc2.png)

Универсальный код ресурса (URI) заголовка расположения можно использовать для получения доступа к новому элементу.

### <a name="update"></a>Обновление

Добавьте приведенный ниже метод `Update`.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

Страница `Update` аналогична странице `Create`, но использует запрос HTTP PUT. Ответ — [204 (Нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Согласно спецификации HTTP, запрос PUT требует, чтобы клиент отправлял всю обновленную сущность, а не только отличия. Чтобы обеспечить поддержку частичных обновлений, используйте HTTP PATCH.

Используйте Postman, чтобы изменить имя элемента задачи на "walk cat":

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmcput.png)

### <a name="delete"></a>Удаление

Добавьте приведенный ниже метод `Delete`.

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

`Delete`Ответ[ — 204 (нет содержимого)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

Используйте Postman для удаления элемента задачи:

![Консоль Postman с ответом 204 (Нет содержимого)](../../tutorials/first-web-api/_static/pmd.png)
