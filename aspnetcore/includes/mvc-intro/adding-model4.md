Выделенный выше код демонстрирует добавление контекста базы данных фильмов в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection). Строка после `services.AddDbContext<MvcMovieContext>(options =>` не отображается (см. код). Она задает используемую базу данных и строку подключения. `=>` — это [лямбда-оператор](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/operators/lambda-operator).

Откройте файл *Controllers/MoviesController.cs* и изучите его конструктор:

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_1)] 

Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`MvcMovieContext `) в контроллер. Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.

<a name=strongly-typed-models-keyword-label></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a>Строго типизированные модели и ключевое слово @model

Ранее в этом руководстве вы ознакомились с передачей данных или объектов из контроллера в представление с использованием словаря `ViewData`. Словарь `ViewData` представляет собой динамический объект, который реализует удобный механизм позднего связывания для передачи информации в представление.

Модель MVC также поддерживает передачу строго типизированных объектов модели в представление. Такой подход обеспечивает более эффективную проверку кода во время компиляции. При создании методов и представлений в этом подходе используется механизм формирования шаблонов (то есть передачи строго типизированной модели) с представлениями и классами `MoviesController`.

Изучите созданный метод `Details` в файле *Controllers/MoviesController.cs*:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

Параметр `id` обычно передается в качестве данных маршрута. Например, `http://localhost:5000/movies/details/1` задает:

* Контроллер `movies` (первый сегмент URL-адреса).
* Действие `details` (второй сегмент URL-адреса).
* Идентификатор 1 (последний сегмент URL-адреса).

Также можно передать `id` с помощью строки запроса следующим образом:

`http://localhost:1234/movies/details?id=1`

Параметр `id` определяется как [тип, допускающий значение NULL](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/index) (`int?`), в случае, если не указано значение идентификатора.

[Лямбда-выражение](https://docs.microsoft.com/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) передается в `SingleOrDefaultAsync` для выбора записей фильмов, которые соответствуют данным маршрута или значению строки запроса.

```csharp
var movie = await _context.Movie
    .SingleOrDefaultAsync(m => m.ID == id);
```

Если фильм найден, экземпляр модели `Movie` передается в представление `Details`:

```csharp
return View(movie);
   ```

Изучите содержимое файла *Views/Movies/Details.cshtml*:

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/DetailsOriginal.cshtml)]

Указав оператор `@model` в начале файла представления, вы можете задать тип объекта, который будет ожидаться представлением. При создании контроллера movie Visual Studio автоматически включает следующий оператор `@model` в начало файла *Details.cshtml*:

```HTML
@model MvcMovie.Models.Movie
   ```

Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в представлении *Details.cshtml* код передает каждое поле фильма во вспомогательные функции HTML `DisplayNameFor` и `DisplayFor` со строго типизированным объектом `Model`. Методы `Create` и `Edit` и представления также передают объект модели `Movie`.

Изучите представление *Index.cshtml* и метод `Index` в контроллере Movies. Обратите внимание на то, как в коде создается объект `List` при вызове метода `View`. Код передает список `Movies` из метода действия `Index` в представление:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MC1.cs?name=snippet_index)]

При создании контроллера movies механизм формирования шаблонов автоматически включает следующий оператор `@model` в начало файла *Index.cshtml*:

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?range=1)]

Эта директива `@model` обеспечивает доступ к списку фильмов, который контроллер передал в представление с использованием строго типизированного объекта `Model`. Например, в представлении *Index.cshtml* код циклически перебирает фильмы с использованием оператора `foreach` для строго типизированного объекта `Model`:

[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

Поскольку объект `Model` является строго типизированным (как и объект `IEnumerable<Movie>`), каждый элемент в цикле получает тип `Movie`. Помимо прочих преимуществ, это означает, что выполняется проверка кода во время компиляции:
