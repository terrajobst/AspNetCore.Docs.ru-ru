---
title: "Страниц Razor с основными EF - чтение связанных данных - 6, 8."
author: rick-anderson
description: "В этом учебнике чтения и отображения связанных данных — то есть данных, загружающий Entity Framework в свойствах навигации."
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="12ba2-103">Чтение связанных данных - Core EF со страницами Razor (6, 8)</span><span class="sxs-lookup"><span data-stu-id="12ba2-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="12ba2-104">По [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="12ba2-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="12ba2-105">В этом учебнике связанных данных считывается и отображается.</span><span class="sxs-lookup"><span data-stu-id="12ba2-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="12ba2-106">Связанные данные — это данные, основные EF загружает в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="12ba2-107">Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="12ba2-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="12ba2-108">На следующих рисунках показаны завершенного страниц для этого учебника:</span><span class="sxs-lookup"><span data-stu-id="12ba2-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="12ba2-111">Упреждающая, явные и отложенной загрузки связанных данных</span><span class="sxs-lookup"><span data-stu-id="12ba2-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="12ba2-112">Существует несколько способов, что EF Core можно загрузить связанные данные в свойства навигации сущности:</span><span class="sxs-lookup"><span data-stu-id="12ba2-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="12ba2-113">[Безотложная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="12ba2-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="12ba2-114">Безотложная загрузка при запросе для одного типа сущности также загружает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="12ba2-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="12ba2-115">При чтении сущности извлекается соответствующими данными.</span><span class="sxs-lookup"><span data-stu-id="12ba2-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="12ba2-116">Обычно в результате запроса одного соединения, который получает все данные, которые необходимы.</span><span class="sxs-lookup"><span data-stu-id="12ba2-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="12ba2-117">EF Core выдаст несколько запросов для некоторых типов упреждающую.</span><span class="sxs-lookup"><span data-stu-id="12ba2-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="12ba2-118">Выдача нескольких запросов может оказаться более эффективным, чем в случае для некоторых запросов в EF6 которых произошла один запрос.</span><span class="sxs-lookup"><span data-stu-id="12ba2-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="12ba2-119">Безотложная загрузка, указанном с помощью `Include` и `ThenInclude` методы.</span><span class="sxs-lookup"><span data-stu-id="12ba2-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![Пример Безотложная загрузка](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="12ba2-121">Безотложная загрузка отправляет несколько запросов, когда включено nvavigation коллекции:</span><span class="sxs-lookup"><span data-stu-id="12ba2-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="12ba2-122">Один запрос в основном запросе</span><span class="sxs-lookup"><span data-stu-id="12ba2-122">One query for the main query</span></span> 
 * <span data-ttu-id="12ba2-123">Один запрос для каждой коллекции «границы» в дереве нагрузочных.</span><span class="sxs-lookup"><span data-stu-id="12ba2-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="12ba2-124">Отдельные запросы с `Load`: данные могут быть получены в отдельных запросах и EF Core «устраняет» свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="12ba2-125">«исправления вверх» означает, что EF Core автоматически заполняет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="12ba2-126">Отдельные запросы с `Load` близко explict, чем упреждающую загрузки.</span><span class="sxs-lookup"><span data-stu-id="12ba2-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="12ba2-128">Примечание: Основной EF автоматически исправляет свойства навигации к другим сущностям, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="12ba2-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="12ba2-129">Даже если данные для свойства навигации *не* явно включен, свойство по-прежнему могут быть заполнены, если некоторые или все связанные сущности были ранее загружены.</span><span class="sxs-lookup"><span data-stu-id="12ba2-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="12ba2-130">[Явная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="12ba2-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="12ba2-131">При считывании сущности связанные данные не получены.</span><span class="sxs-lookup"><span data-stu-id="12ba2-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="12ba2-132">Код должен быть написан для получения взаимосвязанных данных в том случае, когда он необходим.</span><span class="sxs-lookup"><span data-stu-id="12ba2-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="12ba2-133">Явная загрузка с отдельных запросов приводит несколько запросов, отправляемых на базу данных.</span><span class="sxs-lookup"><span data-stu-id="12ba2-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="12ba2-134">При явной загрузке, код задает свойства навигации для загрузки.</span><span class="sxs-lookup"><span data-stu-id="12ba2-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="12ba2-135">Используйте `Load` метод для выполнения явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="12ba2-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="12ba2-136">Пример:</span><span class="sxs-lookup"><span data-stu-id="12ba2-136">For example:</span></span>

 ![Пример явная загрузка](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="12ba2-138">[Отложенная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="12ba2-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="12ba2-139">[Основные EF в настоящее время не поддерживает отложенную загрузку](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="12ba2-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="12ba2-140">При считывании сущности связанные данные не получены.</span><span class="sxs-lookup"><span data-stu-id="12ba2-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="12ba2-141">Время первого обращения к свойству навигации, автоматически извлекается данные, необходимые для этого свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="12ba2-142">Запрос отправляется в базу данных, каждый раз к свойству навигации в первый раз.</span><span class="sxs-lookup"><span data-stu-id="12ba2-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="12ba2-143">`Select` Оператор загружает связанные данные, необходимые.</span><span class="sxs-lookup"><span data-stu-id="12ba2-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="12ba2-144">Создание страницы курсов, отображающий название отдела</span><span class="sxs-lookup"><span data-stu-id="12ba2-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="12ba2-145">Сущность курс включает свойство навигации, которое содержит `Department` сущности.</span><span class="sxs-lookup"><span data-stu-id="12ba2-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="12ba2-146">`Department` Сущность содержит отдела, которые назначены курса.</span><span class="sxs-lookup"><span data-stu-id="12ba2-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="12ba2-147">Чтобы отобразить имя подразделения, назначенный в списке курсов:</span><span class="sxs-lookup"><span data-stu-id="12ba2-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="12ba2-148">Получить `Name` свойство из `Department` сущности.</span><span class="sxs-lookup"><span data-stu-id="12ba2-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="12ba2-149">`Department` Сущность поступают из `Course.Department` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse. Подразделение](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="12ba2-151">Формировать модели курса</span><span class="sxs-lookup"><span data-stu-id="12ba2-151">Scaffold the Course model</span></span>

* <span data-ttu-id="12ba2-152">Выйдите из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12ba2-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="12ba2-153">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="12ba2-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="12ba2-154">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="12ba2-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="12ba2-155">Предыдущий закрепление команда `Course` модели.</span><span class="sxs-lookup"><span data-stu-id="12ba2-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="12ba2-156">Откройте проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12ba2-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="12ba2-157">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="12ba2-157">Build the project.</span></span> <span data-ttu-id="12ba2-158">Сборки приводит к возникновению ошибки следующим образом:</span><span class="sxs-lookup"><span data-stu-id="12ba2-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="12ba2-159">Глобальная замена `_context.Course` для `_context.Courses` (то есть, добавить «s» `Course`).</span><span class="sxs-lookup"><span data-stu-id="12ba2-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="12ba2-160">7 вхождений обнаружены и обновлены.</span><span class="sxs-lookup"><span data-stu-id="12ba2-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="12ba2-161">Откройте *Pages/Courses/Index.cshtml.cs* и изучить `OnGetAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="12ba2-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="12ba2-162">Формирование шаблонов engine указано упреждающую для `Department` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="12ba2-163">`Include` Метод задает упреждающую.</span><span class="sxs-lookup"><span data-stu-id="12ba2-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="12ba2-164">Запустите приложение и выберите **курсы** ссылку.</span><span class="sxs-lookup"><span data-stu-id="12ba2-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="12ba2-165">Отображает столбец отдел `DepartmentID`, который не является полезным.</span><span class="sxs-lookup"><span data-stu-id="12ba2-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="12ba2-166">Обновите метод `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="12ba2-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="12ba2-167">Предыдущий код добавляет `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="12ba2-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="12ba2-168">`AsNoTracking`повышает производительность, поскольку сущностей, возвращаемых не отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="12ba2-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="12ba2-169">Сущности, не отслеживаются, так как они не обновлялись в текущем контексте.</span><span class="sxs-lookup"><span data-stu-id="12ba2-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="12ba2-170">Обновление *Views/Courses/Index.cshtml* с следующую выделенную разметку:</span><span class="sxs-lookup"><span data-stu-id="12ba2-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="12ba2-171">Для формирования шаблонов код были внесены следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="12ba2-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="12ba2-172">Изменить заголовок из индекса к курсам.</span><span class="sxs-lookup"><span data-stu-id="12ba2-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="12ba2-173">Добавлен **номер** столбца, отображающего `CourseID` значение свойства.</span><span class="sxs-lookup"><span data-stu-id="12ba2-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="12ba2-174">По умолчанию первичные ключи не являются формирования шаблонов, так как обычно они имеют смысла для конечных пользователей.</span><span class="sxs-lookup"><span data-stu-id="12ba2-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="12ba2-175">Однако в этом случае первичный ключ имеет значение.</span><span class="sxs-lookup"><span data-stu-id="12ba2-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="12ba2-176">Изменить **отдел** столбец, отображающий название отдела.</span><span class="sxs-lookup"><span data-stu-id="12ba2-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="12ba2-177">Код отображает `Name` свойство `Department` сущность, которая загружается в `Department` свойство навигации:</span><span class="sxs-lookup"><span data-stu-id="12ba2-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="12ba2-178">Запустите приложение и выберите **курсы** вкладку, чтобы просмотреть список с именами отдела.</span><span class="sxs-lookup"><span data-stu-id="12ba2-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="12ba2-180">Выполняется загрузка связанных данных с помощью Select</span><span class="sxs-lookup"><span data-stu-id="12ba2-180">Loading related data with Select</span></span>

<span data-ttu-id="12ba2-181">`OnGetAsync` Метод загружает данные, связанные с `Include` метод:</span><span class="sxs-lookup"><span data-stu-id="12ba2-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="12ba2-182">`Select` Оператор загружает связанные данные, необходимые.</span><span class="sxs-lookup"><span data-stu-id="12ba2-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="12ba2-183">Для отдельных элементов таких как `Department.Name` используется SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="12ba2-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="12ba2-184">Для коллекций, он использует доступ к другой базе данных, но также возрастает.`Include`</span><span class="sxs-lookup"><span data-stu-id="12ba2-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="12ba2-185">оператор с коллекциями.</span><span class="sxs-lookup"><span data-stu-id="12ba2-185">operator on collections.</span></span>

<span data-ttu-id="12ba2-186">Следующий код загружает данные, связанные с `Select` метод:</span><span class="sxs-lookup"><span data-stu-id="12ba2-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="12ba2-187">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="12ba2-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="12ba2-188">В разделе [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) и [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) полный пример.</span><span class="sxs-lookup"><span data-stu-id="12ba2-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="12ba2-189">Создание страницы инструкторов курсов и регистрации</span><span class="sxs-lookup"><span data-stu-id="12ba2-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="12ba2-190">В этом разделе создается инструкторов страницы.</span><span class="sxs-lookup"><span data-stu-id="12ba2-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="12ba2-191">![Страница индекса инструкторов](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="12ba2-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="12ba2-192">Эта страница считывает и взаимосвязанные данные отображаются следующим образом:</span><span class="sxs-lookup"><span data-stu-id="12ba2-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="12ba2-193">В списке инструкторов отображаются связанные данные из `OfficeAssignment` сущности (Office на предыдущем рисунке).</span><span class="sxs-lookup"><span data-stu-id="12ba2-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="12ba2-194">`Instructor` И `OfficeAssignment` сущности находятся в отношении один к нулю или одному.</span><span class="sxs-lookup"><span data-stu-id="12ba2-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="12ba2-195">Безотложная загрузка используется для `OfficeAssignment` сущностей.</span><span class="sxs-lookup"><span data-stu-id="12ba2-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="12ba2-196">Безотложная загрузка обычно более эффективны, когда требуется отобразить связанные данные.</span><span class="sxs-lookup"><span data-stu-id="12ba2-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="12ba2-197">В этом случае отображаются назначения office для инструкторов.</span><span class="sxs-lookup"><span data-stu-id="12ba2-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="12ba2-198">Когда пользователь выбирает инструктора (Harui на предыдущем рисунке), связанные с `Course` сущности отображаются.</span><span class="sxs-lookup"><span data-stu-id="12ba2-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="12ba2-199">`Instructor` И `Course` сущности находятся в отношении многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="12ba2-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="12ba2-200">Загрузка для Eager `Course` сущности и связанных с ними `Department` используется сущностей.</span><span class="sxs-lookup"><span data-stu-id="12ba2-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="12ba2-201">В этом случае отдельные запросы могут оказаться эффективными, поскольку требуются только курсов для выбранного инструктора.</span><span class="sxs-lookup"><span data-stu-id="12ba2-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="12ba2-202">В этом примере показано, как использовать упреждающую для свойств навигации в сущности, находящиеся в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="12ba2-203">Когда пользователь выбирает курса (химии на предыдущем рисунке), связанные с данными из `Enrollments` сущность отображается.</span><span class="sxs-lookup"><span data-stu-id="12ba2-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="12ba2-204">На предыдущем рисунке отображаются студента имя и уровень.</span><span class="sxs-lookup"><span data-stu-id="12ba2-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="12ba2-205">`Course` И `Enrollment` сущности находятся в отношении один ко многим.</span><span class="sxs-lookup"><span data-stu-id="12ba2-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="12ba2-206">Создать модель представлений для представления инструктора индекса</span><span class="sxs-lookup"><span data-stu-id="12ba2-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="12ba2-207">На странице инструкторов отображаются данные из трех разных таблицах.</span><span class="sxs-lookup"><span data-stu-id="12ba2-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="12ba2-208">Модель представления создается, включает три сущности, представляющие три таблицы.</span><span class="sxs-lookup"><span data-stu-id="12ba2-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="12ba2-209">В *SchoolViewModels* папке создайте *InstructorIndexData.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="12ba2-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="12ba2-210">Формировать модели инструктора</span><span class="sxs-lookup"><span data-stu-id="12ba2-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="12ba2-211">Выйдите из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12ba2-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="12ba2-212">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="12ba2-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="12ba2-213">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="12ba2-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="12ba2-214">Предыдущий закрепление команда `Instructor` модели.</span><span class="sxs-lookup"><span data-stu-id="12ba2-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="12ba2-215">Откройте проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="12ba2-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="12ba2-216">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="12ba2-216">Build the project.</span></span> <span data-ttu-id="12ba2-217">Сборки приводит к возникновению ошибки.</span><span class="sxs-lookup"><span data-stu-id="12ba2-217">The build generates errors.</span></span>

<span data-ttu-id="12ba2-218">Глобальная замена `_context.Instructor` для `_context.Instructors` (то есть, добавить «s» `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="12ba2-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="12ba2-219">7 вхождений обнаружены и обновлены.</span><span class="sxs-lookup"><span data-stu-id="12ba2-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="12ba2-220">Запустите приложение и перейдите на страницу инструкторов.</span><span class="sxs-lookup"><span data-stu-id="12ba2-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="12ba2-221">Замените *Pages/Instructors/Index.cshtml.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="12ba2-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="12ba2-222">`OnGetAsync` Метод принимает необязательный маршрутизации данных для идентификатора выбранного инструктора.</span><span class="sxs-lookup"><span data-stu-id="12ba2-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="12ba2-223">Проверьте запрос на *Pages/Instructors/Index.cshtml* страницы:</span><span class="sxs-lookup"><span data-stu-id="12ba2-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="12ba2-224">Запрос имеет два включает в себя:</span><span class="sxs-lookup"><span data-stu-id="12ba2-224">The query has two includes:</span></span>

* <span data-ttu-id="12ba2-225">`OfficeAssignment`: Отображение [инструкторов представление](#IP).</span><span class="sxs-lookup"><span data-stu-id="12ba2-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="12ba2-226">`CourseAssignments`: Вызывает в курсов обучения.</span><span class="sxs-lookup"><span data-stu-id="12ba2-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="12ba2-227">Обновление страницы индекса инструкторов</span><span class="sxs-lookup"><span data-stu-id="12ba2-227">Update the instructors Index page</span></span>

<span data-ttu-id="12ba2-228">Обновление *Pages/Instructors/Index.cshtml* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="12ba2-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="12ba2-229">Предыдущей разметки вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="12ba2-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="12ba2-230">Обновления `page` директив из `@page` для `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="12ba2-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="12ba2-231">`"{id:int?}"`Это шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="12ba2-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="12ba2-232">Шаблон маршрута изменяет целое число строк запроса в URL-адрес для маршрутизации данных.</span><span class="sxs-lookup"><span data-stu-id="12ba2-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="12ba2-233">Например, если щелкнуть **выберите** ссылку для приема инструктора при директивы страницы выводятся URL-адрес, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="12ba2-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="12ba2-234">После директивы страницы `@page "{id:int?}"`, предыдущие URL-адрес:</span><span class="sxs-lookup"><span data-stu-id="12ba2-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="12ba2-235">Заголовок страницы является **инструкторов**.</span><span class="sxs-lookup"><span data-stu-id="12ba2-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="12ba2-236">Добавлен **Office** столбец, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не равно null.</span><span class="sxs-lookup"><span data-stu-id="12ba2-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="12ba2-237">Так как один к нулю или одному связь, могут не существовать OfficeAssignment связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="12ba2-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="12ba2-238">Добавлен **курсы** столбец, отображающий курсы при изучении каждого инструктора.</span><span class="sxs-lookup"><span data-stu-id="12ba2-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="12ba2-239">В разделе [явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) для получения дополнительных сведений о данного синтаксиса razor.</span><span class="sxs-lookup"><span data-stu-id="12ba2-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="12ba2-240">Добавленный код, который динамически добавляет `class="success"` для `tr` инструктора выбранного элемента.</span><span class="sxs-lookup"><span data-stu-id="12ba2-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="12ba2-241">Это задает цвет фона для строк, выделенных с помощью класса начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="12ba2-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="12ba2-242">Добавлена новая гиперссылка с меткой **выберите**.</span><span class="sxs-lookup"><span data-stu-id="12ba2-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="12ba2-243">Идентификатор выбранной инструктора отправляет эту ссылку `Index` метод и задает цвет фона.</span><span class="sxs-lookup"><span data-stu-id="12ba2-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="12ba2-244">Запустите приложение и выберите **инструкторов** вкладки. На странице отображается `Location` (office) из связанного `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="12ba2-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="12ba2-245">Если OfficeAssignment "имеет значение null, отображается по пустой ячейке таблице.</span><span class="sxs-lookup"><span data-stu-id="12ba2-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Страница индекса инструкторов ничего не выбраны](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="12ba2-247">Щелкните **выберите** ссылки.</span><span class="sxs-lookup"><span data-stu-id="12ba2-247">Click on the **Select** link.</span></span> <span data-ttu-id="12ba2-248">Изменения стиля строки.</span><span class="sxs-lookup"><span data-stu-id="12ba2-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="12ba2-249">Добавить выбранные инструктора при изучении курсы</span><span class="sxs-lookup"><span data-stu-id="12ba2-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="12ba2-250">Обновление `OnGetAsync` метод в *Pages/Instructors/Index.cshtml.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="12ba2-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="12ba2-251">Проверьте обновленный запрос:</span><span class="sxs-lookup"><span data-stu-id="12ba2-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="12ba2-252">Предыдущий запрос добавляет `Department` сущностей.</span><span class="sxs-lookup"><span data-stu-id="12ba2-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="12ba2-253">В следующем коде выполняется при выборе инструктор (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="12ba2-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="12ba2-254">Выбранный инструктора извлекается из списка инструкторов в модели представления.</span><span class="sxs-lookup"><span data-stu-id="12ba2-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="12ba2-255">Модель представления `Courses` заполняемые свойства `Course` сущностей из этого инструктора `CourseAssignments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="12ba2-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="12ba2-256">`Where` Метод возвращает коллекцию.</span><span class="sxs-lookup"><span data-stu-id="12ba2-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="12ba2-257">В предыдущем `Where` метод только один `Instructor` возвращается сущность.</span><span class="sxs-lookup"><span data-stu-id="12ba2-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="12ba2-258">`Single` Метод преобразует коллекцию в один `Instructor` сущности.</span><span class="sxs-lookup"><span data-stu-id="12ba2-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="12ba2-259">`Instructor` Сущность предоставляет доступ к `CourseAssignments` свойство.</span><span class="sxs-lookup"><span data-stu-id="12ba2-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="12ba2-260">`CourseAssignments`предоставляет доступ к связанным `Course` сущностей.</span><span class="sxs-lookup"><span data-stu-id="12ba2-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![M:M инструктора курсов](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="12ba2-262">`Single` Метод используется с коллекцией, если в коллекции присутствует только один элемент.</span><span class="sxs-lookup"><span data-stu-id="12ba2-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="12ba2-263">`Single` Метод создает исключение, если коллекция пуста, или если существует более одного элемента.</span><span class="sxs-lookup"><span data-stu-id="12ba2-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="12ba2-264">Альтернативным вариантом является `SingleOrDefault`, который возвращает значение по умолчанию (null в данном случае), если коллекция пуста.</span><span class="sxs-lookup"><span data-stu-id="12ba2-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="12ba2-265">С помощью `SingleOrDefault` для пустой коллекции:</span><span class="sxs-lookup"><span data-stu-id="12ba2-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="12ba2-266">Приводит к возникновению исключения (из найти `Courses` свойство на указатель null).</span><span class="sxs-lookup"><span data-stu-id="12ba2-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="12ba2-267">Сообщение об исключении менее четко укажет причину проблемы.</span><span class="sxs-lookup"><span data-stu-id="12ba2-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="12ba2-268">Следующий код заполняет модель представления `Enrollments` свойств при выборе курса:</span><span class="sxs-lookup"><span data-stu-id="12ba2-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="12ba2-269">Добавьте следующую разметку в конец *Pages/Courses/Index.cshtml* Razor страницы:</span><span class="sxs-lookup"><span data-stu-id="12ba2-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="12ba2-270">Предыдущей разметки список связанных с инструктор при выборе инструктор курсов.</span><span class="sxs-lookup"><span data-stu-id="12ba2-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="12ba2-271">Тестирование приложения.</span><span class="sxs-lookup"><span data-stu-id="12ba2-271">Test the app.</span></span> <span data-ttu-id="12ba2-272">Щелкните **выберите** ссылку на странице инструкторов.</span><span class="sxs-lookup"><span data-stu-id="12ba2-272">Click on a **Select** link on the instructors page.</span></span>

![Инструкторов индекс страницы инструктора выбран](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="12ba2-274">Показать учащихся данные</span><span class="sxs-lookup"><span data-stu-id="12ba2-274">Show student data</span></span>

<span data-ttu-id="12ba2-275">В этом разделе приложение обновляется для отображения данных студента выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="12ba2-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="12ba2-276">Обновить запрос в `OnGetAsync` метод в *Pages/Instructors/Index.cshtml.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="12ba2-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="12ba2-277">Обновление *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="12ba2-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="12ba2-278">Добавьте следующую разметку в конец файла:</span><span class="sxs-lookup"><span data-stu-id="12ba2-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="12ba2-279">Предыдущей разметки список учащихся, которые участвуют в выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="12ba2-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="12ba2-280">Обновите страницу и выберите инструктор.</span><span class="sxs-lookup"><span data-stu-id="12ba2-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="12ba2-281">Выберите курс, чтобы просмотреть список зарегистрированных студентов и их оценки.</span><span class="sxs-lookup"><span data-stu-id="12ba2-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Инструктора страницы индекса инструкторов и выбранного курса](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="12ba2-283">С помощью одного</span><span class="sxs-lookup"><span data-stu-id="12ba2-283">Using Single</span></span>

<span data-ttu-id="12ba2-284">`Single` Метод можно передавать в `Where` условие вместо вызова метода `Where` метод отдельно:</span><span class="sxs-lookup"><span data-stu-id="12ba2-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="12ba2-285">Приведенный выше `Single` подход не дает преимущества по сравнению с использованием `Where`.</span><span class="sxs-lookup"><span data-stu-id="12ba2-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="12ba2-286">Некоторые разработчики предпочитают `Single` подхода стиля.</span><span class="sxs-lookup"><span data-stu-id="12ba2-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="12ba2-287">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="12ba2-287">Explicit loading</span></span>

<span data-ttu-id="12ba2-288">Текущий код указывает упреждающую для `Enrollments` и `Students`:</span><span class="sxs-lookup"><span data-stu-id="12ba2-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="12ba2-289">Предположим, что пользователи редко хотят видеть регистрации курса.</span><span class="sxs-lookup"><span data-stu-id="12ba2-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="12ba2-290">В этом случае оптимизация будет загружать данных регистрации, только если она запрошена.</span><span class="sxs-lookup"><span data-stu-id="12ba2-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="12ba2-291">В этом разделе `OnGetAsync` обновляется для использования явное загрузку `Enrollments` и `Students`.</span><span class="sxs-lookup"><span data-stu-id="12ba2-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="12ba2-292">Обновление `OnGetAsync` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="12ba2-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="12ba2-293">Предыдущий код удаляет *ThenInclude* вызывает метод для регистрации и студентов данных.</span><span class="sxs-lookup"><span data-stu-id="12ba2-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="12ba2-294">Если выбран курса, получает выделенный код:</span><span class="sxs-lookup"><span data-stu-id="12ba2-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="12ba2-295">`Enrollment` Сущностей для выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="12ba2-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="12ba2-296">`Student` Сущностей для каждого `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="12ba2-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="12ba2-297">Обратите внимание на то, приведенный выше код переводит в комментарий `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="12ba2-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="12ba2-298">Свойства навигации могут быть явно загружены только для отслеживаемых объектов.</span><span class="sxs-lookup"><span data-stu-id="12ba2-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="12ba2-299">Тестирование приложения.</span><span class="sxs-lookup"><span data-stu-id="12ba2-299">Test the app.</span></span> <span data-ttu-id="12ba2-300">С точки зрения пользователей приложение работает аналогично предыдущей версии.</span><span class="sxs-lookup"><span data-stu-id="12ba2-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="12ba2-301">Далее учебнике показано, как обновить связанные данные.</span><span class="sxs-lookup"><span data-stu-id="12ba2-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="12ba2-302">[Назад](xref:data/ef-rp/complex-data-model)
>[Вперед](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="12ba2-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
