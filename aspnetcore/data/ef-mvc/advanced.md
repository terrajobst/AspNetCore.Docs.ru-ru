---
title: "Основные ASP.NET MVC с основными EF - Дополнительно - 10 из 10"
author: tdykstra
description: "В этом учебнике рассказывается несколько разделов, которые необходимо иметь в виду при переходе расширенные возможности разработки веб-приложений ASP.NET, использующие Entity Framework Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: 4ee12cae0220825c81bd8b178dea3ac777f97bb6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a><span data-ttu-id="9f98b-103">Дополнительные разделы - Core EF учебнику ASP.NET Core MVC (10, 10)</span><span class="sxs-lookup"><span data-stu-id="9f98b-103">Advanced topics - EF Core with ASP.NET Core MVC tutorial (10 of 10)</span></span>

<span data-ttu-id="9f98b-104">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9f98b-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9f98b-105">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9f98b-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="9f98b-106">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="9f98b-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="9f98b-107">В предыдущем учебнике реализован таблица на иерархию наследования.</span><span class="sxs-lookup"><span data-stu-id="9f98b-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="9f98b-108">В этом учебнике рассказывается несколько разделов, которые необходимо иметь в виду при переходе расширенные возможности разработки веб-приложений ASP.NET Core, использующие Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="9f98b-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="9f98b-109">Необработанный SQL-запросов</span><span class="sxs-lookup"><span data-stu-id="9f98b-109">Raw SQL Queries</span></span>

<span data-ttu-id="9f98b-110">Одним из преимуществ использования платформы Entity Framework является позволяет избежать прерывания кода слишком близко к конкретного метода хранения данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="9f98b-111">Это делается путем создания SQL-запросов и команд, который избавляет от необходимости записывать их.</span><span class="sxs-lookup"><span data-stu-id="9f98b-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="9f98b-112">Но существуют исключительных случаях, когда необходимо запустить конкретных запросов SQL, созданных вручную.</span><span class="sxs-lookup"><span data-stu-id="9f98b-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="9f98b-113">В этих сценариях API первый Entity Framework кода включает методы, которые позволяют передавать команды SQL непосредственно к базе данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="9f98b-114">В версии 1.0 Core EF доступны следующие варианты:</span><span class="sxs-lookup"><span data-stu-id="9f98b-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="9f98b-115">Используйте `DbSet.FromSql` метод для запросов, возвращающих типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="9f98b-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="9f98b-116">Возвращаемые объекты должно относится к типу, ожидаемому `DbSet` объекта, а также выполняется автоматически отслеживаются контекстом контекст базы данных пока не вы [отключить отслеживание](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="9f98b-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="9f98b-117">Используйте `Database.ExecuteSqlCommand` для команды без запроса.</span><span class="sxs-lookup"><span data-stu-id="9f98b-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="9f98b-118">Если необходимо выполнить запрос, возвращающий типы, которые не являются сущностями, можно использовать ADO.NET с помощью подключения к базе данных, предоставляемые EF.</span><span class="sxs-lookup"><span data-stu-id="9f98b-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="9f98b-119">Возвращаемые данные не отслеживается контекст базы данных, даже при использовании этого метода на получение типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="9f98b-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="9f98b-120">Как всегда имеет значение true при выполнении команды SQL для веб-приложения, необходимо принять меры предосторожности, чтобы защитить сайт от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="9f98b-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="9f98b-121">Один из способов сделать это является использование параметризованных запросов, чтобы убедиться в том, что строки, отправленных веб-страницы не может интерпретироваться как команды SQL.</span><span class="sxs-lookup"><span data-stu-id="9f98b-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="9f98b-122">В этом учебнике будет использоваться параметризованные запросы при интеграции ввод данных пользователем в запросе.</span><span class="sxs-lookup"><span data-stu-id="9f98b-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="9f98b-123">Вызовите запрос, возвращающий сущностей</span><span class="sxs-lookup"><span data-stu-id="9f98b-123">Call a query that returns entities</span></span>

<span data-ttu-id="9f98b-124">`DbSet<TEntity>` Класс предоставляет метод, который можно использовать для выполнения запроса, возвращающее сущность типа `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="9f98b-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="9f98b-125">Чтобы увидеть, как это работает, можно будет изменить код в `Details` метод контроллера отдела.</span><span class="sxs-lookup"><span data-stu-id="9f98b-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="9f98b-126">В *DepartmentsController.cs*в `Details` метод, замените код, который извлекает отдел с `FromSql` вызова метода, как показано в следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="9f98b-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="9f98b-127">Чтобы убедиться, что новый код работает неправильно, выберите **отделы** tab и затем **сведения** для одного из отделов.</span><span class="sxs-lookup"><span data-stu-id="9f98b-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Сведения об отделе](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="9f98b-129">Вызовите запрос, возвращающий других типов</span><span class="sxs-lookup"><span data-stu-id="9f98b-129">Call a query that returns other types</span></span>

<span data-ttu-id="9f98b-130">Ранее вы создали сетку статистики студента страницу о программе показано число учеников для каждой даты регистрации.</span><span class="sxs-lookup"><span data-stu-id="9f98b-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="9f98b-131">Получить данные из набора сущностей учащихся (`_context.Students`) и использовать LINQ проецировать результаты в список `EnrollmentDateGroup` просматривать объекты модели.</span><span class="sxs-lookup"><span data-stu-id="9f98b-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="9f98b-132">Предположим, что вы планируете написать SQL себя, а не с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="9f98b-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="9f98b-133">Для выполнения, необходимо выполнить SQL-запрос, который возвращает нечто, отличное от объектов сущностей.</span><span class="sxs-lookup"><span data-stu-id="9f98b-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="9f98b-134">В версии 1.0 Core EF один из способов сделать это — писать код ADO.NET и получить из EF подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="9f98b-135">В *HomeController.cs*, замените `About` метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="9f98b-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="9f98b-136">Добавить с помощью инструкции:</span><span class="sxs-lookup"><span data-stu-id="9f98b-136">Add a using statement:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="9f98b-137">Запустите приложение и перейти на страницу о программе.</span><span class="sxs-lookup"><span data-stu-id="9f98b-137">Run the app and go to the About page.</span></span> <span data-ttu-id="9f98b-138">Он отображает те же данные, которые раньше.</span><span class="sxs-lookup"><span data-stu-id="9f98b-138">It displays the same data it did before.</span></span>

![О странице](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="9f98b-140">Вызовите запрос на обновление</span><span class="sxs-lookup"><span data-stu-id="9f98b-140">Call an update query</span></span>

<span data-ttu-id="9f98b-141">Предположим, что администраторы университета Contoso требуется для выполнения глобальных изменений в базе данных, например изменение количества кредиты для каждого курса.</span><span class="sxs-lookup"><span data-stu-id="9f98b-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="9f98b-142">Если университета имеет большое количество курсов, было бы неэффективно, извлекать их все как сущности и измените их по отдельности.</span><span class="sxs-lookup"><span data-stu-id="9f98b-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="9f98b-143">В этом разделе будет реализован веб-страницы, которая позволяет пользователю указать коэффициент, используемый для изменения номера кредиты для всех курсов и вносятся изменения, выполнив инструкцию SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="9f98b-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="9f98b-144">Веб-страница будет выглядеть как на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="9f98b-144">The web page will look like the following illustration:</span></span>

![Страница «обновление» отзывы о курсе](advanced/_static/update-credits.png)

<span data-ttu-id="9f98b-146">В *CoursesContoller.cs*, добавьте методы UpdateCourseCredits HttpGet и HttpPost:</span><span class="sxs-lookup"><span data-stu-id="9f98b-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="9f98b-147">Когда контроллер обрабатывает запрос HttpGet, ничего не возвращается в `ViewData["RowsAffected"]`, и в представлении отображаются пустые текстовые поля и кнопки "Отправить", как показано на предыдущем рисунке.</span><span class="sxs-lookup"><span data-stu-id="9f98b-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="9f98b-148">Когда **обновление** кнопки, вызывается метод HttpPost и множитель имеет значение, введенное в текстовом поле.</span><span class="sxs-lookup"><span data-stu-id="9f98b-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="9f98b-149">Затем выполняется код SQL, которая обновляет курсы и возвращает количество задействованных строк в представление `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="9f98b-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="9f98b-150">Если представление получает `RowsAffected` значение, отображается количество обновленных строк.</span><span class="sxs-lookup"><span data-stu-id="9f98b-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="9f98b-151">В **обозревателе решений**, щелкните правой кнопкой мыши *представления и курсов* папки, а затем щелкните **Добавить > новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="9f98b-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="9f98b-152">В **Добавление нового элемента** диалоговое окно, нажмите кнопку **ASP.NET** под **установленные** в левой области щелкните **страница представления MVC**и имя нового представления  *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="9f98b-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="9f98b-153">В *Views/Courses/UpdateCourseCredits.cshtml*, замените код шаблона в следующий код:</span><span class="sxs-lookup"><span data-stu-id="9f98b-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="9f98b-154">Запустите `UpdateCourseCredits` метод, выбрав **курсы** вкладку, затем добавляется «/ UpdateCourseCredits» в конец URL-адрес в адресной строке браузера (например: `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="9f98b-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="9f98b-155">Введите число в текстовом поле:</span><span class="sxs-lookup"><span data-stu-id="9f98b-155">Enter a number in the text box:</span></span>

![Страница «обновление» отзывы о курсе](advanced/_static/update-credits.png)

<span data-ttu-id="9f98b-157">Нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="9f98b-157">Click **Update**.</span></span> <span data-ttu-id="9f98b-158">Количество строк, затронутых просмотреть:</span><span class="sxs-lookup"><span data-stu-id="9f98b-158">You see the number of rows affected:</span></span>

![Отзывы о курсе обновления страницы измененных строк](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="9f98b-160">Нажмите кнопку **списка** для просмотра списка курсов с измененный номер кредиты.</span><span class="sxs-lookup"><span data-stu-id="9f98b-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="9f98b-161">Обратите внимание, что рабочий код будет убедитесь, что всегда, обновляющий результат в допустимые данные.</span><span class="sxs-lookup"><span data-stu-id="9f98b-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="9f98b-162">Упрощенная приведенный ниже код может умножить число кредиты достаточно приводят к номера больше 5.</span><span class="sxs-lookup"><span data-stu-id="9f98b-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="9f98b-163">( `Credits` Имеет свойство `[Range(0, 5)]` атрибута.) Запрос на обновление будет работать, но недопустимых данных может привести к непредвиденным результатам в других частях системы, предположим, что количество кредиты пять или меньше.</span><span class="sxs-lookup"><span data-stu-id="9f98b-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="9f98b-164">Дополнительные сведения о необработанных запросов SQL см. в разделе [необработанные запросы SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="9f98b-164">For more information about raw SQL queries, see [Raw SQL Queries](https://docs.microsoft.com/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="9f98b-165">Проверьте, отправляемые в базу данных SQL</span><span class="sxs-lookup"><span data-stu-id="9f98b-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="9f98b-166">Иногда бывает полезно иметь возможность просматривать фактические запросы SQL, которые отправляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="9f98b-167">Функциональные возможности встроенного ведения журнала для ASP.NET Core автоматически используется ядром EF для записи файлы журналов, содержащие SQL для запросов и обновлений.</span><span class="sxs-lookup"><span data-stu-id="9f98b-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="9f98b-168">В этом разделе вы увидите некоторые примеры ведения журнала SQL.</span><span class="sxs-lookup"><span data-stu-id="9f98b-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="9f98b-169">Откройте *StudentsController.cs* и `Details` метод установить точку останова на `if (student == null)` инструкции.</span><span class="sxs-lookup"><span data-stu-id="9f98b-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="9f98b-170">Запустить приложение в режиме отладки и перейти к странице сведений для учащихся.</span><span class="sxs-lookup"><span data-stu-id="9f98b-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="9f98b-171">Последовательно выберите пункты **вывода** вывода окна, показывающая отладки и появится запрос:</span><span class="sxs-lookup"><span data-stu-id="9f98b-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

<span data-ttu-id="9f98b-172">Обратите внимание, что здесь могут оказаться неожиданными: SQL выбирает строки до 2 (`TOP(2)`) из таблицы Person.</span><span class="sxs-lookup"><span data-stu-id="9f98b-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="9f98b-173">`SingleOrDefaultAsync` Метод не разрешается в одну строку на сервере.</span><span class="sxs-lookup"><span data-stu-id="9f98b-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="9f98b-174">Вот почему:</span><span class="sxs-lookup"><span data-stu-id="9f98b-174">Here's why:</span></span>

* <span data-ttu-id="9f98b-175">Если запрос будет возвращать несколько строк, метод возвращает значение null.</span><span class="sxs-lookup"><span data-stu-id="9f98b-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="9f98b-176">Чтобы определить, является ли запрос будет возвращать несколько строк, EF должен проверить, если он возвращает по крайней мере 2.</span><span class="sxs-lookup"><span data-stu-id="9f98b-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="9f98b-177">Обратите внимание, что не нужно использовать режим отладки и точку останова, чтобы получить выходные данные журнала в **вывода** окна.</span><span class="sxs-lookup"><span data-stu-id="9f98b-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="9f98b-178">Это просто удобный способ остановить ведение журнала, в момент, который вы хотите посмотреть на вывод.</span><span class="sxs-lookup"><span data-stu-id="9f98b-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="9f98b-179">Если этого не сделать, продолжает ведение журнала, и необходимо вернуться к найти нужные элементы, которые вас интересуют.</span><span class="sxs-lookup"><span data-stu-id="9f98b-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="9f98b-180">Репозиторий и единицы шаблонов рабочих элементов</span><span class="sxs-lookup"><span data-stu-id="9f98b-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="9f98b-181">Многие разработчики написать код для реализации репозитория и единицы шаблонов рабочих элементов как оболочка для кода, который работает с платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="9f98b-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="9f98b-182">Эти шаблоны предназначены для создания уровень абстракции между уровня доступа к данным и бизнес-логики приложения.</span><span class="sxs-lookup"><span data-stu-id="9f98b-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="9f98b-183">Реализация этих шаблонов для изоляции приложения от изменений в хранилище данных и может упростить автоматических модульного тестирования или разработки через тестирование (TDD).</span><span class="sxs-lookup"><span data-stu-id="9f98b-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="9f98b-184">Тем не менее написать дополнительный код для реализации этих шаблонов не всегда подходит для приложений, использующих EF, по следующим причинам:</span><span class="sxs-lookup"><span data-stu-id="9f98b-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="9f98b-185">EF контекста класса изолирует кода из кода на конкретных хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="9f98b-186">Класс EF контекста может выступать как единица работы класса для базы данных обновляет это сделать с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="9f98b-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="9f98b-187">EF включает возможности для реализации TDD без написания кода репозитория.</span><span class="sxs-lookup"><span data-stu-id="9f98b-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="9f98b-188">Сведения о реализации репозитория и единицы шаблонов рабочих элементов см. в разделе [версию Entity Framework 5 этого учебника ряда](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="9f98b-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="9f98b-189">Entity Framework Core реализует поставщик баз данных в памяти, который может использоваться для тестирования.</span><span class="sxs-lookup"><span data-stu-id="9f98b-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="9f98b-190">Дополнительные сведения см. в разделе [тестирование с помощью InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="9f98b-190">For more information, see [Testing with InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="9f98b-191">Изменение автоматического обнаружения</span><span class="sxs-lookup"><span data-stu-id="9f98b-191">Automatic change detection</span></span>

<span data-ttu-id="9f98b-192">Платформа Entity Framework определяет изменении сущности (и поэтому обновления, которые необходимо передать в базу данных), путем сравнения значений текущей сущности с исходными значениями.</span><span class="sxs-lookup"><span data-stu-id="9f98b-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="9f98b-193">Исходные значения хранятся в том случае, когда сущность запрос или присоединенного.</span><span class="sxs-lookup"><span data-stu-id="9f98b-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="9f98b-194">Ниже перечислены некоторые методы, которые вызывают изменение автоматического обнаружения.</span><span class="sxs-lookup"><span data-stu-id="9f98b-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="9f98b-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="9f98b-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="9f98b-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="9f98b-196">DbContext.Entry</span></span>

* <span data-ttu-id="9f98b-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="9f98b-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="9f98b-198">Если вы отслеживаете большого числа сущностей и вызовите один из этих методов много раз в цикле, может появиться значительное повышение производительности, временно отключив автоматическое изменение отслеживания с помощью `ChangeTracker.AutoDetectChangesEnabled` свойство.</span><span class="sxs-lookup"><span data-stu-id="9f98b-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="9f98b-199">Пример:</span><span class="sxs-lookup"><span data-stu-id="9f98b-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="9f98b-200">Entity Framework Core исходного кода и разработки планов</span><span class="sxs-lookup"><span data-stu-id="9f98b-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="9f98b-201">Entity Framework Core исходный код доступен по [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="9f98b-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="9f98b-202">Репозиторий EF Core содержит Ночные построения отслеживания проблемы, технические характеристики функции, конструктора собраний и [стратегия для будущих разработок](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="9f98b-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and the [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="9f98b-203">Файл или поиск ошибок и участие.</span><span class="sxs-lookup"><span data-stu-id="9f98b-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="9f98b-204">Несмотря на то, что исходный код открыт, Entity Framework Core полностью поддерживается как продуктов корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="9f98b-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="9f98b-205">Команда Microsoft Entity Framework поддерживает элемент управления, по которому принимаются вклад и проверяет все изменения кода, чтобы обеспечить его качество каждого выпуска.</span><span class="sxs-lookup"><span data-stu-id="9f98b-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="9f98b-206">В реконструировании из существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="9f98b-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="9f98b-207">Чтобы реконструировать модель данных, включая классы сущностей из существующей базы данных, используйте [dbcontext формирования шаблонов](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) команды.</span><span class="sxs-lookup"><span data-stu-id="9f98b-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="9f98b-208">В разделе [Приступая к работе учебника](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="9f98b-208">See the [getting-started tutorial](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="9f98b-209">Использовать динамические LINQ для упрощения кода Выбор сортировки</span><span class="sxs-lookup"><span data-stu-id="9f98b-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="9f98b-210">[Третьем учебнике этой серии](sort-filter-page.md) показано, как писать код LINQ, жестко запрограммированные имена столбцов в `switch` инструкции.</span><span class="sxs-lookup"><span data-stu-id="9f98b-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="9f98b-211">С двумя столбцами для выбора это работает отлично, но если имеется много столбцов код давало подробных сведений.</span><span class="sxs-lookup"><span data-stu-id="9f98b-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="9f98b-212">Чтобы устранить эту проблему, можно использовать `EF.Property` метод для указания имени свойства в виде строки.</span><span class="sxs-lookup"><span data-stu-id="9f98b-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="9f98b-213">Чтобы проверить этот подход, замените `Index` метод `StudentsController` следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="9f98b-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="9f98b-214">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="9f98b-214">Next steps</span></span>

<span data-ttu-id="9f98b-215">На этом завершается, серию учебники по использованию основного Entity Framework в приложении ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9f98b-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET MVC application.</span></span>

<span data-ttu-id="9f98b-216">Дополнительные сведения об основных EF см. в разделе [документации по Entity Framework Core](https://docs.microsoft.com/ef/core).</span><span class="sxs-lookup"><span data-stu-id="9f98b-216">For more information about EF Core, see the [Entity Framework Core documentation](https://docs.microsoft.com/ef/core).</span></span> <span data-ttu-id="9f98b-217">Доступна также книги: [Entity Framework Core в действии](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="9f98b-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="9f98b-218">Сведения о развертывании веб-приложения см. в разделе [узла и развернуть](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="9f98b-218">For information on how to deploy a web app, see [Host and deploy](xref:host-and-deploy/index).</span></span>

<span data-ttu-id="9f98b-219">Сведения о других разделов, связанных с ASP.NET MVC основных компонентов, таких как проверка подлинности и авторизации, в разделе [документации ASP.NET Core](https://docs.microsoft.com/aspnet/core/).</span><span class="sxs-lookup"><span data-stu-id="9f98b-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see the [ASP.NET Core documentation](https://docs.microsoft.com/aspnet/core/).</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="9f98b-220">Подтверждения</span><span class="sxs-lookup"><span data-stu-id="9f98b-220">Acknowledgments</span></span>

<span data-ttu-id="9f98b-221">Tom Dykstra и Рик Андерсон (twitter @RickAndMSFT) написал этого учебника.</span><span class="sxs-lookup"><span data-stu-id="9f98b-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="9f98b-222">Роуэном Миллер, слово Диего и другими членами команды Entity Framework вспомогательная с проверками кода и помогли отладки проблем, возникших во время мы написание кода для учебников.</span><span class="sxs-lookup"><span data-stu-id="9f98b-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="9f98b-223">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="9f98b-223">Common errors</span></span>  

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="9f98b-224">ContosoUniversity.dll используется другим процессом</span><span class="sxs-lookup"><span data-stu-id="9f98b-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="9f98b-225">Сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="9f98b-225">Error message:</span></span>

> <span data-ttu-id="9f98b-226">Не удается открыть "... bin\Debug\netcoreapp1.0\ContosoUniversity.dll" для записи — "Нет доступа к файлу"... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll ", так как он используется другим процессом.</span><span class="sxs-lookup"><span data-stu-id="9f98b-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="9f98b-227">Решение:</span><span class="sxs-lookup"><span data-stu-id="9f98b-227">Solution:</span></span>

<span data-ttu-id="9f98b-228">Остановите сайт в IIS Express.</span><span class="sxs-lookup"><span data-stu-id="9f98b-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="9f98b-229">На панели задач Windows найти IIS Express и щелкните ее значок правой кнопкой мыши, выберите сайт Contoso университета и затем нажмите кнопку **остановить узел**.</span><span class="sxs-lookup"><span data-stu-id="9f98b-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="9f98b-230">Формирования шаблонов без кода в методы "вверх" и миграции</span><span class="sxs-lookup"><span data-stu-id="9f98b-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="9f98b-231">Возможная причина:</span><span class="sxs-lookup"><span data-stu-id="9f98b-231">Possible cause:</span></span>

<span data-ttu-id="9f98b-232">Команды EF CLI не автоматически закрыть и сохранить файлы кода.</span><span class="sxs-lookup"><span data-stu-id="9f98b-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="9f98b-233">Если у вас есть несохраненные изменения при выполнении `migrations add` команды EF не найдет изменения.</span><span class="sxs-lookup"><span data-stu-id="9f98b-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="9f98b-234">Решение:</span><span class="sxs-lookup"><span data-stu-id="9f98b-234">Solution:</span></span>

<span data-ttu-id="9f98b-235">Запустите `migrations remove` команды, сохранить изменения кода и снова запустите `migrations add` команды.</span><span class="sxs-lookup"><span data-stu-id="9f98b-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="9f98b-236">Ошибок во время выполнения обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="9f98b-236">Errors while running database update</span></span>

<span data-ttu-id="9f98b-237">Можно получить другие ошибки при внесении изменений схемы в базу данных с существующие данные.</span><span class="sxs-lookup"><span data-stu-id="9f98b-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="9f98b-238">Если возникли ошибки миграции, которые не удается устранить, можно изменить имя базы данных в строке соединения или удалить базу данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="9f98b-239">Нет данных для миграции с новой базой данных, и команду update-database гораздо больше шансов завершиться без ошибок.</span><span class="sxs-lookup"><span data-stu-id="9f98b-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="9f98b-240">Самым простым подходом является переименовать базу данных, в *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9f98b-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="9f98b-241">При очередном запуске `database update`, создается новая база данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="9f98b-242">Удалить базу данных в SSOX, щелкните правой кнопкой мыши базу данных, нажмите кнопку **удаление**, а затем в **удаление базы данных** диалогового окна выберите **закрыть существующие соединения** и нажмите кнопку  **ОК**.</span><span class="sxs-lookup"><span data-stu-id="9f98b-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="9f98b-243">Чтобы удалить базу данных с помощью CLI, запустите `database drop` команду CLI:</span><span class="sxs-lookup"><span data-stu-id="9f98b-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="9f98b-244">Ошибка поиску экземпляра SQL Server</span><span class="sxs-lookup"><span data-stu-id="9f98b-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="9f98b-245">Сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="9f98b-245">Error Message:</span></span>

> <span data-ttu-id="9f98b-246">Связанные с сетью или с определенным экземпляром ошибка при установлении соединения с SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9f98b-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="9f98b-247">Сервер не найден или недоступен.</span><span class="sxs-lookup"><span data-stu-id="9f98b-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="9f98b-248">Проверьте правильность имени экземпляра и настройку сервера SQL Server для удаленных подключений.</span><span class="sxs-lookup"><span data-stu-id="9f98b-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="9f98b-249">(поставщик: сетевые интерфейсы SQL, ошибка: 26: ошибка поиске указанного сервера/экземпляра)</span><span class="sxs-lookup"><span data-stu-id="9f98b-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="9f98b-250">Решение:</span><span class="sxs-lookup"><span data-stu-id="9f98b-250">Solution:</span></span>

<span data-ttu-id="9f98b-251">Проверьте строку подключения.</span><span class="sxs-lookup"><span data-stu-id="9f98b-251">Check the connection string.</span></span> <span data-ttu-id="9f98b-252">Если файл базы данных был удален вручную, измените имя базы данных в строке построения, чтобы начать заново с новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="9f98b-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="9f98b-253">Назад</span><span class="sxs-lookup"><span data-stu-id="9f98b-253">Previous</span></span>](inheritance.md)
