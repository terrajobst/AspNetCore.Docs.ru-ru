# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Добавление поиска в приложение ASP.NET Core MVC

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом разделе вы добавите в метод действия `Index` возможности поиска, которые позволяют выполнять поиск фильмов по *жанру* или *имени*.

Обновите метод `Index`, используя следующий код:
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

В первой строке метода действия `Index` создается запрос [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) для выбора фильмов:

```csharp
var movies = from m in _context.Movie
             select m;
```

Этот запрос определяется *только* в этой точке и **не** выполняется для базы данных.

Если параметр `searchString` содержит строку, запрос фильмов изменяется для фильтрации по значению в строке поиска:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

Приведенный выше код `s => s.Title.Contains()` представляет собой [лямбда-выражение](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Лямбда-выражения используются в запросах [LINQ](https://docs.microsoft.com/dotnet/standard/using-linq) на основе методов в качестве аргументов стандартных методов операторов запроса, таких как метод [Where](https://docs.microsoft.com//dotnet/api/system.linq.enumerable.where) или `Contains` (используется в приведенном выше коде). Запросы LINQ не выполняются, если они определяются или изменяются путем вызова метода (например, `Where`, `Contains` или `OrderBy`). Вместо этого выполнение запроса откладывается.  Это означает, что вычисление выражения откладывается до тех пор, пока не будет выполнена итерация его реализованного значения или пока не будет вызван метод `ToListAsync`. Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Примечание. Метод [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) выполняется в базе данных, а не в коде C#, приведенном выше. Регистр символов запроса учитывается в зависимости от параметров базы данных и сортировки. В SQL Server метод[Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) сопоставляется с [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), в котором регистр символов не учитывается. В SQLite при параметрах сортировки по умолчанию регистр символов учитывается.

Перейдите к `/Movies/Index`. Добавьте в URL-адрес строку запроса, например `?searchString=Ghost`. Отображаются отфильтрованные фильмы.

![Представление Index](../../tutorials/first-mvc-app/search/_static/ghost.png)

Если вы изменили сигнатуру метода `Index` и включили в нее параметр с именем `id`, параметр `id` будет соответствовать необязательному заполнителю `{id}` для маршрутов по умолчанию, который задан в файле *Startup.cs*.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
