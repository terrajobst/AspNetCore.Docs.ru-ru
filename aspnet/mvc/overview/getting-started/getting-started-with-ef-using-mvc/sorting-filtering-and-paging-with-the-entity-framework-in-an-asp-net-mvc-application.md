---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Учебник. Добавить сортировку, фильтрацию и разбиение по страницам с Entity Framework в приложении ASP.NET MVC | Документация Майкрософт
author: tdykstra
description: В этом руководстве описано добавление сортировки, фильтрации и разбиения по страницам для **учащихся** страницы индекса. Можно также создать страницу простой группировкой.
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: b7b5d3d3931f752f2effc044ca8cc52eab22da0a
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889864"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="0dde3-104">Учебник. Добавить сортировку, фильтрацию и разбиение по страницам с Entity Framework в приложении ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="0dde3-104">Tutorial: Add sorting, filtering, and paging with the Entity Framework in an ASP.NET MVC application</span></span>

<span data-ttu-id="0dde3-105">В [предыдущем учебном курсе](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), реализован набор веб-страниц для основных операций CRUD для `Student` сущностей.</span><span class="sxs-lookup"><span data-stu-id="0dde3-105">In the [previous tutorial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md), you implemented a set of web pages for basic CRUD operations for `Student` entities.</span></span> <span data-ttu-id="0dde3-106">В этом руководстве описано добавление сортировки, фильтрации и разбиения по страницам для **учащихся** страницы индекса.</span><span class="sxs-lookup"><span data-stu-id="0dde3-106">In this tutorial you add sorting, filtering, and paging functionality to the **Students** Index page.</span></span> <span data-ttu-id="0dde3-107">Можно также создать страницу простой группировкой.</span><span class="sxs-lookup"><span data-stu-id="0dde3-107">You also create a simple grouping page.</span></span>

<span data-ttu-id="0dde3-108">Ниже показано, что страница будет выглядеть так, когда все будет готово.</span><span class="sxs-lookup"><span data-stu-id="0dde3-108">The following image shows what the page will look like when you're done.</span></span> <span data-ttu-id="0dde3-109">Заголовки столбцов являются ссылками, при нажатии на которые происходит сортировка по этому столбцу.</span><span class="sxs-lookup"><span data-stu-id="0dde3-109">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="0dde3-110">При повторном нажатии на заголовок происходит переключение между сортировкой по возрастанию и убыванию.</span><span class="sxs-lookup"><span data-stu-id="0dde3-110">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

<span data-ttu-id="0dde3-112">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="0dde3-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0dde3-113">Добавление ссылки сортировки столбцов</span><span class="sxs-lookup"><span data-stu-id="0dde3-113">Add column sort links</span></span>
> * <span data-ttu-id="0dde3-114">Добавление поля поиска</span><span class="sxs-lookup"><span data-stu-id="0dde3-114">Add a Search box</span></span>
> * <span data-ttu-id="0dde3-115">Добавление разбиения по страницам</span><span class="sxs-lookup"><span data-stu-id="0dde3-115">Add paging</span></span>
> * <span data-ttu-id="0dde3-116">Создание страницы About</span><span class="sxs-lookup"><span data-stu-id="0dde3-116">Create an About page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0dde3-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0dde3-117">Prerequisites</span></span>

* [<span data-ttu-id="0dde3-118">Реализация базовой функциональности CRUD</span><span class="sxs-lookup"><span data-stu-id="0dde3-118">Implementing Basic CRUD Functionality</span></span>](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a><span data-ttu-id="0dde3-119">Добавление ссылки сортировки столбцов</span><span class="sxs-lookup"><span data-stu-id="0dde3-119">Add column sort links</span></span>

<span data-ttu-id="0dde3-120">Добавление сортировки на страницу указателя учащихся изменим `Index` метод `Student` контроллера и добавьте код в `Student` Индексируйте представление.</span><span class="sxs-lookup"><span data-stu-id="0dde3-120">To add sorting to the Student Index page, you'll change the `Index` method of the `Student` controller and add code to the `Student` Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="0dde3-121">Добавление функции сортировки в метод Index</span><span class="sxs-lookup"><span data-stu-id="0dde3-121">Add sorting functionality to the Index method</span></span>

- <span data-ttu-id="0dde3-122">В *Controllers\StudentController.cs*, замените `Index` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0dde3-122">In *Controllers\StudentController.cs*, replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="0dde3-123">Этот код принимает параметр `sortOrder` из строки запроса в URL.</span><span class="sxs-lookup"><span data-stu-id="0dde3-123">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="0dde3-124">Значение строки запроса, предоставляемые ASP.NET MVC в качестве параметра метода действия.</span><span class="sxs-lookup"><span data-stu-id="0dde3-124">The query string value is provided by ASP.NET MVC as a parameter to the action method.</span></span> <span data-ttu-id="0dde3-125">Параметр — это строка, которая является «Name» или «Date» (необязательно) за которым следует символ подчеркивания и строки «desc», чтобы указать порядок по убыванию.</span><span class="sxs-lookup"><span data-stu-id="0dde3-125">The parameter is a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="0dde3-126">По умолчанию используется порядок сортировки по возрастанию.</span><span class="sxs-lookup"><span data-stu-id="0dde3-126">The default sort order is ascending.</span></span>

<span data-ttu-id="0dde3-127">При первом запросе страницы Index строка запроса отсутствует.</span><span class="sxs-lookup"><span data-stu-id="0dde3-127">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="0dde3-128">Учащиеся отображаются в порядке возрастания значений `LastName`, который используется по умолчанию, установленные по фамилиям в `switch` инструкции.</span><span class="sxs-lookup"><span data-stu-id="0dde3-128">The students are displayed in ascending order by `LastName`, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="0dde3-129">Когда пользователь щелкает гиперссылку заголовка столбца, в строку запроса подставляется соответствующее значение параметра `sortOrder`.</span><span class="sxs-lookup"><span data-stu-id="0dde3-129">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="0dde3-130">Два `ViewBag` переменные используются, чтобы представление можно настроить гиперссылок в заголовки столбцов с соответствующими значениями строки запроса:</span><span class="sxs-lookup"><span data-stu-id="0dde3-130">The two `ViewBag` variables are used so that the view can configure the column heading hyperlinks with the appropriate query string values:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="0dde3-131">Это тернарные условные операторы.</span><span class="sxs-lookup"><span data-stu-id="0dde3-131">These are ternary statements.</span></span> <span data-ttu-id="0dde3-132">Первый из них указывает, что если `sortOrder` параметр равен null или пусто, `ViewBag.NameSortParm` должно быть присвоено «имя\_desc»; в противном случае задается пустая строка.</span><span class="sxs-lookup"><span data-stu-id="0dde3-132">The first one specifies that if the `sortOrder` parameter is null or empty, `ViewBag.NameSortParm` should be set to "name\_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="0dde3-133">Следующие два оператора устанавливают гиперссылки в заголовках столбцов в представлении следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0dde3-133">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

| <span data-ttu-id="0dde3-134">Текущий порядок сортировки</span><span class="sxs-lookup"><span data-stu-id="0dde3-134">Current sort order</span></span> | <span data-ttu-id="0dde3-135">Гиперссылка "Last Name" (Фамилия)</span><span class="sxs-lookup"><span data-stu-id="0dde3-135">Last Name Hyperlink</span></span> | <span data-ttu-id="0dde3-136">Гиперссылка "Date" (Дата)</span><span class="sxs-lookup"><span data-stu-id="0dde3-136">Date Hyperlink</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0dde3-137">"Last Name" (Фамилия) по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-137">Last Name ascending</span></span> | <span data-ttu-id="0dde3-138">по убыванию</span><span class="sxs-lookup"><span data-stu-id="0dde3-138">descending</span></span> | <span data-ttu-id="0dde3-139">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-139">ascending</span></span> |
| <span data-ttu-id="0dde3-140">"Last Name" (Фамилия) по убыванию</span><span class="sxs-lookup"><span data-stu-id="0dde3-140">Last Name descending</span></span> | <span data-ttu-id="0dde3-141">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-141">ascending</span></span> | <span data-ttu-id="0dde3-142">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-142">ascending</span></span> |
| <span data-ttu-id="0dde3-143">"Date" (Дата) по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-143">Date ascending</span></span> | <span data-ttu-id="0dde3-144">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-144">ascending</span></span> | <span data-ttu-id="0dde3-145">по убыванию</span><span class="sxs-lookup"><span data-stu-id="0dde3-145">descending</span></span> |
| <span data-ttu-id="0dde3-146">"Date" (Дата) по убыванию</span><span class="sxs-lookup"><span data-stu-id="0dde3-146">Date descending</span></span> | <span data-ttu-id="0dde3-147">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-147">ascending</span></span> | <span data-ttu-id="0dde3-148">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="0dde3-148">ascending</span></span> |

<span data-ttu-id="0dde3-149">Данный метод использует [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) позволяет выбрать столбец для сортировки.</span><span class="sxs-lookup"><span data-stu-id="0dde3-149">The method uses [LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities) to specify the column to sort by.</span></span> <span data-ttu-id="0dde3-150">Код создает <xref:System.Linq.IQueryable%601> переменной перед `switch` инструкция, изменяет его в `switch` инструкции и вызовы `ToList` метод после `switch` инструкции.</span><span class="sxs-lookup"><span data-stu-id="0dde3-150">The code creates an <xref:System.Linq.IQueryable%601> variable before the `switch` statement, modifies it in the `switch` statement, and calls the `ToList` method after the `switch` statement.</span></span> <span data-ttu-id="0dde3-151">После создания и изменения переменных `IQueryable` запрос в базу данных не отправляется.</span><span class="sxs-lookup"><span data-stu-id="0dde3-151">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="0dde3-152">Запрос не выполняется, пока вы не преобразуете `IQueryable` объект в коллекцию путем вызова метода, например `ToList`.</span><span class="sxs-lookup"><span data-stu-id="0dde3-152">The query is not executed until you convert the `IQueryable` object into a collection by calling a method such as `ToList`.</span></span> <span data-ttu-id="0dde3-153">Таким образом, этот код вызывает в одном запросе, не выполняется до `return View` инструкции.</span><span class="sxs-lookup"><span data-stu-id="0dde3-153">Therefore, this code results in a single query that is not executed until the `return View` statement.</span></span>

<span data-ttu-id="0dde3-154">Вместо написания различных операторов LINQ для каждого порядка сортировки можно динамически создать оператор LINQ.</span><span class="sxs-lookup"><span data-stu-id="0dde3-154">As an alternative to writing different LINQ statements for each sort order, you can dynamically create a LINQ statement.</span></span> <span data-ttu-id="0dde3-155">Сведения о динамических LINQ см. в разделе [динамического LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span><span class="sxs-lookup"><span data-stu-id="0dde3-155">For information about dynamic LINQ, see [Dynamic LINQ](https://go.microsoft.com/fwlink/?LinkID=323957).</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="0dde3-156">Добавление гиперссылок в заголовки столбцов в представление Student index</span><span class="sxs-lookup"><span data-stu-id="0dde3-156">Add column heading hyperlinks to the Student index view</span></span>

1. <span data-ttu-id="0dde3-157">В *Views\Student\Index.cshtml*, замените `<tr>` и `<th>` элементов для строки заголовков с выделенный код:</span><span class="sxs-lookup"><span data-stu-id="0dde3-157">In *Views\Student\Index.cshtml*, replace the `<tr>` and `<th>` elements for the heading row with the highlighted code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   <span data-ttu-id="0dde3-158">Этот код использует эти сведения в `ViewBag` свойства для настройки гиперссылок с использованием соответствующего запроса строковые значения.</span><span class="sxs-lookup"><span data-stu-id="0dde3-158">This code uses the information in the `ViewBag` properties to set up hyperlinks with the appropriate query string values.</span></span>

2. <span data-ttu-id="0dde3-159">Откройте страницу и щелкните **Фамилия** и **Дата регистрации** заголовки столбцов, чтобы убедитесь, что Сортировка работает.</span><span class="sxs-lookup"><span data-stu-id="0dde3-159">Run the page and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

   <span data-ttu-id="0dde3-160">После того как вы щелкнете **Фамилия** заголовок, учащиеся отображаются по убыванию последнее имя.</span><span class="sxs-lookup"><span data-stu-id="0dde3-160">After you click the **Last Name** heading, students are displayed in descending last name order.</span></span>

## <a name="add-a-search-box"></a><span data-ttu-id="0dde3-161">Добавление поля поиска</span><span class="sxs-lookup"><span data-stu-id="0dde3-161">Add a Search box</span></span>

<span data-ttu-id="0dde3-162">Для добавления фильтра на страницу индекса учащихся, вы добавите в представление текстовое поле и кнопка отправки и внести соответствующие изменения в `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="0dde3-162">To add filtering to the Students index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="0dde3-163">Текстовое поле позволяет ввести строку для поиска в имя полях имени и фамилии.</span><span class="sxs-lookup"><span data-stu-id="0dde3-163">The text box lets you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="0dde3-164">Добавление функций фильтрации в метод Index</span><span class="sxs-lookup"><span data-stu-id="0dde3-164">Add filtering functionality to the Index method</span></span>

- <span data-ttu-id="0dde3-165">В *Controllers\StudentController.cs*, замените `Index` метод (изменения выделены) следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0dde3-165">In *Controllers\StudentController.cs*, replace the `Index` method with the following code (the changes are highlighted):</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

<span data-ttu-id="0dde3-166">Код добавляет `searchString` параметр `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="0dde3-166">The code adds a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="0dde3-167">Значение строки поиска получается из текстового поля, которое мы добавили в представление Index.</span><span class="sxs-lookup"><span data-stu-id="0dde3-167">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="0dde3-168">Он также добавляет `where` предложение в оператор LINQ, которое отбирает только студентов, чье имя или Фамилия содержат строку поиска.</span><span class="sxs-lookup"><span data-stu-id="0dde3-168">It also adds a `where` clause to the LINQ statement that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="0dde3-169">Оператор, который добавляет <xref:System.Linq.Queryable.Where%2A> предложение выполняется только в том случае, если отсутствует значение для поиска.</span><span class="sxs-lookup"><span data-stu-id="0dde3-169">The statement that adds the <xref:System.Linq.Queryable.Where%2A> clause executes only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="0dde3-170">Во многих случаях можно вызвать тот же метод, либо на набор сущностей Entity Framework, либо как метода расширения для коллекции в памяти.</span><span class="sxs-lookup"><span data-stu-id="0dde3-170">In many cases you can call the same method either on an Entity Framework entity set or as an extension method on an in-memory collection.</span></span> <span data-ttu-id="0dde3-171">Обычно такие же результаты, но в некоторых случаях может отличаться.</span><span class="sxs-lookup"><span data-stu-id="0dde3-171">The results are normally the same but in some cases may be different.</span></span>
>
> <span data-ttu-id="0dde3-172">Например, реализация .NET Framework `Contains` метод возвращает все строки, передать пустую строку, когда поставщик Entity Framework для SQL Server Compact 4.0 не возвращает строки, наличие пустых строк.</span><span class="sxs-lookup"><span data-stu-id="0dde3-172">For example, the .NET Framework implementation of the `Contains` method returns all rows when you pass an empty string to it, but the Entity Framework provider for SQL Server Compact 4.0 returns zero rows for empty strings.</span></span> <span data-ttu-id="0dde3-173">Поэтому код в примере (размещение `Where` инструкции внутри `if` инструкции) гарантирует, что вы получите те же результаты для всех версий SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0dde3-173">Therefore the code in the example (putting the `Where` statement inside an `if` statement) makes sure that you get the same results for all versions of SQL Server.</span></span> <span data-ttu-id="0dde3-174">Кроме того, реализация .NET Framework `Contains` метод по умолчанию выполняет сравнение с учетом регистра, но поставщики Entity Framework SQL Server по умолчанию выполняют сравнения без учета регистра.</span><span class="sxs-lookup"><span data-stu-id="0dde3-174">Also, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but Entity Framework SQL Server providers perform case-insensitive comparisons by default.</span></span> <span data-ttu-id="0dde3-175">Таким образом, вызов `ToUpper` метод производится проверка явно регистронезависимым гарантирует, что результаты не изменяются при изменении коду позднее использовать хранилище, которое будет возвращать `IEnumerable` , а не `IQueryable` объекта.</span><span class="sxs-lookup"><span data-stu-id="0dde3-175">Therefore, calling the `ToUpper` method to make the test explicitly case-insensitive ensures that results do not change when you change the code later to use a repository, which will return an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="0dde3-176">(При вызове метода `Contains` коллекции `IEnumerable` выполняется реализация .NET Framework; при вызове этого же метода у объекта `IQueryable` выполняется реализация поставщика базы данных.)</span><span class="sxs-lookup"><span data-stu-id="0dde3-176">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.)</span></span>
>
> <span data-ttu-id="0dde3-177">Обработка NULL также может быть разным для разных поставщиков баз данных или при использовании `IQueryable` сравниваемый объект при использовании `IEnumerable` коллекции.</span><span class="sxs-lookup"><span data-stu-id="0dde3-177">Null handling may also be different for different database providers or when you use an `IQueryable` object compared to when you use an `IEnumerable` collection.</span></span> <span data-ttu-id="0dde3-178">Например, в некоторых сценариях `Where` условия, такие как `table.Column != 0` не может возвращать столбцы, имеющие `null` как значение.</span><span class="sxs-lookup"><span data-stu-id="0dde3-178">For example, in some scenarios a `Where` condition such as `table.Column != 0` may not return columns that have `null` as the value.</span></span> <span data-ttu-id="0dde3-179">Дополнительные сведения см. в разделе [неправильной обработкой null переменных в предложение» where"](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span><span class="sxs-lookup"><span data-stu-id="0dde3-179">For more information, see [Incorrect handling of null variables in 'where' clause](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="0dde3-180">Добавление поля поиска в представление Student index</span><span class="sxs-lookup"><span data-stu-id="0dde3-180">Add a search box to the Student index view</span></span>

1. <span data-ttu-id="0dde3-181">В *Views\Student\Index.cshtml*, добавьте выделенный код непосредственно перед открытием `table` тег для создания заголовка, текстового поля и **поиска** кнопку.</span><span class="sxs-lookup"><span data-stu-id="0dde3-181">In *Views\Student\Index.cshtml*, add the highlighted code immediately before the opening `table` tag in order to create a caption, a text box, and a **Search** button.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. <span data-ttu-id="0dde3-182">Запустить эту страницу, введите строку поиска и нажмите кнопку **поиска** для проверки работы фильтра.</span><span class="sxs-lookup"><span data-stu-id="0dde3-182">Run the page, enter a search string, and click **Search** to verify that filtering is working.</span></span>

   <span data-ttu-id="0dde3-183">Обратите внимание на то, что URL-адрес не содержит «» строка поиска, это означает, что если закладку на данной странице, вы не получите отфильтрованного списка при использовании закладки.</span><span class="sxs-lookup"><span data-stu-id="0dde3-183">Notice the URL doesn't contain the "an" search string, which means that if you bookmark this page, you won't get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="0dde3-184">Это также касается ссылки сортировки столбцов, так как они будут отсортированы всего списка.</span><span class="sxs-lookup"><span data-stu-id="0dde3-184">This applies also to the column sort links, as they will sort the whole list.</span></span> <span data-ttu-id="0dde3-185">Вам предстоит изменить **поиска** кнопку, чтобы использовать строки запроса для отбора позже в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="0dde3-185">You'll change the **Search** button to use query strings for filter criteria later in the tutorial.</span></span>

## <a name="add-paging"></a><span data-ttu-id="0dde3-186">Добавление разбиения по страницам</span><span class="sxs-lookup"><span data-stu-id="0dde3-186">Add paging</span></span>

<span data-ttu-id="0dde3-187">Чтобы добавить на страницу индекса учащихся разбиения на страницы, необходимо начать с установки **PagedList.Mvc** пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="0dde3-187">To add paging to the Students index page, you'll start by installing the **PagedList.Mvc** NuGet package.</span></span> <span data-ttu-id="0dde3-188">Затем мы внесем дополнительные изменения в `Index` метод и добавьте ссылки перелистывания, чтобы `Index` представления.</span><span class="sxs-lookup"><span data-stu-id="0dde3-188">Then you'll make additional changes in the `Index` method and add paging links to the `Index` view.</span></span> <span data-ttu-id="0dde3-189">**PagedList.Mvc** является одним из многих хороший по страницам и упорядочения пакеты для ASP.NET MVC и его использование здесь предназначен только в качестве примера, не в виде рекомендаций для него другими вариантами.</span><span class="sxs-lookup"><span data-stu-id="0dde3-189">**PagedList.Mvc** is one of many good paging and sorting packages for ASP.NET MVC, and its use here is intended only as an example, not as a recommendation for it over other options.</span></span>

### <a name="install-the-pagedlistmvc-nuget-package"></a><span data-ttu-id="0dde3-190">Установите пакет PagedList.MVC NuGet</span><span class="sxs-lookup"><span data-stu-id="0dde3-190">Install the PagedList.MVC NuGet package</span></span>

<span data-ttu-id="0dde3-191">NuGet **PagedList.Mvc** пакет автоматически устанавливает **PagedList** пакет как зависимость.</span><span class="sxs-lookup"><span data-stu-id="0dde3-191">The NuGet **PagedList.Mvc** package automatically installs the **PagedList** package as a dependency.</span></span> <span data-ttu-id="0dde3-192">**PagedList** пакет устанавливает `PagedList` коллекции типа и методы расширения для `IQueryable` и `IEnumerable` коллекций.</span><span class="sxs-lookup"><span data-stu-id="0dde3-192">The **PagedList** package installs a `PagedList` collection type and extension methods for `IQueryable` and `IEnumerable` collections.</span></span> <span data-ttu-id="0dde3-193">Методы расширения создают одной странице данных в `PagedList` коллекции из вашей `IQueryable` или `IEnumerable`и `PagedList` коллекции предоставляет несколько свойств и методов, предназначенных для упрощения разбиения на страницы.</span><span class="sxs-lookup"><span data-stu-id="0dde3-193">The extension methods create a single page of data in a `PagedList` collection out of your `IQueryable` or `IEnumerable`, and the `PagedList` collection provides several properties and methods that facilitate paging.</span></span> <span data-ttu-id="0dde3-194">**PagedList.Mvc** пакет устанавливает вспомогательный объект разбиения на страницы, отображающий кнопки перелистывания.</span><span class="sxs-lookup"><span data-stu-id="0dde3-194">The **PagedList.Mvc** package installs a paging helper that displays the paging buttons.</span></span>

1. <span data-ttu-id="0dde3-195">Из **средства** меню, выберите **диспетчер пакетов NuGet** и затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="0dde3-195">From the **Tools** menu, select **NuGet Package Manager** and then **Package Manager Console**.</span></span>

2. <span data-ttu-id="0dde3-196">В **консоль диспетчера пакетов** окна, убедитесь, что **источник пакета** — **nuget.org** и **проект по умолчанию** — **ContosoUniversity**, а затем введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0dde3-196">In the **Package Manager Console** window, make sure the **Package source** is **nuget.org** and the **Default project** is **ContosoUniversity**, and then enter the following command:</span></span>

   ```text
   Install-Package PagedList.Mvc
   ```

3. <span data-ttu-id="0dde3-197">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="0dde3-197">Build the project.</span></span>

### <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="0dde3-198">Добавление разбиения на страницы в метод Index</span><span class="sxs-lookup"><span data-stu-id="0dde3-198">Add paging functionality to the Index method</span></span>

1. <span data-ttu-id="0dde3-199">В *Controllers\StudentController.cs*, добавьте `using` инструкции для `PagedList` пространство имен:</span><span class="sxs-lookup"><span data-stu-id="0dde3-199">In *Controllers\StudentController.cs*, add a `using` statement for the `PagedList` namespace:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. <span data-ttu-id="0dde3-200">Замените метод `Index` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0dde3-200">Replace the `Index` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   <span data-ttu-id="0dde3-201">Этот код добавляет `page` параметра, параметр текущий порядок сортировки и текущий параметр фильтра в сигнатуру метода:</span><span class="sxs-lookup"><span data-stu-id="0dde3-201">This code adds a `page` parameter, a current sort order parameter, and a current filter parameter to the method signature:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   <span data-ttu-id="0dde3-202">При первом отображении страницы, или если пользователь не щелкнет ссылку перелистывания или сортировки, все параметры имеют значение null.</span><span class="sxs-lookup"><span data-stu-id="0dde3-202">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters are null.</span></span> <span data-ttu-id="0dde3-203">При выборе ссылки перелистывания, `page` переменная содержит номер страницы для отображения.</span><span class="sxs-lookup"><span data-stu-id="0dde3-203">If a paging link is clicked, the `page` variable contains the page number to display.</span></span>

   <span data-ttu-id="0dde3-204">Объект `ViewBag` свойство обеспечивает представление с помощью текущий порядок сортировки, так как он должен быть включен в ссылки перелистывания, чтобы сохранить порядок сортировки, то же, при разбиении по страницам:</span><span class="sxs-lookup"><span data-stu-id="0dde3-204">A `ViewBag` property provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   <span data-ttu-id="0dde3-205">Еще одно свойство, `ViewBag.CurrentFilter`, обеспечивает представление текущую строку фильтра.</span><span class="sxs-lookup"><span data-stu-id="0dde3-205">Another property, `ViewBag.CurrentFilter`, provides the view with the current filter string.</span></span> <span data-ttu-id="0dde3-206">Это значение необходимо включить в ссылки для перелистывания, чтобы при смене страницы сохранить настройки фильтра, кроме того, необходимо восстановить значение фильтра в текстовом поле после обновления страницы.</span><span class="sxs-lookup"><span data-stu-id="0dde3-206">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span> <span data-ttu-id="0dde3-207">Если строка поиска изменяется во время перелистывания, то номер страницы должен быть сброшен на 1, так как с новым фильтром изменится состав отображаемых данных.</span><span class="sxs-lookup"><span data-stu-id="0dde3-207">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="0dde3-208">Строка поиска изменяется в том случае, когда значение, введенное в текстовое поле и нажатии кнопки отправки.</span><span class="sxs-lookup"><span data-stu-id="0dde3-208">The search string is changed when a value is entered in the text box and the submit button is pressed.</span></span> <span data-ttu-id="0dde3-209">В этом случае `searchString` параметра не равно null.</span><span class="sxs-lookup"><span data-stu-id="0dde3-209">In that case, the `searchString` parameter is not null.</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   <span data-ttu-id="0dde3-210">В конце метода `ToPagedList` метод расширения для учащихся `IQueryable` объект преобразует результат запроса студентов к одной странице студентов в тип коллекции, который поддерживает разбиение по страницам.</span><span class="sxs-lookup"><span data-stu-id="0dde3-210">At the end of the method, the `ToPagedList` extension method on the students `IQueryable` object converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="0dde3-211">Этот страница со студентами затем передается в представление:</span><span class="sxs-lookup"><span data-stu-id="0dde3-211">That single page of students is then passed to the view:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   <span data-ttu-id="0dde3-212">Метод `ToPagedList` принимает номер страницы.</span><span class="sxs-lookup"><span data-stu-id="0dde3-212">The `ToPagedList` method takes a page number.</span></span> <span data-ttu-id="0dde3-213">Два вопросительных [оператор объединения с null](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span><span class="sxs-lookup"><span data-stu-id="0dde3-213">The two question marks represent the [null-coalescing operator](/dotnet/csharp/language-reference/operators/null-coalescing-operator).</span></span> <span data-ttu-id="0dde3-214">Этот оператор определяет значение по умолчанию для значения null; выражение `(page ?? 1)` возвращает значение переменной `page`, если она имеет значение, и возвращает 1, если переменная `page` имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="0dde3-214">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

### <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="0dde3-215">Добавить ссылки на разбиение на страницы на страницу индекса учащихся</span><span class="sxs-lookup"><span data-stu-id="0dde3-215">Add paging links to the Student index view</span></span>

1. <span data-ttu-id="0dde3-216">В *Views\Student\Index.cshtml*, замените существующий код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="0dde3-216">In *Views\Student\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="0dde3-217">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="0dde3-217">The changes are highlighted.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   <span data-ttu-id="0dde3-218">Оператор `@model` в начале страницы указывает на то, что теперь представление принимает объект `PagedList`, а не объект `List`.</span><span class="sxs-lookup"><span data-stu-id="0dde3-218">The `@model` statement at the top of the page specifies that the view now gets a `PagedList` object instead of a `List` object.</span></span>

   <span data-ttu-id="0dde3-219">`using` Инструкции для `PagedList.Mvc` предоставляет доступ к вспомогательного приложения MVC для кнопки перелистывания.</span><span class="sxs-lookup"><span data-stu-id="0dde3-219">The `using` statement for `PagedList.Mvc` gives access to the MVC helper for the paging buttons.</span></span>

   <span data-ttu-id="0dde3-220">Кода используется перегруженная версия [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) , позволяющее указать [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span><span class="sxs-lookup"><span data-stu-id="0dde3-220">The code uses an overload of [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) that allows it to specify [FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100)).</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   <span data-ttu-id="0dde3-221">Значение по умолчанию [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) отправляет данные формы с помощью запроса POST, это означает, что параметры передаются в тексте сообщения HTTP, а не в URL-адрес как строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0dde3-221">The default [BeginForm](/previous-versions/aspnet/dd492719(v=vs.108)) submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="0dde3-222">При указании метода HTTP GET данные формы передаются в URL-адресе в виде строк запроса, что позволяет добавлять URL-адреса в закладки.</span><span class="sxs-lookup"><span data-stu-id="0dde3-222">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="0dde3-223">[Руководства консорциума W3C для использования HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) рекомендуем следует использовать GET, когда действие не приводит к обновлению.</span><span class="sxs-lookup"><span data-stu-id="0dde3-223">The [W3C guidelines for the use of HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recommend that you should use GET when the action does not result in an update.</span></span>

   <span data-ttu-id="0dde3-224">Текстовое поле инициализируется текущую строку поиска, щелкнув новую страницу можно увидеть текущую строку поиска.</span><span class="sxs-lookup"><span data-stu-id="0dde3-224">The text box is initialized with the current search string so when you click a new page you can see the current search string.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   <span data-ttu-id="0dde3-225">Ссылки в заголовках столбцов передают в контроллер при помощи строки запроса текущее значение строки поиска, чтобы пользователь мог сортировать отфильтрованные данные:</span><span class="sxs-lookup"><span data-stu-id="0dde3-225">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   <span data-ttu-id="0dde3-226">Отображаются текущей страницы и общее число страниц.</span><span class="sxs-lookup"><span data-stu-id="0dde3-226">The current page and total number of pages are displayed.</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   <span data-ttu-id="0dde3-227">Если отсутствуют страницы для отображения, отображается «Страница 0 0».</span><span class="sxs-lookup"><span data-stu-id="0dde3-227">If there are no pages to display, "Page 0 of 0" is shown.</span></span> <span data-ttu-id="0dde3-228">(В этом случае номер страницы больше, чем количество страниц из-за `Model.PageNumber` -1, и `Model.PageCount` равно 0.)</span><span class="sxs-lookup"><span data-stu-id="0dde3-228">(In that case the page number is greater than the page count because `Model.PageNumber` is 1, and `Model.PageCount` is 0.)</span></span>

   <span data-ttu-id="0dde3-229">Кнопки перелистывания отображаются по `PagedListPager` вспомогательный:</span><span class="sxs-lookup"><span data-stu-id="0dde3-229">The paging buttons are displayed by the `PagedListPager` helper:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   <span data-ttu-id="0dde3-230">`PagedListPager` Вспомогательный предоставляет ряд возможностей, которые можно настроить, включая URL-адреса и оформление.</span><span class="sxs-lookup"><span data-stu-id="0dde3-230">The `PagedListPager` helper provides a number of options that you can customize, including URLs and styling.</span></span> <span data-ttu-id="0dde3-231">Дополнительные сведения см. в разделе [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="0dde3-231">For more information, see [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) on the GitHub site.</span></span>

2. <span data-ttu-id="0dde3-232">Откройте страницу.</span><span class="sxs-lookup"><span data-stu-id="0dde3-232">Run the page.</span></span>

   <span data-ttu-id="0dde3-233">Чтобы убедиться, что постраничный просмотр работает, нажимайте кнопки перелистывания при различном порядке сортировки.</span><span class="sxs-lookup"><span data-stu-id="0dde3-233">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="0dde3-234">Затем введите строку поиска и повторите перелистывание, чтобы убедиться, что разбиение на страницы работает корректно вместе с сортировкой и фильтрацией.</span><span class="sxs-lookup"><span data-stu-id="0dde3-234">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page"></a><span data-ttu-id="0dde3-235">Создание страницы About</span><span class="sxs-lookup"><span data-stu-id="0dde3-235">Create an About page</span></span>

<span data-ttu-id="0dde3-236">Для университета Contoso веб сайта о странице как отображать количество учащихся поданы заявки на каждую дату регистрации.</span><span class="sxs-lookup"><span data-stu-id="0dde3-236">For the Contoso University website's About page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="0dde3-237">Для этого понадобится группировка и выполнение простых расчетов в группах.</span><span class="sxs-lookup"><span data-stu-id="0dde3-237">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="0dde3-238">Для выполнения этой задачи нам потребуется следующее:</span><span class="sxs-lookup"><span data-stu-id="0dde3-238">To accomplish this, you'll do the following:</span></span>

- <span data-ttu-id="0dde3-239">Создать класс модели представления для данных, которые необходимо передать в представление.</span><span class="sxs-lookup"><span data-stu-id="0dde3-239">Create a view model class for the data that you need to pass to the view.</span></span>
- <span data-ttu-id="0dde3-240">Изменить `About` метод в `Home` контроллера.</span><span class="sxs-lookup"><span data-stu-id="0dde3-240">Modify the `About` method in the `Home` controller.</span></span>
- <span data-ttu-id="0dde3-241">Изменить `About` представления.</span><span class="sxs-lookup"><span data-stu-id="0dde3-241">Modify the `About` view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="0dde3-242">Создание модели представления</span><span class="sxs-lookup"><span data-stu-id="0dde3-242">Create the View Model</span></span>

<span data-ttu-id="0dde3-243">Создание *ViewModels* папку в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="0dde3-243">Create a *ViewModels* folder in the project folder.</span></span> <span data-ttu-id="0dde3-244">В этой папке добавьте файл класса *EnrollmentDateGroup.cs* и замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0dde3-244">In that folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="0dde3-245">Изменение контроллера Home</span><span class="sxs-lookup"><span data-stu-id="0dde3-245">Modify the Home Controller</span></span>

1. <span data-ttu-id="0dde3-246">В *HomeController.cs*, добавьте следующий `using` инструкций в верхней части файла:</span><span class="sxs-lookup"><span data-stu-id="0dde3-246">In *HomeController.cs*, add the following `using` statements at the top of the file:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. <span data-ttu-id="0dde3-247">Добавьте переменную класса для контекста базы данных сразу после открывающей фигурной скобки для класса:</span><span class="sxs-lookup"><span data-stu-id="0dde3-247">Add a class variable for the database context immediately after the opening curly brace for the class:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. <span data-ttu-id="0dde3-248">Замените метод `About` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0dde3-248">Replace the `About` method with the following code:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   <span data-ttu-id="0dde3-249">Запрос LINQ группирует записи из таблицы студентов по дате зачисления, вычисляет число записей в каждой группе и сохраняет результаты в коллекцию объектов моделей представления `EnrollmentDateGroup`.</span><span class="sxs-lookup"><span data-stu-id="0dde3-249">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>

4. <span data-ttu-id="0dde3-250">Добавление `Dispose` метод:</span><span class="sxs-lookup"><span data-stu-id="0dde3-250">Add a `Dispose` method:</span></span>

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a><span data-ttu-id="0dde3-251">Изменение представления About</span><span class="sxs-lookup"><span data-stu-id="0dde3-251">Modify the About View</span></span>

1. <span data-ttu-id="0dde3-252">Замените код в *Views\Home\About.cshtml* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0dde3-252">Replace the code in the *Views\Home\About.cshtml* file with the following code:</span></span>

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. <span data-ttu-id="0dde3-253">Запустите приложение и нажмите кнопку **о** ссылку.</span><span class="sxs-lookup"><span data-stu-id="0dde3-253">Run the app and click the **About** link.</span></span>

   <span data-ttu-id="0dde3-254">Количество студентов, зачисленных отображает в таблице.</span><span class="sxs-lookup"><span data-stu-id="0dde3-254">The count of students for each enrollment date displays in a table.</span></span>

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a><span data-ttu-id="0dde3-256">Получение кода</span><span class="sxs-lookup"><span data-stu-id="0dde3-256">Get the code</span></span>

[<span data-ttu-id="0dde3-257">Скачивание готового проекта</span><span class="sxs-lookup"><span data-stu-id="0dde3-257">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="0dde3-258">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0dde3-258">Additional resources</span></span>

<span data-ttu-id="0dde3-259">Ссылки на другие ресурсы Entity Framework можно найти в [доступ к данным ASP.NET — рекомендуемые ресурсы](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="0dde3-259">Links to other Entity Framework resources can be found in [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0dde3-260">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0dde3-260">Next steps</span></span>

<span data-ttu-id="0dde3-261">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="0dde3-261">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0dde3-262">Добавление ссылки сортировки столбцов</span><span class="sxs-lookup"><span data-stu-id="0dde3-262">Add column sort links</span></span>
> * <span data-ttu-id="0dde3-263">Добавление поля поиска</span><span class="sxs-lookup"><span data-stu-id="0dde3-263">Add a Search box</span></span>
> * <span data-ttu-id="0dde3-264">Добавление разбиения по страницам</span><span class="sxs-lookup"><span data-stu-id="0dde3-264">Add paging</span></span>
> * <span data-ttu-id="0dde3-265">Создание страницы About</span><span class="sxs-lookup"><span data-stu-id="0dde3-265">Create an About page</span></span>

<span data-ttu-id="0dde3-266">Перейдите к следующей статье, чтобы сведения об использовании перехвата устойчивость и команду подключения.</span><span class="sxs-lookup"><span data-stu-id="0dde3-266">Advance to the next article to learn how to use connection resiliency and command interception.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0dde3-267">Перехват подключения устойчивость и команды</span><span class="sxs-lookup"><span data-stu-id="0dde3-267">Connection resiliency and command interception</span></span>](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)