---
title: ASP.NET Core MVC с EF Core — расширенные возможности — 10 из 10
author: rick-anderson
description: В этом учебнике описываются полезные рекомендации по расширенным возможностям разработки веб-приложений ASP.NET Core, использующих платформу Entity Framework Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-mvc/advanced
ms.openlocfilehash: ba3834b29e78972bf914a5cba1a2cae3cc19a315
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/18/2019
ms.locfileid: "50090788"
---
# <a name="aspnet-core-mvc-with-ef-core---advanced---10-of-10"></a><span data-ttu-id="858b0-103">ASP.NET Core MVC с EF Core — расширенные возможности — 10 из 10</span><span class="sxs-lookup"><span data-stu-id="858b0-103">ASP.NET Core MVC with EF Core - Advanced - 10 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="858b0-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="858b0-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="858b0-105">На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений ASP.NET Core MVC с помощью Entity Framework Core и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="858b0-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="858b0-106">Сведения о серии руководств см. в [первом руководстве серии](intro.md).</span><span class="sxs-lookup"><span data-stu-id="858b0-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="858b0-107">В предыдущем учебнике было реализовано наследование типа "одна таблица на иерархию".</span><span class="sxs-lookup"><span data-stu-id="858b0-107">In the previous tutorial, you implemented table-per-hierarchy inheritance.</span></span> <span data-ttu-id="858b0-108">В этом учебнике описываются некоторые расширенные возможности, не относящиеся к базовой разработке веб-приложений ASP.NET Core, использующих платформу Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="858b0-108">This tutorial introduces several topics that are useful to be aware of when you go beyond the basics of developing ASP.NET Core web applications that use Entity Framework Core.</span></span>

## <a name="raw-sql-queries"></a><span data-ttu-id="858b0-109">Необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="858b0-109">Raw SQL Queries</span></span>

<span data-ttu-id="858b0-110">Одним из преимуществ использования платформы Entity Framework является возможность избежать слишком тесной привязки кода к конкретному способу хранения данных.</span><span class="sxs-lookup"><span data-stu-id="858b0-110">One of the advantages of using the Entity Framework is that it avoids tying your code too closely to a particular method of storing data.</span></span> <span data-ttu-id="858b0-111">Это достигается путем автоматического создания запросов и команд SQL, что позволяет упростить написание кода.</span><span class="sxs-lookup"><span data-stu-id="858b0-111">It does this by generating SQL queries and commands for you, which also frees you from having to write them yourself.</span></span> <span data-ttu-id="858b0-112">Тем не менее в редких случаях требуется выполнять созданные вручную SQL-запросы.</span><span class="sxs-lookup"><span data-stu-id="858b0-112">But there are exceptional scenarios when you need to run specific SQL queries that you have manually created.</span></span> <span data-ttu-id="858b0-113">Для таких сценариев в API Entity Framework Code First включены методы, позволяющие передавать команды SQL напрямую в базу данных.</span><span class="sxs-lookup"><span data-stu-id="858b0-113">For these scenarios, the Entity Framework Code First API includes methods that enable you to pass SQL commands directly to the database.</span></span> <span data-ttu-id="858b0-114">В EF Core 1.0 доступны следующие варианты:</span><span class="sxs-lookup"><span data-stu-id="858b0-114">You have the following options in EF Core 1.0:</span></span>

* <span data-ttu-id="858b0-115">Использование метода `DbSet.FromSql` для запросов, которые возвращают типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="858b0-115">Use the `DbSet.FromSql` method for queries that return entity types.</span></span> <span data-ttu-id="858b0-116">Возвращаемые объекты должны иметь тип, ожидаемый объектом `DbSet`, и автоматически отслеживаются контекстом базы данных, кроме случаев, когда [отслеживание отключено](crud.md#no-tracking-queries).</span><span class="sxs-lookup"><span data-stu-id="858b0-116">The returned objects must be of the type expected by the `DbSet` object, and they're automatically tracked by the database context unless you [turn tracking off](crud.md#no-tracking-queries).</span></span>

* <span data-ttu-id="858b0-117">Использование `Database.ExecuteSqlCommand` для команд, не относящихся к запросам.</span><span class="sxs-lookup"><span data-stu-id="858b0-117">Use the `Database.ExecuteSqlCommand` for non-query commands.</span></span>

<span data-ttu-id="858b0-118">Если вам необходимо выполнить запрос, который возвращает типы, не являющиеся сущностями, можно использовать ADO.NET с подключением к базе данных, предоставленным платформой EF.</span><span class="sxs-lookup"><span data-stu-id="858b0-118">If you need to run a query that returns types that aren't entities, you can use ADO.NET with the database connection provided by EF.</span></span> <span data-ttu-id="858b0-119">Возвращаемые данные не отслеживаются контекстом базы данных, даже если вы используете этот метод для извлечения типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="858b0-119">The returned data isn't tracked by the database context, even if you use this method to retrieve entity types.</span></span>

<span data-ttu-id="858b0-120">Как и всегда при выполнении команд SQL в веб-приложении, необходимо принимать меры предосторожности для защиты сайта от атак путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="858b0-120">As is always true when you execute SQL commands in a web application, you must take precautions to protect your site against SQL injection attacks.</span></span> <span data-ttu-id="858b0-121">Одним из способов защиты является применение параметризованных запросов, которые гарантируют, что строки, отправляемые веб-страницей, не могут быть интерпретированы как команды SQL.</span><span class="sxs-lookup"><span data-stu-id="858b0-121">One way to do that is to use parameterized queries to make sure that strings submitted by a web page can't be interpreted as SQL commands.</span></span> <span data-ttu-id="858b0-122">В рамках этого учебника вы будете использовать параметризованные запросы при интеграции вводимых пользователем данных в запрос.</span><span class="sxs-lookup"><span data-stu-id="858b0-122">In this tutorial you'll use parameterized queries when integrating user input into a query.</span></span>

## <a name="call-a-query-that-returns-entities"></a><span data-ttu-id="858b0-123">Вызов запроса, возвращающего сущности</span><span class="sxs-lookup"><span data-stu-id="858b0-123">Call a query that returns entities</span></span>

<span data-ttu-id="858b0-124">Класс `DbSet<TEntity>` предоставляет метод, который можно использовать для выполнения запроса, возвращающего сущность типа `TEntity`.</span><span class="sxs-lookup"><span data-stu-id="858b0-124">The `DbSet<TEntity>` class provides a method that you can use to execute a query that returns an entity of type `TEntity`.</span></span> <span data-ttu-id="858b0-125">Чтобы увидеть, как это работает, измените код в методе `Details` для контроллера Department.</span><span class="sxs-lookup"><span data-stu-id="858b0-125">To see how this works you'll change the code in the `Details` method of the Department controller.</span></span>

<span data-ttu-id="858b0-126">В файле *DepartmentsController.cs* в методе `Details` замените код, извлекающий кафедру, вызовом метода `FromSql`, как показано ниже в выделенном коде:</span><span class="sxs-lookup"><span data-stu-id="858b0-126">In *DepartmentsController.cs*, in the `Details` method, replace the code that retrieves a department with a `FromSql` method call, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

<span data-ttu-id="858b0-127">Чтобы убедиться, что новый код работает правильно, выберите вкладку **Departments** (Кафедры) и щелкните **Details** (Сведения) для одной из кафедр.</span><span class="sxs-lookup"><span data-stu-id="858b0-127">To verify that the new code works correctly, select the **Departments** tab and then **Details** for one of the departments.</span></span>

![Сведения о кафедре](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a><span data-ttu-id="858b0-129">Вызов запроса, возвращающего другие типы</span><span class="sxs-lookup"><span data-stu-id="858b0-129">Call a query that returns other types</span></span>

<span data-ttu-id="858b0-130">Ранее вы создали таблицу статистики учащихся на странице сведений, в которой было показано число учащихся на каждую дату регистрации.</span><span class="sxs-lookup"><span data-stu-id="858b0-130">Earlier you created a student statistics grid for the About page that showed the number of students for each enrollment date.</span></span> <span data-ttu-id="858b0-131">Вы получали эти данные из набора сущностей Students (`_context.Students`) и использовали LINQ, чтобы спроецировать результаты в список объектов модели представления `EnrollmentDateGroup`.</span><span class="sxs-lookup"><span data-stu-id="858b0-131">You got the data from the Students entity set (`_context.Students`) and used LINQ to project the results into a list of `EnrollmentDateGroup` view model objects.</span></span> <span data-ttu-id="858b0-132">Предположим, что вы хотите написать код SQL вместо использования LINQ.</span><span class="sxs-lookup"><span data-stu-id="858b0-132">Suppose you want to write the SQL itself rather than using LINQ.</span></span> <span data-ttu-id="858b0-133">Для этого вам необходимо выполнить SQL-запрос, который возвращает объекты, не являющиеся сущностями.</span><span class="sxs-lookup"><span data-stu-id="858b0-133">To do that you need to run a SQL query that returns something other than entity objects.</span></span> <span data-ttu-id="858b0-134">В EF Core 1.0 одним из способов сделать это является написание кода ADO.NET и получение подключения к базе данных из EF.</span><span class="sxs-lookup"><span data-stu-id="858b0-134">In EF Core 1.0, one way to do that is write ADO.NET code and get the database connection from EF.</span></span>

<span data-ttu-id="858b0-135">В файле *HomeController.cs* замените метод `About` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="858b0-135">In *HomeController.cs*, replace the `About` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

<span data-ttu-id="858b0-136">Добавьте инструкцию using:</span><span class="sxs-lookup"><span data-stu-id="858b0-136">Add a using statement:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

<span data-ttu-id="858b0-137">Запустите приложение и перейдите на страницу About.</span><span class="sxs-lookup"><span data-stu-id="858b0-137">Run the app and go to the About page.</span></span> <span data-ttu-id="858b0-138">На экран будут выведены те же данные, что и ранее.</span><span class="sxs-lookup"><span data-stu-id="858b0-138">It displays the same data it did before.</span></span>

![Страница About](advanced/_static/about.png)

## <a name="call-an-update-query"></a><span data-ttu-id="858b0-140">Вызов запроса на обновление</span><span class="sxs-lookup"><span data-stu-id="858b0-140">Call an update query</span></span>

<span data-ttu-id="858b0-141">Предположим, что администраторам университета Suppose необходимо внести глобальные изменения в базу данных, например изменить число зачетных баллов для каждого курса.</span><span class="sxs-lookup"><span data-stu-id="858b0-141">Suppose Contoso University administrators want to perform global changes in the database, such as changing the number of credits for every course.</span></span> <span data-ttu-id="858b0-142">Поскольку в университете ведется множество курсов, будет неэффективно извлекать их в виде сущностей и изменять по отдельности.</span><span class="sxs-lookup"><span data-stu-id="858b0-142">If the university has a large number of courses, it would be inefficient to retrieve them all as entities and change them individually.</span></span> <span data-ttu-id="858b0-143">В этом разделе вы реализуете веб-страницу, на которой пользователь может задать множитель, который будет применен к числу зачетных баллов для каждого курса, после чего изменения будут внесены с помощью инструкции SQL UPDATE.</span><span class="sxs-lookup"><span data-stu-id="858b0-143">In this section you'll implement a web page that enables the user to specify a factor by which to change the number of credits for all courses, and you'll make the change by executing a SQL UPDATE statement.</span></span> <span data-ttu-id="858b0-144">Веб-страница должна выглядеть так, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="858b0-144">The web page will look like the following illustration:</span></span>

![Страница обновления числа зачетных баллов для курсов](advanced/_static/update-credits.png)

<span data-ttu-id="858b0-146">В файле *CoursesContoller.cs* добавьте методы UpdateCourseCredits для HttpGet и HttpPost:</span><span class="sxs-lookup"><span data-stu-id="858b0-146">In *CoursesContoller.cs*, add UpdateCourseCredits methods for HttpGet and HttpPost:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

<span data-ttu-id="858b0-147">Когда контроллер обрабатывает запрос HttpGet, в `ViewData["RowsAffected"]` ничего не возвращается, а в представлении отображается пустое текстовое поле и кнопка отправки, как показано на предыдущем рисунке.</span><span class="sxs-lookup"><span data-stu-id="858b0-147">When the controller processes an HttpGet request, nothing is returned in `ViewData["RowsAffected"]`, and the view displays an empty text box and a submit button, as shown in the preceding illustration.</span></span>

<span data-ttu-id="858b0-148">При нажатии кнопки **Update** (Обновить) вызывается метод HttpPost, а множителю присваивается значение, введенное в текстовое поле.</span><span class="sxs-lookup"><span data-stu-id="858b0-148">When the **Update** button is clicked, the HttpPost method is called, and multiplier has the value entered in the text box.</span></span> <span data-ttu-id="858b0-149">После этого код выполняет SQL-запрос, который обновляет курсы и возвращает число затронутых строк в представление в `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="858b0-149">The code then executes the SQL that updates courses and returns the number of affected rows to the view in `ViewData`.</span></span> <span data-ttu-id="858b0-150">После того как в представление передано значение `RowsAffected`, оно отображает число обновленных строк.</span><span class="sxs-lookup"><span data-stu-id="858b0-150">When the view gets a `RowsAffected` value, it displays the number of rows updated.</span></span>

<span data-ttu-id="858b0-151">В **обозревателе решений** щелкните правой кнопкой мыши папку *Views/Courses* и выберите **Добавить > Новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="858b0-151">In **Solution Explorer**, right-click the *Views/Courses* folder, and then click **Add > New Item**.</span></span>

<span data-ttu-id="858b0-152">В диалоговом окне **Добавление нового элемента** щелкните элемент **ASP.NET** в разделе **Установленные** в области слева, выберите **Страница представления MVC** и присвойте новому представлению имя *UpdateCourseCredits.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="858b0-152">In the **Add New Item** dialog, click **ASP.NET** under **Installed** in the left pane, click **MVC View Page**, and name the new view *UpdateCourseCredits.cshtml*.</span></span>

<span data-ttu-id="858b0-153">В файле *Views/Courses/UpdateCourseCredits.cshtml* замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="858b0-153">In *Views/Courses/UpdateCourseCredits.cshtml*, replace the template code with the following code:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

<span data-ttu-id="858b0-154">Выполните метод `UpdateCourseCredits`, выбрав вкладку **Courses** (Курсы), а затем добавив "/UpdateCourseCredits" в конец URL-адреса в адресной строке браузера (например, `http://localhost:5813/Courses/UpdateCourseCredits`).</span><span class="sxs-lookup"><span data-stu-id="858b0-154">Run the `UpdateCourseCredits` method by selecting the **Courses** tab, then adding "/UpdateCourseCredits" to the end of the URL in the browser's address bar (for example: `http://localhost:5813/Courses/UpdateCourseCredits`).</span></span> <span data-ttu-id="858b0-155">Введите число в текстовое поле:</span><span class="sxs-lookup"><span data-stu-id="858b0-155">Enter a number in the text box:</span></span>

![Страница обновления числа зачетных баллов для курсов](advanced/_static/update-credits.png)

<span data-ttu-id="858b0-157">Нажмите кнопку **Обновить**.</span><span class="sxs-lookup"><span data-stu-id="858b0-157">Click **Update**.</span></span> <span data-ttu-id="858b0-158">Отобразится число обработанных строк:</span><span class="sxs-lookup"><span data-stu-id="858b0-158">You see the number of rows affected:</span></span>

![Число затронутых строк на странице обновления числа зачетных баллов для курсов](advanced/_static/update-credits-rows-affected.png)

<span data-ttu-id="858b0-160">Нажмите кнопку **Back to List** (Вернуться к списку), чтобы просмотреть список курсов с измененным числом зачетных баллов.</span><span class="sxs-lookup"><span data-stu-id="858b0-160">Click **Back to List** to see the list of courses with the revised number of credits.</span></span>

<span data-ttu-id="858b0-161">Обратите внимание, что в рабочем коде необходимо всегда проверять допустимость новых данных.</span><span class="sxs-lookup"><span data-stu-id="858b0-161">Note that production code would ensure that updates always result in valid data.</span></span> <span data-ttu-id="858b0-162">В приведенном здесь упрощенном коде в результате применения множителя могут получиться значения больше 5.</span><span class="sxs-lookup"><span data-stu-id="858b0-162">The simplified code shown here could multiply the number of credits enough to result in numbers greater than 5.</span></span> <span data-ttu-id="858b0-163">(Свойство `Credits` имеет атрибут `[Range(0, 5)]`.) Запрос на обновление будет выполнен, однако из-за недопустимых данных в других частях системы, в которых число зачетных баллов должно быть не больше 5, могут возникать неожиданные результаты.</span><span class="sxs-lookup"><span data-stu-id="858b0-163">(The `Credits` property has a `[Range(0, 5)]` attribute.) The update query would work but the invalid data could cause unexpected results in other parts of the system that assume the number of credits is 5 or less.</span></span>

<span data-ttu-id="858b0-164">Дополнительные сведения о необработанных SQL-запросах см. в разделе [Необработанные SQL-запросы](/ef/core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="858b0-164">For more information about raw SQL queries, see [Raw SQL Queries](/ef/core/querying/raw-sql).</span></span>

## <a name="examine-sql-sent-to-the-database"></a><span data-ttu-id="858b0-165">Проверка кода SQL, отправляемого в базу данных</span><span class="sxs-lookup"><span data-stu-id="858b0-165">Examine SQL sent to the database</span></span>

<span data-ttu-id="858b0-166">В некоторых случаях полезно иметь возможность просмотреть фактические SQL-запросы, отправляемые в базу данных.</span><span class="sxs-lookup"><span data-stu-id="858b0-166">Sometimes it's helpful to be able to see the actual SQL queries that are sent to the database.</span></span> <span data-ttu-id="858b0-167">Платформа EF Core использует встроенные функции ASP.NET Core для автоматического ведения журналов, содержащих код SQL запросов и обновлений.</span><span class="sxs-lookup"><span data-stu-id="858b0-167">Built-in logging functionality for ASP.NET Core is automatically used by EF Core to write logs that contain the SQL for queries and updates.</span></span> <span data-ttu-id="858b0-168">В этом разделе приводятся некоторые примеры ведения журналов кода SQL.</span><span class="sxs-lookup"><span data-stu-id="858b0-168">In this section you'll see some examples of SQL logging.</span></span>

<span data-ttu-id="858b0-169">Откройте файл *StudentsController.cs* и установите в методе `Details` точку останова на инструкции `if (student == null)`.</span><span class="sxs-lookup"><span data-stu-id="858b0-169">Open *StudentsController.cs* and in the `Details` method set a breakpoint on the `if (student == null)` statement.</span></span>

<span data-ttu-id="858b0-170">Запустите приложение в режиме отладки и перейдите на страницу Details (Сведения) для учащегося.</span><span class="sxs-lookup"><span data-stu-id="858b0-170">Run the app in debug mode, and go to the Details page for a student.</span></span>

<span data-ttu-id="858b0-171">Перейдите в **окно вывода**, в котором отображаются результаты отладки. В нем будет показан запрос:</span><span class="sxs-lookup"><span data-stu-id="858b0-171">Go to the **Output** window showing debug output, and you see the query:</span></span>

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

<span data-ttu-id="858b0-172">Вы можете заметить удивительные результаты: код SQL выбирает до 2 строк (`TOP(2)`) из таблицы Person.</span><span class="sxs-lookup"><span data-stu-id="858b0-172">You'll notice something here that might surprise you: the SQL selects up to 2 rows (`TOP(2)`) from the Person table.</span></span> <span data-ttu-id="858b0-173">Метод `SingleOrDefaultAsync` не разрешается в 1 строку на сервере.</span><span class="sxs-lookup"><span data-stu-id="858b0-173">The `SingleOrDefaultAsync` method doesn't resolve to 1 row on the server.</span></span> <span data-ttu-id="858b0-174">Далее описывается, почему это происходит:</span><span class="sxs-lookup"><span data-stu-id="858b0-174">Here's why:</span></span>

* <span data-ttu-id="858b0-175">Если запрос возвращает несколько строк, метод возвращает значение NULL.</span><span class="sxs-lookup"><span data-stu-id="858b0-175">If the query would return multiple rows, the method returns null.</span></span>
* <span data-ttu-id="858b0-176">Чтобы определить, будет ли запрос возвращать несколько строк, платформа EF проверяет, возвращаются ли как минимум 2 строки.</span><span class="sxs-lookup"><span data-stu-id="858b0-176">To determine whether the query would return multiple rows, EF has to check if it returns at least 2.</span></span>

<span data-ttu-id="858b0-177">Обратите внимание, что вам необязательно использовать режим отладки и доходить до точки останова, чтобы просмотреть содержимое журнала в **окне вывода**.</span><span class="sxs-lookup"><span data-stu-id="858b0-177">Note that you don't have to use debug mode and stop at a breakpoint to get logging output in the **Output** window.</span></span> <span data-ttu-id="858b0-178">Это просто удобный способ остановить ведение журнала в тот момент, когда вам нужно просмотреть выходные данные.</span><span class="sxs-lookup"><span data-stu-id="858b0-178">It's just a convenient way to stop the logging at the point you want to look at the output.</span></span> <span data-ttu-id="858b0-179">Если не сделать этого, ведение журнала продолжится и вам придется прокручивать окно до нужного места.</span><span class="sxs-lookup"><span data-stu-id="858b0-179">If you don't do that, logging continues and you have to scroll back to find the parts you're interested in.</span></span>

## <a name="repository-and-unit-of-work-patterns"></a><span data-ttu-id="858b0-180">Шаблоны репозитория и единиц работы</span><span class="sxs-lookup"><span data-stu-id="858b0-180">Repository and unit of work patterns</span></span>

<span data-ttu-id="858b0-181">Многие разработчики пишут код, реализующий шаблоны репозитория и единиц работы, в качестве оболочки для кода, работающего с платформой Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="858b0-181">Many developers write code to implement the repository and unit of work patterns as a wrapper around code that works with the Entity Framework.</span></span> <span data-ttu-id="858b0-182">Эти шаблоны позволяют создать уровень абстракции между уровнями доступа к данным и бизнес-логики приложения.</span><span class="sxs-lookup"><span data-stu-id="858b0-182">These patterns are intended to create an abstraction layer between the data access layer and the business logic layer of an application.</span></span> <span data-ttu-id="858b0-183">Реализация таких шаблонов позволяет изолировать приложение от изменений в хранилище данных и упрощает автоматическое модульное тестирование или разработку на основе тестирования.</span><span class="sxs-lookup"><span data-stu-id="858b0-183">Implementing these patterns can help insulate your application from changes in the data store and can facilitate automated unit testing or test-driven development (TDD).</span></span> <span data-ttu-id="858b0-184">Тем не менее написание дополнительного кода для реализации этих шаблонов не всегда подходит для приложений, которые используют платформу EF. Этому есть несколько причин:</span><span class="sxs-lookup"><span data-stu-id="858b0-184">However, writing additional code to implement these patterns isn't always the best choice for applications that use EF, for several reasons:</span></span>

* <span data-ttu-id="858b0-185">Класс контекста EF сам по себе изолирует код от кода хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="858b0-185">The EF context class itself insulates your code from data-store-specific code.</span></span>

* <span data-ttu-id="858b0-186">Класс контекста EF может выступать в качестве класса единиц работы для обновления базы данных, которые выполняются с помощью EF.</span><span class="sxs-lookup"><span data-stu-id="858b0-186">The EF context class can act as a unit-of-work class for database updates that you do using EF.</span></span>

* <span data-ttu-id="858b0-187">Платформа EF предусматривает функции для реализации разработки на основе тестирования, не требующие написания кода репозитория.</span><span class="sxs-lookup"><span data-stu-id="858b0-187">EF includes features for implementing TDD without writing repository code.</span></span>

<span data-ttu-id="858b0-188">Дополнительные сведения о реализации шаблонов репозитория и единиц работы см. в [версии этой серии учебников для платформы Entity Framework 5](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="858b0-188">For information about how to implement the repository and unit of work patterns, see [the Entity Framework 5 version of this tutorial series](/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).</span></span>

<span data-ttu-id="858b0-189">Платформа Entity Framework Core реализует выполняющийся в памяти поставщик базы данных, который может использоваться для тестирования.</span><span class="sxs-lookup"><span data-stu-id="858b0-189">Entity Framework Core implements an in-memory database provider that can be used for testing.</span></span> <span data-ttu-id="858b0-190">Дополнительные сведения см. в разделе [Тестирование с помощью InMemory](/ef/core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="858b0-190">For more information, see [Test with InMemory](/ef/core/miscellaneous/testing/in-memory).</span></span>

## <a name="automatic-change-detection"></a><span data-ttu-id="858b0-191">Автоматическое обнаружение изменений</span><span class="sxs-lookup"><span data-stu-id="858b0-191">Automatic change detection</span></span>

<span data-ttu-id="858b0-192">Платформа Entity Framework определяет, как была изменена сущность (и, соответственно, какие обновления требуется отправить в базу данных), сравнивая текущие значения сущности с исходными.</span><span class="sxs-lookup"><span data-stu-id="858b0-192">The Entity Framework determines how an entity has changed (and therefore which updates need to be sent to the database) by comparing the current values of an entity with the original values.</span></span> <span data-ttu-id="858b0-193">Исходные значения сохраняются при запросе или присоединении сущности.</span><span class="sxs-lookup"><span data-stu-id="858b0-193">The original values are stored when the entity is queried or attached.</span></span> <span data-ttu-id="858b0-194">Ниже перечислены некоторые из методов, которые приводят к автоматическому обнаружению изменений:</span><span class="sxs-lookup"><span data-stu-id="858b0-194">Some of the methods that cause automatic change detection are the following:</span></span>

* <span data-ttu-id="858b0-195">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="858b0-195">DbContext.SaveChanges</span></span>

* <span data-ttu-id="858b0-196">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="858b0-196">DbContext.Entry</span></span>

* <span data-ttu-id="858b0-197">ChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="858b0-197">ChangeTracker.Entries</span></span>

<span data-ttu-id="858b0-198">Если вы отслеживаете большое число сущностей и многократно вызываете один из этих методов в цикле, вы сможете добиться заметного повышения производительности, отключив автоматическое обнаружение изменений с помощью свойства `ChangeTracker.AutoDetectChangesEnabled`.</span><span class="sxs-lookup"><span data-stu-id="858b0-198">If you're tracking a large number of entities and you call one of these methods many times in a loop, you might get significant performance improvements by temporarily turning off automatic change detection using the `ChangeTracker.AutoDetectChangesEnabled` property.</span></span> <span data-ttu-id="858b0-199">Например:</span><span class="sxs-lookup"><span data-stu-id="858b0-199">For example:</span></span>

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a><span data-ttu-id="858b0-200">Исходный код Entity Framework Core и планы разработки</span><span class="sxs-lookup"><span data-stu-id="858b0-200">Entity Framework Core source code and development plans</span></span>

<span data-ttu-id="858b0-201">Источник Entity Framework Core расположен на странице [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="858b0-201">The Entity Framework Core source is at [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore).</span></span> <span data-ttu-id="858b0-202">Репозиторий EF Core содержит ночные сборки, результаты отслеживания проблем, спецификации функций, протоколы совещаний по проекту, а также [стратегию дальнейшей разработки](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="858b0-202">The EF Core repository contains nightly builds, issue tracking, feature specs, design meeting notes, and [the roadmap for future development](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap).</span></span> <span data-ttu-id="858b0-203">Вы также можете сообщать об ошибках, находить сведения об обнаруженных проблемах и участвовать в работе сообщества.</span><span class="sxs-lookup"><span data-stu-id="858b0-203">You can file or find bugs, and contribute.</span></span>

<span data-ttu-id="858b0-204">Несмотря на открытый исходный код, платформа Entity Framework Core полностью поддерживается как продукт корпорации Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="858b0-204">Although the source code is open, Entity Framework Core is fully supported as a Microsoft product.</span></span> <span data-ttu-id="858b0-205">Команда Microsoft Entity Framework контролирует предложения участников, принимает их и тестирует любые изменения кода, чтобы обеспечить максимальное качество каждого выпуска.</span><span class="sxs-lookup"><span data-stu-id="858b0-205">The Microsoft Entity Framework team keeps control over which contributions are accepted and tests all code changes to ensure the quality of each release.</span></span>

## <a name="reverse-engineer-from-existing-database"></a><span data-ttu-id="858b0-206">Реконструирование из существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="858b0-206">Reverse engineer from existing database</span></span>

<span data-ttu-id="858b0-207">Чтобы реконструировать модель данных, включая классы сущностей, из существующей базы данных, используйте команду [scaffold-dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="858b0-207">To reverse engineer a data model including entity classes from an existing database, use the [scaffold-dbcontext](/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) command.</span></span> <span data-ttu-id="858b0-208">Ознакомьтесь с [учебником по началу работы](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="858b0-208">See the [getting-started tutorial](/ef/core/get-started/aspnetcore/existing-db).</span></span>

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a><span data-ttu-id="858b0-209">Использование динамического LINQ для упрощения кода выборки сортировки</span><span class="sxs-lookup"><span data-stu-id="858b0-209">Use dynamic LINQ to simplify sort selection code</span></span>

<span data-ttu-id="858b0-210">В [третьем учебнике этой серии](sort-filter-page.md) демонстрируется написание кода LINQ с жестко запрограммированными именами столбцов в инструкции `switch`.</span><span class="sxs-lookup"><span data-stu-id="858b0-210">The [third tutorial in this series](sort-filter-page.md) shows how to write LINQ code by hard-coding column names in a `switch` statement.</span></span> <span data-ttu-id="858b0-211">При наличии всего двух столбцов такой подход эффективен, однако если столбцов много, код может стать слишком громоздким.</span><span class="sxs-lookup"><span data-stu-id="858b0-211">With two columns to choose from, this works fine, but if you have many columns the code could get verbose.</span></span> <span data-ttu-id="858b0-212">Чтобы устранить эту проблему, можно использовать метод `EF.Property` для указания имени свойства в виде строки.</span><span class="sxs-lookup"><span data-stu-id="858b0-212">To solve that problem, you can use the `EF.Property` method to specify the name of the property as a string.</span></span> <span data-ttu-id="858b0-213">Чтобы попробовать этот подход, замените метод `Index` в `StudentsController` следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="858b0-213">To try out this approach, replace the `Index` method in the `StudentsController` with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a><span data-ttu-id="858b0-214">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="858b0-214">Next steps</span></span>

<span data-ttu-id="858b0-215">На этом серия учебников, посвященных использованию платформы Entity Framework Core в приложении ASP.NET Core MVC, завершена.</span><span class="sxs-lookup"><span data-stu-id="858b0-215">This completes this series of tutorials on using the Entity Framework Core in an ASP.NET Core MVC application.</span></span>

<span data-ttu-id="858b0-216">Дополнительные сведения о EF Core см. в [документации по платформе Entity Framework Core](/ef/core).</span><span class="sxs-lookup"><span data-stu-id="858b0-216">For more information about EF Core, see the [Entity Framework Core documentation](/ef/core).</span></span> <span data-ttu-id="858b0-217">Можно также прочесть книгу [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span><span class="sxs-lookup"><span data-stu-id="858b0-217">A book is also available: [Entity Framework Core in Action](https://www.manning.com/books/entity-framework-core-in-action).</span></span>

<span data-ttu-id="858b0-218">Сведения о развертывании веб-приложения см. в <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="858b0-218">For information on how to deploy a web app, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="858b0-219">Дополнительные сведения по другим вопросам, связанным с использованием ASP.NET Core MVC, включая способы проверки подлинности и авторизации, см. в <xref:index>.</span><span class="sxs-lookup"><span data-stu-id="858b0-219">For information about other topics related to ASP.NET Core MVC, such as authentication and authorization, see <xref:index>.</span></span>

## <a name="acknowledgments"></a><span data-ttu-id="858b0-220">Благодарности</span><span class="sxs-lookup"><span data-stu-id="858b0-220">Acknowledgments</span></span>

<span data-ttu-id="858b0-221">Этот учебник написан Томом Дайкстра (Tom Dykstra) и Риком Андерсоном (Rick Anderson) (twitter @RickAndMSFT).</span><span class="sxs-lookup"><span data-stu-id="858b0-221">Tom Dykstra and Rick Anderson (twitter @RickAndMSFT) wrote this tutorial.</span></span> <span data-ttu-id="858b0-222">Помощь в проверке кода для учебника и отладке проблем, возникавших при его написании, оказывали Роуэн Миллер (Rowan Miller), Диего Вега (Diego Vega) и другие участники команды Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="858b0-222">Rowan Miller, Diego Vega, and other members of the Entity Framework team assisted with code reviews and helped debug issues that arose while we were writing code for the tutorials.</span></span>

## <a name="common-errors"></a><span data-ttu-id="858b0-223">Распространенные ошибки</span><span class="sxs-lookup"><span data-stu-id="858b0-223">Common errors</span></span>

### <a name="contosouniversitydll-used-by-another-process"></a><span data-ttu-id="858b0-224">Библиотека ContosoUniversity.dll используется другим процессом</span><span class="sxs-lookup"><span data-stu-id="858b0-224">ContosoUniversity.dll used by another process</span></span>

<span data-ttu-id="858b0-225">Сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="858b0-225">Error message:</span></span>

> <span data-ttu-id="858b0-226">Не удается открыть файл "...bin\Debug\netcoreapp1.0\ContosoUniversity.dll" для записи — "Процесс не может получить доступ к файлу "...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll", так как этот файл занят другим процессом.</span><span class="sxs-lookup"><span data-stu-id="858b0-226">Cannot open '...bin\Debug\netcoreapp1.0\ContosoUniversity.dll' for writing -- 'The process cannot access the file '...\bin\Debug\netcoreapp1.0\ContosoUniversity.dll' because it is being used by another process.</span></span>

<span data-ttu-id="858b0-227">Решение:</span><span class="sxs-lookup"><span data-stu-id="858b0-227">Solution:</span></span>

<span data-ttu-id="858b0-228">Остановите сайт в IIS Express.</span><span class="sxs-lookup"><span data-stu-id="858b0-228">Stop the site in IIS Express.</span></span> <span data-ttu-id="858b0-229">Найдите значок IIS Express в панели задач Windows, щелкните его правой кнопкой мыши, выберите сайт университета Contoso и затем выберите **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="858b0-229">Go to the Windows System Tray, find IIS Express and right-click its icon, select the Contoso University site, and then click **Stop Site**.</span></span>

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a><span data-ttu-id="858b0-230">Формирование шаблонов миграции без кода в методах Up и Down</span><span class="sxs-lookup"><span data-stu-id="858b0-230">Migration scaffolded with no code in Up and Down methods</span></span>

<span data-ttu-id="858b0-231">Возможная причина:</span><span class="sxs-lookup"><span data-stu-id="858b0-231">Possible cause:</span></span>

<span data-ttu-id="858b0-232">Команды интерфейса командной строки EF не выполняют автоматическое закрытие и сохранение файлов кода.</span><span class="sxs-lookup"><span data-stu-id="858b0-232">The EF CLI commands don't automatically close and save code files.</span></span> <span data-ttu-id="858b0-233">Если при выполнении команды `migrations add` у вас есть несохраненные изменения, платформа EF не сможет обнаружить их.</span><span class="sxs-lookup"><span data-stu-id="858b0-233">If you have unsaved changes when you run the `migrations add` command, EF won't find your changes.</span></span>

<span data-ttu-id="858b0-234">Решение:</span><span class="sxs-lookup"><span data-stu-id="858b0-234">Solution:</span></span>

<span data-ttu-id="858b0-235">Выполните команду `migrations remove`, сохраните изменения в коде и повторно выполните команду `migrations add`.</span><span class="sxs-lookup"><span data-stu-id="858b0-235">Run the `migrations remove` command, save your code changes and rerun the `migrations add` command.</span></span>

### <a name="errors-while-running-database-update"></a><span data-ttu-id="858b0-236">Ошибки во время обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="858b0-236">Errors while running database update</span></span>

<span data-ttu-id="858b0-237">При изменении схемы в базе, содержащей существующие данные, возможны другие ошибки.</span><span class="sxs-lookup"><span data-stu-id="858b0-237">It's possible to get other errors when making schema changes in a database that has existing data.</span></span> <span data-ttu-id="858b0-238">Если вы получаете ошибки миграции, которые не удается устранить, измените имя базы данных в строке подключения или удалите базу данных.</span><span class="sxs-lookup"><span data-stu-id="858b0-238">If you get migration errors you can't resolve, you can either change the database name in the connection string or delete the database.</span></span> <span data-ttu-id="858b0-239">В новой базе не будет данных, которые требуется перенести, в результате чего команда обновления базы данных с большей долей вероятности завершится без ошибок.</span><span class="sxs-lookup"><span data-stu-id="858b0-239">With a new database, there's no data to migrate, and the update-database command is much more likely to complete without errors.</span></span>

<span data-ttu-id="858b0-240">Проще всего в этом случае переименовать базу данных в файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="858b0-240">The simplest approach is to rename the database in *appsettings.json*.</span></span> <span data-ttu-id="858b0-241">В следующий раз при выполнении команды `database update` будет создана новая база данных.</span><span class="sxs-lookup"><span data-stu-id="858b0-241">The next time you run `database update`, a new database will be created.</span></span>

<span data-ttu-id="858b0-242">Чтобы удалить базу данных в средстве SSOX, щелкните ее правой кнопкой мыши, выберите **Удалить**, а затем в диалоговом окне **Удаление базы данных** выберите **Закрыть существующие соединения** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="858b0-242">To delete a database in SSOX, right-click the database, click **Delete**, and then in the **Delete Database** dialog box select **Close existing connections** and click **OK**.</span></span>

<span data-ttu-id="858b0-243">Чтобы удалить базу данных с помощью интерфейса командной строки, выполните команду `database drop`:</span><span class="sxs-lookup"><span data-stu-id="858b0-243">To delete a database by using the CLI, run the `database drop` CLI command:</span></span>

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a><span data-ttu-id="858b0-244">Ошибка при обнаружении экземпляра SQL Server</span><span class="sxs-lookup"><span data-stu-id="858b0-244">Error locating SQL Server instance</span></span>

<span data-ttu-id="858b0-245">Сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="858b0-245">Error Message:</span></span>

> <span data-ttu-id="858b0-246">При установлении подключения к SQL Server произошла ошибка сети или ошибка экземпляра.</span><span class="sxs-lookup"><span data-stu-id="858b0-246">A network-related or instance-specific error occurred while establishing a connection to SQL Server.</span></span> <span data-ttu-id="858b0-247">Сервер не найден или недоступен.</span><span class="sxs-lookup"><span data-stu-id="858b0-247">The server was not found or was not accessible.</span></span> <span data-ttu-id="858b0-248">Проверьте правильность имени экземпляра и настройку сервера SQL Server для удаленных подключений.</span><span class="sxs-lookup"><span data-stu-id="858b0-248">Verify that the instance name is correct and that SQL Server is configured to allow remote connections.</span></span> <span data-ttu-id="858b0-249">(поставщик: сетевые интерфейсы SQL, ошибка: 26 — ошибка при обнаружении указанного сервера или экземпляра)</span><span class="sxs-lookup"><span data-stu-id="858b0-249">(provider: SQL Network Interfaces, error: 26 - Error Locating Server/Instance Specified)</span></span>

<span data-ttu-id="858b0-250">Решение:</span><span class="sxs-lookup"><span data-stu-id="858b0-250">Solution:</span></span>

<span data-ttu-id="858b0-251">Проверьте строку подключения.</span><span class="sxs-lookup"><span data-stu-id="858b0-251">Check the connection string.</span></span> <span data-ttu-id="858b0-252">Если вы вручную удалили файл базы данных, измените имя базы данных в строке подключения, чтобы начать работу с новой базой.</span><span class="sxs-lookup"><span data-stu-id="858b0-252">If you have manually deleted the database file, change the name of the database in the construction string to start over with a new database.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> [<span data-ttu-id="858b0-253">Назад</span><span class="sxs-lookup"><span data-stu-id="858b0-253">Previous</span></span>](inheritance.md)
