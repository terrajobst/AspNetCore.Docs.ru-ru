---
title: "Основные ASP.NET MVC с основными EF - чтение связанных данных - 6 из 10"
author: tdykstra
description: "В этом учебнике вам чтения и отображения связанных данных — то есть данных, загружающий Entity Framework в свойствах навигации."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 1321cb00a432669b4a97ad20063b6cf9ea75f24c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="3e31c-103">Чтение связанных данных - Core EF учебнику ASP.NET Core MVC (6 из 10)</span><span class="sxs-lookup"><span data-stu-id="3e31c-103">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="3e31c-104">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3e31c-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3e31c-105">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3e31c-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="3e31c-106">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="3e31c-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="3e31c-107">В предыдущем учебнике завершить модели данных School.</span><span class="sxs-lookup"><span data-stu-id="3e31c-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="3e31c-108">В этом учебнике предстоит чтения и отображения связанных данных — то есть данных, загружающий Entity Framework в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="3e31c-109">На следующих рисунках страницы, вы будете работать с.</span><span class="sxs-lookup"><span data-stu-id="3e31c-109">The following illustrations show the pages that you'll work with.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="3e31c-112">Упреждающая, явные и отложенной загрузки связанных данных</span><span class="sxs-lookup"><span data-stu-id="3e31c-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="3e31c-113">Существует несколько способов объектно-реляционного сопоставления (ORM) программного обеспечения, например Entity Framework можно загрузить связанные данные в свойства навигации сущности:</span><span class="sxs-lookup"><span data-stu-id="3e31c-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="3e31c-114">Безотложная загрузка.</span><span class="sxs-lookup"><span data-stu-id="3e31c-114">Eager loading.</span></span> <span data-ttu-id="3e31c-115">При чтении сущности связанные данные извлекаются вместе с ним.</span><span class="sxs-lookup"><span data-stu-id="3e31c-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="3e31c-116">Обычно в результате запроса одного соединения, который получает все данные, которые необходимы.</span><span class="sxs-lookup"><span data-stu-id="3e31c-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="3e31c-117">Указать упреждающую в Entity Framework Core, используя `Include` и `ThenInclude` методы.</span><span class="sxs-lookup"><span data-stu-id="3e31c-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![Пример Безотложная загрузка](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="3e31c-119">Вы можете получить некоторые данные в отдельных запросах и EF» по исправлению» свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="3e31c-120">То есть EF автоматически добавляет отдельно извлеченных сущностей, к которым они принадлежат свойств навигации ранее извлеченных объектов.</span><span class="sxs-lookup"><span data-stu-id="3e31c-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="3e31c-121">Для запроса, который получает данные, можно использовать `Load` метод вместо метода, который возвращает список или объекта, таких как `ToList` или `Single`.</span><span class="sxs-lookup"><span data-stu-id="3e31c-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="3e31c-123">Явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="3e31c-123">Explicit loading.</span></span> <span data-ttu-id="3e31c-124">При считывании сущности связанные данные не получены.</span><span class="sxs-lookup"><span data-stu-id="3e31c-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="3e31c-125">Написать код, который извлекает данные при необходимости.</span><span class="sxs-lookup"><span data-stu-id="3e31c-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="3e31c-126">Как в случае загрузки с отдельных запросов eager явную загрузку результатов в нескольких запросах отправляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="3e31c-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="3e31c-127">Разница заключается в том, что с явная загрузка код задает свойства навигации для загрузки.</span><span class="sxs-lookup"><span data-stu-id="3e31c-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="3e31c-128">В Entity Framework Core 1.1 можно использовать `Load` метод для выполнения явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="3e31c-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="3e31c-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="3e31c-129">For example:</span></span>

  ![Пример явная загрузка](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="3e31c-131">Отложенная загрузка.</span><span class="sxs-lookup"><span data-stu-id="3e31c-131">Lazy loading.</span></span> <span data-ttu-id="3e31c-132">При считывании сущности связанные данные не получены.</span><span class="sxs-lookup"><span data-stu-id="3e31c-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="3e31c-133">Однако первой попытке доступа к свойству навигации, данные, необходимые для этого свойства навигации автоматически извлекается.</span><span class="sxs-lookup"><span data-stu-id="3e31c-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="3e31c-134">Отправляется запрос к базе данных каждый раз при попытке получить данные из свойства навигации в первый раз.</span><span class="sxs-lookup"><span data-stu-id="3e31c-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="3e31c-135">Entity Framework Core 1.0 не поддерживает отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="3e31c-135">Entity Framework Core 1.0 does not support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="3e31c-136">Особенности производительности</span><span class="sxs-lookup"><span data-stu-id="3e31c-136">Performance considerations</span></span>

<span data-ttu-id="3e31c-137">Если известно, что требуются связанные данные для каждой сущности, которые получены, активная загрузка часто обеспечивает оптимальную производительность, поскольку одного запроса, отправляемые в базу данных обычно более эффективно, чем отдельные запросы для каждой сущности, которые получены.</span><span class="sxs-lookup"><span data-stu-id="3e31c-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="3e31c-138">Например предположим, что для каждого отдела содержит десять связанных курсов.</span><span class="sxs-lookup"><span data-stu-id="3e31c-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="3e31c-139">Безотложная загрузка всех связанных данных приведет к запрос одним (join) и одним кругового пути к базе данных.</span><span class="sxs-lookup"><span data-stu-id="3e31c-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="3e31c-140">Отдельный запрос для курсов для каждого отдела приведет к одиннадцать циклов приема-передачи в базу данных.</span><span class="sxs-lookup"><span data-stu-id="3e31c-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="3e31c-141">При высокой задержки, дополнительных циклов обращения к базе данных, особенно к падению производительности.</span><span class="sxs-lookup"><span data-stu-id="3e31c-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="3e31c-142">С другой стороны в некоторых сценариях отдельные запросы более эффективно.</span><span class="sxs-lookup"><span data-stu-id="3e31c-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="3e31c-143">Безотложная загрузка всех связанных данных в одном запросе может привести к очень сложного соединения для создаваемого, который SQL Server не может эффективно обработать.</span><span class="sxs-lookup"><span data-stu-id="3e31c-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="3e31c-144">Или, если необходимо получить доступ к свойствам навигации сущности только для подмножества набора сущностей, который обработка отдельных запросов могут выполняться эффективнее, так как Безотложная загрузка всего заранее бы получить больше данных, чем необходимо.</span><span class="sxs-lookup"><span data-stu-id="3e31c-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="3e31c-145">Если важна производительность, рекомендуется проверить производительность оба способа, чтобы выбрать наиболее подходящую.</span><span class="sxs-lookup"><span data-stu-id="3e31c-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="3e31c-146">Создание страницы курсов, отображающий название отдела</span><span class="sxs-lookup"><span data-stu-id="3e31c-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="3e31c-147">Сущность курс включает свойство навигации, которое содержит сущности «отдел» отдела, назначенный курса.</span><span class="sxs-lookup"><span data-stu-id="3e31c-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="3e31c-148">Чтобы отобразить имя подразделения, назначенный в списке курсов, необходимо получить имя свойства из сущности «отдел», который находится в `Course.Department` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="3e31c-149">Контроллер с именем CoursesController для типа сущности курса, используя те же параметры для создания **контроллер MVC с представлениями, использующий Entity Framework** scaffolder, которые были ранее для контроллера учащихся, как показано в на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="3e31c-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![Добавить контроллер курсов](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="3e31c-151">Откройте *CoursesController.cs* и изучить `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="3e31c-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="3e31c-152">Автоматическое формирование шаблонов указал упреждающую для `Department` свойство навигации с помощью `Include` метод.</span><span class="sxs-lookup"><span data-stu-id="3e31c-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="3e31c-153">Замените `Index` метод следующим кодом, который использует более подходящее имя для `IQueryable` , возвращающий курса сущностей (`courses` вместо `schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="3e31c-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="3e31c-154">Откройте *Views/Courses/Index.cshtml* и замените код шаблона с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="3e31c-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="3e31c-155">Изменения выделены:</span><span class="sxs-lookup"><span data-stu-id="3e31c-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="3e31c-156">Для формирования шаблонов код внесли следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="3e31c-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="3e31c-157">Изменить заголовок из индекса к курсам.</span><span class="sxs-lookup"><span data-stu-id="3e31c-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="3e31c-158">Добавлен **номер** столбца, отображающего `CourseID` значение свойства.</span><span class="sxs-lookup"><span data-stu-id="3e31c-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="3e31c-159">По умолчанию первичные ключи не являются формирования шаблонов, так как обычно они не имеют смысла для конечных пользователей.</span><span class="sxs-lookup"><span data-stu-id="3e31c-159">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="3e31c-160">Однако в этом случае первичный ключ имеет смысл, и вы хотите отображать их.</span><span class="sxs-lookup"><span data-stu-id="3e31c-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="3e31c-161">Изменить **отдел** столбец, отображающий название отдела.</span><span class="sxs-lookup"><span data-stu-id="3e31c-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="3e31c-162">Код отображает `Name` свойства сущности «отдел», который загружается `Department` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="3e31c-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="3e31c-163">Запустите приложение и выберите **курсы** вкладку, чтобы просмотреть список с именами отдела.</span><span class="sxs-lookup"><span data-stu-id="3e31c-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="3e31c-165">Создание страницы инструкторов курсов и регистрации</span><span class="sxs-lookup"><span data-stu-id="3e31c-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="3e31c-166">В этом разделе вы создадите представления сущность инструктора и контроллер, чтобы отобразить страницу «преподаватели»:</span><span class="sxs-lookup"><span data-stu-id="3e31c-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)

<span data-ttu-id="3e31c-168">Эта страница считывает и взаимосвязанные данные отображаются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3e31c-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="3e31c-169">Появится список инструкторов взаимосвязанные данные из OfficeAssignment сущности.</span><span class="sxs-lookup"><span data-stu-id="3e31c-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="3e31c-170">Сущности инструктора и OfficeAssignment являются отношением один к нулю или одному.</span><span class="sxs-lookup"><span data-stu-id="3e31c-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="3e31c-171">Вы воспользуетесь упреждающую OfficeAssignment сущностей.</span><span class="sxs-lookup"><span data-stu-id="3e31c-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="3e31c-172">Как упоминалось ранее, упреждающую обычно более эффективна при необходимости связанные данные для всех извлеченных строк таблицы первичного.</span><span class="sxs-lookup"><span data-stu-id="3e31c-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="3e31c-173">В этом случае будет отображаться office назначения для всех отображаемых инструкторов.</span><span class="sxs-lookup"><span data-stu-id="3e31c-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="3e31c-174">Когда пользователь выбирает инструктор, отображаются связанные сущности курса.</span><span class="sxs-lookup"><span data-stu-id="3e31c-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="3e31c-175">В отношении многие ко многим являются сущности инструктора и курса.</span><span class="sxs-lookup"><span data-stu-id="3e31c-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="3e31c-176">Безотложная загрузка будет использоваться для набора сущностей и их связанных сущностей отдела.</span><span class="sxs-lookup"><span data-stu-id="3e31c-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="3e31c-177">В этом случае отдельные запросы могут оказаться эффективными, поскольку требуется курсы только для выбранных инструктора.</span><span class="sxs-lookup"><span data-stu-id="3e31c-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="3e31c-178">Однако в этом примере показано, как использовать упреждающую свойств навигации в сущностях, которые сами в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="3e31c-179">Когда пользователь выбирает курса, отображается взаимосвязанные данные из набора сущностей регистраций.</span><span class="sxs-lookup"><span data-stu-id="3e31c-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="3e31c-180">Сущности курс и регистрации находятся в отношении один ко многим.</span><span class="sxs-lookup"><span data-stu-id="3e31c-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="3e31c-181">Вы используете отдельные запросы для регистрации объектов и их связанные сущности студента.</span><span class="sxs-lookup"><span data-stu-id="3e31c-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="3e31c-182">Создать модель представлений для представления инструктора индекса</span><span class="sxs-lookup"><span data-stu-id="3e31c-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="3e31c-183">На странице инструкторов отображаются данные из трех разных таблицах.</span><span class="sxs-lookup"><span data-stu-id="3e31c-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="3e31c-184">Таким образом вы создадите модель представления, которая включает три свойства каждого хранения данных для одной из таблиц.</span><span class="sxs-lookup"><span data-stu-id="3e31c-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="3e31c-185">В *SchoolViewModels* папке создайте *InstructorIndexData.cs* и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="3e31c-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="3e31c-186">Создание контроллера инструктора и представления</span><span class="sxs-lookup"><span data-stu-id="3e31c-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="3e31c-187">Создайте инструкторов контроллер с действиями чтения и записи EF, как показано на следующем рисунке:</span><span class="sxs-lookup"><span data-stu-id="3e31c-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![Добавление контроллера преподавателей](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="3e31c-189">Откройте *InstructorsController.cs* и добавить с помощью инструкции для пространства имен ViewModels:</span><span class="sxs-lookup"><span data-stu-id="3e31c-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="3e31c-190">Замените метод индекс следующий код, чтобы сделать безотложной загрузкой связанных данных и поместить его в модели представления.</span><span class="sxs-lookup"><span data-stu-id="3e31c-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="3e31c-191">Этот метод принимает необязательный маршрутизации данных (`id`) и параметр строки запроса (`courseID`), содержащие значения ID выбранного инструктора и выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="3e31c-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="3e31c-192">Параметры, предоставляемые **выберите** гиперссылок на странице.</span><span class="sxs-lookup"><span data-stu-id="3e31c-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="3e31c-193">Код начинается с создания экземпляра модели представления и помещения его в списке инструкторов.</span><span class="sxs-lookup"><span data-stu-id="3e31c-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="3e31c-194">В коде задается упреждающую для `Instructor.OfficeAssignment` и `Instructor.CourseAssignments` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="3e31c-195">В `CourseAssignments` свойство, `Course` свойство загружен и в этом домене, `Enrollments` и `Department` свойства уже загружены и в каждом `Enrollment` сущности `Student` свойство загружено.</span><span class="sxs-lookup"><span data-stu-id="3e31c-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="3e31c-196">Так как представление всегда требует OfficeAssignment сущности, значительно эффективнее извлекать, в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="3e31c-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="3e31c-197">При инструктор выделено на веб-странице, так что один запрос лучше, чем несколько запросов только в том случае, если эта страница отображается чаще с курсом выбран чем без курса сущности не требуется.</span><span class="sxs-lookup"><span data-stu-id="3e31c-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="3e31c-198">Код повторяется `CourseAssignments` и `Course` так, как требуется два свойства из `Course`.</span><span class="sxs-lookup"><span data-stu-id="3e31c-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="3e31c-199">Первая строка `ThenInclude` вызывает возвращает `CourseAssignment.Course`, `Course.Enrollments`, и `Enrollment.Student`.</span><span class="sxs-lookup"><span data-stu-id="3e31c-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="3e31c-200">На этом этапе в коде другой `ThenInclude` бы для свойства навигации `Student`, которой не требуется.</span><span class="sxs-lookup"><span data-stu-id="3e31c-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="3e31c-201">Но вызывающий `Include` начинает ее заново с `Instructor` свойства, поэтому вам придется проходить по цепочке еще раз, указав это время `Course.Department` вместо `Course.Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="3e31c-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="3e31c-202">Следующий код выполняется, когда был выбран инструктор.</span><span class="sxs-lookup"><span data-stu-id="3e31c-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="3e31c-203">Выбранный инструктора извлекается из списка инструкторов в модели представления.</span><span class="sxs-lookup"><span data-stu-id="3e31c-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="3e31c-204">Модель представления `Courses` свойство затем загружен с курса сущностей из этого инструктора `CourseAssignments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="3e31c-205">`Where` Метод возвращает коллекцию, но в этом случае условия, переданные в результат метода только одну инструктора сущность возвращается.</span><span class="sxs-lookup"><span data-stu-id="3e31c-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="3e31c-206">`Single` Метод преобразует коллекцию в одной сущности инструктора предоставляет доступ к этой сущности `CourseAssignments` свойство.</span><span class="sxs-lookup"><span data-stu-id="3e31c-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="3e31c-207">`CourseAssignments` Свойство содержит `CourseAssignment` сущности, с которых будет осуществляться только связанные `Course` сущностей.</span><span class="sxs-lookup"><span data-stu-id="3e31c-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="3e31c-208">Вы используете `Single` метод в коллекции, если известно коллекции будет иметь только один элемент.</span><span class="sxs-lookup"><span data-stu-id="3e31c-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="3e31c-209">Один метод создает исключение, если возвращается пустая коллекция, переданы в него, или если существует более одного элемента.</span><span class="sxs-lookup"><span data-stu-id="3e31c-209">The Single method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="3e31c-210">Альтернативным вариантом является `SingleOrDefault`, который возвращает значение по умолчанию (null в данном случае), если коллекция пуста.</span><span class="sxs-lookup"><span data-stu-id="3e31c-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="3e31c-211">Однако в этом случае, по-прежнему приведет к исключению (попытки найти `Courses` свойство на указатель null), и сообщение об исключении менее четко указывает на причину проблемы.</span><span class="sxs-lookup"><span data-stu-id="3e31c-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="3e31c-212">При вызове `Single` метод, можно также передать в предложении Where условие вместо вызова метода `Where` метод отдельно:</span><span class="sxs-lookup"><span data-stu-id="3e31c-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="3e31c-213">вместо следующего кода:</span><span class="sxs-lookup"><span data-stu-id="3e31c-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="3e31c-214">Далее Если был выбран курса, выбранного курса извлекается из списка курсов в модели представления.</span><span class="sxs-lookup"><span data-stu-id="3e31c-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="3e31c-215">Затем модели представления `Enrollments` свойство загружается с регистрации сущностей из этого курса `Enrollments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="3e31c-216">Изменить представление Index инструктора</span><span class="sxs-lookup"><span data-stu-id="3e31c-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="3e31c-217">В *Views/Instructors/Index.cshtml*, замените код шаблона в следующий код.</span><span class="sxs-lookup"><span data-stu-id="3e31c-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="3e31c-218">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="3e31c-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="3e31c-219">Следующие изменения, внесенные в существующий код:</span><span class="sxs-lookup"><span data-stu-id="3e31c-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="3e31c-220">Класс модели, чтобы изменить `InstructorIndexData`.</span><span class="sxs-lookup"><span data-stu-id="3e31c-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="3e31c-221">Изменить заголовок страницы из **индекс** для **инструкторов**.</span><span class="sxs-lookup"><span data-stu-id="3e31c-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="3e31c-222">Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="3e31c-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="3e31c-223">(Так как один к нулю или одному связь, могут не существовать связанной сущности OfficeAssignment.)</span><span class="sxs-lookup"><span data-stu-id="3e31c-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="3e31c-224">Добавлен **курсы** столбец, отображающий курсы при изучении каждого инструктора.</span><span class="sxs-lookup"><span data-stu-id="3e31c-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="3e31c-225">В разделе [явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) для получения дополнительных сведений о данного синтаксиса razor.</span><span class="sxs-lookup"><span data-stu-id="3e31c-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="3e31c-226">Добавленный код, который динамически добавляет `class="success"` для `tr` инструктора выбранного элемента.</span><span class="sxs-lookup"><span data-stu-id="3e31c-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="3e31c-227">Это задает цвет фона для строк, выделенных с помощью класса начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="3e31c-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="3e31c-228">Добавлена новая гиперссылка с меткой **выберите** непосредственно перед другие ссылки в каждой строке, что вызывает идентификатор выбранного инструктора отправку `Index` метод.</span><span class="sxs-lookup"><span data-stu-id="3e31c-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="3e31c-229">Запустите приложение и выберите **инструкторов** вкладки. На странице отображается свойство Location для связанных сущностей OfficeAssignment и по пустой ячейке таблице при нет связанной сущности OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="3e31c-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Страница индекса инструкторов ничего не выбраны](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="3e31c-231">В *Views/Instructors/Index.cshtml* файла после закрытия таблицы элемента (в конце файла), добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3e31c-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="3e31c-232">Этот код отображает список связанных с инструктор при выборе инструктор курсов.</span><span class="sxs-lookup"><span data-stu-id="3e31c-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="3e31c-233">Этот код считывает `Courses` свойство модели представления для отображения списка курсов.</span><span class="sxs-lookup"><span data-stu-id="3e31c-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="3e31c-234">Он также предоставляет **выберите** гиперссылку, которая отправляет идентификатор курса для `Index` метода действия.</span><span class="sxs-lookup"><span data-stu-id="3e31c-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="3e31c-235">Обновите страницу и выберите инструктор.</span><span class="sxs-lookup"><span data-stu-id="3e31c-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="3e31c-236">Вы увидите на таблице, которая отображает курсов, которые назначены выбранной инструктора, и для каждого курса вы увидите имя подразделения, назначенный.</span><span class="sxs-lookup"><span data-stu-id="3e31c-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Инструкторов индекс страницы инструктора выбран](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="3e31c-238">После только что добавленного блока кода добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="3e31c-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="3e31c-239">Это список учащихся, которые участвуют в курса при выборе этого курса.</span><span class="sxs-lookup"><span data-stu-id="3e31c-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="3e31c-240">Этот код считывает свойство регистраций модели представления для отображения списка учащихся, зарегистрированных в этом курсе.</span><span class="sxs-lookup"><span data-stu-id="3e31c-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="3e31c-241">Обновите страницу еще раз и выберите инструктор.</span><span class="sxs-lookup"><span data-stu-id="3e31c-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="3e31c-242">Выберите курс, чтобы просмотреть список зарегистрированных студентов и их оценки.</span><span class="sxs-lookup"><span data-stu-id="3e31c-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Инструктора страницы индекса инструкторов и выбранного курса](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="3e31c-244">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="3e31c-244">Explicit loading</span></span>

<span data-ttu-id="3e31c-245">При получении списка инструкторов в *InstructorsController.cs*, указан упреждающую для `CourseAssignments` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="3e31c-246">Предположим, что ожидается пользователям лишь изредка могут понадобиться регистраций в выбранной инструктора и курс.</span><span class="sxs-lookup"><span data-stu-id="3e31c-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="3e31c-247">В этом случае может потребоваться загрузить данные регистрации только в том случае, если она запрошена.</span><span class="sxs-lookup"><span data-stu-id="3e31c-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="3e31c-248">Чтобы увидеть пример того, как сделать явная загрузка, замените `Index` метод следующим кодом, который удаляет упреждающую для регистрации и явно загружает свойства.</span><span class="sxs-lookup"><span data-stu-id="3e31c-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="3e31c-249">Изменения в коде, выделяются.</span><span class="sxs-lookup"><span data-stu-id="3e31c-249">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="3e31c-250">Новый код удаляет *ThenInclude* вызывает метод для регистрации данных из кода, которая извлекает сущности инструктора.</span><span class="sxs-lookup"><span data-stu-id="3e31c-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="3e31c-251">Если выбраны инструктора и курс, выделенный код извлекает сущности регистрации выбранного курса и студента сущности для каждой регистрации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="3e31c-252">Запуск приложения, перейдите на страницу индекса инструкторов и вы увидите не отличается от отображаемых на странице, несмотря на то, что вы изменили извлечением данных.</span><span class="sxs-lookup"><span data-stu-id="3e31c-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="3e31c-253">Сводка</span><span class="sxs-lookup"><span data-stu-id="3e31c-253">Summary</span></span>

<span data-ttu-id="3e31c-254">Теперь использовалась упреждающую с одного запроса и несколько запросов на чтение связанных данных в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="3e31c-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="3e31c-255">В следующем уроке будет рассмотрено обновления связанных данных.</span><span class="sxs-lookup"><span data-stu-id="3e31c-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="3e31c-256">[Назад](complex-data-model.md)
>[Вперед](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="3e31c-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
