---
title: Razor Pages с EF Core в ASP.NET Core — чтение связанных данных — 6 из 8
author: tdykstra
description: Из этого руководства вы узнаете, как читать и отображать связанные данные — данные, которые Entity Framework загружает в свойства навигации.
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: f53f8edef7fe8690d0e414bc094d81dc99ad198a
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082088"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="5478c-103">Razor Pages с EF Core в ASP.NET Core — чтение связанных данных — 6 из 8</span><span class="sxs-lookup"><span data-stu-id="5478c-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="5478c-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra), [Йон П. Смит](https://twitter.com/thereformedprog) (Jon P Smith) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5478c-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5478c-105">Этот учебник посвящен чтению и отображению связанных данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="5478c-106">Связанными называются данные, которые EF Core загружает в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="5478c-107">На следующих рисунках изображены готовые страницы для этого руководства:</span><span class="sxs-lookup"><span data-stu-id="5478c-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index30.png)

![Страница индекса преподавателей](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="5478c-110">Безотложная, явная и отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="5478c-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="5478c-111">Существует несколько способов, которыми EF Core может загружать связанные данные в свойства навигации сущности:</span><span class="sxs-lookup"><span data-stu-id="5478c-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="5478c-112">[Безотложная загрузка](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="5478c-113">Безотложной является загрузка, когда запрос для одного типа сущности также загружает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="5478c-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="5478c-114">При чтении сущности извлекаются ее связанные данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="5478c-115">Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="5478c-116">Для некоторых типов безотложной загрузки EF Core выдает несколько запросов.</span><span class="sxs-lookup"><span data-stu-id="5478c-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="5478c-117">Отправка нескольких запросов может оказаться эффективнее, чем выполнение одного большого запроса.</span><span class="sxs-lookup"><span data-stu-id="5478c-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="5478c-118">Безотложная загрузка указывается с помощью методов `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="5478c-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Пример безотложной загрузки](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="5478c-120">Безотложная загрузка отправляет несколько запросов, когда включена навигация коллекции:</span><span class="sxs-lookup"><span data-stu-id="5478c-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="5478c-121">Один запрос для основного запроса</span><span class="sxs-lookup"><span data-stu-id="5478c-121">One query for the main query</span></span> 
  * <span data-ttu-id="5478c-122">Один запрос для каждого "края" коллекции в дереве загрузки.</span><span class="sxs-lookup"><span data-stu-id="5478c-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="5478c-123">Отдельные запросы с `Load`: данные можно получить в отдельных запросах, а EF Core исправляет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="5478c-124">Термин "исправляет" означает, что EF Core автоматически заполняет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="5478c-125">Отдельные запросы с `Load` больше похожи на явную загрузку, чем на безотложную.</span><span class="sxs-lookup"><span data-stu-id="5478c-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="5478c-127">Примечание. EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="5478c-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="5478c-128">Даже если данные для свойства навигации *не* включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="5478c-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="5478c-129">[Явная загрузка](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="5478c-130">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="5478c-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5478c-131">Нужно написать код, извлекающий связанные данные, когда они необходимы.</span><span class="sxs-lookup"><span data-stu-id="5478c-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="5478c-132">Явная загрузка с отдельными запросами приводит к отправке нескольких запросов к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="5478c-133">При явной загрузке код указывает, какие свойства навигации нужно загрузить.</span><span class="sxs-lookup"><span data-stu-id="5478c-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="5478c-134">Для выполнения явной загрузки используется метод `Load`.</span><span class="sxs-lookup"><span data-stu-id="5478c-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="5478c-135">Например:</span><span class="sxs-lookup"><span data-stu-id="5478c-135">For example:</span></span>

  ![Пример явной загрузки](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="5478c-137">[Отложенная загрузка](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="5478c-138">[В EF Core версии 2.1 добавлена отложенная загрузка](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="5478c-139">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="5478c-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5478c-140">При первом обращении к свойству навигации необходимые для этого свойства данные извлекаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="5478c-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="5478c-141">Запрос к базе данных отправляется при каждом первом обращении к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="5478c-142">Создание страниц курсов</span><span class="sxs-lookup"><span data-stu-id="5478c-142">Create Course pages</span></span>

<span data-ttu-id="5478c-143">Сущность `Course` включает в себя свойство навигации, содержащее связанную сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<span data-ttu-id="5478c-145">Чтобы отобразить имя кафедры, которой назначен курс, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="5478c-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="5478c-146">Загрузите связанную сущность `Department` в свойство навигации `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="5478c-147">Получите имя из свойства `Name` сущности `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="5478c-148">Формирование шаблона для страниц курсов</span><span class="sxs-lookup"><span data-stu-id="5478c-148">Scaffold Course pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5478c-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5478c-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5478c-150">Следуйте инструкциям в разделе [Формирование шаблона для страниц Student](xref:data/ef-rp/intro#scaffold-student-pages), за исключением следующего:</span><span class="sxs-lookup"><span data-stu-id="5478c-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="5478c-151">Создайте папку *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="5478c-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="5478c-152">Используйте класс модели `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="5478c-153">Используйте существующий класс контекста вместо создания нового.</span><span class="sxs-lookup"><span data-stu-id="5478c-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5478c-154">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5478c-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5478c-155">Создайте папку *Pages/Courses*.</span><span class="sxs-lookup"><span data-stu-id="5478c-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="5478c-156">Выполните приведенную ниже команду, чтобы сформировать шаблон для страниц курсов.</span><span class="sxs-lookup"><span data-stu-id="5478c-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="5478c-157">**В Windows:**</span><span class="sxs-lookup"><span data-stu-id="5478c-157">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="5478c-158">**В Linux или macOS:**</span><span class="sxs-lookup"><span data-stu-id="5478c-158">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="5478c-159">Откройте *Pages/Courses/Index.cshtml.cs* и изучите метод `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="5478c-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="5478c-160">Подсистема формирования шаблонов указала безотложную загрузку для свойства навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="5478c-161">Метод `Include` задает безотложную загрузку.</span><span class="sxs-lookup"><span data-stu-id="5478c-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="5478c-162">Запустите приложение и выберите ссылку **Courses** (Курсы).</span><span class="sxs-lookup"><span data-stu-id="5478c-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="5478c-163">В столбце кафедры отображается бесполезный `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="5478c-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="5478c-164">Отображение названия кафедры</span><span class="sxs-lookup"><span data-stu-id="5478c-164">Display the department name</span></span>

<span data-ttu-id="5478c-165">Измените файл Pages/Courses/Index.cshtml.cs, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="5478c-166">Приведенный выше код изменяет свойство `Course` на `Courses` и добавляет `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="5478c-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="5478c-167">`AsNoTracking` повышает производительность, так как возвращаемые сущности не отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="5478c-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="5478c-168">Отслеживать сущности не нужно, так как они не изменяются в текущем контексте.</span><span class="sxs-lookup"><span data-stu-id="5478c-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="5478c-169">Измените файл *Pages/Courses/Index.cshtml*, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="5478c-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="5478c-170">В шаблонный код были внесены следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="5478c-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="5478c-171">Имя свойства `Course` изменилось на `Courses`.</span><span class="sxs-lookup"><span data-stu-id="5478c-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="5478c-172">Добавлен столбец **Number** (Номер), отображающий значение свойства `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="5478c-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="5478c-173">По умолчанию в шаблоне отсутствуют первичные ключи, поскольку для конечных пользователей они не имеют смысла.</span><span class="sxs-lookup"><span data-stu-id="5478c-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="5478c-174">Однако в нашем случае первичный ключ имеет смысл.</span><span class="sxs-lookup"><span data-stu-id="5478c-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="5478c-175">Изменен столбец **Department** (Кафедра) для отображения названия кафедры.</span><span class="sxs-lookup"><span data-stu-id="5478c-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="5478c-176">Код отображает свойство `Name` сущности `Department`, которая загружена в свойство навигации `Department`:</span><span class="sxs-lookup"><span data-stu-id="5478c-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="5478c-177">Для просмотра списка с названиями кафедр запустите приложение и выберите вкладку **Courses** (Курсы).</span><span class="sxs-lookup"><span data-stu-id="5478c-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="5478c-179">Загрузка связанных данных с помощью "Select"</span><span class="sxs-lookup"><span data-stu-id="5478c-179">Loading related data with Select</span></span>

<span data-ttu-id="5478c-180">Метод `OnGetAsync` загружает связанные данные с помощью метода `Include`.</span><span class="sxs-lookup"><span data-stu-id="5478c-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="5478c-181">Метод `Select` является альтернативным вариантом, который загружает только необходимые связанные данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="5478c-182">Для отдельных элементов, таких как `Department.Name`, используется SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="5478c-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="5478c-183">Для коллекций используется доступ к другой базе данных, но это же делает и оператор `Include` в коллекциях.</span><span class="sxs-lookup"><span data-stu-id="5478c-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="5478c-184">Следующий код загружает связанные данные с помощью метода `Select`:</span><span class="sxs-lookup"><span data-stu-id="5478c-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="5478c-185">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="5478c-185">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="5478c-186">Полный пример см. в описании [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) и [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="5478c-186">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="5478c-187">Создание страниц преподавателей</span><span class="sxs-lookup"><span data-stu-id="5478c-187">Create Instructor pages</span></span>

<span data-ttu-id="5478c-188">В этом разделе формируется шаблон для страниц преподавателей, а затем на страницу указателя преподавателей добавляются связанные курсы и регистрации.</span><span class="sxs-lookup"><span data-stu-id="5478c-188">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="5478c-189">![Страница индекса преподавателей](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="5478c-189">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="5478c-190">Эта страница считывает и отображает связанные данные следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5478c-190">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="5478c-191">Список преподавателей отображает связанные данные из сущности `OfficeAssignment` ("Office" (Кабинет) на предыдущем изображении).</span><span class="sxs-lookup"><span data-stu-id="5478c-191">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="5478c-192">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="5478c-192">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="5478c-193">Безотложная загрузка используется для сущностей `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5478c-193">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="5478c-194">Безотложная загрузка обычно более эффективна, когда требуется отобразить связанные данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-194">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="5478c-195">В этом случае отображается принадлежность к кабинету для каждого преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-195">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="5478c-196">Когда пользователь выбирает преподавателя, отображаются связанные сущности `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-196">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="5478c-197">Между сущностями `Instructor` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="5478c-197">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="5478c-198">Используется безотложная загрузка для сущностей `Course` и связанных сущностей `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-198">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="5478c-199">В этом случае отдельные запросы могут оказаться эффективнее, так как требуются только курсы для выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-199">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="5478c-200">Этот пример показывает, как использовать безотложную загрузку для свойств навигации в сущностях, находящихся в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-200">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="5478c-201">Когда пользователь выбирает курс, отображаются связанные данные из сущности `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="5478c-201">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="5478c-202">На предыдущем изображении отображается имя и оценка учащегося.</span><span class="sxs-lookup"><span data-stu-id="5478c-202">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="5478c-203">Между сущностями `Course` и `Enrollment` действует связь один ко многим.</span><span class="sxs-lookup"><span data-stu-id="5478c-203">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="5478c-204">Создание модели представления</span><span class="sxs-lookup"><span data-stu-id="5478c-204">Create a view model</span></span>

<span data-ttu-id="5478c-205">На странице "Instructors" (Преподаватели) отображаются данные из трех различных таблиц.</span><span class="sxs-lookup"><span data-stu-id="5478c-205">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="5478c-206">Требуется модель представления, которая включает в себя три свойства, представляющие три таблицы.</span><span class="sxs-lookup"><span data-stu-id="5478c-206">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="5478c-207">Создайте файл *SchoolViewModels/InstructorIndexData.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5478c-207">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="5478c-208">Формирование шаблона для страниц преподавателей</span><span class="sxs-lookup"><span data-stu-id="5478c-208">Scaffold Instructor pages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5478c-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5478c-209">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="5478c-210">Следуйте инструкциям в разделе [Формирование шаблона для страниц Student](xref:data/ef-rp/intro#scaffold-student-pages), за исключением следующего:</span><span class="sxs-lookup"><span data-stu-id="5478c-210">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="5478c-211">Создайте папку *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="5478c-211">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="5478c-212">Используйте класс модели `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5478c-212">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="5478c-213">Используйте существующий класс контекста вместо создания нового.</span><span class="sxs-lookup"><span data-stu-id="5478c-213">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5478c-214">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5478c-214">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="5478c-215">Создайте папку *Pages/Instructors*.</span><span class="sxs-lookup"><span data-stu-id="5478c-215">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="5478c-216">Выполните приведенную ниже команду, чтобы сформировать шаблон для страниц преподавателей.</span><span class="sxs-lookup"><span data-stu-id="5478c-216">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="5478c-217">**В Windows:**</span><span class="sxs-lookup"><span data-stu-id="5478c-217">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="5478c-218">**В Linux или macOS:**</span><span class="sxs-lookup"><span data-stu-id="5478c-218">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="5478c-219">Чтобы увидеть, как выглядит шаблонная страница до ее изменения, запустите приложение и перейдите на страницу преподавателей.</span><span class="sxs-lookup"><span data-stu-id="5478c-219">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="5478c-220">Измените файл *Pages/Instructors/Index.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-220">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="5478c-221">Метод `OnGetAsync` принимает необязательные данные маршрутизации для идентификатора выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-221">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="5478c-222">Изучите запрос в файле *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5478c-222">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="5478c-223">В коде задается безотложная загрузка для следующих свойств навигации:</span><span class="sxs-lookup"><span data-stu-id="5478c-223">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="5478c-224">Обратите внимание на то, что методы `Include` и `ThenInclude` повторяются для `CourseAssignments` и `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-224">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="5478c-225">Это необходимо для того, чтобы задать безотложную загрузку для двух свойств навигации сущности `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-225">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="5478c-226">Приведенный ниже код выполняется при выборе преподавателя (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="5478c-226">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="5478c-227">Выбранный преподаватель извлекается из списка преподавателей в модели представления.</span><span class="sxs-lookup"><span data-stu-id="5478c-227">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="5478c-228">Из свойства навигации `CourseAssignments` этого преподавателя загружается свойство модели представления `Courses` вместе с сущностями `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-228">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="5478c-229">Метод `Where` возвращает коллекцию.</span><span class="sxs-lookup"><span data-stu-id="5478c-229">The `Where` method returns a collection.</span></span> <span data-ttu-id="5478c-230">Однако в этом случае фильтр выберет одну сущность.</span><span class="sxs-lookup"><span data-stu-id="5478c-230">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="5478c-231">Поэтому вызывается метод `Single` для преобразования коллекции в отдельную сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5478c-231">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="5478c-232">Сущность `Instructor` предоставляет доступ к свойству `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="5478c-232">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="5478c-233">`CourseAssignments` предоставляет доступ к связанным сущностям `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-233">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="5478c-235">Метод `Single` используется для коллекции, когда она содержит всего один элемент.</span><span class="sxs-lookup"><span data-stu-id="5478c-235">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="5478c-236">Метод `Single` вызывает исключение, если коллекция пуста или содержит больше одного элемента.</span><span class="sxs-lookup"><span data-stu-id="5478c-236">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="5478c-237">Альтернативным вариантом является метод `SingleOrDefault`, который возвращает значение по умолчанию (в данном случае null), если коллекция пуста.</span><span class="sxs-lookup"><span data-stu-id="5478c-237">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="5478c-238">Следующий код заполняет свойство `Enrollments` модели представления при выборе курса:</span><span class="sxs-lookup"><span data-stu-id="5478c-238">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="5478c-239">Изменение страницы индекса преподавателей</span><span class="sxs-lookup"><span data-stu-id="5478c-239">Update the instructors Index page</span></span>

<span data-ttu-id="5478c-240">Измените файл *Pages/Instructors/Index.cshtml*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-240">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="5478c-241">Приведенный выше код вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="5478c-241">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="5478c-242">Изменяет директиву `page` с `@page` на `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="5478c-242">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="5478c-243">`"{id:int?}"` является шаблоном маршрута.</span><span class="sxs-lookup"><span data-stu-id="5478c-243">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="5478c-244">Шаблон маршрута изменяет целочисленные строки запроса в URL-адресе для маршрутизации данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-244">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="5478c-245">Например, при выборе ссылки **Select** для преподавателя только с директивой `@page` формируется URL-адрес следующего вида:</span><span class="sxs-lookup"><span data-stu-id="5478c-245">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="5478c-246">Когда используется директива страницы `@page "{id:int?}"`, URL-адрес имеет следующее значение:</span><span class="sxs-lookup"><span data-stu-id="5478c-246">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="5478c-247">Добавляет столбец **Office**, в котором отображается `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не равно NULL.</span><span class="sxs-lookup"><span data-stu-id="5478c-247">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="5478c-248">Так как это связь один к нулю или одному, то связанная сущность OfficeAssignment может отсутствовать.</span><span class="sxs-lookup"><span data-stu-id="5478c-248">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="5478c-249">Добавляет столбец **Courses**, содержащий курсы, которые ведет конкретный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="5478c-249">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="5478c-250">Подробные сведения об использовании синтаксиса Razor см. в разделе [Явные строки перехода с `@:` ](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="5478c-250">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="5478c-251">Добавляет код, который динамически добавляет `class="success"` к элементу `tr` выбранного преподавателя и курса.</span><span class="sxs-lookup"><span data-stu-id="5478c-251">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="5478c-252">Этот параметр задает цвет фона для выделенных строк c помощью класса Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="5478c-252">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="5478c-253">Добавляет новую гиперссылку с меткой **Select** (Выбрать).</span><span class="sxs-lookup"><span data-stu-id="5478c-253">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="5478c-254">Она отправляет идентификатор выбранного преподавателя в метод `Index` и задает цвет фона.</span><span class="sxs-lookup"><span data-stu-id="5478c-254">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="5478c-255">Добавляет таблицу курсов для выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-255">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="5478c-256">Добавляет таблицу регистраций учащихся для выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="5478c-256">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="5478c-257">Запустите приложение и выберите вкладку **Instructors**. На странице отображается `Location` (кабинет) из связанной сущности `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5478c-257">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="5478c-258">Если `OfficeAssignment` имеет значение NULL, отображается пустая ячейка таблицы.</span><span class="sxs-lookup"><span data-stu-id="5478c-258">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="5478c-259">Щелкните ссылку **Select** для преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-259">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="5478c-260">Стиль строки изменится, и отобразятся курсы, назначенные этому преподавателю.</span><span class="sxs-lookup"><span data-stu-id="5478c-260">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="5478c-261">Выберите курс, чтобы увидеть список зачисленных учащихся и их оценки.</span><span class="sxs-lookup"><span data-stu-id="5478c-261">Select a course to see the list of enrolled students and their grades.</span></span>

![Страница индекса преподавателей, выбраны преподаватель и курс](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="5478c-263">Использование метода Single</span><span class="sxs-lookup"><span data-stu-id="5478c-263">Using Single</span></span>

<span data-ttu-id="5478c-264">Метод `Single` может передать условие `Where` вместо отдельного вызова метода `Where`:</span><span class="sxs-lookup"><span data-stu-id="5478c-264">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="5478c-265">Использование `Single` с условием WHERE — это вопрос личных предпочтений.</span><span class="sxs-lookup"><span data-stu-id="5478c-265">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="5478c-266">Оно не дает преимуществ по сравнению с использованием метода `Where`.</span><span class="sxs-lookup"><span data-stu-id="5478c-266">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="5478c-267">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="5478c-267">Explicit loading</span></span>

<span data-ttu-id="5478c-268">Текущий код указывает упреждающую загрузку для `Enrollments` и `Students`:</span><span class="sxs-lookup"><span data-stu-id="5478c-268">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="5478c-269">Предположим, что пользователям редко требуется просматривать зачисления на курс.</span><span class="sxs-lookup"><span data-stu-id="5478c-269">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="5478c-270">В этом случае можно оптимизировать работу, загружая данные о зачислении только при их запросе.</span><span class="sxs-lookup"><span data-stu-id="5478c-270">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="5478c-271">В этом разделе изменяется `OnGetAsync` для использования явной загрузки `Enrollments` и `Students`.</span><span class="sxs-lookup"><span data-stu-id="5478c-271">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="5478c-272">Измените файл *Pages/Instructors/Index.cshtml.cs*, используя приведенный ниже код.</span><span class="sxs-lookup"><span data-stu-id="5478c-272">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="5478c-273">Предыдущий код удаляет вызовы метода *ThenInclude* для данных о зачислении и учащихся.</span><span class="sxs-lookup"><span data-stu-id="5478c-273">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="5478c-274">Если курс выбран, код явной загрузки извлекает следующее:</span><span class="sxs-lookup"><span data-stu-id="5478c-274">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="5478c-275">Сущности `Enrollment` для выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="5478c-275">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="5478c-276">Сущности `Student` для каждого `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="5478c-276">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="5478c-277">Обратите внимание, что предыдущий код закомментирует `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="5478c-277">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="5478c-278">Для отслеживаемых сущностей свойства навигации можно загрузить лишь явно.</span><span class="sxs-lookup"><span data-stu-id="5478c-278">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="5478c-279">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="5478c-279">Test the app.</span></span> <span data-ttu-id="5478c-280">С точки зрения пользователя приложение работает аналогично предыдущей версии.</span><span class="sxs-lookup"><span data-stu-id="5478c-280">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5478c-281">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="5478c-281">Next steps</span></span>

<span data-ttu-id="5478c-282">Следующее руководство посвящено обновлению связанных данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-282">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="5478c-283">[Предыдущий учебник](xref:data/ef-rp/complex-data-model)
>[Следующий учебник](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="5478c-283">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5478c-284">В этом руководстве выполняется чтение и отображение связанных данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-284">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="5478c-285">Связанными называются данные, которые EF Core загружает в свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-285">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="5478c-286">При возникновении проблем, которые вам не удается устранить, [скачайте или просмотрите готовое приложение.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="5478c-286">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="5478c-287">[Указания по скачиванию](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="5478c-287">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="5478c-288">На следующих рисунках изображены готовые страницы для этого руководства:</span><span class="sxs-lookup"><span data-stu-id="5478c-288">The following illustrations show the completed pages for this tutorial:</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="5478c-291">Безотложная, явная и отложенная загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="5478c-291">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="5478c-292">Существует несколько способов, которыми EF Core может загружать связанные данные в свойства навигации сущности:</span><span class="sxs-lookup"><span data-stu-id="5478c-292">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="5478c-293">[Безотложная загрузка](/ef/core/querying/related-data#eager-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-293">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="5478c-294">Безотложной является загрузка, когда запрос для одного типа сущности также загружает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="5478c-294">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="5478c-295">При чтении сущности связанные данные извлекаются.</span><span class="sxs-lookup"><span data-stu-id="5478c-295">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="5478c-296">Обычно такая загрузка представляет собой одиночный запрос с соединением, который получает все необходимые данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-296">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="5478c-297">Для некоторых типов безотложной загрузки EF Core выдает несколько запросов.</span><span class="sxs-lookup"><span data-stu-id="5478c-297">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="5478c-298">Выдача нескольких запросов может оказаться более эффективной по сравнению с отдельными ситуациями в EF6, когда выполнялся отдельный запрос.</span><span class="sxs-lookup"><span data-stu-id="5478c-298">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="5478c-299">Безотложная загрузка указывается с помощью методов `Include` и `ThenInclude`.</span><span class="sxs-lookup"><span data-stu-id="5478c-299">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![Пример безотложной загрузки](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="5478c-301">Безотложная загрузка отправляет несколько запросов, когда включена навигация коллекции:</span><span class="sxs-lookup"><span data-stu-id="5478c-301">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="5478c-302">Один запрос для основного запроса</span><span class="sxs-lookup"><span data-stu-id="5478c-302">One query for the main query</span></span> 
  * <span data-ttu-id="5478c-303">Один запрос для каждого "края" коллекции в дереве загрузки.</span><span class="sxs-lookup"><span data-stu-id="5478c-303">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="5478c-304">Отдельные запросы с `Load`: данные можно получить в отдельных запросах, а EF Core исправляет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-304">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="5478c-305">Термин "исправляет" означает, что EF Core автоматически заполняет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-305">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="5478c-306">Отдельные запросы с `Load` больше похожи на явную загрузку, чем на безотложную.</span><span class="sxs-lookup"><span data-stu-id="5478c-306">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![Пример отдельных запросов](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="5478c-308">Примечание. EF Core автоматически исправляет свойства навигации для других сущностей, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="5478c-308">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="5478c-309">Даже если данные для свойства навигации *не* включены явно, свойство все равно можно заполнить при условии, что ранее были загружены некоторые или все связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="5478c-309">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="5478c-310">[Явная загрузка](/ef/core/querying/related-data#explicit-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-310">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="5478c-311">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="5478c-311">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5478c-312">Нужно написать код, извлекающий связанные данные, когда они необходимы.</span><span class="sxs-lookup"><span data-stu-id="5478c-312">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="5478c-313">Явная загрузка с отдельными запросами приводит к отправке нескольких запросов к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-313">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="5478c-314">При явной загрузке код указывает, какие свойства навигации нужно загрузить.</span><span class="sxs-lookup"><span data-stu-id="5478c-314">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="5478c-315">Для выполнения явной загрузки используется метод `Load`.</span><span class="sxs-lookup"><span data-stu-id="5478c-315">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="5478c-316">Например:</span><span class="sxs-lookup"><span data-stu-id="5478c-316">For example:</span></span>

  ![Пример явной загрузки](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="5478c-318">[Отложенная загрузка](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-318">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="5478c-319">[В EF Core версии 2.1 добавлена отложенная загрузка](/ef/core/querying/related-data#lazy-loading).</span><span class="sxs-lookup"><span data-stu-id="5478c-319">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="5478c-320">При первом чтении сущности связанные данные не извлекаются.</span><span class="sxs-lookup"><span data-stu-id="5478c-320">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="5478c-321">При первом обращении к свойству навигации необходимые для этого свойства данные извлекаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="5478c-321">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="5478c-322">Запрос к базе данных отправляется при каждом первом обращении к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-322">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="5478c-323">Оператор `Select` загружает только необходимые связанные данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-323">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="5478c-324">Создание страницы "Course" (Курс) с отображением названий кафедр</span><span class="sxs-lookup"><span data-stu-id="5478c-324">Create a Course page that displays department name</span></span>

<span data-ttu-id="5478c-325">Сущность "Course" включает в себя свойство навигации, содержащее сущность `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-325">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="5478c-326">Сущность `Department` содержит кафедру, которой назначен курс.</span><span class="sxs-lookup"><span data-stu-id="5478c-326">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="5478c-327">Отображение имени назначенной кафедры в списке курсов:</span><span class="sxs-lookup"><span data-stu-id="5478c-327">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="5478c-328">Получите свойство `Name` из сущности `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-328">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="5478c-329">Сущность `Department` получается из свойства навигации `Course.Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-329">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="5478c-331">Формирование шаблона для модели Course</span><span class="sxs-lookup"><span data-stu-id="5478c-331">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5478c-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5478c-332">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="5478c-333">Следуйте инструкциям в разделе [Формирование шаблона для модели Student](xref:data/ef-rp/intro#scaffold-the-student-model) и используйте `Course` для класса модели.</span><span class="sxs-lookup"><span data-stu-id="5478c-333">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5478c-334">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5478c-334">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="5478c-335">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5478c-335">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="5478c-336">Предыдущая команда формирует шаблон для модели `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-336">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="5478c-337">Откройте проект в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5478c-337">Open the project in Visual Studio.</span></span>

<span data-ttu-id="5478c-338">Откройте *Pages/Courses/Index.cshtml.cs* и изучите метод `OnGetAsync`.</span><span class="sxs-lookup"><span data-stu-id="5478c-338">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="5478c-339">Подсистема формирования шаблонов указала безотложную загрузку для свойства навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-339">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="5478c-340">Метод `Include` задает безотложную загрузку.</span><span class="sxs-lookup"><span data-stu-id="5478c-340">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="5478c-341">Запустите приложение и выберите ссылку **Courses** (Курсы).</span><span class="sxs-lookup"><span data-stu-id="5478c-341">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="5478c-342">В столбце кафедры отображается бесполезный `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="5478c-342">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="5478c-343">Обновите метод `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-343">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="5478c-344">Предыдущий код добавляет `AsNoTracking`.</span><span class="sxs-lookup"><span data-stu-id="5478c-344">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="5478c-345">`AsNoTracking` повышает производительность, так как возвращаемые сущности не отслеживаются.</span><span class="sxs-lookup"><span data-stu-id="5478c-345">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="5478c-346">Отслеживание отсутствует, так как сущности не изменяются в текущем контексте.</span><span class="sxs-lookup"><span data-stu-id="5478c-346">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="5478c-347">Измените *Pages/Courses/Index.cshtml*, используя выделенную ниже разметку:</span><span class="sxs-lookup"><span data-stu-id="5478c-347">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="5478c-348">В шаблонный код были внесены следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="5478c-348">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="5478c-349">Изменен заголовок с "Index" (Индекс) на "Courses" (Курсы).</span><span class="sxs-lookup"><span data-stu-id="5478c-349">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="5478c-350">Добавлен столбец **Number** (Номер), отображающий значение свойства `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="5478c-350">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="5478c-351">По умолчанию в шаблоне отсутствуют первичные ключи, поскольку для конечных пользователей они не имеют смысла.</span><span class="sxs-lookup"><span data-stu-id="5478c-351">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="5478c-352">Однако в нашем случае первичный ключ имеет смысл.</span><span class="sxs-lookup"><span data-stu-id="5478c-352">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="5478c-353">Изменен столбец **Department** (Кафедра) для отображения названия кафедры.</span><span class="sxs-lookup"><span data-stu-id="5478c-353">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="5478c-354">Код отображает свойство `Name` сущности `Department`, которая загружена в свойство навигации `Department`:</span><span class="sxs-lookup"><span data-stu-id="5478c-354">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="5478c-355">Для просмотра списка с названиями кафедр запустите приложение и выберите вкладку **Courses** (Курсы).</span><span class="sxs-lookup"><span data-stu-id="5478c-355">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Страница индекса курсов](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="5478c-357">Загрузка связанных данных с помощью "Select"</span><span class="sxs-lookup"><span data-stu-id="5478c-357">Loading related data with Select</span></span>

<span data-ttu-id="5478c-358">Метод `OnGetAsync` загружает связанные данные с помощью метода `Include`:</span><span class="sxs-lookup"><span data-stu-id="5478c-358">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="5478c-359">Оператор `Select` загружает только необходимые связанные данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-359">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="5478c-360">Для отдельных элементов, таких как `Department.Name`, используется SQL INNER JOIN.</span><span class="sxs-lookup"><span data-stu-id="5478c-360">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="5478c-361">Для коллекций используется доступ к другой базе данных, но это же делает и оператор `Include` в коллекциях.</span><span class="sxs-lookup"><span data-stu-id="5478c-361">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="5478c-362">Следующий код загружает связанные данные с помощью метода `Select`:</span><span class="sxs-lookup"><span data-stu-id="5478c-362">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="5478c-363">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="5478c-363">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="5478c-364">Полный пример см. в описании [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) и [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs).</span><span class="sxs-lookup"><span data-stu-id="5478c-364">See [IndexSelect.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="5478c-365">Создание страницы "Instructors" (Преподаватели) с отображением курсов и дат зачисления</span><span class="sxs-lookup"><span data-stu-id="5478c-365">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="5478c-366">В этом разделе создается страница "Instructors" (Преподаватели).</span><span class="sxs-lookup"><span data-stu-id="5478c-366">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="5478c-367">![Страница индекса преподавателей](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="5478c-367">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="5478c-368">Эта страница считывает и отображает связанные данные следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5478c-368">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="5478c-369">Список преподавателей отображает связанные данные из сущности `OfficeAssignment` ("Office" (Кабинет) на предыдущем изображении).</span><span class="sxs-lookup"><span data-stu-id="5478c-369">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="5478c-370">Между сущностями `Instructor` и `OfficeAssignment` действует связь один к нулю или к одному.</span><span class="sxs-lookup"><span data-stu-id="5478c-370">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="5478c-371">Безотложная загрузка используется для сущностей `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5478c-371">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="5478c-372">Безотложная загрузка обычно более эффективна, когда требуется отобразить связанные данные.</span><span class="sxs-lookup"><span data-stu-id="5478c-372">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="5478c-373">В этом случае отображается принадлежность к кабинету для каждого преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-373">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="5478c-374">Когда пользователь выбирает преподавателя (Harui на предыдущем изображении), отображаются связанные сущности `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-374">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="5478c-375">Между сущностями `Instructor` и `Course` действует связь многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="5478c-375">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="5478c-376">Используется безотложная загрузка для сущностей `Course` и связанных сущностей `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-376">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="5478c-377">В этом случае отдельные запросы могут оказаться эффективнее, так как требуются только курсы для выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-377">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="5478c-378">Этот пример показывает, как использовать безотложную загрузку для свойств навигации в сущностях, находящихся в свойствах навигации.</span><span class="sxs-lookup"><span data-stu-id="5478c-378">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="5478c-379">Когда пользователь выбирает курс ("Chemistry" (Химия) на предыдущем изображении), отображаются связанные данные из сущности `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="5478c-379">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="5478c-380">На предыдущем изображении отображается имя и оценка учащегося.</span><span class="sxs-lookup"><span data-stu-id="5478c-380">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="5478c-381">Между сущностями `Course` и `Enrollment` действует связь один ко многим.</span><span class="sxs-lookup"><span data-stu-id="5478c-381">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="5478c-382">Создание модели для представления индекса преподавателей</span><span class="sxs-lookup"><span data-stu-id="5478c-382">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="5478c-383">На странице "Instructors" (Преподаватели) отображаются данные из трех различных таблиц.</span><span class="sxs-lookup"><span data-stu-id="5478c-383">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="5478c-384">Создается модель представления, которая включает три сущности, представляющие три таблицы.</span><span class="sxs-lookup"><span data-stu-id="5478c-384">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="5478c-385">Создайте в папке *SchoolViewModels* файл *InstructorIndexData.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="5478c-385">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="5478c-386">Формирование шаблона для модели "Instructor" (Преподаватель)</span><span class="sxs-lookup"><span data-stu-id="5478c-386">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5478c-387">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5478c-387">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="5478c-388">Следуйте инструкциям в разделе [Формирование шаблона для модели Student](xref:data/ef-rp/intro#scaffold-the-student-model) и используйте `Instructor` для класса модели.</span><span class="sxs-lookup"><span data-stu-id="5478c-388">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5478c-389">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="5478c-389">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="5478c-390">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="5478c-390">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="5478c-391">Предыдущая команда формирует шаблон для модели `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5478c-391">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="5478c-392">Запустите приложение и перейдите на страницу преподавателей.</span><span class="sxs-lookup"><span data-stu-id="5478c-392">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="5478c-393">Замените содержимое *Pages/Instructors/Index.cshtml.cs* на следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-393">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="5478c-394">Метод `OnGetAsync` принимает необязательные данные маршрутизации для идентификатора выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-394">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="5478c-395">Изучите запрос в файле *Pages/Instructors/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5478c-395">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="5478c-396">Запрос имеет две операции включения:</span><span class="sxs-lookup"><span data-stu-id="5478c-396">The query has two includes:</span></span>

* <span data-ttu-id="5478c-397">`OfficeAssignment`: отображается в [представлении преподавателей](#IP).</span><span class="sxs-lookup"><span data-stu-id="5478c-397">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="5478c-398">`CourseAssignments`: вызывает проводимые курсы.</span><span class="sxs-lookup"><span data-stu-id="5478c-398">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="5478c-399">Изменение страницы индекса преподавателей</span><span class="sxs-lookup"><span data-stu-id="5478c-399">Update the instructors Index page</span></span>

<span data-ttu-id="5478c-400">Измените *Pages/Instructors/Index.cshtml*, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="5478c-400">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="5478c-401">Приведенная выше разметка вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="5478c-401">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="5478c-402">Изменяет директиву `page` с `@page` на `@page "{id:int?}"`.</span><span class="sxs-lookup"><span data-stu-id="5478c-402">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="5478c-403">`"{id:int?}"` является шаблоном маршрута.</span><span class="sxs-lookup"><span data-stu-id="5478c-403">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="5478c-404">Шаблон маршрута изменяет целочисленные строки запроса в URL-адресе для маршрутизации данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-404">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="5478c-405">Например, при выборе ссылки **Select** для преподавателя только с директивой `@page` формируется URL-адрес следующего вида:</span><span class="sxs-lookup"><span data-stu-id="5478c-405">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="5478c-406">Когда используется директива страницы `@page "{id:int?}"`, предыдущий URL-адрес имеет следующее значение:</span><span class="sxs-lookup"><span data-stu-id="5478c-406">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="5478c-407">Заголовком страницы является **Instructors**.</span><span class="sxs-lookup"><span data-stu-id="5478c-407">Page title is **Instructors**.</span></span>
* <span data-ttu-id="5478c-408">Добавили столбец **Office**, отображающий `item.OfficeAssignment.Location` только тогда, когда `item.OfficeAssignment` не равно null.</span><span class="sxs-lookup"><span data-stu-id="5478c-408">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="5478c-409">Так как это связь один к нулю или одному, то связанная сущность OfficeAssignment может отсутствовать.</span><span class="sxs-lookup"><span data-stu-id="5478c-409">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="5478c-410">Добавили столбец **Courses**, отображающий курсы, которые ведет конкретный преподаватель.</span><span class="sxs-lookup"><span data-stu-id="5478c-410">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="5478c-411">Подробные сведения об использовании синтаксиса Razor см. в разделе [Явный перенос строки с помощью `@:`](xref:mvc/views/razor#explicit-line-transition-with-).</span><span class="sxs-lookup"><span data-stu-id="5478c-411">See [Explicit line transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="5478c-412">Добавили код, который динамически добавляет `class="success"` к элементу `tr` выбранного преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-412">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="5478c-413">Этот параметр задает цвет фона для выделенных строк c помощью класса Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="5478c-413">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="5478c-414">Добавлена новая гиперссылка с меткой **Select** (Выбрать).</span><span class="sxs-lookup"><span data-stu-id="5478c-414">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="5478c-415">Она отправляет идентификатор выбранного преподавателя в метод `Index` и задает цвет фона.</span><span class="sxs-lookup"><span data-stu-id="5478c-415">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="5478c-416">Запустите приложение и выберите вкладку **Instructors**. На странице отображается `Location` (кабинет) из связанной сущности `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="5478c-416">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="5478c-417">Если OfficeAssignment имеет значение null, отображается пустая ячейка таблицы.</span><span class="sxs-lookup"><span data-stu-id="5478c-417">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="5478c-418">Щелкните ссылку **Select** (Выбрать).</span><span class="sxs-lookup"><span data-stu-id="5478c-418">Click on the **Select** link.</span></span> <span data-ttu-id="5478c-419">Изменяется стиль строки.</span><span class="sxs-lookup"><span data-stu-id="5478c-419">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="5478c-420">Добавление курсов, проводимых выбранным преподавателем</span><span class="sxs-lookup"><span data-stu-id="5478c-420">Add courses taught by selected instructor</span></span>

<span data-ttu-id="5478c-421">Измените метод `OnGetAsync` в *Pages/Instructors/Index.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-421">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="5478c-422">Добавление `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="5478c-422">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="5478c-423">Проверьте измененный запрос:</span><span class="sxs-lookup"><span data-stu-id="5478c-423">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="5478c-424">Предыдущий запрос добавляет сущности `Department`.</span><span class="sxs-lookup"><span data-stu-id="5478c-424">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="5478c-425">Приведенный ниже код выполняется при выборе преподавателя (`id != null`).</span><span class="sxs-lookup"><span data-stu-id="5478c-425">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="5478c-426">Выбранный преподаватель извлекается из списка преподавателей в модели представления.</span><span class="sxs-lookup"><span data-stu-id="5478c-426">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="5478c-427">Из свойства навигации `CourseAssignments` этого преподавателя загружается свойство модели представления `Courses` вместе с сущностями `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-427">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="5478c-428">Метод `Where` возвращает коллекцию.</span><span class="sxs-lookup"><span data-stu-id="5478c-428">The `Where` method returns a collection.</span></span> <span data-ttu-id="5478c-429">В предыдущем методе `Where` возвращается всего одна сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5478c-429">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="5478c-430">Метод `Single` преобразует коллекцию в отдельную сущность `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="5478c-430">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="5478c-431">Сущность `Instructor` предоставляет доступ к свойству `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="5478c-431">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="5478c-432">`CourseAssignments` предоставляет доступ к связанным сущностям `Course`.</span><span class="sxs-lookup"><span data-stu-id="5478c-432">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![Связь многие ко многим между Instructor и Courses](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="5478c-434">Метод `Single` используется для коллекции, когда она содержит всего один элемент.</span><span class="sxs-lookup"><span data-stu-id="5478c-434">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="5478c-435">Метод `Single` вызывает исключение, если коллекция пуста или содержит больше одного элемента.</span><span class="sxs-lookup"><span data-stu-id="5478c-435">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="5478c-436">Альтернативным вариантом является метод `SingleOrDefault`, который возвращает значение по умолчанию (в данном случае null), если коллекция пуста.</span><span class="sxs-lookup"><span data-stu-id="5478c-436">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="5478c-437">Использование `SingleOrDefault` для пустой коллекции:</span><span class="sxs-lookup"><span data-stu-id="5478c-437">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="5478c-438">Возникает исключение (из-за попытки найти свойство `Courses` по пустой ссылке).</span><span class="sxs-lookup"><span data-stu-id="5478c-438">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="5478c-439">Сообщение об исключении не так четко указывает на причину проблемы.</span><span class="sxs-lookup"><span data-stu-id="5478c-439">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="5478c-440">Следующий код заполняет свойство `Enrollments` модели представления при выборе курса:</span><span class="sxs-lookup"><span data-stu-id="5478c-440">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="5478c-441">Добавьте следующую разметку в конец страницы Razor *Pages/Instructors/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5478c-441">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="5478c-442">Предыдущая разметка отображает список связанных с преподавателем курсов при выборе преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-442">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="5478c-443">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="5478c-443">Test the app.</span></span> <span data-ttu-id="5478c-444">Щелкните ссылку **Select** (Выбрать) на странице преподавателей.</span><span class="sxs-lookup"><span data-stu-id="5478c-444">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="5478c-445">Отображение данных об учащихся</span><span class="sxs-lookup"><span data-stu-id="5478c-445">Show student data</span></span>

<span data-ttu-id="5478c-446">В этом разделе приложение изменяется, чтобы отобразить данные об учащихся для выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="5478c-446">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="5478c-447">Измените запрос в методе `OnGetAsync` в *Pages/Instructors/Index.cshtml.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-447">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="5478c-448">Измените *Pages/Instructors/Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="5478c-448">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="5478c-449">Добавьте следующую разметку в конец файла:</span><span class="sxs-lookup"><span data-stu-id="5478c-449">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="5478c-450">Предыдущая разметка отображает список учащихся, которые зачислены на курс.</span><span class="sxs-lookup"><span data-stu-id="5478c-450">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="5478c-451">Обновите страницу и выберите преподавателя.</span><span class="sxs-lookup"><span data-stu-id="5478c-451">Refresh the page and select an instructor.</span></span> <span data-ttu-id="5478c-452">Выберите курс, чтобы увидеть список зачисленных учащихся и их оценки.</span><span class="sxs-lookup"><span data-stu-id="5478c-452">Select a course to see the list of enrolled students and their grades.</span></span>

![Страница индекса преподавателей, выбраны преподаватель и курс](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="5478c-454">Использование метода Single</span><span class="sxs-lookup"><span data-stu-id="5478c-454">Using Single</span></span>

<span data-ttu-id="5478c-455">Метод `Single` может передать условие `Where` вместо отдельного вызова метода `Where`:</span><span class="sxs-lookup"><span data-stu-id="5478c-455">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="5478c-456">Приведенный выше подход на основе `Single` не дает преимуществ по сравнению с использованием `Where`.</span><span class="sxs-lookup"><span data-stu-id="5478c-456">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="5478c-457">Некоторые разработчики предпочитают использовать подход `Single`.</span><span class="sxs-lookup"><span data-stu-id="5478c-457">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="5478c-458">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="5478c-458">Explicit loading</span></span>

<span data-ttu-id="5478c-459">Текущий код указывает упреждающую загрузку для `Enrollments` и `Students`:</span><span class="sxs-lookup"><span data-stu-id="5478c-459">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="5478c-460">Предположим, что пользователям редко требуется просматривать зачисления на курс.</span><span class="sxs-lookup"><span data-stu-id="5478c-460">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="5478c-461">В этом случае можно оптимизировать работу, загружая данные о зачислении только при их запросе.</span><span class="sxs-lookup"><span data-stu-id="5478c-461">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="5478c-462">В этом разделе изменяется `OnGetAsync` для использования явной загрузки `Enrollments` и `Students`.</span><span class="sxs-lookup"><span data-stu-id="5478c-462">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="5478c-463">Измените `OnGetAsync`, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="5478c-463">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="5478c-464">Предыдущий код удаляет вызовы метода *ThenInclude* для данных о зачислении и учащихся.</span><span class="sxs-lookup"><span data-stu-id="5478c-464">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="5478c-465">Если курс выбран, выделенный код извлекает следующее:</span><span class="sxs-lookup"><span data-stu-id="5478c-465">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="5478c-466">Сущности `Enrollment` для выбранного курса.</span><span class="sxs-lookup"><span data-stu-id="5478c-466">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="5478c-467">Сущности `Student` для каждого `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="5478c-467">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="5478c-468">Обратите внимание, что предыдущий код закомментирует `.AsNoTracking()`.</span><span class="sxs-lookup"><span data-stu-id="5478c-468">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="5478c-469">Для отслеживаемых сущностей свойства навигации можно загрузить лишь явно.</span><span class="sxs-lookup"><span data-stu-id="5478c-469">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="5478c-470">Проверьте работу приложения.</span><span class="sxs-lookup"><span data-stu-id="5478c-470">Test the app.</span></span> <span data-ttu-id="5478c-471">С точки зрения пользователей приложение работает аналогично предыдущей версии.</span><span class="sxs-lookup"><span data-stu-id="5478c-471">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="5478c-472">Следующее руководство посвящено обновлению связанных данных.</span><span class="sxs-lookup"><span data-stu-id="5478c-472">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5478c-473">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5478c-473">Additional resources</span></span>

* [<span data-ttu-id="5478c-474">Версия руководства на YouTube (часть 1)</span><span class="sxs-lookup"><span data-stu-id="5478c-474">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="5478c-475">Версия руководства на YouTube (часть 2)</span><span class="sxs-lookup"><span data-stu-id="5478c-475">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="5478c-476">[Назад](xref:data/ef-rp/complex-data-model)
>[Вперед](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="5478c-476">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end
