---
title: ASP.NET Core MVC и EF Core — чтение связанных данных — 6 из 10
author: rick-anderson
description: Из этого руководства вы узнаете, как читать и отображать связанные данные — данные, которые Entity Framework загружает в свойства навигации.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 8c634bb1ae715776e18b847574ce03791f2ede03
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277216"
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="afa04-103">ASP.NET Core MVC и EF Core — чтение связанных данных — 6 из 10</span><span class="sxs-lookup"><span data-stu-id="afa04-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

<span data-ttu-id="afa04-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="afa04-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="afa04-105">На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений ASP.NET Core MVC с помощью Entity Framework Core и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="afa04-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="afa04-106">Сведения о серии руководств см. в [первом руководстве серии](intro.md).</span><span class="sxs-lookup"><span data-stu-id="afa04-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="afa04-107">В предыдущем руководстве мы завершили разработку модели данных School.</span><span class="sxs-lookup"><span data-stu-id="afa04-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="afa04-108">Из этого руководства вы узнаете, как читать и отображать связанные данные — данные, которые Entity Framework загружает в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="afa04-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="afa04-109">На следующих рисунках изображены страницы, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="afa04-109">The following illustrations show the pages that you'll work with.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="afa04-112">Безотложная, явная и отложенная загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="afa04-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="afa04-113">Существует несколько способов, которыми программное обеспечение объектно-реляционного сопоставления (ORM), такое как Entity Framework, может загружать связанные данные в свойства навигации сущности:</span><span class="sxs-lookup"><span data-stu-id="afa04-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="afa04-114">Безотложная загрузка.</span><span class="sxs-lookup"><span data-stu-id="afa04-114">Eager loading.</span></span> <span data-ttu-id="afa04-115">При чтении сущности связанные данные извлекаются вместе с ней.</span><span class="sxs-lookup"><span data-stu-id="afa04-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="afa04-116">Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные.</span><span class="sxs-lookup"><span data-stu-id="afa04-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="afa04-117">Настроить Entity Framework Core на использование безотложной загрузки можно при помощи методов `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="afa04-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Пример безотложной загрузки](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="afa04-119">Вы можете получить часть данных в отдельных запросах, и EF "исправит" свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="afa04-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="afa04-120">То есть EF автоматически добавляет раздельно извлеченные сущности к соответствующим свойствам навигации ранее извлеченных объектов.</span><span class="sxs-lookup"><span data-stu-id="afa04-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="afa04-121">Для запроса, получающего связанные данные, можно использовать метод `Load` вместо метода, который возвращает список или объект, такого как `ToList` или `Single`.</span><span class="sxs-lookup"><span data-stu-id="afa04-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="afa04-123">Явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="afa04-123">Explicit loading.</span></span> <span data-ttu-id="afa04-124">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="afa04-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="afa04-125">Если требуется получение связанных данных, то пишется дополнительный код.</span><span class="sxs-lookup"><span data-stu-id="afa04-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="afa04-126">Как и в случае безотложной загрузки с отдельными запросами, явная загрузка представляет собой несколько запросов к базе данных.</span><span class="sxs-lookup"><span data-stu-id="afa04-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="afa04-127">Отличие заключается в том, что при явной загрузке в коде указывается, какие свойства навигации будут загружены.</span><span class="sxs-lookup"><span data-stu-id="afa04-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="afa04-128">В Entity Framework Core 1.1 для выполнения явной загрузки можно использовать метод `Load`.</span><span class="sxs-lookup"><span data-stu-id="afa04-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="afa04-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="afa04-129">For example:</span></span>

  ![Пример явной загрузки](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="afa04-131">Отложенная загрузка.</span><span class="sxs-lookup"><span data-stu-id="afa04-131">Lazy loading.</span></span> <span data-ttu-id="afa04-132">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="afa04-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="afa04-133">Однако при первой попытке доступа к свойству навигации необходимые для этого свойства навигации данные извлекаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="afa04-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="afa04-134">Запрос к базе данных отправляется каждый раз, когда вы в первый раз пытаетесь получить данные из свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="afa04-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="afa04-135">Entity Framework Core 1.0 не поддерживает отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="afa04-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="afa04-136">Особенности производительности</span><span class="sxs-lookup"><span data-stu-id="afa04-136">Performance considerations</span></span>

<span data-ttu-id="afa04-137">Если известно, что связанные данные потребуются для каждой полученной сущности, то безотложная загрузка обычно обеспечивает наилучшую производительность, поскольку одиночный запрос к базе данных обычно эффективнее нескольких отдельных запросов для каждой полученной сущности.</span><span class="sxs-lookup"><span data-stu-id="afa04-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="afa04-138">Пусть, например, на каждом факультете есть десять связанных курсов.</span><span class="sxs-lookup"><span data-stu-id="afa04-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="afa04-139">Безотложная загрузка всех связанных данных приведет к одиночному запросу (с соединением) и одним циклом приема-передачи данных из базы.</span><span class="sxs-lookup"><span data-stu-id="afa04-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="afa04-140">Отдельные запросы по курсам для каждого факультета приведут к одиннадцати циклам приема-передачи данных из базы.</span><span class="sxs-lookup"><span data-stu-id="afa04-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="afa04-141">При высокой задержке дополнительные циклы приема-передачи данных особенно сильно влияют на производительность.</span><span class="sxs-lookup"><span data-stu-id="afa04-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="afa04-142">С другой стороны, в некоторых случаях отдельные запросы более эффективны.</span><span class="sxs-lookup"><span data-stu-id="afa04-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="afa04-143">Безотложная загрузка всех связанных данных в одном запросе может привести к формированию очень сложного соединения, которое SQL сервер не сможет эффективно обработать.</span><span class="sxs-lookup"><span data-stu-id="afa04-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="afa04-144">Либо, если необходимо получить свойства навигации сущности только для подмножества обрабатываемого набора сущностей, отдельные запросы могут показать большую производительность, поскольку при безотложной загрузке всех данных будет получено больше данных, чем вам необходимо.</span><span class="sxs-lookup"><span data-stu-id="afa04-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="afa04-145">Если важна производительность, то для выбора наилучшего решения рекомендуется протестировать производительность для обоих случаев.</span><span class="sxs-lookup"><span data-stu-id="afa04-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="afa04-146">Создание страницы "Courses" (Курсы) с отображением названий кафедр</span><span class="sxs-lookup"><span data-stu-id="afa04-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="afa04-147">Сущность Course включает свойство навигации, которое содержит сущность Department факультета, к которому привязан курс.</span><span class="sxs-lookup"><span data-stu-id="afa04-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="afa04-148">Чтобы отобразить в списке курсов название связанного факультета, необходимо получить свойство Name из сущности Department, находящейся в свойстве навигации `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="afa04-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="afa04-149">Создайте для типа сущности Course контроллер с именем CoursesController с теми же параметрами шаблона **Контроллер MVC с представлениями, использующий Entity Framework**, которые мы ранее задали для контроллера Students, как это показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="afa04-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Добавление контроллера Courses](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="afa04-151">Откройте файл *CoursesController.cs* и изучите метод `Index`.</span><span class="sxs-lookup"><span data-stu-id="afa04-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="afa04-152">В автоматически сформированном шаблоне установлена безотложная загрузка свойства навигации `Department` при помощи метода `Include`.</span><span class="sxs-lookup"><span data-stu-id="afa04-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="afa04-153">Замените метод `Index` следующим кодом, который использует более подходящее имя для `IQueryable`, возвращающего сущности Course (`courses` вместо `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="afa04-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="afa04-154">Откройте файл *Views/Courses/Index.cshtml* и замените код шаблона следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="afa04-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="afa04-155">Изменения выделены:</span><span class="sxs-lookup"><span data-stu-id="afa04-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="afa04-156">Мы внесли следующие изменения в код шаблона:</span><span class="sxs-lookup"><span data-stu-id="afa04-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="afa04-157">Изменен заголовок с "Index" (Индекс) на "Courses" (Курсы).</span><span class="sxs-lookup"><span data-stu-id="afa04-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="afa04-158">Добавлен столбец **Number** (Номер), отображающий значение свойства `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="afa04-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="afa04-159">По умолчанию в шаблоне отсутствуют первичные ключи, поскольку для конечных пользователей они не имеют смысла.</span><span class="sxs-lookup"><span data-stu-id="afa04-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="afa04-160">Однако в нашем случае первичный ключ имеет смысл, и мы хотим его отобразить.</span><span class="sxs-lookup"><span data-stu-id="afa04-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="afa04-161">Изменен столбец **Department** (Кафедра) для отображения названия кафедры.</span><span class="sxs-lookup"><span data-stu-id="afa04-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="afa04-162">Код отображает свойство `Name` сущности Department, которая загружена в свойство навигации `Department`:</span><span class="sxs-lookup"><span data-stu-id="afa04-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="afa04-163">Для просмотра списка с названиями кафедр запустите приложение и выберите вкладку **Courses** (Курсы).</span><span class="sxs-lookup"><span data-stu-id="afa04-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="afa04-165">Создание страницы "Instructors" (Преподаватели) с отображением курсов и дат зачисления</span><span class="sxs-lookup"><span data-stu-id="afa04-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="afa04-166">В этом разделе мы создадим контроллер и представление для сущности Instructor, чтобы отобразить страницу Instructors:</span><span class="sxs-lookup"><span data-stu-id="afa04-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)

<span data-ttu-id="afa04-168">Эта страница считывает и отображает связанные данные следующим образом:</span><span class="sxs-lookup"><span data-stu-id="afa04-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="afa04-169">Список преподавателей отображает связанные данные сущности OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="afa04-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="afa04-170">Сущности Instructor и OfficeAssignment связаны отношением один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="afa04-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="afa04-171">Для сущностей OfficeAssignment установлена безотложная загрузка.</span><span class="sxs-lookup"><span data-stu-id="afa04-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="afa04-172">Как упоминалось ранее, безотложная загрузка обычно эффективнее при получении связанных данных для всех строк главной таблицы.</span><span class="sxs-lookup"><span data-stu-id="afa04-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="afa04-173">В нашем случае мы хотим отобразить принадлежность к кабинету для каждого преподавателя.</span><span class="sxs-lookup"><span data-stu-id="afa04-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="afa04-174">Когда пользователь выбирает преподавателя, отображаются связанные сущности Course.</span><span class="sxs-lookup"><span data-stu-id="afa04-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="afa04-175">Сущности Instructor и Course находятся в отношении многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="afa04-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="afa04-176">Как мы видим, безотложная загрузка также установлена для сущностей Course и связанных с ними сущностями Department.</span><span class="sxs-lookup"><span data-stu-id="afa04-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="afa04-177">В этом случае отдельные запросы могут оказаться эффективнее, поскольку нам требуются курсы только для выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="afa04-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="afa04-178">Этот пример, однако, показывает, как использовать безотложную загрузку для свойств навигации сущностей, которые сами находятся в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="afa04-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="afa04-179">Когда пользователь выбирает курс, отображаются связанные данные из набора сущностей Enrollments.</span><span class="sxs-lookup"><span data-stu-id="afa04-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="afa04-180">Сущности Course и Enrollment находятся в отношении один ко многим.</span><span class="sxs-lookup"><span data-stu-id="afa04-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="afa04-181">Мы используем отдельные запросы для сущностей Enrollment и связанных с ними сущностями Student.</span><span class="sxs-lookup"><span data-stu-id="afa04-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="afa04-182">Создание модели для представления индекса преподавателей</span><span class="sxs-lookup"><span data-stu-id="afa04-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="afa04-183">На странице "Instructors" (Преподаватели) отображаются данные из трех различных таблиц.</span><span class="sxs-lookup"><span data-stu-id="afa04-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="afa04-184">Таким образом, мы создаем модель представления, которая включает три свойства, каждое из которых содержит данные из одной таблицы.</span><span class="sxs-lookup"><span data-stu-id="afa04-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="afa04-185">Создайте в папке *SchoolViewModels* файл *InstructorIndexData.cs* и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="afa04-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="afa04-186">Создание контроллера и представлений Instructor</span><span class="sxs-lookup"><span data-stu-id="afa04-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="afa04-187">Создайте контроллер Instructors с действиями чтения/записи Entity Framework, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="afa04-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Добавление контроллера Instructors](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="afa04-189">Откройте файл *InstructorsController.cs* и добавьте директиву using для пространства имен ViewModels:</span><span class="sxs-lookup"><span data-stu-id="afa04-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="afa04-190">Замените код Index следующим кодом для выполнения безотложной загрузки связанных данных и размещения их в модели представления.</span><span class="sxs-lookup"><span data-stu-id="afa04-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="afa04-191">Метод принимает необязательные данные маршрута (`id`) и строку запроса (`courseID`), которые содержат значения идентификатора выбранного преподавателя и выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="afa04-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="afa04-192">Параметры передаются гиперссылками **Select** на странице.</span><span class="sxs-lookup"><span data-stu-id="afa04-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="afa04-193">Код начинается с создания экземпляра модели представления и помещения его в список преподавателей.</span><span class="sxs-lookup"><span data-stu-id="afa04-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="afa04-194">В коде задается безотложная загрузка для свойств навигации `Instructor.OfficeAssignment` и `Instructor.CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="afa04-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="afa04-195">Вместе со свойством `CourseAssignments` загружается свойство `Course`, с которым загружаются свойства `Enrollments` и `Department`, а с каждой сущностью `Enrollment` загружается свойство `Student`.</span><span class="sxs-lookup"><span data-stu-id="afa04-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="afa04-196">Так как для представления требуется сущность OfficeAssignment, значительно эффективнее извлекать ее в том же запросе.</span><span class="sxs-lookup"><span data-stu-id="afa04-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="afa04-197">Получение сущностей Course необходимо при выборе преподавателя на веб-странице, таким образом одиночный запрос окажется предпочтительнее нескольких запросов, только если страница отображается с курсами чаще, чем без них.</span><span class="sxs-lookup"><span data-stu-id="afa04-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="afa04-198">В коде повторяются `CourseAssignments` и `Course`, так как требуется получить два свойства из `Course`.</span><span class="sxs-lookup"><span data-stu-id="afa04-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="afa04-199">Первая строка `ThenInclude` вызывает получение `CourseAssignment.Course`, `Course.Enrollments` и `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="afa04-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="afa04-200">В этой точке кода вызов метода `ThenInclude` получал бы свойства навигации `Student`, которые нам требуются.</span><span class="sxs-lookup"><span data-stu-id="afa04-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="afa04-201">Но вызов `Include` начнет заново получать свойства `Instructor`, поэтому нам придется еще раз выполнить последовательность команд, указав в этот раз `Course.Department` вместо `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="afa04-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="afa04-202">Следующий код выполняется при выборе преподавателя.</span><span class="sxs-lookup"><span data-stu-id="afa04-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="afa04-203">Выбранный преподаватель извлекается из списка преподавателей в модели представления.</span><span class="sxs-lookup"><span data-stu-id="afa04-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="afa04-204">Затем из свойства навигации `CourseAssignments` этого преподавателя получается свойство модели представления `Courses` вместе с сущностями Course.</span><span class="sxs-lookup"><span data-stu-id="afa04-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="afa04-205">Метод `Where` возвращает коллекцию, но с учетом переданных в метод условий в данном случае возвращается только одна сущность Instructor.</span><span class="sxs-lookup"><span data-stu-id="afa04-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="afa04-206">Метод `Single` преобразует коллекцию в отдельную сущность Instructor, что позволяет получить доступ к ее свойству `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="afa04-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="afa04-207">Свойство `CourseAssignments` содержит сущности `CourseAssignment`, из которых нам нужны только связанные сущности `Course`.</span><span class="sxs-lookup"><span data-stu-id="afa04-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="afa04-208">Использовать метод коллекции `Single` можно, если известно, что коллекция содержит только один элемент.</span><span class="sxs-lookup"><span data-stu-id="afa04-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="afa04-209">Метод Single вызывает исключение, если передаваемая ему коллекция пустая или содержит больше одного элемента.</span><span class="sxs-lookup"><span data-stu-id="afa04-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="afa04-210">Альтернативным вариантом является метод `SingleOrDefault`, который возвращает значение по умолчанию (в данном случае null), если коллекция пуста.</span><span class="sxs-lookup"><span data-stu-id="afa04-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="afa04-211">Однако в этом случае это все равно приведет к исключению (из-за попытки найти свойство `Courses` у указателя на null), и из сообщения об исключении нелегко будет понять причину проблемы.</span><span class="sxs-lookup"><span data-stu-id="afa04-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="afa04-212">При вызове метода `Single` вы можете также передать условие Where вместо отдельного вызова метода `Where`:</span><span class="sxs-lookup"><span data-stu-id="afa04-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="afa04-213">вместо следующего кода:</span><span class="sxs-lookup"><span data-stu-id="afa04-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="afa04-214">Далее, если был выбран курс, то он получается из списка курсов модели представления.</span><span class="sxs-lookup"><span data-stu-id="afa04-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="afa04-215">Затем из свойства навигации `Enrollments` этого курса получается свойство модели представления `Enrollments` вместе с сущностями Enrollment.</span><span class="sxs-lookup"><span data-stu-id="afa04-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="afa04-216">Изменение представления Instructor Index</span><span class="sxs-lookup"><span data-stu-id="afa04-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="afa04-217">В файле *Views/Instructors/Index.cshtml* замените код шаблона следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="afa04-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="afa04-218">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="afa04-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="afa04-219">Мы внесли следующие изменения в существующий код:</span><span class="sxs-lookup"><span data-stu-id="afa04-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="afa04-220">Изменили класс модели на `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="afa04-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="afa04-221">Изменили заголовок страницы с **Index** на **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="afa04-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="afa04-222">Добавили столбец **Office**, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не равно null.</span><span class="sxs-lookup"><span data-stu-id="afa04-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="afa04-223">(Так как здесь отношение один к нулю или к одному, то связанной сущности OfficeAssignment может не существовать.)</span><span class="sxs-lookup"><span data-stu-id="afa04-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="afa04-224">Добавили столбец **Courses**, отображающий курсы, которые ведет конкретный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="afa04-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="afa04-225">Подробные сведения об использовании синтаксиса Razor см. в разделе [Явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="afa04-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="afa04-226">Добавили код, который динамически добавляет `class="success"` к элементу `tr` выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="afa04-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="afa04-227">Этот параметр задает цвет фона для выделенных строк c помощью класса Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="afa04-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="afa04-228">В каждой строке непосредственно перед другими ссылками добавили новую гиперссылку с меткой **Select**, которая отправляет идентификатор выбранного преподавателя в метод `Index`.</span><span class="sxs-lookup"><span data-stu-id="afa04-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="afa04-229">Запустите приложение и выберите вкладку **Instructors**. На странице отображается свойство Location связанных сущностей OfficeAssignment либо пустая ячейка таблицы при отсутствии связанной сущности OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="afa04-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Страница индекса преподавателей, когда ничего не выбрано](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="afa04-231">В файле *Views/Instructors/Index.cshtml* после закрывающего таблицу элемента (в конце файла) добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="afa04-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="afa04-232">Этот код отображает список связанных с преподавателем курсов, когда преподаватель выбран.</span><span class="sxs-lookup"><span data-stu-id="afa04-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="afa04-233">Этот код считывает свойство `Courses` модели представления для отображения списка курсов.</span><span class="sxs-lookup"><span data-stu-id="afa04-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="afa04-234">Он также предоставляет гиперссылку **Select**, которая отправляет идентификатор выбранного курса в метод действия `Index`.</span><span class="sxs-lookup"><span data-stu-id="afa04-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="afa04-235">Обновите страницу и выберите преподавателя.</span><span class="sxs-lookup"><span data-stu-id="afa04-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="afa04-236">Вы увидите сетку, которая отображает курсы, назначенные выбранному преподавателю, и для каждого курса отобразится имя связанного факультета.</span><span class="sxs-lookup"><span data-stu-id="afa04-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Страница индекса преподавателей, выбран преподаватель](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="afa04-238">После только что добавленного блока кода добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="afa04-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="afa04-239">Он отображает список студентов, которые зачислены на курс при выборе этого курса.</span><span class="sxs-lookup"><span data-stu-id="afa04-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="afa04-240">Этот код считывает свойство Enrollments модели представления для отображения списка студентов, зачисленных на этот курс.</span><span class="sxs-lookup"><span data-stu-id="afa04-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="afa04-241">Снова обновите страницу и выберите преподавателя.</span><span class="sxs-lookup"><span data-stu-id="afa04-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="afa04-242">Затем выберите курс, чтобы увидеть список зачисленных студентов и их оценки.</span><span class="sxs-lookup"><span data-stu-id="afa04-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Страница индекса преподавателей, выбраны преподаватель и курс](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="afa04-244">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="afa04-244">Explicit loading</span></span>

<span data-ttu-id="afa04-245">При получении списка преподавателей в файле *InstructorsController.cs* мы указали безотложную загрузку для свойства навигации `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="afa04-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="afa04-246">Пусть ожидается, что пользователи лишь изредка будут просматривать список зачисления для выбранных преподавателя и курса.</span><span class="sxs-lookup"><span data-stu-id="afa04-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="afa04-247">В этом случае, возможно, потребуется загружать данные о зачислении, только когда они запрошены.</span><span class="sxs-lookup"><span data-stu-id="afa04-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="afa04-248">Чтобы продемонстрировать пример того, как выполнять явную загрузку, заменим метод `Index` следующим кодом, который удаляет упреждающую загрузку для Enrollments и загружает это свойство явно.</span><span class="sxs-lookup"><span data-stu-id="afa04-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="afa04-249">Изменения в коде выделены.</span><span class="sxs-lookup"><span data-stu-id="afa04-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="afa04-250">В новом коде из блока, который получает сущности преподавателей, удален вызов метода *ThenInclude*.</span><span class="sxs-lookup"><span data-stu-id="afa04-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="afa04-251">Если выбраны преподаватели и курс, выделенный код получает сущности Enrollment выбранного курса и сущности Student для каждой сущности Enrollment.</span><span class="sxs-lookup"><span data-stu-id="afa04-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="afa04-252">Запустите приложение, перейдите на страницу Instructors Index, и вы не увидите никаких изменений, несмотря на то, что мы изменили способ получения данных.</span><span class="sxs-lookup"><span data-stu-id="afa04-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="afa04-253">Сводка</span><span class="sxs-lookup"><span data-stu-id="afa04-253">Summary</span></span>

<span data-ttu-id="afa04-254">Вы научились использовать безотложную загрузку с одним запросом и несколькими запросами для чтения связанных данных в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="afa04-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="afa04-255">В следующем руководстве вы узнаете, как обновлять связанные данные.</span><span class="sxs-lookup"><span data-stu-id="afa04-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="afa04-256">[Назад](complex-data-model.md)
>[Вперед](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="afa04-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
