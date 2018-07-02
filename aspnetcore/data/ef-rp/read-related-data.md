---
title: Razor Pages с EF Core в ASP.NET Core — чтение связанных данных — 6 из 8
author: rick-anderson
description: Из этого руководства вы узнаете, как читать и отображать связанные данные — данные, которые Entity Framework загружает в свойства навигации.
ms.author: riande
ms.date: 11/05/2017
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 4e0aa7151cc54f666202458ba60500a7c04f5ebb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276764"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="86a7e-103">Razor Pages с EF Core в ASP.NET Core — чтение связанных данных — 6 из 8</span><span class="sxs-lookup"><span data-stu-id="86a7e-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="86a7e-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="86a7e-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="86a7e-105">В этом руководстве выполняется чтение и отображение связанных данных.</span><span class="sxs-lookup"><span data-stu-id="86a7e-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="86a7e-106">Связанными называются данные, которые EF Core загружает в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="86a7e-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="86a7e-107">При возникновении проблем, которые вам не удается устранить, скачайте [готовое приложение для этого этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span><span class="sxs-lookup"><span data-stu-id="86a7e-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="86a7e-108">На следующих рисунках изображены готовые страницы для этого руководства:</span><span class="sxs-lookup"><span data-stu-id="86a7e-108">The following illustrations show the completed pages for this tutorial:</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="86a7e-111">Безотложная, явная и отложенная загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="86a7e-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="86a7e-112">Существует несколько способов, которыми EF Core может загружать связанные данные в свойства навигации сущности:</span><span class="sxs-lookup"><span data-stu-id="86a7e-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="86a7e-113">[Безотложная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="86a7e-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="86a7e-114">Безотложной является загрузка, когда запрос для одного типа сущности также загружает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="86a7e-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="86a7e-115">При чтении сущности связанные данные извлекаются.</span><span class="sxs-lookup"><span data-stu-id="86a7e-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="86a7e-116">Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные.</span><span class="sxs-lookup"><span data-stu-id="86a7e-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="86a7e-117">Для некоторых типов безотложной загрузки EF Core выдает несколько запросов.</span><span class="sxs-lookup"><span data-stu-id="86a7e-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="86a7e-118">Выдача нескольких запросов может оказаться более эффективной по сравнению с отдельными ситуациями в EF6, когда выполнялся отдельный запрос.</span><span class="sxs-lookup"><span data-stu-id="86a7e-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="86a7e-119">Безотложная загрузка указывается с помощью методов `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Пример безотложной загрузки](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="86a7e-121">Безотложная загрузка отправляет несколько запросов, когда включена навигация коллекции:</span><span class="sxs-lookup"><span data-stu-id="86a7e-121">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="86a7e-122">Один запрос для основного запроса</span><span class="sxs-lookup"><span data-stu-id="86a7e-122">One query for the main query</span></span> 
  * <span data-ttu-id="86a7e-123">Один запрос для каждого "края" коллекции в дереве загрузки.</span><span class="sxs-lookup"><span data-stu-id="86a7e-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="86a7e-124">Отдельные запросы с `Load`: данные можно получить в отдельных запросах, а EF Core "исправляет" свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="86a7e-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="86a7e-125">Термин "исправляет" означает, что EF Core автоматически заполняет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="86a7e-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="86a7e-126">Отдельные запросы с `Load` больше похожи на явную загрузку, чем безотложную.</span><span class="sxs-lookup"><span data-stu-id="86a7e-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="86a7e-128">Примечание: EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="86a7e-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="86a7e-129">Даже если данные для свойства навигации *не* включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="86a7e-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="86a7e-130">[Явная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="86a7e-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="86a7e-131">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="86a7e-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="86a7e-132">Нужно написать код, извлекающий связанные данные, когда они необходимы.</span><span class="sxs-lookup"><span data-stu-id="86a7e-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="86a7e-133">Явная загрузка с отдельными запросами приводит к отправке нескольких запросов к базе данных.</span><span class="sxs-lookup"><span data-stu-id="86a7e-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="86a7e-134">При явной загрузке код указывает, какие свойства навигации нужно загрузить.</span><span class="sxs-lookup"><span data-stu-id="86a7e-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="86a7e-135">Для выполнения явной загрузки используется метод `Load`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="86a7e-136">Пример:</span><span class="sxs-lookup"><span data-stu-id="86a7e-136">For example:</span></span>

  ![Пример явной загрузки](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="86a7e-138">[Отложенная загрузка](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="86a7e-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="86a7e-139">[Сейчас EF Core не поддерживает отложенную загрузку](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span><span class="sxs-lookup"><span data-stu-id="86a7e-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="86a7e-140">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="86a7e-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="86a7e-141">При первом обращении к свойству навигации необходимые для этого свойства данные извлекаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="86a7e-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="86a7e-142">Запрос к базе данных отправляется при каждом первом обращении к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="86a7e-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="86a7e-143">Оператор `Select` загружает только необходимые связанные данные.</span><span class="sxs-lookup"><span data-stu-id="86a7e-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="86a7e-144">Создание страницы "Courses" (Курсы) с отображением названий кафедр</span><span class="sxs-lookup"><span data-stu-id="86a7e-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="86a7e-145">Сущность "Course" включает в себя свойство навигации, содержащее сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="86a7e-146">Сущность `Department` содержит кафедру, которой назначен курс.</span><span class="sxs-lookup"><span data-stu-id="86a7e-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="86a7e-147">Отображение имени назначенной кафедры в списке курсов:</span><span class="sxs-lookup"><span data-stu-id="86a7e-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="86a7e-148">Получите свойство `Name` из сущности `Department`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="86a7e-149">Сущность `Department` получается из свойства навигации `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="86a7e-151">Формирование шаблона для модели Course</span><span class="sxs-lookup"><span data-stu-id="86a7e-151">Scaffold the Course model</span></span>

* <span data-ttu-id="86a7e-152">Закройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86a7e-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="86a7e-153">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="86a7e-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="86a7e-154">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="86a7e-154">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

<span data-ttu-id="86a7e-155">Предыдущая команда формирует шаблон для модели `Course`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="86a7e-156">Откройте проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86a7e-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="86a7e-157">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="86a7e-157">Build the project.</span></span> <span data-ttu-id="86a7e-158">Эта сборка выдает ошибки, например следующего характера:</span><span class="sxs-lookup"><span data-stu-id="86a7e-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="86a7e-159">Выполните глобальную замену `_context.Course` на `_context.Courses` (она заключается в добавлении "s" к `Course`).</span><span class="sxs-lookup"><span data-stu-id="86a7e-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="86a7e-160">Обнаруживаются и изменяются 7 экземпляров.</span><span class="sxs-lookup"><span data-stu-id="86a7e-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="86a7e-161">Откройте *Pages/Courses/Index.cshtml.cs* и изучите метод `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="86a7e-162">Подсистема формирования шаблонов указала безотложную загрузку для свойства навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="86a7e-163">Метод `Include` задает безотложную загрузку.</span><span class="sxs-lookup"><span data-stu-id="86a7e-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="86a7e-164">Запустите приложение и выберите ссылку **Courses** (Курсы).</span><span class="sxs-lookup"><span data-stu-id="86a7e-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="86a7e-165">В столбце кафедры отображается бесполезный `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="86a7e-166">Обновите метод `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="86a7e-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="86a7e-167">Предыдущий код добавляет `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="86a7e-168">`AsNoTracking` повышает производительность, так как возвращаемые сущности не отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="86a7e-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="86a7e-169">Отслеживание отсутствует, так как сущности не изменяются в текущем контексте.</span><span class="sxs-lookup"><span data-stu-id="86a7e-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="86a7e-170">Измените *Pages/Courses/Index.cshtml*, используя выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="86a7e-170">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="86a7e-171">В шаблонный код были внесены следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="86a7e-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="86a7e-172">Изменен заголовок с "Index" (Индекс) на "Courses" (Курсы).</span><span class="sxs-lookup"><span data-stu-id="86a7e-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="86a7e-173">Добавлен столбец **Number** (Номер), отображающий значение свойства `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="86a7e-174">По умолчанию в шаблоне отсутствуют первичные ключи, поскольку для конечных пользователей они не имеют смысла.</span><span class="sxs-lookup"><span data-stu-id="86a7e-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="86a7e-175">Однако в нашем случае первичный ключ имеет смысл.</span><span class="sxs-lookup"><span data-stu-id="86a7e-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="86a7e-176">Изменен столбец **Department** (Кафедра) для отображения названия кафедры.</span><span class="sxs-lookup"><span data-stu-id="86a7e-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="86a7e-177">Код отображает свойство `Name` сущности `Department`, которая загружена в свойство навигации `Department`:</span><span class="sxs-lookup"><span data-stu-id="86a7e-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="86a7e-178">Для просмотра списка с названиями кафедр запустите приложение и выберите вкладку **Courses** (Курсы).</span><span class="sxs-lookup"><span data-stu-id="86a7e-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="86a7e-180">Загрузка связанных данных с помощью "Select"</span><span class="sxs-lookup"><span data-stu-id="86a7e-180">Loading related data with Select</span></span>

<span data-ttu-id="86a7e-181">Метод `OnGetAsync` загружает связанные данные с помощью метода `Include`:</span><span class="sxs-lookup"><span data-stu-id="86a7e-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="86a7e-182">Оператор `Select` загружает только необходимые связанные данные.</span><span class="sxs-lookup"><span data-stu-id="86a7e-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="86a7e-183">Для отдельных элементов, таких как `Department.Name`, используется SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="86a7e-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="86a7e-184">Для коллекций используется доступ к другой базе данных, но это же делает и оператор `Include` в коллекциях.</span><span class="sxs-lookup"><span data-stu-id="86a7e-184">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="86a7e-185">Следующий код загружает связанные данные с помощью метода `Select`:</span><span class="sxs-lookup"><span data-stu-id="86a7e-185">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="86a7e-186">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="86a7e-186">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="86a7e-187">Полный пример см. в описании [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) и [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="86a7e-187">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="86a7e-188">Создание страницы "Instructors" (Преподаватели) с отображением курсов и дат зачисления</span><span class="sxs-lookup"><span data-stu-id="86a7e-188">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="86a7e-189">В этом разделе создается страница "Instructors" (Преподаватели).</span><span class="sxs-lookup"><span data-stu-id="86a7e-189">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="86a7e-190">![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="86a7e-190">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="86a7e-191">Эта страница считывает и отображает связанные данные следующим образом:</span><span class="sxs-lookup"><span data-stu-id="86a7e-191">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="86a7e-192">Список преподавателей отображает связанные данные из сущности `OfficeAssignment` ("Office" (Кабинет) на предыдущем изображении).</span><span class="sxs-lookup"><span data-stu-id="86a7e-192">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="86a7e-193">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="86a7e-193">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="86a7e-194">Безотложная загрузка используется для сущностей `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-194">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="86a7e-195">Безотложная загрузка обычно более эффективна, когда требуется отобразить связанные данные.</span><span class="sxs-lookup"><span data-stu-id="86a7e-195">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="86a7e-196">В этом случае отображается принадлежность к кабинету для каждого преподавателя.</span><span class="sxs-lookup"><span data-stu-id="86a7e-196">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="86a7e-197">Когда пользователь выбирает преподавателя (Harui на предыдущем изображении), отображаются связанные сущности `Course`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-197">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="86a7e-198">Между сущностями `Instructor` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="86a7e-198">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="86a7e-199">Используется безотложная загрузка для сущностей `Course` и связанных сущностей `Department`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-199">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="86a7e-200">В этом случае отдельные запросы могут оказаться эффективнее, так как требуются только курсы для выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="86a7e-200">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="86a7e-201">Этот пример показывает, как использовать безотложную загрузку для свойств навигации в сущностях, находящихся в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="86a7e-201">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="86a7e-202">Когда пользователь выбирает курс ("Chemistry" (Химия) на предыдущем изображении), отображаются связанные данные из сущности `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-202">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="86a7e-203">На предыдущем изображении отображается имя и оценка учащегося.</span><span class="sxs-lookup"><span data-stu-id="86a7e-203">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="86a7e-204">Между сущностями `Course` и `Enrollment` действует связь один ко многим.</span><span class="sxs-lookup"><span data-stu-id="86a7e-204">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="86a7e-205">Создание модели для представления индекса преподавателей</span><span class="sxs-lookup"><span data-stu-id="86a7e-205">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="86a7e-206">На странице "Instructors" (Преподаватели) отображаются данные из трех различных таблиц.</span><span class="sxs-lookup"><span data-stu-id="86a7e-206">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="86a7e-207">Создается модель представления, которая включает три сущности, представляющие три таблицы.</span><span class="sxs-lookup"><span data-stu-id="86a7e-207">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="86a7e-208">Создайте в папке *SchoolViewModels* файл *InstructorIndexData.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="86a7e-208">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="86a7e-209">Формирование шаблона для модели "Instructor" (Преподаватель)</span><span class="sxs-lookup"><span data-stu-id="86a7e-209">Scaffold the Instructor model</span></span>

* <span data-ttu-id="86a7e-210">Закройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86a7e-210">Exit Visual Studio.</span></span>
* <span data-ttu-id="86a7e-211">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="86a7e-211">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="86a7e-212">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="86a7e-212">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

<span data-ttu-id="86a7e-213">Предыдущая команда формирует шаблон для модели `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-213">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="86a7e-214">Откройте проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="86a7e-214">Open the project in Visual Studio.</span></span>

<span data-ttu-id="86a7e-215">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="86a7e-215">Build the project.</span></span> <span data-ttu-id="86a7e-216">Эта сборка выдает ошибки.</span><span class="sxs-lookup"><span data-stu-id="86a7e-216">The build generates errors.</span></span>

<span data-ttu-id="86a7e-217">Выполните глобальную замену `_context.Instructor` на `_context.Instructors` (она заключается в добавлении "s" к `Instructor`).</span><span class="sxs-lookup"><span data-stu-id="86a7e-217">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="86a7e-218">Обнаруживаются и изменяются 7 экземпляров.</span><span class="sxs-lookup"><span data-stu-id="86a7e-218">7 occurrences are found and updated.</span></span>

<span data-ttu-id="86a7e-219">Запустите приложение и перейдите на страницу преподавателей.</span><span class="sxs-lookup"><span data-stu-id="86a7e-219">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="86a7e-220">Замените содержимое *Pages/Instructors/Index.cshtml.cs* на следующий код:</span><span class="sxs-lookup"><span data-stu-id="86a7e-220">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-99)]

<span data-ttu-id="86a7e-221">Метод `OnGetAsync` принимает необязательные данные маршрутизации для идентификатора выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="86a7e-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="86a7e-222">Изучите запрос в файле *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="86a7e-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="86a7e-223">Запрос имеет две операции включения:</span><span class="sxs-lookup"><span data-stu-id="86a7e-223">The query has two includes:</span></span>

* <span data-ttu-id="86a7e-224">`OfficeAssignment`: отображается в [представлении преподавателей](#IP).</span><span class="sxs-lookup"><span data-stu-id="86a7e-224">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="86a7e-225">`CourseAssignments`: вызывает проводимые курсы.</span><span class="sxs-lookup"><span data-stu-id="86a7e-225">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="86a7e-226">Изменение страницы индекса преподавателей</span><span class="sxs-lookup"><span data-stu-id="86a7e-226">Update the instructors Index page</span></span>

<span data-ttu-id="86a7e-227">Измените *Pages/Instructors/Index.cshtml*, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="86a7e-227">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="86a7e-228">Приведенная выше разметка вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="86a7e-228">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="86a7e-229">Изменяет директиву `page` с `@page` на `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-229">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="86a7e-230">`"{id:int?}"` является шаблоном маршрута.</span><span class="sxs-lookup"><span data-stu-id="86a7e-230">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="86a7e-231">Шаблон маршрута изменяет целочисленные строки запроса в URL-адресе для маршрутизации данных.</span><span class="sxs-lookup"><span data-stu-id="86a7e-231">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="86a7e-232">Например, при выборе ссылки **Select** для преподавателя только с директивой `@page` формируется URL-адрес следующего вида:</span><span class="sxs-lookup"><span data-stu-id="86a7e-232">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="86a7e-233">Когда используется директива страницы `@page "{id:int?}"`, предыдущий URL-адрес имеет следующее значение:</span><span class="sxs-lookup"><span data-stu-id="86a7e-233">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="86a7e-234">Заголовком страницы является **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="86a7e-234">Page title is **Instructors**.</span></span>
* <span data-ttu-id="86a7e-235">Добавили столбец **Office**, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не равно null.</span><span class="sxs-lookup"><span data-stu-id="86a7e-235">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="86a7e-236">Так как это связь один к нулю или одному, то связанная сущность OfficeAssignment может отсутствовать.</span><span class="sxs-lookup"><span data-stu-id="86a7e-236">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="86a7e-237">Добавили столбец **Courses**, отображающий курсы, которые ведет конкретный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="86a7e-237">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="86a7e-238">Подробные сведения об использовании синтаксиса Razor см. в разделе [Явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="86a7e-238">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="86a7e-239">Добавили код, который динамически добавляет `class="success"` к элементу `tr` выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="86a7e-239">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="86a7e-240">Этот параметр задает цвет фона для выделенных строк c помощью класса Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="86a7e-240">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="86a7e-241">Добавлена новая гиперссылка с меткой **Select** (Выбрать).</span><span class="sxs-lookup"><span data-stu-id="86a7e-241">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="86a7e-242">Она отправляет идентификатор выбранного преподавателя в метод `Index` и задает цвет фона.</span><span class="sxs-lookup"><span data-stu-id="86a7e-242">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="86a7e-243">Запустите приложение и выберите вкладку **Instructors**. На странице отображается `Location` (кабинет) из связанной сущности `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-243">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="86a7e-244">Если OfficeAssignment имеет значение null, отображается пустая ячейка таблицы.</span><span class="sxs-lookup"><span data-stu-id="86a7e-244">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Страница индекса преподавателей, когда ничего не выбрано](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="86a7e-246">Щелкните ссылку **Select** (Выбрать).</span><span class="sxs-lookup"><span data-stu-id="86a7e-246">Click on the **Select** link.</span></span> <span data-ttu-id="86a7e-247">Изменяется стиль строки.</span><span class="sxs-lookup"><span data-stu-id="86a7e-247">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="86a7e-248">Добавление курсов, проводимых выбранным преподавателем</span><span class="sxs-lookup"><span data-stu-id="86a7e-248">Add courses taught by selected instructor</span></span>

<span data-ttu-id="86a7e-249">Измените метод `OnGetAsync` в *Pages/Instructors/Index.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="86a7e-249">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="86a7e-250">Добавление `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="86a7e-250">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="86a7e-251">Проверьте измененный запрос:</span><span class="sxs-lookup"><span data-stu-id="86a7e-251">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="86a7e-252">Предыдущий запрос добавляет сущности `Department`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="86a7e-253">Приведенный ниже код выполняется при выборе преподавателя (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="86a7e-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="86a7e-254">Выбранный преподаватель извлекается из списка преподавателей в модели представления.</span><span class="sxs-lookup"><span data-stu-id="86a7e-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="86a7e-255">Из свойства навигации `CourseAssignments` этого преподавателя загружается свойство модели представления `Courses` вместе с сущностями `Course`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="86a7e-256">Метод `Where` возвращает коллекцию.</span><span class="sxs-lookup"><span data-stu-id="86a7e-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="86a7e-257">В предыдущем методе `Where` возвращается всего одна сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="86a7e-258">Метод `Single` преобразует коллекцию в отдельную сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="86a7e-259">Сущность `Instructor` предоставляет доступ к свойству `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="86a7e-260">`CourseAssignments` предоставляет доступ к связанным сущностям `Course`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="86a7e-262">Метод `Single` используется для коллекции, когда она содержит всего один элемент.</span><span class="sxs-lookup"><span data-stu-id="86a7e-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="86a7e-263">Метод `Single` вызывает исключение, если коллекция пуста или содержит больше одного элемента.</span><span class="sxs-lookup"><span data-stu-id="86a7e-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="86a7e-264">Альтернативным вариантом является метод `SingleOrDefault`, который возвращает значение по умолчанию (в данном случае null), если коллекция пуста.</span><span class="sxs-lookup"><span data-stu-id="86a7e-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="86a7e-265">Использование `SingleOrDefault` для пустой коллекции:</span><span class="sxs-lookup"><span data-stu-id="86a7e-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="86a7e-266">Возникает исключение (из-за попытки найти свойство `Courses` по пустой ссылке).</span><span class="sxs-lookup"><span data-stu-id="86a7e-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="86a7e-267">Сообщение об исключении не так четко указывает на причину проблемы.</span><span class="sxs-lookup"><span data-stu-id="86a7e-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="86a7e-268">Следующий код заполняет свойство `Enrollments` модели представления при выборе курса:</span><span class="sxs-lookup"><span data-stu-id="86a7e-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="86a7e-269">Добавьте следующую разметку в конец страницы Razor *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="86a7e-269">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="86a7e-270">Предыдущая разметка отображает список связанных с преподавателем курсов при выборе преподавателя.</span><span class="sxs-lookup"><span data-stu-id="86a7e-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="86a7e-271">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="86a7e-271">Test the app.</span></span> <span data-ttu-id="86a7e-272">Щелкните ссылку **Select** (Выбрать) на странице преподавателей.</span><span class="sxs-lookup"><span data-stu-id="86a7e-272">Click on a **Select** link on the instructors page.</span></span>

![Страница индекса преподавателей, выбран преподаватель](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="86a7e-274">Отображение данных об учащихся</span><span class="sxs-lookup"><span data-stu-id="86a7e-274">Show student data</span></span>

<span data-ttu-id="86a7e-275">В этом разделе приложение изменяется, чтобы отобразить данные об учащихся для выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="86a7e-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="86a7e-276">Измените запрос в методе `OnGetAsync` в *Pages/Instructors/Index.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="86a7e-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="86a7e-277">Измените *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="86a7e-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="86a7e-278">Добавьте следующую разметку в конец файла:</span><span class="sxs-lookup"><span data-stu-id="86a7e-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="86a7e-279">Предыдущая разметка отображает список учащихся, которые зачислены на курс.</span><span class="sxs-lookup"><span data-stu-id="86a7e-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="86a7e-280">Обновите страницу и выберите преподавателя.</span><span class="sxs-lookup"><span data-stu-id="86a7e-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="86a7e-281">Выберите курс, чтобы увидеть список зачисленных учащихся и их оценки.</span><span class="sxs-lookup"><span data-stu-id="86a7e-281">Select a course to see the list of enrolled students and their grades.</span></span>

![Страница индекса преподавателей, выбраны преподаватель и курс](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="86a7e-283">Использование метода Single</span><span class="sxs-lookup"><span data-stu-id="86a7e-283">Using Single</span></span>

<span data-ttu-id="86a7e-284">Метод `Single` может передать условие `Where` вместо отдельного вызова метода `Where`:</span><span class="sxs-lookup"><span data-stu-id="86a7e-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="86a7e-285">Приведенный выше подход на основе `Single` не дает преимуществ по сравнению с использованием `Where`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="86a7e-286">Некоторые разработчики предпочитают использовать подход `Single`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="86a7e-287">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="86a7e-287">Explicit loading</span></span>

<span data-ttu-id="86a7e-288">Текущий код указывает упреждающую загрузку для `Enrollments` и `Students`:</span><span class="sxs-lookup"><span data-stu-id="86a7e-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="86a7e-289">Предположим, что пользователям редко требуется просматривать зачисления на курс.</span><span class="sxs-lookup"><span data-stu-id="86a7e-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="86a7e-290">В этом случае можно оптимизировать работу, загружая данные о зачислении только при их запросе.</span><span class="sxs-lookup"><span data-stu-id="86a7e-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="86a7e-291">В этом разделе изменяется `OnGetAsync` для использования явной загрузки `Enrollments` и `Students`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="86a7e-292">Измените `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="86a7e-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="86a7e-293">Предыдущий код удаляет вызовы метода *ThenInclude* для данных о зачислении и учащихся.</span><span class="sxs-lookup"><span data-stu-id="86a7e-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="86a7e-294">Если курс выбран, выделенный код извлекает следующее:</span><span class="sxs-lookup"><span data-stu-id="86a7e-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="86a7e-295">Сущности `Enrollment` для выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="86a7e-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="86a7e-296">Сущности `Student` для каждого `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="86a7e-297">Обратите внимание, что предыдущий код закомментирует `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="86a7e-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="86a7e-298">Для отслеживаемых сущностей свойства навигации можно загрузить лишь явно.</span><span class="sxs-lookup"><span data-stu-id="86a7e-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="86a7e-299">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="86a7e-299">Test the app.</span></span> <span data-ttu-id="86a7e-300">С точки зрения пользователей приложение работает аналогично предыдущей версии.</span><span class="sxs-lookup"><span data-stu-id="86a7e-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="86a7e-301">Следующее руководство посвящено обновлению связанных данных.</span><span class="sxs-lookup"><span data-stu-id="86a7e-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="86a7e-302">[Назад](xref:data/ef-rp/complex-data-model)
>[Вперед](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="86a7e-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
