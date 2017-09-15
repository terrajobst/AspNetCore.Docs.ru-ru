---
title: "Добавление поиска на страницы Razor ASP.NET Core"
author: rick-anderson
description: "Инструкции по добавлению поиска на страницы Razor ASP.NET Core"
keywords: "ASP.NET Core,поиск,страницы Razor"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/razor-pages/search
ms.openlocfilehash: f70f1e9b0e085f5aa90fcca499526588662c3cfd
ms.sourcegitcommit: ffac7e195bd7f99364f3aab45e491eeaf8f173b0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/12/2017
---
# <a name="adding-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="df204-104">Добавление поиска в приложение ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="df204-104">Adding search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="df204-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="df204-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="df204-106">В этом документе на страницу Index добавляется возможность поиска фильмов по *жанру* или *названию*.</span><span class="sxs-lookup"><span data-stu-id="df204-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="df204-107">Обновите метод `OnGetAsync` страницы Index, добавив следующий код:</span><span class="sxs-lookup"><span data-stu-id="df204-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

<span data-ttu-id="df204-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]</span><span class="sxs-lookup"><span data-stu-id="df204-108">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]</span></span>

<span data-ttu-id="df204-109">В первой строке метода `OnGetAsync` создается запрос [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) для выбора фильмов:</span><span class="sxs-lookup"><span data-stu-id="df204-109">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
 var movies = from m in _context.Movie
              select m;
```

<span data-ttu-id="df204-110">Этот запрос определяется *только* в этой точке и **не** выполняется для базы данных.</span><span class="sxs-lookup"><span data-stu-id="df204-110">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="df204-111">Если параметр `searchString` содержит строку, запрос фильмов изменяется для фильтрации по строке поиска:</span><span class="sxs-lookup"><span data-stu-id="df204-111">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

<span data-ttu-id="df204-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]</span><span class="sxs-lookup"><span data-stu-id="df204-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]</span></span>

<span data-ttu-id="df204-113">Код `s => s.Title.Contains()` представляет собой [лямбда-выражение](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="df204-113">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="df204-114">Лямбда-выражения используются в запросах [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) на основе методов в качестве аргументов стандартных методов операторов запроса, таких как метод [Where](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) или `Contains` (используется в предшествующем коде).</span><span class="sxs-lookup"><span data-stu-id="df204-114">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="df204-115">Запросы LINQ не выполняются в том случае, если они определяются или изменяются путем вызова метода (например, `Where`, `Contains` или `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="df204-115">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="df204-116">Вместо этого выполнение запроса откладывается.</span><span class="sxs-lookup"><span data-stu-id="df204-116">Rather, query execution is deferred.</span></span> <span data-ttu-id="df204-117">Это означает, что вычисление выражения откладывается до тех пор, пока не будет выполнена итерация его реализованного значения или не будет вызван метод `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="df204-117">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="df204-118">Дополнительные сведения см. в разделе [Выполнение запроса](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="df204-118">See [Query Execution](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="df204-119">**Примечание.** Метод [Contains](http://msdn.microsoft.com/library/bb155125.aspx) выполняется в базе данных, а не в коде C#.</span><span class="sxs-lookup"><span data-stu-id="df204-119">**Note:** The [Contains](http://msdn.microsoft.com/library/bb155125.aspx) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="df204-120">Регистр символов запроса учитывается в зависимости от параметров базы данных и сортировки.</span><span class="sxs-lookup"><span data-stu-id="df204-120">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="df204-121">В SQL Server метод `Contains` сопоставляется с [SQL LIKE](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql), в котором регистр символов не учитывается.</span><span class="sxs-lookup"><span data-stu-id="df204-121">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="df204-122">В SQLite при параметрах сортировки по умолчанию регистр символов учитывается.</span><span class="sxs-lookup"><span data-stu-id="df204-122">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="df204-123">Перейдите на страницу Movies и добавьте строку запроса, такую как `?searchString=Ghost`, к URL-адресу (например, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="df204-123">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="df204-124">Отображаются отфильтрованные фильмы.</span><span class="sxs-lookup"><span data-stu-id="df204-124">The filtered movies are displayed.</span></span>

![Представление Index](search/_static/ghost.png)

<span data-ttu-id="df204-126">Если на страницу Index добавлен следующий шаблон маршрута, строку поиска можно передать в виде сегмента URL-адреса (например, `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="df204-126">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="df204-127">Предыдущее ограничение маршрута разрешало поиск названия в виде данных маршрута (сегмент URL-адреса) вместо значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="df204-127">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="df204-128">Символ `?` в `"{searchString?}"` означает, что этот параметр является необязательным.</span><span class="sxs-lookup"><span data-stu-id="df204-128">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Представление Index, в URL-адрес которого добавлено слово ghost, возвращает два фильма: Ghostbusters и Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="df204-130">Тем не менее пользователи вряд ли будут изменять URL-адрес для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="df204-130">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="df204-131">На этом шаге добавляется пользовательский интерфейс для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="df204-131">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="df204-132">Если было добавлено ограничение маршрута `"{searchString?}"`, удалите его.</span><span class="sxs-lookup"><span data-stu-id="df204-132">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="df204-133">Откройте файл *Pages/Movies/Index.cshtml* и добавьте разметку `<form>`, которая выделена в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="df204-133">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

<span data-ttu-id="df204-134">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]</span><span class="sxs-lookup"><span data-stu-id="df204-134">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]</span></span>

<span data-ttu-id="df204-135">Тег HTML `<form>` использует [вспомогательную функцию тега Form](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="df204-135">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="df204-136">При отправке формы строка фильтра отправляется на страницу *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="df204-136">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="df204-137">Сохраните изменения и проверьте работу фильтра.</span><span class="sxs-lookup"><span data-stu-id="df204-137">Save the changes and test the filter.</span></span>

![Представление Index со словом ghost в текстовом поле фильтра по названию](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="df204-139">Поиск по жанру</span><span class="sxs-lookup"><span data-stu-id="df204-139">Search by genre</span></span>

<span data-ttu-id="df204-140">Добавьте следующие выделенные свойства в файл *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="df204-140">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

<span data-ttu-id="df204-141">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]</span><span class="sxs-lookup"><span data-stu-id="df204-141">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]</span></span>

<span data-ttu-id="df204-142">Свойство `SelectList Genres` содержит список жанров.</span><span class="sxs-lookup"><span data-stu-id="df204-142">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="df204-143">В этом списке пользователь может выбрать жанр фильма.</span><span class="sxs-lookup"><span data-stu-id="df204-143">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="df204-144">Свойство `MovieGenre` содержит конкретный жанр, выбранный пользователем, например "Western" (Вестерн).</span><span class="sxs-lookup"><span data-stu-id="df204-144">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="df204-145">Обновите метод `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="df204-145">Update the `OnGetAsync` method with the following code:</span></span>

<span data-ttu-id="df204-146">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]</span><span class="sxs-lookup"><span data-stu-id="df204-146">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]</span></span>

<span data-ttu-id="df204-147">Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.</span><span class="sxs-lookup"><span data-stu-id="df204-147">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

<span data-ttu-id="df204-148">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]</span><span class="sxs-lookup"><span data-stu-id="df204-148">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]</span></span>

<span data-ttu-id="df204-149">Список жанров `SelectList` создается путем проецирования отдельных жанров.</span><span class="sxs-lookup"><span data-stu-id="df204-149">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="df204-150">Добавление поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="df204-150">Adding search by genre</span></span>

<span data-ttu-id="df204-151">Обновите файл *Index.cshtml* следующим образом:</span><span class="sxs-lookup"><span data-stu-id="df204-151">Update *Index.cshtml* as follows:</span></span>

<span data-ttu-id="df204-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]</span><span class="sxs-lookup"><span data-stu-id="df204-152">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]</span></span>

<span data-ttu-id="df204-153">Проверьте работу приложения, выполнив поиск по жанру, по названию фильма и по обоим этим параметрам.</span><span class="sxs-lookup"><span data-stu-id="df204-153">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="df204-154">[Предыдущая статья — "Обновление страниц"](xref:tutorials/razor-pages/da1)
[Следующая статья — "Добавление нового поля"](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="df204-154">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>