# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Добавление поиска в приложение ASP.NET Core MVC

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

В этом разделе вы добавите в метод действия `Index` возможности поиска, которые позволяют выполнять поиск фильмов по *жанру* или *имени*.

Обновите метод `Index`, используя следующий код:
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

В первой строке метода действия `Index` создается запрос [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) для выбора фильмов:

```csharp
var movies = from m in _context.Movie
             select m;
```

Этот запрос определяется *только* в этой точке и **не** выполняется для базы данных.

Если параметр `searchString` содержит строку, запрос фильмов изменяется для фильтрации по значению в строке поиска:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

Приведенный выше код `s => s.Title.Contains()` представляет собой [лямбда-выражение](http://msdn.microsoft.com/library/bb397687.aspx). Лямбда-выражения используются в запросах [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) на основе методов в качестве аргументов стандартных методов операторов запроса, таких как метод [Where](http://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) или `Contains` (используется в приведенном выше коде). Запросы LINQ не выполняются в том случае, если они определяются или изменяются путем вызова метода (например, `Where`, `Contains` или `OrderBy`). Вместо этого выполнение запроса откладывается.  Это означает, что вычисление выражения откладывается до тех пор, пока не будет выполнена итерация его реализованного значения или пока не будет вызван метод `ToListAsync`. Дополнительные сведения об отложенном и немедленном выполнении запросов см. в разделе [Выполнение запроса](http://msdn.microsoft.com/library/bb738633.aspx).

Примечание. Метод [Contains](http://msdn.microsoft.com/library/bb155125.aspx) выполняется в базе данных, а не в коде C#, приведенном выше. Регистр символов запроса учитывается в зависимости от параметров базы данных и сортировки. В SQL Server метод[Contains](http://msdn.microsoft.com/library/bb155125.aspx) сопоставляется с [SQL LIKE](http://msdn.microsoft.com/library/ms179859.aspx), в котором регистр символов не учитывается. В SQLite при параметрах сортировки по умолчанию регистр символов учитывается.

Перейдите к `/Movies/Index`. Добавьте в URL-адрес строку запроса, например `?searchString=Ghost`. Отображаются отфильтрованные фильмы.

![Представление Index](../../tutorials/first-mvc-app/search/_static/ghost.png)

Если вы изменили сигнатуру метода `Index` и включили в нее параметр с именем `id`, параметр `id` будет соответствовать необязательному заполнителю `{id}` для маршрутов по умолчанию, который задан в файле *Startup.cs*.

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]