---
title: "Основные ASP.NET MVC с основными EF - сортировки, фильтрации, разбиение на страницы: 3 из 10"
author: tdykstra
description: "В этом учебнике предстоит добавить сортировку, фильтрацию и разбиение по страницам функциональные возможности для разбиения на страницы с помощью ASP.NET Core и Entity Framework Core."
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: 60ac1844e7747002d72aa892a47490cb7a416359
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a><span data-ttu-id="a9c43-103">Сортировка, фильтрация, разбиение по страницам и группирование - Core EF учебнику ASP.NET Core MVC (3 из 10)</span><span class="sxs-lookup"><span data-stu-id="a9c43-103">Sorting, filtering, paging, and grouping - EF Core with ASP.NET Core MVC tutorial (3 of 10)</span></span>

<span data-ttu-id="a9c43-104">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a9c43-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a9c43-105">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a9c43-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="a9c43-106">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="a9c43-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="a9c43-107">В предыдущем учебнике реализован ряд веб-страниц для основные операции CRUD для сущностей студента.</span><span class="sxs-lookup"><span data-stu-id="a9c43-107">In the previous tutorial, you implemented a set of web pages for basic CRUD operations for Student entities.</span></span> <span data-ttu-id="a9c43-108">В этом учебнике вы добавите сортировку, фильтрацию и разбиения по страницам на страницу индекса учащихся.</span><span class="sxs-lookup"><span data-stu-id="a9c43-108">In this tutorial you'll add sorting, filtering, and paging functionality to the Students Index page.</span></span> <span data-ttu-id="a9c43-109">Здесь также создается страница, выполняющий простым группированием.</span><span class="sxs-lookup"><span data-stu-id="a9c43-109">You'll also create a page that does simple grouping.</span></span>

<span data-ttu-id="a9c43-110">Ниже показано, как будет выглядеть страница после завершения.</span><span class="sxs-lookup"><span data-stu-id="a9c43-110">The following illustration shows what the page will look like when you're done.</span></span> <span data-ttu-id="a9c43-111">Заголовки столбцов являются ссылками, пользователь может щелкнуть для сортировки по этому столбцу.</span><span class="sxs-lookup"><span data-stu-id="a9c43-111">The column headings are links that the user can click to sort by that column.</span></span> <span data-ttu-id="a9c43-112">При щелчке заголовка многократно переключается между сортировкой по возрастанию и убыванию.</span><span class="sxs-lookup"><span data-stu-id="a9c43-112">Clicking a column heading repeatedly toggles between ascending and descending sort order.</span></span>

![Страница индекса студентов](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a><span data-ttu-id="a9c43-114">Добавить столбец сортировки ссылки на страницы индекса для учащихся</span><span class="sxs-lookup"><span data-stu-id="a9c43-114">Add Column Sort Links to the Students Index Page</span></span>

<span data-ttu-id="a9c43-115">Для добавления, страница индекса студента сортировки, мы изменим `Index` метод контроллера студентов и добавьте код в представлении индекса студента.</span><span class="sxs-lookup"><span data-stu-id="a9c43-115">To add sorting to the Student Index page, you'll change the `Index` method of the Students controller and add code to the Student Index view.</span></span>

### <a name="add-sorting-functionality-to-the-index-method"></a><span data-ttu-id="a9c43-116">Добавление возможности сортировки в метод индекса</span><span class="sxs-lookup"><span data-stu-id="a9c43-116">Add sorting Functionality to the Index method</span></span>

<span data-ttu-id="a9c43-117">В *StudentsController.cs*, замените `Index` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a9c43-117">In *StudentsController.cs*, replace the `Index` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

<span data-ttu-id="a9c43-118">Этот код получает `sortOrder` параметра из строки запроса в URL-АДРЕСЕ.</span><span class="sxs-lookup"><span data-stu-id="a9c43-118">This code receives a `sortOrder` parameter from the query string in the URL.</span></span> <span data-ttu-id="a9c43-119">Значение строки запроса, предоставляемые ASP.NET Core MVC как параметр методу действия.</span><span class="sxs-lookup"><span data-stu-id="a9c43-119">The query string value is provided by ASP.NET Core MVC as a parameter to the action method.</span></span> <span data-ttu-id="a9c43-120">Параметр будет строка, являющаяся «Name» или «Date» последовать знак подчеркивания и строку «desc», чтобы указать порядок по убыванию.</span><span class="sxs-lookup"><span data-stu-id="a9c43-120">The parameter will be a string that's either "Name" or "Date", optionally followed by an underscore and the string "desc" to specify descending order.</span></span> <span data-ttu-id="a9c43-121">По умолчанию используется порядок сортировки по возрастанию.</span><span class="sxs-lookup"><span data-stu-id="a9c43-121">The default sort order is ascending.</span></span>

<span data-ttu-id="a9c43-122">При первом запросе страницы индекса имеется строка запроса отсутствует.</span><span class="sxs-lookup"><span data-stu-id="a9c43-122">The first time the Index page is requested, there's no query string.</span></span> <span data-ttu-id="a9c43-123">Студенты отображаются в порядке возрастания по фамилии, используемом по умолчанию, установленные с проходом регистром `switch` инструкции.</span><span class="sxs-lookup"><span data-stu-id="a9c43-123">The students are displayed in ascending order by last name, which is the default as established by the fall-through case in the `switch` statement.</span></span> <span data-ttu-id="a9c43-124">Когда пользователь щелкает гиперссылку заголовок столбца, соответствующего `sortOrder` значение указано в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="a9c43-124">When the user clicks a column heading hyperlink, the appropriate `sortOrder` value is provided in the query string.</span></span>

<span data-ttu-id="a9c43-125">Два `ViewData` элементы (NameSortParm и DateSortParm) используются в представлении для настройки гиперссылки заголовок столбца со строковыми значениями соответствующий запрос.</span><span class="sxs-lookup"><span data-stu-id="a9c43-125">The two `ViewData` elements (NameSortParm and DateSortParm) are used by the view to configure the column heading hyperlinks with the appropriate query string values.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

<span data-ttu-id="a9c43-126">Это троичный инструкции.</span><span class="sxs-lookup"><span data-stu-id="a9c43-126">These are ternary statements.</span></span> <span data-ttu-id="a9c43-127">Указывает, что если первый `sortOrder` параметр имеет значение null или пусто, NameSortParm должно быть равно «name_desc»; в противном случае оно должно быть присвоено пустая строка.</span><span class="sxs-lookup"><span data-stu-id="a9c43-127">The first one specifies that if the `sortOrder` parameter is null or empty, NameSortParm should be set to "name_desc"; otherwise, it should be set to an empty string.</span></span> <span data-ttu-id="a9c43-128">Следующие два оператора включите представление для задания гиперссылки заголовка столбца следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a9c43-128">These two statements enable the view to set the column heading hyperlinks as follows:</span></span>

|  <span data-ttu-id="a9c43-129">Текущий порядок сортировки</span><span class="sxs-lookup"><span data-stu-id="a9c43-129">Current sort order</span></span>  | <span data-ttu-id="a9c43-130">Последнее имя гиперссылки</span><span class="sxs-lookup"><span data-stu-id="a9c43-130">Last Name Hyperlink</span></span> | <span data-ttu-id="a9c43-131">Дата гиперссылки</span><span class="sxs-lookup"><span data-stu-id="a9c43-131">Date Hyperlink</span></span> |
|:--------------------:|:-------------------:|:--------------:|
| <span data-ttu-id="a9c43-132">Последняя имени: по возрастанию</span><span class="sxs-lookup"><span data-stu-id="a9c43-132">Last Name ascending</span></span>  | <span data-ttu-id="a9c43-133">по убыванию</span><span class="sxs-lookup"><span data-stu-id="a9c43-133">descending</span></span>          | <span data-ttu-id="a9c43-134">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="a9c43-134">ascending</span></span>      |
| <span data-ttu-id="a9c43-135">Последняя имени: по убыванию</span><span class="sxs-lookup"><span data-stu-id="a9c43-135">Last Name descending</span></span> | <span data-ttu-id="a9c43-136">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="a9c43-136">ascending</span></span>           | <span data-ttu-id="a9c43-137">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="a9c43-137">ascending</span></span>      |
| <span data-ttu-id="a9c43-138">По возрастанию даты</span><span class="sxs-lookup"><span data-stu-id="a9c43-138">Date ascending</span></span>       | <span data-ttu-id="a9c43-139">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="a9c43-139">ascending</span></span>           | <span data-ttu-id="a9c43-140">по убыванию</span><span class="sxs-lookup"><span data-stu-id="a9c43-140">descending</span></span>     |
| <span data-ttu-id="a9c43-141">Дата по убыванию</span><span class="sxs-lookup"><span data-stu-id="a9c43-141">Date descending</span></span>      | <span data-ttu-id="a9c43-142">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="a9c43-142">ascending</span></span>           | <span data-ttu-id="a9c43-143">по возрастанию</span><span class="sxs-lookup"><span data-stu-id="a9c43-143">ascending</span></span>      |

<span data-ttu-id="a9c43-144">Метод использует LINQ to Entities позволяет выбрать столбец для сортировки.</span><span class="sxs-lookup"><span data-stu-id="a9c43-144">The method uses LINQ to Entities to specify the column to sort by.</span></span> <span data-ttu-id="a9c43-145">Код создает `IQueryable` переменной перед оператора switch, выполняется его изменение в операторе switch и вызовы `ToListAsync` метод после `switch` инструкции.</span><span class="sxs-lookup"><span data-stu-id="a9c43-145">The code creates an `IQueryable` variable before the switch statement, modifies it in the switch statement, and calls the `ToListAsync` method after the `switch` statement.</span></span> <span data-ttu-id="a9c43-146">После создания и изменения `IQueryable` переменные, не отправляется запрос базы данных.</span><span class="sxs-lookup"><span data-stu-id="a9c43-146">When you create and modify `IQueryable` variables, no query is sent to the database.</span></span> <span data-ttu-id="a9c43-147">Запрос не выполняется до перехода `IQueryable` объект в коллекции, такие как вызов метода `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="a9c43-147">The query isn't executed until you convert the `IQueryable` object into a collection by calling a method such as `ToListAsync`.</span></span> <span data-ttu-id="a9c43-148">Таким образом, этот код вызывает один запрос, который не выполняется до `return View` инструкции.</span><span class="sxs-lookup"><span data-stu-id="a9c43-148">Therefore, this code results in a single query that's not executed until the `return View` statement.</span></span>

<span data-ttu-id="a9c43-149">Этот код может получить подробный с большим числом столбцов.</span><span class="sxs-lookup"><span data-stu-id="a9c43-149">This code could get verbose with a large number of columns.</span></span> <span data-ttu-id="a9c43-150">[Последний учебнике этой серии](advanced.md#dynamic-linq) показано, как написать код, который позволяет передать имя `OrderBy` столбца в строковой переменной.</span><span class="sxs-lookup"><span data-stu-id="a9c43-150">[The last tutorial in this series](advanced.md#dynamic-linq) shows how to write code that lets you pass the name of the `OrderBy` column in a string variable.</span></span>

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a><span data-ttu-id="a9c43-151">Добавление гиперссылок заголовок столбца в представлении студента индекса</span><span class="sxs-lookup"><span data-stu-id="a9c43-151">Add column heading hyperlinks to the Student Index view</span></span>

<span data-ttu-id="a9c43-152">Замените код в *Views/Students/Index.cshtml*, добавление гиперссылок заголовка столбца следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="a9c43-152">Replace the code in *Views/Students/Index.cshtml*, with the following code to add column heading hyperlinks.</span></span> <span data-ttu-id="a9c43-153">Измененные строки будут выделены.</span><span class="sxs-lookup"><span data-stu-id="a9c43-153">The changed lines are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

<span data-ttu-id="a9c43-154">Этот код использует эти сведения в `ViewData` свойства для настройки гиперссылки с соответствующего запроса строковые значения.</span><span class="sxs-lookup"><span data-stu-id="a9c43-154">This code uses the information in `ViewData` properties to set up hyperlinks with the appropriate query string values.</span></span>

<span data-ttu-id="a9c43-155">Запустите приложение, выберите **учащихся** и нажмите кнопку **Фамилия** и **Дата регистрации** заголовки столбцов, чтобы проверить, Сортировка работает.</span><span class="sxs-lookup"><span data-stu-id="a9c43-155">Run the app, select the **Students** tab, and click the **Last Name** and **Enrollment Date** column headings to verify that sorting works.</span></span>

![Страница индекса студентов в порядке по имени](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a><span data-ttu-id="a9c43-157">Добавить поле поиска на страницу индекса студентов</span><span class="sxs-lookup"><span data-stu-id="a9c43-157">Add a Search Box to the Students Index page</span></span>

<span data-ttu-id="a9c43-158">Добавление фильтрации на страницу индекса учащихся, мы добавим текстовое поле и кнопку отправки для представления и внесите соответствующие изменения в `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="a9c43-158">To add filtering to the Students Index page, you'll add a text box and a submit button to the view and make corresponding changes in the `Index` method.</span></span> <span data-ttu-id="a9c43-159">Текстовое поле можно ввести строку для поиска в полями имени и имени.</span><span class="sxs-lookup"><span data-stu-id="a9c43-159">The text box will let you enter a string to search for in the first name and last name fields.</span></span>

### <a name="add-filtering-functionality-to-the-index-method"></a><span data-ttu-id="a9c43-160">Добавьте функцию фильтра в метод индекса</span><span class="sxs-lookup"><span data-stu-id="a9c43-160">Add filtering functionality to the Index method</span></span>

<span data-ttu-id="a9c43-161">В *StudentsController.cs*, замените `Index` метод (изменения выделены) следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="a9c43-161">In *StudentsController.cs*, replace the `Index` method with the following code (the changes are highlighted).</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

<span data-ttu-id="a9c43-162">Вы добавили `searchString` параметр `Index` метода.</span><span class="sxs-lookup"><span data-stu-id="a9c43-162">You've added a `searchString` parameter to the `Index` method.</span></span> <span data-ttu-id="a9c43-163">Строковое значение поиска получается из текстового поля, который вам предстоит добавить в представление индекс.</span><span class="sxs-lookup"><span data-stu-id="a9c43-163">The search string value is received from a text box that you'll add to the Index view.</span></span> <span data-ttu-id="a9c43-164">Вы также добавили в инструкции LINQ where предложение, которое выбирает только студентов, имя или фамилия содержит строку поиска.</span><span class="sxs-lookup"><span data-stu-id="a9c43-164">You've also added to the LINQ statement a where clause that selects only students whose first name or last name contains the search string.</span></span> <span data-ttu-id="a9c43-165">Инструкцию, которая добавляет where предложение выполняется только в том случае, если значение для поиска.</span><span class="sxs-lookup"><span data-stu-id="a9c43-165">The statement that adds the where clause is executed only if there's a value to search for.</span></span>

> [!NOTE]
> <span data-ttu-id="a9c43-166">Здесь вы вызываете `Where` метод `IQueryable` объекта и фильтр будет обрабатываться на сервере.</span><span class="sxs-lookup"><span data-stu-id="a9c43-166">Here you are calling the `Where` method on an `IQueryable` object, and the filter will be processed on the server.</span></span> <span data-ttu-id="a9c43-167">В некоторых сценариях может вызов вы `Where` метод как метод расширения для коллекции в памяти.</span><span class="sxs-lookup"><span data-stu-id="a9c43-167">In some scenarios you might be calling the `Where` method as an extension method on an in-memory collection.</span></span> <span data-ttu-id="a9c43-168">(Например, предположим, что изменить ссылку на `_context.Students` таким образом, чтобы вместо объекта EF `DbSet` оно ссылается на метод репозитория, который возвращает `IEnumerable` коллекции.) Результат обычно останется прежним, но в некоторых случаях могут различаться.</span><span class="sxs-lookup"><span data-stu-id="a9c43-168">(For example, suppose you change the reference to `_context.Students` so that instead of an EF `DbSet` it references a repository method that returns an `IEnumerable` collection.) The result would normally be the same but in some cases may be different.</span></span>
>
><span data-ttu-id="a9c43-169">Например, реализация .NET Framework `Contains` метод выполняет сравнение с учетом регистра по умолчанию, но в SQL Server это определяется параметров сортировки экземпляра SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a9c43-169">For example, the .NET Framework implementation of the `Contains` method performs a case-sensitive comparison by default, but in SQL Server this is determined by the collation setting of the SQL Server instance.</span></span> <span data-ttu-id="a9c43-170">Эту настройку по умолчанию с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="a9c43-170">That setting defaults to case-insensitive.</span></span> <span data-ttu-id="a9c43-171">Вы можете вызвать `ToUpper` метод производится проверка явно без учета регистра: *где (s = > s.LastName.ToUpper(). CONTAINS(searchString.ToUpper())*.</span><span class="sxs-lookup"><span data-stu-id="a9c43-171">You could call the `ToUpper` method to make the test explicitly case-insensitive:  *Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*.</span></span> <span data-ttu-id="a9c43-172">Гарантирует, что результаты остаются теми же при изменении кода позже в репозиторий, который возвращает `IEnumerable` коллекции вместо `IQueryable` объекта.</span><span class="sxs-lookup"><span data-stu-id="a9c43-172">That would ensure that results stay the same if you change the code later to use a repository which returns   an `IEnumerable` collection instead of an `IQueryable` object.</span></span> <span data-ttu-id="a9c43-173">(При вызове `Contains` метод `IEnumerable` коллекции, получить реализации .NET Framework; при его вызове на `IQueryable` объекта, вы получаете реализации поставщика базы данных.) Тем не менее является снижение производительности для этого решения.</span><span class="sxs-lookup"><span data-stu-id="a9c43-173">(When you call the `Contains` method on an `IEnumerable` collection, you get the .NET Framework implementation; when you call it on an `IQueryable` object, you get the database provider implementation.) However, there's a performance penalty for this solution.</span></span> <span data-ttu-id="a9c43-174">`ToUpper` Поместить код функции в предложении WHERE инструкции TSQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="a9c43-174">The `ToUpper` code would put a function in the WHERE clause of the TSQL SELECT statement.</span></span> <span data-ttu-id="a9c43-175">Оптимизатор, не с помощью индекса.</span><span class="sxs-lookup"><span data-stu-id="a9c43-175">That would prevent the optimizer from using an index.</span></span> <span data-ttu-id="a9c43-176">Учитывая, что SQL главным образом установлен без учета регистра, рекомендуется избежать `ToUpper` кода до переноса хранилища данных с учетом регистра.</span><span class="sxs-lookup"><span data-stu-id="a9c43-176">Given that SQL is mostly installed as case-insensitive, it's best to avoid the `ToUpper` code until you migrate to a case-sensitive data store.</span></span>

### <a name="add-a-search-box-to-the-student-index-view"></a><span data-ttu-id="a9c43-177">Добавить поле поиска в представление индекс студента</span><span class="sxs-lookup"><span data-stu-id="a9c43-177">Add a Search Box to the Student Index View</span></span>

<span data-ttu-id="a9c43-178">В *Views/Student/Index.cshtml*, добавьте выделенный код непосредственно перед открывающий тег таблицы для создания заголовка и текстовое поле и **поиска** кнопку.</span><span class="sxs-lookup"><span data-stu-id="a9c43-178">In *Views/Student/Index.cshtml*, add the highlighted code immediately before the opening table tag in order to create a caption, a text box, and a **Search** button.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

<span data-ttu-id="a9c43-179">Этот код использует `<form>` [тег вспомогательный](xref:mvc/views/tag-helpers/intro) Добавление поиска текстовое поле и кнопку.</span><span class="sxs-lookup"><span data-stu-id="a9c43-179">This code uses the `<form>` [tag helper](xref:mvc/views/tag-helpers/intro) to add the search text box and button.</span></span> <span data-ttu-id="a9c43-180">По умолчанию `<form>` тег вспомогательный отправки данных с помощью запроса POST, это означает, что параметры передаются в тексте сообщения HTTP, а не в URL-адрес как строки запросов.</span><span class="sxs-lookup"><span data-stu-id="a9c43-180">By default, the `<form>` tag helper submits form data with a POST, which means that parameters are passed in the HTTP message body and not in the URL as query strings.</span></span> <span data-ttu-id="a9c43-181">При указании HTTP GET, данные формы переданный URL-адрес как строки запроса, который позволяет пользователям bookmark URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="a9c43-181">When you specify HTTP GET, the form data is passed in the URL as query strings, which enables users to bookmark the URL.</span></span> <span data-ttu-id="a9c43-182">Действия не приведут к обновление получение рекомендуется рекомендации W3C, который следует использовать.</span><span class="sxs-lookup"><span data-stu-id="a9c43-182">The W3C guidelines recommend that you should use GET when the action doesn't result in an update.</span></span>

<span data-ttu-id="a9c43-183">Запустите приложение, выберите **учащихся** введите строку поиска и нажмите кнопку поиска, чтобы проверить работоспособность фильтрации.</span><span class="sxs-lookup"><span data-stu-id="a9c43-183">Run the app, select the **Students** tab, enter a search string, and click Search to verify that filtering is working.</span></span>

![Страница индекса учащихся с фильтрацией](sort-filter-page/_static/filtering.png)

<span data-ttu-id="a9c43-185">Обратите внимание, что URL-адрес содержит строку поиска.</span><span class="sxs-lookup"><span data-stu-id="a9c43-185">Notice that the URL contains the search string.</span></span>

```html
http://localhost:5813/Students?SearchString=an
```

<span data-ttu-id="a9c43-186">Если вы установите закладку эту страницу, вы сможете получить отфильтрованный список при использовании закладки.</span><span class="sxs-lookup"><span data-stu-id="a9c43-186">If you bookmark this page, you'll get the filtered list when you use the bookmark.</span></span> <span data-ttu-id="a9c43-187">Добавление `method="get"` для `form` тег является причину строке запроса должен быть создан.</span><span class="sxs-lookup"><span data-stu-id="a9c43-187">Adding `method="get"` to the `form` tag is what caused the query string to be generated.</span></span>

<span data-ttu-id="a9c43-188">На этом этапе, если по ссылке сортировки в заголовок столбца, будут потеряны значение фильтра, введенного в **поиска** поле.</span><span class="sxs-lookup"><span data-stu-id="a9c43-188">At this stage, if you click a column heading sort link you'll lose the filter value that you entered in the **Search** box.</span></span> <span data-ttu-id="a9c43-189">Это будет исправлено в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="a9c43-189">You'll fix that in the next section.</span></span>

## <a name="add-paging-functionality-to-the-students-index-page"></a><span data-ttu-id="a9c43-190">Добавить на страницу индекса учащихся разбиения по страницам</span><span class="sxs-lookup"><span data-stu-id="a9c43-190">Add paging functionality to the Students Index page</span></span>

<span data-ttu-id="a9c43-191">Чтобы добавить на страницу индекса учащихся разбиения на страницы, следует создать `PaginatedList` класс, который использует `Skip` и `Take` инструкций для фильтрации данных на сервере, а не всегда быть получение всех строк таблицы.</span><span class="sxs-lookup"><span data-stu-id="a9c43-191">To add paging to the Students Index page, you'll create a `PaginatedList` class that uses `Skip` and `Take` statements to filter data on the server instead of always retrieving all rows of the table.</span></span> <span data-ttu-id="a9c43-192">Затем будет вносить дополнительные изменения в `Index` метод и добавить кнопки разбиения по страницам для `Index` представления.</span><span class="sxs-lookup"><span data-stu-id="a9c43-192">Then you'll make additional changes in the `Index` method and add paging buttons to the `Index` view.</span></span> <span data-ttu-id="a9c43-193">На следующем рисунке кнопки разбиения по страницам.</span><span class="sxs-lookup"><span data-stu-id="a9c43-193">The following illustration shows the paging buttons.</span></span>

![Студенты индексная страница со ссылками на разбиение на страницы](sort-filter-page/_static/paging.png)

<span data-ttu-id="a9c43-195">Создайте в папке проекта `PaginatedList.cs`, а затем замените код шаблона с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="a9c43-195">In the project folder, create `PaginatedList.cs`, and then replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

<span data-ttu-id="a9c43-196">`CreateAsync` Метод в этом коде принимает размер страницы и номер страницы и применяет соответствующие `Skip` и `Take` инструкции, чтобы `IQueryable`.</span><span class="sxs-lookup"><span data-stu-id="a9c43-196">The `CreateAsync` method in this code takes page size and page number and applies the appropriate `Skip` and `Take` statements to the `IQueryable`.</span></span> <span data-ttu-id="a9c43-197">Когда `ToListAsync` будет вызван на `IQueryable`, он возвращает список, содержащий только запрошенную страницу.</span><span class="sxs-lookup"><span data-stu-id="a9c43-197">When `ToListAsync` is called on the `IQueryable`, it will return a List containing only the requested page.</span></span> <span data-ttu-id="a9c43-198">Свойства `HasPreviousPage` и `HasNextPage` может использоваться для включения или отключения **Назад** и **Далее** разбиение по страницам кнопок.</span><span class="sxs-lookup"><span data-stu-id="a9c43-198">The properties `HasPreviousPage` and `HasNextPage` can be used to enable or disable **Previous** and **Next** paging buttons.</span></span>

<span data-ttu-id="a9c43-199">Объект `CreateAsync` метод вместо конструктор используется для создания `PaginatedList<T>` объекта, так как конструкторы не могут выполнять асинхронный код.</span><span class="sxs-lookup"><span data-stu-id="a9c43-199">A `CreateAsync` method is used instead of a constructor to create the `PaginatedList<T>` object because constructors can't run asynchronous code.</span></span>

## <a name="add-paging-functionality-to-the-index-method"></a><span data-ttu-id="a9c43-200">Добавьте в метод индекс разбиения по страницам</span><span class="sxs-lookup"><span data-stu-id="a9c43-200">Add paging functionality to the Index method</span></span>

<span data-ttu-id="a9c43-201">В *StudentsController.cs*, замените `Index` метод следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="a9c43-201">In *StudentsController.cs*, replace the `Index` method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

<span data-ttu-id="a9c43-202">Этот код добавляет числовой параметр страницы, текущий параметр порядка сортировки и текущий параметр фильтра к сигнатуре метода.</span><span class="sxs-lookup"><span data-stu-id="a9c43-202">This code adds a page number parameter, a current sort order parameter, and a current filter parameter to the method signature.</span></span>

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

<span data-ttu-id="a9c43-203">Первый раз, когда страница отображается или если пользователь не щелкнет подкачки или сортировки ссылку, все параметры будут иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="a9c43-203">The first time the page is displayed, or if the user hasn't clicked a paging or sorting link, all the parameters will be null.</span></span>  <span data-ttu-id="a9c43-204">Если щелкнуть ссылку подкачки переменной страницы будет содержать номер страницы для отображения.</span><span class="sxs-lookup"><span data-stu-id="a9c43-204">If a paging link is clicked, the page variable will contain the page number to display.</span></span>

<span data-ttu-id="a9c43-205">`ViewData` Элемента с именем CurrentSort обеспечивает представление определения порядка сортировки, так как это должны быть включены в ссылки разбиения на страницы для сохранит порядок сортировки при разбиении по страницам.</span><span class="sxs-lookup"><span data-stu-id="a9c43-205">The `ViewData` element named CurrentSort provides the view with the current sort order, because this must be included in the paging links in order to keep the sort order the same while paging.</span></span>

<span data-ttu-id="a9c43-206">`ViewData` Элемента с именем ТекущийФильтр обеспечивает представление с текущей строки фильтра.</span><span class="sxs-lookup"><span data-stu-id="a9c43-206">The `ViewData` element named CurrentFilter provides the view with the current filter string.</span></span> <span data-ttu-id="a9c43-207">Это значение должны быть включены в ссылки разбиения на страницы для поддержки настройки фильтра во время разбиения на страницы, и она должна быть восстановлена к текстовому полю, когда отобразится страница.</span><span class="sxs-lookup"><span data-stu-id="a9c43-207">This value must be included in the paging links in order to maintain the filter settings during paging, and it must be restored to the text box when the page is redisplayed.</span></span>

<span data-ttu-id="a9c43-208">Если строка поиска, которая изменяется во время разбиения на страницы, страницы должен быть равным 1, так как новый фильтр может показать различные данные.</span><span class="sxs-lookup"><span data-stu-id="a9c43-208">If the search string is changed during paging, the page has to be reset to 1, because the new filter can result in different data to display.</span></span> <span data-ttu-id="a9c43-209">Строка поиска, которая изменяется при нажатии кнопки "Отправить", если ввести значение в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="a9c43-209">The search string is changed when a value is entered in the text box and the Submit button is pressed.</span></span> <span data-ttu-id="a9c43-210">В этом случае `searchString` параметра не равно null.</span><span class="sxs-lookup"><span data-stu-id="a9c43-210">In that case, the `searchString` parameter isn't null.</span></span>

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

<span data-ttu-id="a9c43-211">В конце `Index` метода `PaginatedList.CreateAsync` метод преобразует запрос студента на одной странице учеников в тип коллекции, который поддерживает разбиение по страницам.</span><span class="sxs-lookup"><span data-stu-id="a9c43-211">At the end of the `Index` method, the `PaginatedList.CreateAsync` method converts the student query to a single page of students in a collection type that supports paging.</span></span> <span data-ttu-id="a9c43-212">Страницы учащихся затем передается в представление.</span><span class="sxs-lookup"><span data-stu-id="a9c43-212">That single page of students is then passed to the view.</span></span>

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

<span data-ttu-id="a9c43-213">`PaginatedList.CreateAsync` Метод принимает номер страницы.</span><span class="sxs-lookup"><span data-stu-id="a9c43-213">The `PaginatedList.CreateAsync` method takes a page number.</span></span> <span data-ttu-id="a9c43-214">Два вопросительные знаки представления оператора объединения с null.</span><span class="sxs-lookup"><span data-stu-id="a9c43-214">The two question marks represent the null-coalescing operator.</span></span> <span data-ttu-id="a9c43-215">Оператор объединения с null определяет значение по умолчанию для типа значения NULL; выражение `(page ?? 1)` означает возвращают значение `page` если он имеет значение, или возвращает 1, если `page` имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="a9c43-215">The null-coalescing operator defines a default value for a nullable type; the expression `(page ?? 1)` means return the value of `page` if it has a value, or return 1 if `page` is null.</span></span>

## <a name="add-paging-links-to-the-student-index-view"></a><span data-ttu-id="a9c43-216">Добавить разбиение на страницы ссылки на представление Index студента</span><span class="sxs-lookup"><span data-stu-id="a9c43-216">Add paging links to the Student Index view</span></span>

<span data-ttu-id="a9c43-217">В *Views/Students/Index.cshtml*, замените существующий код следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="a9c43-217">In *Views/Students/Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="a9c43-218">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="a9c43-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

<span data-ttu-id="a9c43-219">`@model` В верхней части страницы указывает теперь возвращает представление `PaginatedList<T>` объекта вместо `List<T>` объекта.</span><span class="sxs-lookup"><span data-stu-id="a9c43-219">The `@model` statement at the top of the page specifies that the view now gets a `PaginatedList<T>` object instead of a `List<T>` object.</span></span>

<span data-ttu-id="a9c43-220">Ссылки в заголовках столбцов использовать строку запроса для передачи текущей искомой строки к контроллеру, чтобы пользователь может сортировать в результаты фильтрации:</span><span class="sxs-lookup"><span data-stu-id="a9c43-220">The column header links use the query string to pass the current search string to the controller so that the user can sort within filter results:</span></span>

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

<span data-ttu-id="a9c43-221">По вспомогательных функций тегов отображаются кнопки разбиения на страницы:</span><span class="sxs-lookup"><span data-stu-id="a9c43-221">The paging buttons are displayed by tag helpers:</span></span>

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

<span data-ttu-id="a9c43-222">Запустите приложение и перейти на страницу учащихся.</span><span class="sxs-lookup"><span data-stu-id="a9c43-222">Run the app and go to the Students page.</span></span>

![Студенты индексная страница со ссылками на разбиение на страницы](sort-filter-page/_static/paging.png)

<span data-ttu-id="a9c43-224">По ссылкам разбиения на страницы в различными порядками сортировки, чтобы убедиться, что работает разбиения на страницы.</span><span class="sxs-lookup"><span data-stu-id="a9c43-224">Click the paging links in different sort orders to make sure paging works.</span></span> <span data-ttu-id="a9c43-225">Затем введите строку поиска и повторите подкачки еще раз, чтобы проверить правильность разбиения на страницы также с сортировкой и фильтрацией.</span><span class="sxs-lookup"><span data-stu-id="a9c43-225">Then enter a search string and try paging again to verify that paging also works correctly with sorting and filtering.</span></span>

## <a name="create-an-about-page-that-shows-student-statistics"></a><span data-ttu-id="a9c43-226">Создайте страницу о программе, отображается статистика студента</span><span class="sxs-lookup"><span data-stu-id="a9c43-226">Create an About page that shows Student statistics</span></span>

<span data-ttu-id="a9c43-227">Для веб-сайта компании Contoso университета **о** страницы, как отображать количество студентов регистрации для каждой даты регистрации.</span><span class="sxs-lookup"><span data-stu-id="a9c43-227">For the Contoso University website's **About** page, you'll display how many students have enrolled for each enrollment date.</span></span> <span data-ttu-id="a9c43-228">Для этого простого группировки и выполнения расчетов по группам.</span><span class="sxs-lookup"><span data-stu-id="a9c43-228">This requires grouping and simple calculations on the groups.</span></span> <span data-ttu-id="a9c43-229">Для выполнения этой задачи вы выполните следующее:</span><span class="sxs-lookup"><span data-stu-id="a9c43-229">To accomplish this, you'll do the following:</span></span>

* <span data-ttu-id="a9c43-230">Создание класса модели представления для данных, которые нужно передать в представление.</span><span class="sxs-lookup"><span data-stu-id="a9c43-230">Create a view model class for the data that you need to pass to the view.</span></span>

* <span data-ttu-id="a9c43-231">Измените метод "о программе" в контроллер Home.</span><span class="sxs-lookup"><span data-stu-id="a9c43-231">Modify the About method in the Home controller.</span></span>

* <span data-ttu-id="a9c43-232">Измените представление о программе.</span><span class="sxs-lookup"><span data-stu-id="a9c43-232">Modify the About view.</span></span>

### <a name="create-the-view-model"></a><span data-ttu-id="a9c43-233">Создание модели представления</span><span class="sxs-lookup"><span data-stu-id="a9c43-233">Create the view model</span></span>

<span data-ttu-id="a9c43-234">Создание *SchoolViewModels* папки в *моделей* папки.</span><span class="sxs-lookup"><span data-stu-id="a9c43-234">Create a *SchoolViewModels* folder in the *Models* folder.</span></span>

<span data-ttu-id="a9c43-235">В новой папке, добавьте файл класса *EnrollmentDateGroup.cs* и замените код шаблона с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="a9c43-235">In the new folder, add a class file *EnrollmentDateGroup.cs* and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a><span data-ttu-id="a9c43-236">Контроллер Home</span><span class="sxs-lookup"><span data-stu-id="a9c43-236">Modify the Home Controller</span></span>

<span data-ttu-id="a9c43-237">В *HomeController.cs*, добавьте следующие операторы using в верхней части файла:</span><span class="sxs-lookup"><span data-stu-id="a9c43-237">In *HomeController.cs*, add the following using statements at the top of the file:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

<span data-ttu-id="a9c43-238">Добавление переменной класса для контекста базы данных сразу же после открывающей фигурной скобки для класса и получить экземпляр контекста из ASP.NET Core DI:</span><span class="sxs-lookup"><span data-stu-id="a9c43-238">Add a class variable for the database context immediately after the opening curly brace for the class, and get an instance of the context from ASP.NET Core DI:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

<span data-ttu-id="a9c43-239">Замените метод `About` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a9c43-239">Replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

<span data-ttu-id="a9c43-240">Инструкции LINQ группирует учащихся сущностей, Дата регистрации, вычисляет число сущностей в каждой группе и сохраняет результаты в коллекцию `EnrollmentDateGroup` просматривать объекты модели.</span><span class="sxs-lookup"><span data-stu-id="a9c43-240">The LINQ statement groups the student entities by enrollment date, calculates the number of entities in each group, and stores the results in a collection of `EnrollmentDateGroup` view model objects.</span></span>
> [!NOTE] 
> <span data-ttu-id="a9c43-241">В версии 1.0 Entity Framework Core весь результирующий набор возвращается клиенту, и группированием на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="a9c43-241">In the 1.0 version of Entity Framework Core, the entire result set is returned to the client, and grouping is done on the client.</span></span> <span data-ttu-id="a9c43-242">В некоторых случаях это может создать проблемы с производительностью.</span><span class="sxs-lookup"><span data-stu-id="a9c43-242">In some scenarios this could create performance problems.</span></span> <span data-ttu-id="a9c43-243">Убедитесь, что для тестирования производительности с помощью объемов данных в рабочей среде и при необходимости использовать необработанные SQL для выполнения группирования на сервере.</span><span class="sxs-lookup"><span data-stu-id="a9c43-243">Be sure to test performance with production volumes of data, and if necessary use raw SQL to do the grouping on the server.</span></span> <span data-ttu-id="a9c43-244">Сведения об использовании необработанные SQL см. в разделе [последнего учебнике этой серии](advanced.md).</span><span class="sxs-lookup"><span data-stu-id="a9c43-244">For information about how to use raw SQL, see [the last tutorial in this series](advanced.md).</span></span>

### <a name="modify-the-about-view"></a><span data-ttu-id="a9c43-245">Изменить представление</span><span class="sxs-lookup"><span data-stu-id="a9c43-245">Modify the About View</span></span>

<span data-ttu-id="a9c43-246">Замените код в *Views/Home/About.cshtml* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="a9c43-246">Replace the code in the *Views/Home/About.cshtml* file with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

<span data-ttu-id="a9c43-247">Запустите приложение и перейти на страницу о программе.</span><span class="sxs-lookup"><span data-stu-id="a9c43-247">Run the app and go to the About page.</span></span> <span data-ttu-id="a9c43-248">Количества учащихся для каждой даты регистрации отображается в таблице.</span><span class="sxs-lookup"><span data-stu-id="a9c43-248">The count of students for each enrollment date is displayed in a table.</span></span>

![О странице](sort-filter-page/_static/about.png)

## <a name="summary"></a><span data-ttu-id="a9c43-250">Сводка</span><span class="sxs-lookup"><span data-stu-id="a9c43-250">Summary</span></span>

<span data-ttu-id="a9c43-251">В этом учебнике вы знаете, как выполнять сортировку, фильтрацию, разбиение на страницы и группирования.</span><span class="sxs-lookup"><span data-stu-id="a9c43-251">In this tutorial, you've seen how to perform sorting, filtering, paging, and grouping.</span></span> <span data-ttu-id="a9c43-252">В следующем уроке вы узнаете, как обрабатывать изменения в модели данных с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="a9c43-252">In the next tutorial, you'll learn how to handle data model changes by using migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a9c43-253">[Назад](crud.md)
[Вперед](migrations.md)</span><span class="sxs-lookup"><span data-stu-id="a9c43-253">[Previous](crud.md)
[Next](migrations.md)</span></span>  
