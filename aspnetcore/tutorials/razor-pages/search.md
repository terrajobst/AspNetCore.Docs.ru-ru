---
title: Добавление поиска на страницы Razor ASP.NET Core
author: rick-anderson
description: Инструкции по добавлению поиска на страницы Razor ASP.NET Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/search
ms.openlocfilehash: 80292f8cfecd5363fb8acc8578f9bb0ca9ee5969
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090167"
---
# <a name="add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="1a5b6-103">Добавление поиска на страницы Razor ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1a5b6-103">Add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="1a5b6-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="1a5b6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1a5b6-105">В этом документе на страницу Index добавляется возможность поиска фильмов по *жанру* или *названию*.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-105">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="1a5b6-106">Добавьте следующие выделенные свойства в файл *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1a5b6-106">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

::: moniker-end

* <span data-ttu-id="1a5b6-107">`SearchString`: содержит текст, который пользователи вводят в поле поиска.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-107">`SearchString`: contains the text users enter in the search text box.</span></span>
* <span data-ttu-id="1a5b6-108">`Genres`: содержит список жанров.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-108">`Genres`: contains the list of genres.</span></span> <span data-ttu-id="1a5b6-109">В этом списке пользователь может выбрать жанр фильма.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-109">This allows the user to select a genre from the list.</span></span>
* <span data-ttu-id="1a5b6-110">`MovieGenre`: содержит конкретный жанр, выбранный пользователем, например "Western" (Вестерн).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-110">`MovieGenre`: contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="1a5b6-111">В этом документе вы будете работать со свойствами `Genres` и `MovieGenre`.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-111">You'll work with the `Genres` and `MovieGenre` properties later in this document.</span></span>

<span data-ttu-id="1a5b6-112">Обновите метод `OnGetAsync` страницы Index, добавив следующий код:</span><span class="sxs-lookup"><span data-stu-id="1a5b6-112">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="1a5b6-113">В первой строке метода `OnGetAsync` создается запрос [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) для выбора фильмов:</span><span class="sxs-lookup"><span data-stu-id="1a5b6-113">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="1a5b6-114">Этот запрос *только* определяется в этой точке и **не** выполняется для базы данных.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-114">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="1a5b6-115">Если параметр `searchString` содержит строку, запрос фильмов изменяется для фильтрации по строке поиска:</span><span class="sxs-lookup"><span data-stu-id="1a5b6-115">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="1a5b6-116">Код `s => s.Title.Contains()` представляет собой [лямбда-выражение](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-116">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="1a5b6-117">Лямбда-выражения используются в запросах [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) на основе методов в качестве аргументов стандартных методов операторов запроса, таких как метод [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) или `Contains` (используется в предшествующем коде).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-117">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="1a5b6-118">Запросы LINQ не выполняются, если они определяются или изменяются путем вызова метода (например, `Where`, `Contains` или `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-118">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="1a5b6-119">Вместо этого выполнение запроса откладывается.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-119">Rather, query execution is deferred.</span></span> <span data-ttu-id="1a5b6-120">Это означает, что вычисление выражения откладывается до тех пор, пока не будет выполнена итерация его реализованного значения или не будет вызван метод `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-120">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="1a5b6-121">Дополнительные сведения см. в разделе [Выполнение запроса](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-121">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="1a5b6-122">**Примечание.** Метод [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) выполняется в базе данных, а не в коде C#.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-122">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="1a5b6-123">Регистр символов запроса учитывается в зависимости от параметров базы данных и сортировки.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-123">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="1a5b6-124">В SQL Server метод `Contains` сопоставляется с [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), в котором регистр символов не учитывается.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-124">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="1a5b6-125">В SQLite при параметрах сортировки по умолчанию регистр символов учитывается.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-125">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="1a5b6-126">Наконец, последняя строка метода `OnGetAsync` заполняет свойство `SearchString` искомым значением.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-126">Lastly, the final line of the `OnGetAsync` method populates the `SearchString` property with the user's search value.</span></span> <span data-ttu-id="1a5b6-127">Если свойство `SearchString` заполнено, искомое значение сохраняется в поле поиска после выполнения операции поиска.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-127">With the `SearchString` property populated, the search value is retained in the search box after the search executes.</span></span>

<span data-ttu-id="1a5b6-128">Перейдите на страницу Movies и добавьте строку запроса, такую как `?searchString=Ghost`, к URL-адресу (например, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-128">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="1a5b6-129">Отображаются отфильтрованные фильмы.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-129">The filtered movies are displayed.</span></span>

![Представление Index](search/_static/ghost.png)

<span data-ttu-id="1a5b6-131">Если на страницу Index добавлен следующий шаблон маршрута, строку поиска можно передать в виде сегмента URL-адреса (например, `http://localhost:5000/Movies/Ghost`).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-131">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/Ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="1a5b6-132">Предыдущее ограничение маршрута разрешало поиск названия в виде данных маршрута (сегмент URL-адреса) вместо значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-132">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="1a5b6-133">Символ `?` в `"{searchString?}"` означает, что этот параметр является необязательным.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-133">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Представление Index, в URL-адрес которого добавлено слово ghost, возвращает два фильма: Ghostbusters и Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="1a5b6-135">Тем не менее пользователи вряд ли будут изменять URL-адрес для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-135">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="1a5b6-136">На этом шаге добавляется пользовательский интерфейс для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-136">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="1a5b6-137">Если было добавлено ограничение маршрута `"{searchString?}"`, удалите его.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-137">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="1a5b6-138">Откройте файл *Pages/Movies/Index.cshtml* и добавьте разметку `<form>`, которая выделена в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="1a5b6-138">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="1a5b6-139">Тег HTML `<form>` использует [вспомогательную функцию тега Form](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="1a5b6-139">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="1a5b6-140">При отправке формы строка фильтра отправляется на страницу *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-140">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="1a5b6-141">Сохраните изменения и проверьте работу фильтра.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-141">Save the changes and test the filter.</span></span>

![Представление Index со словом ghost в текстовом поле фильтра по названию](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="1a5b6-143">Поиск по жанру</span><span class="sxs-lookup"><span data-stu-id="1a5b6-143">Search by genre</span></span>

<span data-ttu-id="1a5b6-144">Обновите метод `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="1a5b6-144">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="1a5b6-145">Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-145">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="1a5b6-146">Список жанров `SelectList` создается путем проецирования отдельных жанров.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-146">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="1a5b6-147">Добавление поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="1a5b6-147">Adding search by genre</span></span>

<span data-ttu-id="1a5b6-148">Обновите файл *Index.cshtml* следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1a5b6-148">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="1a5b6-149">Проверьте работу приложения, выполнив поиск по жанру, по названию фильма и по обоим этим параметрам.</span><span class="sxs-lookup"><span data-stu-id="1a5b6-149">Test the app by searching by genre, by movie title, and by both.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1a5b6-150">[Предыдущая статья — "Обновление страниц"](xref:tutorials/razor-pages/da1)
> [Следующая статья — "Добавление нового поля"](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="1a5b6-150">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
