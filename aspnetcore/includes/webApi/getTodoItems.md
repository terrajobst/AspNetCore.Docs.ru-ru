::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

В предыдущем коде определяется класс контроллера API без методов. В следующих разделах добавляются методы для реализации API-интерфейса.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

В предыдущем коде определяется класс контроллера API без методов. В следующих разделах добавляются методы для реализации API-интерфейса. Класс помечается атрибутом `[ApiController]` для включения некоторых удобных функций. Сведения о функциях, включаемых этим атрибутом, см. в разделе [Аннотирование класса атрибутом ApiControllerAttribute](xref:web-api/index#annotate-class-with-apicontrollerattribute).
::: moniker-end

Конструктор контроллера применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`TodoContext`) в контроллер. Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере. Конструктор добавляет элемент в базу данных в памяти, если он не существует.

## <a name="get-to-do-items"></a>Получение элементов задачи

Чтобы получить элементы задачи, добавьте к классу `TodoController` следующие методы:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]
::: moniker-end

Эти методы реализуют два метода GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Ниже приведен пример ответа HTTP для метода `GetAll`:

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

Далее в этом руководстве мы покажем, как можно просмотреть ответ HTTP с использованием [Postman](https://www.getpostman.com/) или [curl](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/curl.1.html).

### <a name="routing-and-url-paths"></a>Маршрутизация и пути URL

Атрибут `[HttpGet]` обозначает метод, который отвечает на запрос HTTP GET. Путь URL для каждого метода формируется следующим образом:

* Принимается строка шаблона в атрибуте `Route` контроллера:

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]
::: moniker-end

* Замените `[controller]` именем контроллера (имя класса контроллера без суффикса "Controller"). В этом примере класс контроллера носит имя **Todo**Controller, а сам контроллер, соответственно, — "todo". В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.
* Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("/products")]`), добавьте его к пути. В этом примере шаблон не используется. Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

В следующем методе `GetById` `"{id}"` — это переменная-заполнитель для уникального идентификатора элемента задачи. При вызове `GetById` параметру метода `id` присваивается значение `"{id}"` в URL.

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]
::: moniker-end

`Name = "GetTodo"` создает именованный маршрут. Именованные маршруты:

* Предоставляют приложению возможность создать ссылку HTTP, используя имя маршрута.
* Описываются далее в этом руководстве.

### <a name="return-values"></a>Возвращаемые значения

Метод `GetAll` возвращает коллекцию объектов `TodoItem`. MVC автоматически сериализует объект в формат [JSON](https://www.json.org/) и записывает данные JSON в тело сообщения ответа. Код ответа для этого метода равен 200, что свидетельствует об отсутствии необработанных исключений. Необработанные исключения преобразуются в ошибки 5xx.

::: moniker range="<= aspnetcore-2.0"
Напротив, метод `GetById` возвращает более общий тип [IActionResult type](xref:web-api/action-return-types#iactionresult-type), который представляет широкий диапазон типов возвращаемых значений. `GetById` имеет два разных типа возвращаемых значений:

* Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404. При возвращении [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) возвращается ответ HTTP 404.
* В противном случае метод возвращает код 200 с телом ответа JSON. При возвращении [ОК](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.ok) возвращается ответ HTTP 200.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
Напротив, метод `GetById` возвращает тип [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type), который представляет широкий диапазон типов возвращаемых значений. `GetById` имеет два разных типа возвращаемых значений:

* Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404. При возвращении [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) возвращается ответ HTTP 404.
* В противном случае метод возвращает код 200 с телом ответа JSON. При возвращении `item` возвращается ответ HTTP 200.
::: moniker-end