---
title: "Страниц Razor с основными EF - обновления связанных данных - 7, 8."
author: rick-anderson
description: "В этом учебнике будет обновить связанные данные, обновив внешних ключевых полей и свойств навигации."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 817bfd48dce94e7dbad96cb6f822494e3adfae1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a><span data-ttu-id="cfac2-103">Обновление связанных данных - страниц Razor EF Core (7, 8)</span><span class="sxs-lookup"><span data-stu-id="cfac2-103">Updating related data - EF Core Razor Pages (7 of 8)</span></span>

<span data-ttu-id="cfac2-104">По [Tom Dykstra](https://github.com/tdykstra), и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cfac2-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="cfac2-105">В этом учебнике показано обновление связанных данных.</span><span class="sxs-lookup"><span data-stu-id="cfac2-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="cfac2-106">Если возникли проблемы, не удается устранить, загрузите [завершенное приложение для данного этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span><span class="sxs-lookup"><span data-stu-id="cfac2-106">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).</span></span>

<span data-ttu-id="cfac2-107">На следующих рисунках показаны некоторые завершенной страницы.</span><span class="sxs-lookup"><span data-stu-id="cfac2-107">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="cfac2-108">![Страница изменения курса](update-related-data/_static/course-edit.png)
![инструктора изменения страницы](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="cfac2-108">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="cfac2-109">Проверьте и тестирование Создание и редактирование страниц курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-109">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="cfac2-110">Создание нового курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-110">Create a new course.</span></span> <span data-ttu-id="cfac2-111">Подразделение выбирается по первичному ключу (целое число), не его имя.</span><span class="sxs-lookup"><span data-stu-id="cfac2-111">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="cfac2-112">Изменение нового курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-112">Edit the new course.</span></span> <span data-ttu-id="cfac2-113">После завершения тестирования удалите нового курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-113">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="cfac2-114">Создание базового класса для совместного использования кода</span><span class="sxs-lookup"><span data-stu-id="cfac2-114">Create a base class to share common code</span></span>

<span data-ttu-id="cfac2-115">Курсы/Create курсы или изменение страницы и каждого требуется список названий отделов.</span><span class="sxs-lookup"><span data-stu-id="cfac2-115">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="cfac2-116">Создание *Pages/Courses/DepartmentNamePageModel.cshtml.cs* базовый класс для создания и редактирования страниц:</span><span class="sxs-lookup"><span data-stu-id="cfac2-116">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="cfac2-117">Приведенный выше код создает [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) будет содержать список имен отдела.</span><span class="sxs-lookup"><span data-stu-id="cfac2-117">The preceding code creates a [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="cfac2-118">Если `selectedDepartment` указан, этот отдел выбран в `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="cfac2-118">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="cfac2-119">Создание и изменение классы модели страницы, производной от `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="cfac2-119">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="cfac2-120">Настройка страниц курсов</span><span class="sxs-lookup"><span data-stu-id="cfac2-120">Customize the Courses Pages</span></span>

<span data-ttu-id="cfac2-121">При создании новой сущности курса, он должен иметь отношение к отдела.</span><span class="sxs-lookup"><span data-stu-id="cfac2-121">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="cfac2-122">Чтобы добавить подразделение при создании курса, базовый класс для создания и редактирования содержит раскрывающегося списка для выбора подразделения.</span><span class="sxs-lookup"><span data-stu-id="cfac2-122">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="cfac2-123">В раскрывающемся списке устанавливается `Course.DepartmentID` свойство внешний ключ (FK).</span><span class="sxs-lookup"><span data-stu-id="cfac2-123">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="cfac2-124">EF Core использует `Course.DepartmentID` внешнего ключа для загрузки `Department` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="cfac2-124">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Создание курса](update-related-data/_static/ddl.png)

<span data-ttu-id="cfac2-126">Обновление создать модель страницы с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="cfac2-126">Update the Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

<span data-ttu-id="cfac2-127">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="cfac2-127">The preceding code:</span></span>

* <span data-ttu-id="cfac2-128">Происходит от `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="cfac2-128">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="cfac2-129">Использует `TryUpdateModelAsync` для предотвращения [оверпостинга](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="cfac2-129">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="cfac2-130">Заменяет `ViewData["DepartmentID"]` с `DepartmentNameSL` (от базового класса).</span><span class="sxs-lookup"><span data-stu-id="cfac2-130">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="cfac2-131">`ViewData["DepartmentID"]`заменяется со строгой типизацией `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="cfac2-131">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="cfac2-132">Строго типизированные моделей предпочтительные через слабо типизированным.</span><span class="sxs-lookup"><span data-stu-id="cfac2-132">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="cfac2-133">Дополнительные сведения см. в разделе [слабо типизированных данных (ViewData и ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="cfac2-133">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="cfac2-134">Страница «обновление» создайте курсов</span><span class="sxs-lookup"><span data-stu-id="cfac2-134">Update the Courses Create page</span></span>

<span data-ttu-id="cfac2-135">Обновление *Pages/Courses/Create.cshtml* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="cfac2-135">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="cfac2-136">Предыдущей разметки вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="cfac2-136">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="cfac2-137">Изменяет заголовок из **DepartmentID** для **отдел**.</span><span class="sxs-lookup"><span data-stu-id="cfac2-137">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="cfac2-138">Заменяет `"ViewBag.DepartmentID"` с `DepartmentNameSL` (от базового класса).</span><span class="sxs-lookup"><span data-stu-id="cfac2-138">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="cfac2-139">Добавлен параметр «Выберите отдела».</span><span class="sxs-lookup"><span data-stu-id="cfac2-139">Adds the "Select Department" option.</span></span> <span data-ttu-id="cfac2-140">Это изменение отображает «Select отдела» вместо первого отдела.</span><span class="sxs-lookup"><span data-stu-id="cfac2-140">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="cfac2-141">Добавляет сообщение проверки, если отдел не выбран.</span><span class="sxs-lookup"><span data-stu-id="cfac2-141">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="cfac2-142">На странице Razor используется [выберите тег вспомогательный](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="cfac2-142">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="cfac2-143">Проверьте страницы создания.</span><span class="sxs-lookup"><span data-stu-id="cfac2-143">Test the Create page.</span></span> <span data-ttu-id="cfac2-144">На странице создания имеется название отдела, а не идентификатор отдела.</span><span class="sxs-lookup"><span data-stu-id="cfac2-144">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="cfac2-145">Страница «обновление» изменить курсы.</span><span class="sxs-lookup"><span data-stu-id="cfac2-145">Update the Courses Edit page.</span></span>

<span data-ttu-id="cfac2-146">Обновление модели редактирования страниц с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="cfac2-146">Update the edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

<span data-ttu-id="cfac2-147">Изменения, аналогичны внесенные в модель создания страницы.</span><span class="sxs-lookup"><span data-stu-id="cfac2-147">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="cfac2-148">В приведенном выше коде `PopulateDepartmentsDropDownList` пройден в Идентификаторе отдела, что подразделение, указанное в раскрывающемся списке выберите.</span><span class="sxs-lookup"><span data-stu-id="cfac2-148">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="cfac2-149">Обновление *Pages/Courses/Edit.cshtml* следующей разметкой:</span><span class="sxs-lookup"><span data-stu-id="cfac2-149">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="cfac2-150">Предыдущей разметки вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="cfac2-150">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="cfac2-151">Отображает идентификатор курса</span><span class="sxs-lookup"><span data-stu-id="cfac2-151">Displays the course ID.</span></span> <span data-ttu-id="cfac2-152">Обычно первичный ключ (PK) сущности не отображаются.</span><span class="sxs-lookup"><span data-stu-id="cfac2-152">Generally the Primary Key (PK) of an entity is not displayed.</span></span> <span data-ttu-id="cfac2-153">Первичные ключи не обычно имеют смысла для пользователей.</span><span class="sxs-lookup"><span data-stu-id="cfac2-153">PKs are usually meaningless to users.</span></span> <span data-ttu-id="cfac2-154">В этом случае первичного ключа является номером курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-154">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="cfac2-155">Изменяет заголовок из **DepartmentID** для **отдел**.</span><span class="sxs-lookup"><span data-stu-id="cfac2-155">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="cfac2-156">Заменяет `"ViewBag.DepartmentID"` с `DepartmentNameSL` (от базового класса).</span><span class="sxs-lookup"><span data-stu-id="cfac2-156">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="cfac2-157">Добавлен параметр «Выберите отдела».</span><span class="sxs-lookup"><span data-stu-id="cfac2-157">Adds the "Select Department" option.</span></span> <span data-ttu-id="cfac2-158">Это изменение отображает «Select отдела» вместо первого отдела.</span><span class="sxs-lookup"><span data-stu-id="cfac2-158">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="cfac2-159">Добавляет сообщение проверки, если отдел не выбран.</span><span class="sxs-lookup"><span data-stu-id="cfac2-159">Adds a validation message when the department is not selected.</span></span>

<span data-ttu-id="cfac2-160">На этой странице содержатся скрытое поле (`<input type="hidden">`) для номера курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-160">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="cfac2-161">Добавление `<label>` тег вспомогательное приложение с помощью `asp-for="Course.CourseID"` не устраняют необходимость в скрытое поле.</span><span class="sxs-lookup"><span data-stu-id="cfac2-161">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="cfac2-162">`<input type="hidden">`требуется для должны быть включены в данные, когда пользователь щелкает номер **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="cfac2-162">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="cfac2-163">Проверьте обновленный код.</span><span class="sxs-lookup"><span data-stu-id="cfac2-163">Test the updated code.</span></span> <span data-ttu-id="cfac2-164">Создание, изменение и удаление курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-164">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="cfac2-165">Добавьте сведения об AsNoTracking и удаление моделей страницы</span><span class="sxs-lookup"><span data-stu-id="cfac2-165">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="cfac2-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) может повысить производительность, если трассировка не требуется.</span><span class="sxs-lookup"><span data-stu-id="cfac2-166">[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking is not required.</span></span> <span data-ttu-id="cfac2-167">Добавить `AsNoTracking` Delete и сведения о модели страницы.</span><span class="sxs-lookup"><span data-stu-id="cfac2-167">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="cfac2-168">В следующем коде показано обновленной модели страницы Delete:</span><span class="sxs-lookup"><span data-stu-id="cfac2-168">The following code shows the updated Delete page model:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="cfac2-169">Обновление `OnGetAsync` метод в *Pages/Courses/Details.cshtml.cs* файла:</span><span class="sxs-lookup"><span data-stu-id="cfac2-169">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="cfac2-170">Изменять страницы, Delete и подробные сведения</span><span class="sxs-lookup"><span data-stu-id="cfac2-170">Modify the Delete and Details pages</span></span>

<span data-ttu-id="cfac2-171">Обновление страницы Razor удалить следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="cfac2-171">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="cfac2-172">Внесения изменений на страницу сведений.</span><span class="sxs-lookup"><span data-stu-id="cfac2-172">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="cfac2-173">Тестирование страниц курса</span><span class="sxs-lookup"><span data-stu-id="cfac2-173">Test the Course pages</span></span>

<span data-ttu-id="cfac2-174">Тест создать, изменить сведения и удалить.</span><span class="sxs-lookup"><span data-stu-id="cfac2-174">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="cfac2-175">Обновление страниц инструктора</span><span class="sxs-lookup"><span data-stu-id="cfac2-175">Update the instructor pages</span></span>

<span data-ttu-id="cfac2-176">Следующие разделы обновить инструктора страницы.</span><span class="sxs-lookup"><span data-stu-id="cfac2-176">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="cfac2-177">Добавить расположение office</span><span class="sxs-lookup"><span data-stu-id="cfac2-177">Add office location</span></span>

<span data-ttu-id="cfac2-178">При изменении записи инструктора, может потребоваться обновить назначение инструктора office.</span><span class="sxs-lookup"><span data-stu-id="cfac2-178">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="cfac2-179">`Instructor` Сущность имеет отношение "один к нулю или одному" с `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="cfac2-179">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="cfac2-180">Код инструктора должен обрабатывать:</span><span class="sxs-lookup"><span data-stu-id="cfac2-180">The instructor code must handle:</span></span>

* <span data-ttu-id="cfac2-181">Если пользователь удаляет назначение office, удалите `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="cfac2-181">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="cfac2-182">Если пользователь вводит назначения office, и он был пуст, создайте новый `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="cfac2-182">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="cfac2-183">Если пользователь изменяет office назначения, обновить `OfficeAssignment` сущности.</span><span class="sxs-lookup"><span data-stu-id="cfac2-183">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="cfac2-184">Обновление модели страницы инструкторов изменить следующий код:</span><span class="sxs-lookup"><span data-stu-id="cfac2-184">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

<span data-ttu-id="cfac2-185">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="cfac2-185">The preceding code:</span></span>

- <span data-ttu-id="cfac2-186">Возвращает текущий `Instructor` сущностей из базы данных, используя упреждающую для `OfficeAssignment` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="cfac2-186">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
- <span data-ttu-id="cfac2-187">Обновляет извлеченного `Instructor` сущности значениями из связывателя модели.</span><span class="sxs-lookup"><span data-stu-id="cfac2-187">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="cfac2-188">`TryUpdateModel`предотвращает [оверпостинга](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="cfac2-188">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
- <span data-ttu-id="cfac2-189">Если отсутствует в расположении офиса, задает `Instructor.OfficeAssignment` значение null.</span><span class="sxs-lookup"><span data-stu-id="cfac2-189">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="cfac2-190">Когда `Instructor.OfficeAssignment` равен null, связанной строки в `OfficeAssignment` удалить таблицу.</span><span class="sxs-lookup"><span data-stu-id="cfac2-190">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="cfac2-191">Обновление страницы правки инструктора</span><span class="sxs-lookup"><span data-stu-id="cfac2-191">Update the instructor Edit page</span></span>

<span data-ttu-id="cfac2-192">Обновление *Pages/Instructors/Edit.cshtml* с расположением office:</span><span class="sxs-lookup"><span data-stu-id="cfac2-192">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="cfac2-193">Убедитесь, что можно изменить расположение инструкторов office.</span><span class="sxs-lookup"><span data-stu-id="cfac2-193">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="cfac2-194">Добавление назначения курса инструктора страницы правки</span><span class="sxs-lookup"><span data-stu-id="cfac2-194">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="cfac2-195">Преподаватели могут обучить любое количество курсов.</span><span class="sxs-lookup"><span data-stu-id="cfac2-195">Instructors may teach any number of courses.</span></span> <span data-ttu-id="cfac2-196">В этом разделе добавьте возможность изменения курса назначения.</span><span class="sxs-lookup"><span data-stu-id="cfac2-196">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="cfac2-197">Ниже приведен инструктора обновленные страницы правки:</span><span class="sxs-lookup"><span data-stu-id="cfac2-197">The following image shows the updated instructor Edit page:</span></span>

![Страница изменения инструктора курсы](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="cfac2-199">`Course`и `Instructor` имеет отношение многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="cfac2-199">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="cfac2-200">Чтобы добавить и удалить связи, можно добавлять и удалять сущности из `CourseAssignments` join набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="cfac2-200">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="cfac2-201">Флажки производить изменения курсов, присвоенного инструктор.</span><span class="sxs-lookup"><span data-stu-id="cfac2-201">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="cfac2-202">Флажок отображается для каждого курса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cfac2-202">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="cfac2-203">Курсы, которые назначены инструктора проверяются.</span><span class="sxs-lookup"><span data-stu-id="cfac2-203">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="cfac2-204">Пользователь может установите или снимите флажки, чтобы изменить назначения курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-204">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="cfac2-205">Если количество курсов были гораздо выше:</span><span class="sxs-lookup"><span data-stu-id="cfac2-205">If the number of courses were much greater:</span></span>

* <span data-ttu-id="cfac2-206">Возможно, используется другой пользовательский интерфейс для отображения курсов.</span><span class="sxs-lookup"><span data-stu-id="cfac2-206">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="cfac2-207">Метод управления сущности объединения можно создавать и удалять связи не будут изменяться.</span><span class="sxs-lookup"><span data-stu-id="cfac2-207">The method of manipulating a join entity to create or delete relationships would not change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="cfac2-208">Добавлять классы для поддержки создания и редактирования страниц инструктора</span><span class="sxs-lookup"><span data-stu-id="cfac2-208">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="cfac2-209">Создание *SchoolViewModels/AssignedCourseData.cs* следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="cfac2-209">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="cfac2-210">`AssignedCourseData` Класс содержит данные, создавать флажки для назначенного курсов по инструктор.</span><span class="sxs-lookup"><span data-stu-id="cfac2-210">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="cfac2-211">Создание *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* базового класса:</span><span class="sxs-lookup"><span data-stu-id="cfac2-211">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="cfac2-212">`InstructorCoursesPageModel` Является базовым классом, будет использоваться для изменения и создавать модели страницы.</span><span class="sxs-lookup"><span data-stu-id="cfac2-212">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="cfac2-213">`PopulateAssignedCourseData`Считывает все `Course` сущностей для заполнения `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="cfac2-213">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="cfac2-214">Для каждого курса код задает `CourseID`, заголовок и ли инструктора назначается курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-214">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="cfac2-215">Объект [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) используется для создания эффективного поиска.</span><span class="sxs-lookup"><span data-stu-id="cfac2-215">A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="cfac2-216">Модель страницы редактирование инструкторов</span><span class="sxs-lookup"><span data-stu-id="cfac2-216">Instructors Edit page model</span></span>

<span data-ttu-id="cfac2-217">Обновление модели страницы инструктора изменить следующий код:</span><span class="sxs-lookup"><span data-stu-id="cfac2-217">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

<span data-ttu-id="cfac2-218">Предыдущий код обрабатывает изменения в назначения office.</span><span class="sxs-lookup"><span data-stu-id="cfac2-218">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="cfac2-219">Обновление представления Razor инструктора:</span><span class="sxs-lookup"><span data-stu-id="cfac2-219">Update the instructor Razor View:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="cfac2-220">При вставке кода в Visual Studio способом, нарушающим код изменяются разрывы строки.</span><span class="sxs-lookup"><span data-stu-id="cfac2-220">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="cfac2-221">Нажмите один раз сочетание клавиш Ctrl + Z для отмены автоматического форматирования.</span><span class="sxs-lookup"><span data-stu-id="cfac2-221">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="cfac2-222">CTRL + Z устраняет разрывы строки, чтобы они выглядят как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="cfac2-222">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="cfac2-223">Отступы не должен быть идеальным, но `@</tr><tr>`, `@:<td>`, `@:</td>`, и `@:</tr>` строки должны быть в одной строке, как показано.</span><span class="sxs-lookup"><span data-stu-id="cfac2-223">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="cfac2-224">С блоком выбран новый код нажмите клавишу Tab трижды Чтобы выровнять новый код с существующим кодом.</span><span class="sxs-lookup"><span data-stu-id="cfac2-224">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="cfac2-225">Проголосовать или просмотр состояния этой ошибки [с этой ссылкой](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="cfac2-225">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="cfac2-226">Приведенный выше код создает таблицу HTML, который содержит три столбца.</span><span class="sxs-lookup"><span data-stu-id="cfac2-226">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="cfac2-227">Каждый столбец имеет флажок и заголовок, содержащий номер и название.</span><span class="sxs-lookup"><span data-stu-id="cfac2-227">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="cfac2-228">Все флажки имеют одно и то же имя («selectedCourses»).</span><span class="sxs-lookup"><span data-stu-id="cfac2-228">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="cfac2-229">С тем же именем информирует связывателя модели, обрабатывать их как группу.</span><span class="sxs-lookup"><span data-stu-id="cfac2-229">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="cfac2-230">Присвоено недопустимое значение атрибута каждого флажка `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="cfac2-230">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="cfac2-231">При отправке страницы связывателя модели передает массив, состоящий из `CourseID` только флажки, выбранные значения.</span><span class="sxs-lookup"><span data-stu-id="cfac2-231">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="cfac2-232">При первоначальном отображении флажки курсов, назначенные инструктора проверки атрибутов.</span><span class="sxs-lookup"><span data-stu-id="cfac2-232">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="cfac2-233">Запустите приложение и проверить обновленные инструкторов страницы правки.</span><span class="sxs-lookup"><span data-stu-id="cfac2-233">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="cfac2-234">Измените некоторые назначения курса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-234">Change some course assignments.</span></span> <span data-ttu-id="cfac2-235">Изменения отражаются на странице индекса.</span><span class="sxs-lookup"><span data-stu-id="cfac2-235">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="cfac2-236">Примечание: Подходом, который используется здесь для изменения данных курса инструктора удобно использовать, когда имеется ограниченное число курсов.</span><span class="sxs-lookup"><span data-stu-id="cfac2-236">Note: The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="cfac2-237">Для коллекций, которые являются гораздо большего размера более готовый к применению и эффективный будет другой пользовательский Интерфейс и другой метод обновления.</span><span class="sxs-lookup"><span data-stu-id="cfac2-237">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="cfac2-238">Обновление страницы создания инструкторов</span><span class="sxs-lookup"><span data-stu-id="cfac2-238">Update the instructors Create page</span></span>

<span data-ttu-id="cfac2-239">Обновление модели инструктора создать страницу следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="cfac2-239">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="cfac2-240">Предыдущий код аналогичен *Pages/Instructors/Edit.cshtml.cs* кода.</span><span class="sxs-lookup"><span data-stu-id="cfac2-240">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="cfac2-241">Обновление страницы инструктора Razor, создайте следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="cfac2-241">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="cfac2-242">Тестовая страница создания инструктора.</span><span class="sxs-lookup"><span data-stu-id="cfac2-242">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="cfac2-243">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="cfac2-243">Update the Delete page</span></span>

<span data-ttu-id="cfac2-244">Обновление модели страницы удаления со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="cfac2-244">Update the Delete page model with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

<span data-ttu-id="cfac2-245">Приведенный выше код выполняет следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="cfac2-245">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="cfac2-246">Использует упреждающую для `CourseAssignments` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="cfac2-246">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="cfac2-247">`CourseAssignments`должен быть включен или они не удаляются при удалении инструктора.</span><span class="sxs-lookup"><span data-stu-id="cfac2-247">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="cfac2-248">Чтобы избежать необходимости их прочитать, настройте каскадное удаление в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cfac2-248">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="cfac2-249">Если администратор любые отделы назначается инструктора для удаления, удаляет инструктора назначения из тех отделов.</span><span class="sxs-lookup"><span data-stu-id="cfac2-249">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="cfac2-250">[Назад](xref:data/ef-rp/read-related-data)
[Вперед](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="cfac2-250">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
