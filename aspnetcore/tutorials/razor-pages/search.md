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
ms.openlocfilehash: 2ffb6f13a7303527444085d137d1acac02d7e0ef
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2017
---
# <a name="adding-search-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="43642-104">Добавление поиска в приложение ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="43642-104">Adding search to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="43642-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="43642-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="43642-106">В этом документе на страницу Index добавляется возможность поиска фильмов по *жанру* или *названию*.</span><span class="sxs-lookup"><span data-stu-id="43642-106">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="43642-107">Обновите метод `OnGetAsync` страницы Index, добавив следующий код:</span><span class="sxs-lookup"><span data-stu-id="43642-107">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="43642-108">В первой строке метода `OnGetAsync` создается запрос [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) для выбора фильмов:</span><span class="sxs-lookup"><span data-stu-id="43642-108">The first line of the `OnGetAsync` method creates a [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
 var movies = from m in _context.Movie
              select m;
```

<span data-ttu-id="43642-109">Этот запрос определяется *только* в этой точке и **не** выполняется для базы данных.</span><span class="sxs-lookup"><span data-stu-id="43642-109">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="43642-110">Если параметр `searchString` содержит строку, запрос фильмов изменяется для фильтрации по строке поиска:</span><span class="sxs-lookup"><span data-stu-id="43642-110">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="43642-111">Код `s => s.Title.Contains()` представляет собой [лямбда-выражение](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="43642-111">The `s => s.Title.Contains()` code is a [Lambda Expression](https://docs.microsoft.com/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="43642-112">Лямбда-выражения используются в запросах [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) на основе методов в качестве аргументов стандартных методов операторов запроса, таких как метод [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) или `Contains` (используется в предшествующем коде).</span><span class="sxs-lookup"><span data-stu-id="43642-112">Lambdas are used in method-based [LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="43642-113">Запросы LINQ не выполняются в том случае, если они определяются или изменяются путем вызова метода (например, `Where`, `Contains` или `OrderBy`).</span><span class="sxs-lookup"><span data-stu-id="43642-113">LINQ queries are not executed when they are defined or when they are modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="43642-114">Вместо этого выполнение запроса откладывается.</span><span class="sxs-lookup"><span data-stu-id="43642-114">Rather, query execution is deferred.</span></span> <span data-ttu-id="43642-115">Это означает, что вычисление выражения откладывается до тех пор, пока не будет выполнена итерация его реализованного значения или не будет вызван метод `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="43642-115">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="43642-116">Дополнительные сведения см. в разделе [Выполнение запроса](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="43642-116">See [Query Execution](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="43642-117">**Примечание.** Метод [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) выполняется в базе данных, а не в коде C#.</span><span class="sxs-lookup"><span data-stu-id="43642-117">**Note:** The [Contains](https://docs.microsoft.com//dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="43642-118">Регистр символов запроса учитывается в зависимости от параметров базы данных и сортировки.</span><span class="sxs-lookup"><span data-stu-id="43642-118">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="43642-119">В SQL Server метод `Contains` сопоставляется с [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), в котором регистр символов не учитывается.</span><span class="sxs-lookup"><span data-stu-id="43642-119">On SQL Server, `Contains` maps to [SQL LIKE](https://docs.microsoft.com/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="43642-120">В SQLite при параметрах сортировки по умолчанию регистр символов учитывается.</span><span class="sxs-lookup"><span data-stu-id="43642-120">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="43642-121">Перейдите на страницу Movies и добавьте строку запроса, такую как `?searchString=Ghost`, к URL-адресу (например, `http://localhost:5000/Movies?searchString=Ghost`).</span><span class="sxs-lookup"><span data-stu-id="43642-121">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="43642-122">Отображаются отфильтрованные фильмы.</span><span class="sxs-lookup"><span data-stu-id="43642-122">The filtered movies are displayed.</span></span>

![Представление Index](search/_static/ghost.png)

<span data-ttu-id="43642-124">Если на страницу Index добавлен следующий шаблон маршрута, строку поиска можно передать в виде сегмента URL-адреса (например, `http://localhost:5000/Movies/ghost`).</span><span class="sxs-lookup"><span data-stu-id="43642-124">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="43642-125">Предыдущее ограничение маршрута разрешало поиск названия в виде данных маршрута (сегмент URL-адреса) вместо значения строки запроса.</span><span class="sxs-lookup"><span data-stu-id="43642-125">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="43642-126">Символ `?` в `"{searchString?}"` означает, что этот параметр является необязательным.</span><span class="sxs-lookup"><span data-stu-id="43642-126">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Представление Index, в URL-адрес которого добавлено слово ghost, возвращает два фильма: Ghostbusters и Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="43642-128">Тем не менее пользователи вряд ли будут изменять URL-адрес для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="43642-128">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="43642-129">На этом шаге добавляется пользовательский интерфейс для поиска фильмов.</span><span class="sxs-lookup"><span data-stu-id="43642-129">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="43642-130">Если было добавлено ограничение маршрута `"{searchString?}"`, удалите его.</span><span class="sxs-lookup"><span data-stu-id="43642-130">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="43642-131">Откройте файл *Pages/Movies/Index.cshtml* и добавьте разметку `<form>`, которая выделена в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="43642-131">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="43642-132">Тег HTML `<form>` использует [вспомогательную функцию тега Form](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="43642-132">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="43642-133">При отправке формы строка фильтра отправляется на страницу *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="43642-133">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="43642-134">Сохраните изменения и проверьте работу фильтра.</span><span class="sxs-lookup"><span data-stu-id="43642-134">Save the changes and test the filter.</span></span>

![Представление Index со словом ghost в текстовом поле фильтра по названию](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="43642-136">Поиск по жанру</span><span class="sxs-lookup"><span data-stu-id="43642-136">Search by genre</span></span>

<span data-ttu-id="43642-137">Добавьте следующие выделенные свойства в файл *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="43642-137">Add the the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-)]

<span data-ttu-id="43642-138">Свойство `SelectList Genres` содержит список жанров.</span><span class="sxs-lookup"><span data-stu-id="43642-138">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="43642-139">В этом списке пользователь может выбрать жанр фильма.</span><span class="sxs-lookup"><span data-stu-id="43642-139">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="43642-140">Свойство `MovieGenre` содержит конкретный жанр, выбранный пользователем, например "Western" (Вестерн).</span><span class="sxs-lookup"><span data-stu-id="43642-140">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="43642-141">Обновите метод `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="43642-141">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="43642-142">Следующий код определяет запрос LINQ, который извлекает все жанры из базы данных.</span><span class="sxs-lookup"><span data-stu-id="43642-142">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="43642-143">Список жанров `SelectList` создается путем проецирования отдельных жанров.</span><span class="sxs-lookup"><span data-stu-id="43642-143">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There is no start line.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="43642-144">Добавление поиска по жанру</span><span class="sxs-lookup"><span data-stu-id="43642-144">Adding search by genre</span></span>

<span data-ttu-id="43642-145">Обновите файл *Index.cshtml* следующим образом:</span><span class="sxs-lookup"><span data-stu-id="43642-145">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="43642-146">Проверьте работу приложения, выполнив поиск по жанру, по названию фильма и по обоим этим параметрам.</span><span class="sxs-lookup"><span data-stu-id="43642-146">Test the app by searching by genre, by movie title, and by both.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="43642-147">[Предыдущая статья — "Обновление страниц"](xref:tutorials/razor-pages/da1)
[Следующая статья — "Добавление нового поля"](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="43642-147">[Previous: Updating the pages](xref:tutorials/razor-pages/da1)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
