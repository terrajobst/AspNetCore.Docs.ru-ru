[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

Предыдущий код:

* определяет пустой класс контроллера. В следующих разделах добавляются методы для реализации API-интерфейса.
* Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`TodoContext `) в контроллер. Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.
* Конструктор добавляет элемент в базу данных в памяти, если он не существует.

## <a name="getting-to-do-items"></a>Получение элементов задачи

Чтобы получить элементы задачи, добавьте к классу `TodoController` следующие методы.

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

Эти методы реализуют два метода GET:

* `GET /api/todo`
* `GET /api/todo/{id}`

Ниже приведен пример ответа HTTP для метода `GetAll`:

```
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

Атрибут `[HttpGet]` задает метод HTTP GET. Путь URL для каждого метода формируется следующим образом:

* Принимается строка шаблона в атрибуте `Route` контроллера:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* `[controller]` заменяется именем контроллера (имя класса контроллера без суффикса Controller). В этом примере класс контроллера носит имя **Todo**Controller, а сам контроллер, соответственно, — "todo". В ASP.NET Core [маршрутизация](xref:mvc/controllers/routing) реализуется без учета регистра символов.
* Если атрибут `[HttpGet]` имеет шаблон маршрута (например, `[HttpGet("/products")]`), добавьте его к пути. В этом примере шаблон не используется. Дополнительные сведения см. в разделе [Маршрутизация атрибутов с помощью атрибутов Http[Verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).

В методе `GetById`:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

`"{id}"` — это переменная-заполнитель для идентификатора элемента `todo`. При вызове `GetById` параметру метода `id` присваивается значение "{id}" в URL.

`Name = "GetTodo"` создает именованный маршрут. Именованные маршруты:

* Предоставляют приложению возможность создать ссылку HTTP, используя имя маршрута.
* Описываются далее в этом руководстве.

### <a name="return-values"></a>Возвращаемые значения

Метод `GetAll` возвращает значение `IEnumerable`. MVC автоматически сериализует объект в формат [JSON](http://www.json.org/) и записывает данные JSON в тело сообщения ответа. Код ответа для этого метода равен 200, что свидетельствует об отсутствии необработанных исключений. (Необработанные исключения преобразуются в ошибки 5xx.)

Напротив, метод `GetById` возвращает более общий тип `IActionResult`, который представляет широкий диапазон типов возвращаемых значений. `GetById` имеет два разных типа возвращаемых значений:

* Если запрошенному идентификатору не соответствует ни один элемент, метод возвращает ошибку 404. При возвращении `NotFound` возвращается ответ HTTP 404.

* В противном случае метод возвращает код 200 с телом ответа JSON. При возвращении `ObjectResult` возвращается ответ HTTP 200.
