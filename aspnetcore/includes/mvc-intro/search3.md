<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="8c2b2-101">После отправки поиска URL-адрес содержит строку поискового запроса.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="8c2b2-102">Поиск также переносится в метод `HttpGet Index`, даже если у вас определен метод `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Окно браузера с фрагментом searchString=ghost в URL-адресе, которое возвращает фильмы Ghostbusters и Ghostbusters 2, с текстом ghost в названии](../../tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="8c2b2-104">В следующем примере разметки показано изменение тега `form`:</span><span class="sxs-lookup"><span data-stu-id="8c2b2-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="8c2b2-105">Добавление поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="8c2b2-105">Adding Search by genre</span></span>

<span data-ttu-id="8c2b2-106">Добавьте следующий класс `MovieGenreViewModel` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="8c2b2-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

<span data-ttu-id="8c2b2-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="8c2b2-107">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]</span></span>

<span data-ttu-id="8c2b2-108">Модель представления фильмов по жанру будет содержать:</span><span class="sxs-lookup"><span data-stu-id="8c2b2-108">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="8c2b2-109">Список фильмов.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-109">A list of movies.</span></span>
   * <span data-ttu-id="8c2b2-110">Объект `SelectList` со списком жанров.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-110">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="8c2b2-111">В этом списке пользователь может выбрать жанр фильма.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-111">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="8c2b2-112">Объект `movieGenre`, содержащий выбранный жанр.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-112">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="8c2b2-113">Замените метод `Index` в файле `MoviesController.cs` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="8c2b2-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

<span data-ttu-id="8c2b2-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span><span class="sxs-lookup"><span data-stu-id="8c2b2-114">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]</span></span>

<span data-ttu-id="8c2b2-115">Следующий код определяет запрос `LINQ`, который извлекает все жанры из базы данных.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-115">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

<span data-ttu-id="8c2b2-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span><span class="sxs-lookup"><span data-stu-id="8c2b2-116">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]</span></span>

<span data-ttu-id="8c2b2-117">Объект `SelectList` со списком жанров создается путем проецирования отдельных жанров (это необходимо, чтобы исключить повторяющиеся жанры).</span><span class="sxs-lookup"><span data-stu-id="8c2b2-117">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="8c2b2-118">Добавление поиска по жанру в представление Index</span><span class="sxs-lookup"><span data-stu-id="8c2b2-118">Adding search by genre to the Index view</span></span>

<span data-ttu-id="8c2b2-119">Обновите файл `Index.cshtml` следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8c2b2-119">Update `Index.cshtml` as follows:</span></span>

<span data-ttu-id="8c2b2-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span><span class="sxs-lookup"><span data-stu-id="8c2b2-120">[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]</span></span>

<span data-ttu-id="8c2b2-121">Проверьте лямбда-выражение, которое используется в следующем вспомогательном методе HTML:</span><span class="sxs-lookup"><span data-stu-id="8c2b2-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="8c2b2-122">В предыдущем коде вспомогательный метод HTML `DisplayNameFor` проверяет свойство `Title`, указанное в лямбда-выражении, и определяет отображаемое имя.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="8c2b2-123">Поскольку лямбда-выражение проверяется, а не вычисляется, в том случае, если `model`, `model.movies` или `model.movies[0]` имеют значение `null` или пусты, не происходит нарушение прав доступа.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="8c2b2-124">При вычислении лямбда-выражения (например, `@Html.DisplayFor(modelItem => item.Title)`) вычисляются значения для свойств модели.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="8c2b2-125">Проверьте работу приложения, выполнив поиск по жанру, по названию фильма и по обоим этим параметрам.</span><span class="sxs-lookup"><span data-stu-id="8c2b2-125">Test the app by searching by genre, by movie title, and by both.</span></span>
