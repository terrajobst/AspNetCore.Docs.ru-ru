<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

После отправки поиска URL-адрес содержит строку поискового запроса. Поиск также переносится в метод `HttpGet Index`, даже если у вас определен метод `HttpPost Index`.

![Окно браузера с фрагментом searchString=ghost в URL-адресе, которое возвращает фильмы Ghostbusters и Ghostbusters 2, с текстом ghost в названии](~/tutorials/first-mvc-app/search/_static/search_get.png)

В следующем примере разметки показано изменение тега `form`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Добавление поиска по жанру

Добавьте следующий класс `MovieGenreViewModel` в папку *Models*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Модель представления фильмов по жанру будет содержать:

   * Список фильмов.
   * Объект `SelectList` со списком жанров. В этом списке пользователь может выбрать жанр фильма.
   * Объект `MovieGenre`, содержащий выбранный жанр.
   * `SearchString`, содержащий текст, который пользователи вводят в поле поиска.

Замените метод `Index` в файле `MoviesController.cs` следующим кодом:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Следующий код определяет запрос `LINQ`, который извлекает все жанры из базы данных.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

Объект `SelectList` со списком жанров создается путем проецирования отдельных жанров (это необходимо, чтобы исключить повторяющиеся жанры).

Когда пользователь выполняет поиск элемента, значение поиска сохраняется в поле поиска. Чтобы сохранить значение поиска, укажите в свойстве `SearchString` значение поиска. Значение поиска является параметром `searchString` для действия контроллера `Index`.

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a>Добавление поиска по жанру в представление Index

Обновите файл `Index.cshtml` следующим образом:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Проверьте лямбда-выражение, которое используется в следующем вспомогательном методе HTML:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
В предыдущем коде вспомогательный метод HTML `DisplayNameFor` проверяет свойство `Title`, указанное в лямбда-выражении, и определяет отображаемое имя. Поскольку лямбда-выражение проверяется, а не вычисляется, в том случае, если `model`, `model.Movies` или `model.Movies[0]` имеют значение `null` или пусты, не происходит нарушение прав доступа. При вычислении лямбда-выражения (например, `@Html.DisplayFor(modelItem => item.Title)`) вычисляются значения для свойств модели.

Проверьте работу приложения, выполнив поиск по жанру, по названию фильма и по обоим этим параметрам.
