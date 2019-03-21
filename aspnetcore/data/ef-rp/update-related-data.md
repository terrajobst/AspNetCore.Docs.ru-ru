---
title: Razor Pages с EF Core в ASP.NET Core — обновление связанных данных — 7 из 8
author: rick-anderson
description: В этом руководстве описано обновление связанных данных путем обновления полей внешнего ключа и свойств навигации.
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/update-related-data
ms.openlocfilehash: e1407048157fccdbfdd854071a6fbf9785356026
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208613"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a><span data-ttu-id="f67d0-103">Razor Pages с EF Core в ASP.NET Core — обновление связанных данных — 7 из 8</span><span class="sxs-lookup"><span data-stu-id="f67d0-103">Razor Pages with EF Core in ASP.NET Core - Update Related Data - 7 of 8</span></span>

<span data-ttu-id="f67d0-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f67d0-104">By [Tom Dykstra](https://github.com/tdykstra), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="f67d0-105">В этом учебнике демонстрируется обновление связанных данных.</span><span class="sxs-lookup"><span data-stu-id="f67d0-105">This tutorial demonstrates updating related data.</span></span> <span data-ttu-id="f67d0-106">При возникновении проблем, которые вам не удается устранить, [скачайте или просмотрите готовое приложение.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="f67d0-106">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="f67d0-107">[Указания по скачиванию](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f67d0-107">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="f67d0-108">На следующих рисунках изображены некоторые готовые страницы.</span><span class="sxs-lookup"><span data-stu-id="f67d0-108">The following illustrations shows some of the completed pages.</span></span>

<span data-ttu-id="f67d0-109">![Страница редактирования курса](update-related-data/_static/course-edit.png)
![Страница редактирования преподавателя](update-related-data/_static/instructor-edit-courses.png)</span><span class="sxs-lookup"><span data-stu-id="f67d0-109">![Course Edit page](update-related-data/_static/course-edit.png)
![Instructor Edit page](update-related-data/_static/instructor-edit-courses.png)</span></span>

<span data-ttu-id="f67d0-110">Просмотрите и протестируйте страницы создания и редактирования курса.</span><span class="sxs-lookup"><span data-stu-id="f67d0-110">Examine and test the Create and Edit course pages.</span></span> <span data-ttu-id="f67d0-111">Создайте новый курс.</span><span class="sxs-lookup"><span data-stu-id="f67d0-111">Create a new course.</span></span> <span data-ttu-id="f67d0-112">Кафедра выбирается по целочисленному первичному ключу, а не по названию.</span><span class="sxs-lookup"><span data-stu-id="f67d0-112">The department is selected by its primary key (an integer), not its name.</span></span> <span data-ttu-id="f67d0-113">Измените новый курс.</span><span class="sxs-lookup"><span data-stu-id="f67d0-113">Edit the new course.</span></span> <span data-ttu-id="f67d0-114">После завершения тестирования удалите новый курс.</span><span class="sxs-lookup"><span data-stu-id="f67d0-114">When you have finished testing, delete the new course.</span></span>

## <a name="create-a-base-class-to-share-common-code"></a><span data-ttu-id="f67d0-115">Создание базового класса для совместного использования общего кода</span><span class="sxs-lookup"><span data-stu-id="f67d0-115">Create a base class to share common code</span></span>

<span data-ttu-id="f67d0-116">На страницах Courses/Create и Courses/Edit используется список названий кафедр.</span><span class="sxs-lookup"><span data-stu-id="f67d0-116">The Courses/Create and Courses/Edit pages each need a list of department names.</span></span> <span data-ttu-id="f67d0-117">Создайте базовый класс *Pages/Courses/DepartmentNamePageModel.cshtml.cs* для страниц Create и Edit:</span><span class="sxs-lookup"><span data-stu-id="f67d0-117">Create the *Pages/Courses/DepartmentNamePageModel.cshtml.cs* base class for the Create and Edit pages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

<span data-ttu-id="f67d0-118">Приведенный выше код создает [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0), содержащий список названий кафедр.</span><span class="sxs-lookup"><span data-stu-id="f67d0-118">The preceding code creates a [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) to contain the list of department names.</span></span> <span data-ttu-id="f67d0-119">Если указан параметр `selectedDepartment`, кафедра выбрана в списке `SelectList`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-119">If `selectedDepartment` is specified, that department is selected in the `SelectList`.</span></span>

<span data-ttu-id="f67d0-120">Классы моделей страниц Create и Edit являются производными от `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-120">The Create and Edit page model classes will derive from `DepartmentNamePageModel`.</span></span>

## <a name="customize-the-courses-pages"></a><span data-ttu-id="f67d0-121">Страницы настройки курсов</span><span class="sxs-lookup"><span data-stu-id="f67d0-121">Customize the Courses Pages</span></span>

<span data-ttu-id="f67d0-122">Создаваемая сущность курса должна иметь связь с существующей кафедрой.</span><span class="sxs-lookup"><span data-stu-id="f67d0-122">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="f67d0-123">Чтобы добавить кафедру при создании курса, в базовом классе для страниц Create и Edit реализован раскрывающийся список, в котором можно выбрать кафедру.</span><span class="sxs-lookup"><span data-stu-id="f67d0-123">To add a department while creating a course, the base class for Create and Edit contains a drop-down list for selecting the department.</span></span> <span data-ttu-id="f67d0-124">Этот раскрывающийся список устанавливает свойство внешнего ключа `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-124">The drop-down list sets the `Course.DepartmentID` foreign key (FK) property.</span></span> <span data-ttu-id="f67d0-125">Платформа EF Core использует внешний ключ `Course.DepartmentID` для загрузки свойства навигации `Department`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-125">EF Core uses the `Course.DepartmentID` FK to load the `Department` navigation property.</span></span>

![Создание курса](update-related-data/_static/ddl.png)

<span data-ttu-id="f67d0-127">Обновите модель страницы Create, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="f67d0-127">Update the Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

<span data-ttu-id="f67d0-128">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="f67d0-128">The preceding code:</span></span>

* <span data-ttu-id="f67d0-129">Происходит от `DepartmentNamePageModel`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-129">Derives from `DepartmentNamePageModel`.</span></span>
* <span data-ttu-id="f67d0-130">Использует `TryUpdateModelAsync`, чтобы предотвратить [чрезмерную передачу данных](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="f67d0-130">Uses `TryUpdateModelAsync` to prevent [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="f67d0-131">Заменяет `ViewData["DepartmentID"]` на `DepartmentNameSL` (из базового класса).</span><span class="sxs-lookup"><span data-stu-id="f67d0-131">Replaces `ViewData["DepartmentID"]` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="f67d0-132">`ViewData["DepartmentID"]` заменяется строго типизированным `DepartmentNameSL`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-132">`ViewData["DepartmentID"]` is replaced with the strongly typed `DepartmentNameSL`.</span></span> <span data-ttu-id="f67d0-133">Вместо слабо типизированных моделей рекомендуется использовать строго типизированные.</span><span class="sxs-lookup"><span data-stu-id="f67d0-133">Strongly typed models are preferred over weakly typed.</span></span> <span data-ttu-id="f67d0-134">Дополнительные сведения см. в разделе [Слабо типизированные данные (ViewData и ViewBag)](xref:mvc/views/overview#VD_VB).</span><span class="sxs-lookup"><span data-stu-id="f67d0-134">For more information, see [Weakly typed data (ViewData and ViewBag)](xref:mvc/views/overview#VD_VB).</span></span>

### <a name="update-the-courses-create-page"></a><span data-ttu-id="f67d0-135">Обновление страницы создания курсов</span><span class="sxs-lookup"><span data-stu-id="f67d0-135">Update the Courses Create page</span></span>

<span data-ttu-id="f67d0-136">Обновите файл *Pages/Courses/Create.cshtml*, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="f67d0-136">Update *Pages/Courses/Create.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

<span data-ttu-id="f67d0-137">Приведенная выше разметка вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="f67d0-137">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="f67d0-138">Изменяет заголовок с **DepartmentID** на **Department**.</span><span class="sxs-lookup"><span data-stu-id="f67d0-138">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="f67d0-139">Заменяет `"ViewBag.DepartmentID"` на `DepartmentNameSL` (из базового класса).</span><span class="sxs-lookup"><span data-stu-id="f67d0-139">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>
* <span data-ttu-id="f67d0-140">Добавляет параметр "Select Department" (Выбор кафедры).</span><span class="sxs-lookup"><span data-stu-id="f67d0-140">Adds the "Select Department" option.</span></span> <span data-ttu-id="f67d0-141">В результате этого изменения вместо первой кафедры отображается параметр "Select Department" (Выбор кафедры).</span><span class="sxs-lookup"><span data-stu-id="f67d0-141">This change renders "Select Department" rather than the first department.</span></span>
* <span data-ttu-id="f67d0-142">Добавляет сообщение о проверке в том случае, если не выбрана кафедра.</span><span class="sxs-lookup"><span data-stu-id="f67d0-142">Adds a validation message when the department isn't selected.</span></span>

<span data-ttu-id="f67d0-143">На странице Razor Pages используется [вспомогательная функция тега Select](xref:mvc/views/working-with-forms#the-select-tag-helper):</span><span class="sxs-lookup"><span data-stu-id="f67d0-143">The Razor Page uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper):</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

<span data-ttu-id="f67d0-144">Протестируйте страницу создания.</span><span class="sxs-lookup"><span data-stu-id="f67d0-144">Test the Create page.</span></span> <span data-ttu-id="f67d0-145">На странице Create отображается название, а не идентификатор кафедры.</span><span class="sxs-lookup"><span data-stu-id="f67d0-145">The Create page displays the department name rather than the department ID.</span></span>

### <a name="update-the-courses-edit-page"></a><span data-ttu-id="f67d0-146">Обновите страницу редактирования курсов.</span><span class="sxs-lookup"><span data-stu-id="f67d0-146">Update the Courses Edit page.</span></span>

<span data-ttu-id="f67d0-147">Обновите модель страницы редактирования, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="f67d0-147">Update the edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

<span data-ttu-id="f67d0-148">Изменения аналогичны внесенным в модель страницы Create.</span><span class="sxs-lookup"><span data-stu-id="f67d0-148">The changes are similar to those made in the Create page model.</span></span> <span data-ttu-id="f67d0-149">В приведенном выше коде `PopulateDepartmentsDropDownList` передает идентификатор кафедры, по которому выбирается кафедра, заданная в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="f67d0-149">In the preceding code, `PopulateDepartmentsDropDownList` passes in the department ID, which select the department specified in the drop-down list.</span></span>

<span data-ttu-id="f67d0-150">Обновите файл *Pages/Courses/Edit.cshtml*, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="f67d0-150">Update *Pages/Courses/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

<span data-ttu-id="f67d0-151">Приведенная выше разметка вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="f67d0-151">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="f67d0-152">Отображает идентификатор курса.</span><span class="sxs-lookup"><span data-stu-id="f67d0-152">Displays the course ID.</span></span> <span data-ttu-id="f67d0-153">Как правило, первичный ключ сущности не отображается.</span><span class="sxs-lookup"><span data-stu-id="f67d0-153">Generally the Primary Key (PK) of an entity isn't displayed.</span></span> <span data-ttu-id="f67d0-154">Первичные ключи для пользователей обычно не имеют значения.</span><span class="sxs-lookup"><span data-stu-id="f67d0-154">PKs are usually meaningless to users.</span></span> <span data-ttu-id="f67d0-155">В этом случае в качестве первичного ключа используется номер курса.</span><span class="sxs-lookup"><span data-stu-id="f67d0-155">In this case, the PK is the course number.</span></span>
* <span data-ttu-id="f67d0-156">Изменяет заголовок с **DepartmentID** на **Department**.</span><span class="sxs-lookup"><span data-stu-id="f67d0-156">Changes the caption from **DepartmentID** to **Department**.</span></span>
* <span data-ttu-id="f67d0-157">Заменяет `"ViewBag.DepartmentID"` на `DepartmentNameSL` (из базового класса).</span><span class="sxs-lookup"><span data-stu-id="f67d0-157">Replaces `"ViewBag.DepartmentID"` with `DepartmentNameSL` (from the base class).</span></span>

<span data-ttu-id="f67d0-158">На этой странице содержится скрытое поле (`<input type="hidden">`) с номером курса.</span><span class="sxs-lookup"><span data-stu-id="f67d0-158">The page contains a hidden field (`<input type="hidden">`) for the course number.</span></span> <span data-ttu-id="f67d0-159">Добавление вспомогательной функции тега `<label>` с `asp-for="Course.CourseID"` не избавляет от необходимости использовать это скрытое поле.</span><span class="sxs-lookup"><span data-stu-id="f67d0-159">Adding a `<label>` tag helper with `asp-for="Course.CourseID"` doesn't eliminate the need for the hidden field.</span></span> <span data-ttu-id="f67d0-160">`<input type="hidden">` необходимо, чтобы включить номер курса в отправляемые данные при нажатии пользователем кнопки **Save** (Сохранить).</span><span class="sxs-lookup"><span data-stu-id="f67d0-160">`<input type="hidden">` is required for the course number to be included in the posted data when the user clicks **Save**.</span></span>

<span data-ttu-id="f67d0-161">Протестируйте обновленный код.</span><span class="sxs-lookup"><span data-stu-id="f67d0-161">Test the updated code.</span></span> <span data-ttu-id="f67d0-162">Создайте, измените и удалите курс.</span><span class="sxs-lookup"><span data-stu-id="f67d0-162">Create, edit, and delete a course.</span></span>

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a><span data-ttu-id="f67d0-163">Добавление AsNoTracking в модели страниц Details и Delete</span><span class="sxs-lookup"><span data-stu-id="f67d0-163">Add AsNoTracking to the Details and Delete page models</span></span>

<span data-ttu-id="f67d0-164">Применение [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) позволяет повысить производительность в тех сценариях, где не требуется отслеживание.</span><span class="sxs-lookup"><span data-stu-id="f67d0-164">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) can improve performance when tracking isn't required.</span></span> <span data-ttu-id="f67d0-165">Добавьте `AsNoTracking` в модели страниц Delete и Details.</span><span class="sxs-lookup"><span data-stu-id="f67d0-165">Add `AsNoTracking` to the Delete and Details page model.</span></span> <span data-ttu-id="f67d0-166">Следующий код отображает обновленную модель страницы Delete:</span><span class="sxs-lookup"><span data-stu-id="f67d0-166">The following code shows the updated Delete page model:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

<span data-ttu-id="f67d0-167">Обновите метод `OnGetAsync` в файле *Pages/Courses/Details.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f67d0-167">Update the `OnGetAsync` method in the *Pages/Courses/Details.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a><span data-ttu-id="f67d0-168">Изменение страниц Delete и Details</span><span class="sxs-lookup"><span data-stu-id="f67d0-168">Modify the Delete and Details pages</span></span>

<span data-ttu-id="f67d0-169">Обновите страницу Razor Pages Delete, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="f67d0-169">Update the Delete Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

<span data-ttu-id="f67d0-170">Выполните те же изменения для страницы Details.</span><span class="sxs-lookup"><span data-stu-id="f67d0-170">Make the same changes to the Details page.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="f67d0-171">Тестирование страниц курса</span><span class="sxs-lookup"><span data-stu-id="f67d0-171">Test the Course pages</span></span>

<span data-ttu-id="f67d0-172">Протестируйте страницы создания, редактирования и сведений, после чего удалите их.</span><span class="sxs-lookup"><span data-stu-id="f67d0-172">Test create, edit, details, and delete.</span></span>

## <a name="update-the-instructor-pages"></a><span data-ttu-id="f67d0-173">Обновление страниц преподавателя</span><span class="sxs-lookup"><span data-stu-id="f67d0-173">Update the instructor pages</span></span>

<span data-ttu-id="f67d0-174">В следующих разделах обновляются страницы преподавателя.</span><span class="sxs-lookup"><span data-stu-id="f67d0-174">The following sections update the instructor pages.</span></span>

### <a name="add-office-location"></a><span data-ttu-id="f67d0-175">Добавить расположения кабинета</span><span class="sxs-lookup"><span data-stu-id="f67d0-175">Add office location</span></span>

<span data-ttu-id="f67d0-176">При редактировании записи преподавателя может потребоваться обновить назначенный преподавателю кабинет.</span><span class="sxs-lookup"><span data-stu-id="f67d0-176">When editing an instructor record, you may want to update the instructor's office assignment.</span></span> <span data-ttu-id="f67d0-177">Сущность `Instructor` имеет отношение "один к нулю или к одному" с сущностью `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-177">The `Instructor` entity has a one-to-zero-or-one relationship with the `OfficeAssignment` entity.</span></span> <span data-ttu-id="f67d0-178">Код преподавателя должен обрабатывать следующее:</span><span class="sxs-lookup"><span data-stu-id="f67d0-178">The instructor code must handle:</span></span>

* <span data-ttu-id="f67d0-179">Если пользователь удаляет назначение кабинета, удаляется сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-179">If the user clears the office assignment, delete the `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="f67d0-180">Если пользователь вводит назначение пустого кабинета, создается новая сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-180">If the user enters an office assignment and it was empty, create a new `OfficeAssignment` entity.</span></span>
* <span data-ttu-id="f67d0-181">Если пользователь изменяет назначение кабинета, обновляется сущность `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-181">If the user changes the office assignment, update the `OfficeAssignment` entity.</span></span>

<span data-ttu-id="f67d0-182">Обновите модель страницы редактирования преподавателей, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="f67d0-182">Update the instructors Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

<span data-ttu-id="f67d0-183">Предыдущий код:</span><span class="sxs-lookup"><span data-stu-id="f67d0-183">The preceding code:</span></span>

* <span data-ttu-id="f67d0-184">Получает текущую сущность `Instructor` из базы данных, используя безотложную загрузку для свойства навигации `OfficeAssignment`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-184">Gets the current `Instructor` entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span>
* <span data-ttu-id="f67d0-185">Обновляет извлеченную сущность `Instructor`, используя значения из связывателя модели.</span><span class="sxs-lookup"><span data-stu-id="f67d0-185">Updates the retrieved `Instructor` entity with values from the model binder.</span></span> <span data-ttu-id="f67d0-186">`TryUpdateModel` позволяет предотвратить [чрезмерную передачу данных](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="f67d0-186">`TryUpdateModel` prevents [overposting](xref:data/ef-rp/crud#overposting).</span></span>
* <span data-ttu-id="f67d0-187">Если расположение кабинета пусто, `Instructor.OfficeAssignment` получает значение NULL.</span><span class="sxs-lookup"><span data-stu-id="f67d0-187">If the office location is blank, sets `Instructor.OfficeAssignment` to null.</span></span> <span data-ttu-id="f67d0-188">Если `Instructor.OfficeAssignment` имеет значение NULL, связанная строка в таблице `OfficeAssignment` удаляется.</span><span class="sxs-lookup"><span data-stu-id="f67d0-188">When `Instructor.OfficeAssignment` is null, the related row in the `OfficeAssignment` table is deleted.</span></span>

### <a name="update-the-instructor-edit-page"></a><span data-ttu-id="f67d0-189">Обновление страницы редактирования преподавателя</span><span class="sxs-lookup"><span data-stu-id="f67d0-189">Update the instructor Edit page</span></span>

<span data-ttu-id="f67d0-190">Обновите файл *Pages/Instructors/Edit.cshtml*, указав расположение кабинета:</span><span class="sxs-lookup"><span data-stu-id="f67d0-190">Update *Pages/Instructors/Edit.cshtml* with the office location:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

<span data-ttu-id="f67d0-191">Убедитесь, что можно изменить расположение кабинета для преподавателя.</span><span class="sxs-lookup"><span data-stu-id="f67d0-191">Verify you can change an instructors office location.</span></span>

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="f67d0-192">Добавление назначений курсов на страницу редактирования преподавателя</span><span class="sxs-lookup"><span data-stu-id="f67d0-192">Add Course assignments to the instructor Edit page</span></span>

<span data-ttu-id="f67d0-193">Преподаватели могут вести любое число курсов.</span><span class="sxs-lookup"><span data-stu-id="f67d0-193">Instructors may teach any number of courses.</span></span> <span data-ttu-id="f67d0-194">В этом разделе вы добавите возможность изменять назначения курсов.</span><span class="sxs-lookup"><span data-stu-id="f67d0-194">In this section, you add the ability to change course assignments.</span></span> <span data-ttu-id="f67d0-195">На следующем рисунке показана обновленная страница редактирования преподавателя:</span><span class="sxs-lookup"><span data-stu-id="f67d0-195">The following image shows the updated instructor Edit page:</span></span>

![Страница редактирования преподавателя с курсами](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="f67d0-197">Между сущностями `Course` и `Instructor` существует отношение "многие ко многим".</span><span class="sxs-lookup"><span data-stu-id="f67d0-197">`Course` and `Instructor` has a many-to-many relationship.</span></span> <span data-ttu-id="f67d0-198">Для добавления и удаления отношений можно добавлять сущности в список объединенного набора сущностей `CourseAssignments` и удалять их из него.</span><span class="sxs-lookup"><span data-stu-id="f67d0-198">To add and remove relationships, you add and remove entities from the `CourseAssignments` join entity set.</span></span>

<span data-ttu-id="f67d0-199">С помощью флажков можно изменять курсы, которым назначен преподаватель.</span><span class="sxs-lookup"><span data-stu-id="f67d0-199">Check boxes enable changes to courses an instructor is assigned to.</span></span> <span data-ttu-id="f67d0-200">Флажок отображается для каждого курса в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f67d0-200">A check box is displayed for every course in the database.</span></span> <span data-ttu-id="f67d0-201">Для курсов, которым назначен преподаватель, флажок установлен.</span><span class="sxs-lookup"><span data-stu-id="f67d0-201">Courses that the instructor is assigned to are checked.</span></span> <span data-ttu-id="f67d0-202">Пользователь может устанавливать и снимать флажки, изменяя назначения курсов.</span><span class="sxs-lookup"><span data-stu-id="f67d0-202">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="f67d0-203">Если число курсов было бы гораздо больше:</span><span class="sxs-lookup"><span data-stu-id="f67d0-203">If the number of courses were much greater:</span></span>

* <span data-ttu-id="f67d0-204">В таком сценарии, скорее всего, применялся бы другой пользовательский интерфейс для отображения курсов.</span><span class="sxs-lookup"><span data-stu-id="f67d0-204">You'd probably use a different user interface to display the courses.</span></span>
* <span data-ttu-id="f67d0-205">Способ управления объединением сущностей для создания или удаления отношений при этом не изменился бы.</span><span class="sxs-lookup"><span data-stu-id="f67d0-205">The method of manipulating a join entity to create or delete relationships wouldn't change.</span></span>

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a><span data-ttu-id="f67d0-206">Добавление классов для поддержки страниц создания и редактирования преподавателя</span><span class="sxs-lookup"><span data-stu-id="f67d0-206">Add classes to support Create and Edit instructor pages</span></span>

<span data-ttu-id="f67d0-207">Создайте файл *SchoolViewModels/AssignedCourseData.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="f67d0-207">Create *SchoolViewModels/AssignedCourseData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="f67d0-208">Класс `AssignedCourseData` содержит данные для создания флажков, определяющих назначенные преподавателю курсы.</span><span class="sxs-lookup"><span data-stu-id="f67d0-208">The `AssignedCourseData` class contains data to create the check boxes for assigned courses by an instructor.</span></span>

<span data-ttu-id="f67d0-209">Создайте базовый класс *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f67d0-209">Create the *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* base class:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

<span data-ttu-id="f67d0-210">Базовый класс `InstructorCoursesPageModel` будет использоваться для моделей страниц редактирования и создания.</span><span class="sxs-lookup"><span data-stu-id="f67d0-210">The `InstructorCoursesPageModel` is the base class you will use for the Edit and Create page models.</span></span> <span data-ttu-id="f67d0-211">`PopulateAssignedCourseData` считывает все сущности `Course` для заполнения списка `AssignedCourseDataList`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-211">`PopulateAssignedCourseData` reads all `Course` entities to populate `AssignedCourseDataList`.</span></span> <span data-ttu-id="f67d0-212">Для каждого курса код задает `CourseID`, название, а также сведения о назначении курсу преподавателя.</span><span class="sxs-lookup"><span data-stu-id="f67d0-212">For each course, the code sets the `CourseID`, title, and whether or not the instructor is assigned to the course.</span></span> <span data-ttu-id="f67d0-213">Для реализации эффективного поиска используется класс [HashSet](/dotnet/api/system.collections.generic.hashset-1).</span><span class="sxs-lookup"><span data-stu-id="f67d0-213">A [HashSet](/dotnet/api/system.collections.generic.hashset-1) is used to create efficient lookups.</span></span>

### <a name="instructors-edit-page-model"></a><span data-ttu-id="f67d0-214">Модель страницы редактирования преподавателей</span><span class="sxs-lookup"><span data-stu-id="f67d0-214">Instructors Edit page model</span></span>

<span data-ttu-id="f67d0-215">Обновите модель страницы редактирования преподавателя, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="f67d0-215">Update the instructor Edit page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

<span data-ttu-id="f67d0-216">Приведенный выше код обрабатывает изменения в назначении кабинета.</span><span class="sxs-lookup"><span data-stu-id="f67d0-216">The preceding code handles office assignment changes.</span></span>

<span data-ttu-id="f67d0-217">Обновите представление Razor преподавателя:</span><span class="sxs-lookup"><span data-stu-id="f67d0-217">Update the instructor Razor View:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> <span data-ttu-id="f67d0-218">При вставке кода в Visual Studio разрывы строк изменяются, нарушая код.</span><span class="sxs-lookup"><span data-stu-id="f67d0-218">When you paste the code in Visual Studio, line breaks are changed in a way that breaks the code.</span></span> <span data-ttu-id="f67d0-219">Один раз нажмите клавиши CTRL+Z, чтобы отменить автоматическое форматирование.</span><span class="sxs-lookup"><span data-stu-id="f67d0-219">Press Ctrl+Z one time to undo the automatic formatting.</span></span> <span data-ttu-id="f67d0-220">При нажатии клавиш CTRL+Z разрывы строк исправляются, благодаря чему код приобретает показанный здесь вид.</span><span class="sxs-lookup"><span data-stu-id="f67d0-220">Ctrl+Z fixes the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="f67d0-221">Выравнивать отступы необязательно, однако строки `@</tr><tr>`, `@:<td>`, `@:</td>` и `@:</tr>` должны находиться на одной строке, как показано здесь.</span><span class="sxs-lookup"><span data-stu-id="f67d0-221">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown.</span></span> <span data-ttu-id="f67d0-222">Выделите блок нового кода и три раза нажмите клавишу TAB, чтобы выровнять его с существующим кодом.</span><span class="sxs-lookup"><span data-stu-id="f67d0-222">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="f67d0-223">Чтобы проголосовать за эту ошибку или проверить ее статус, воспользуйтесь [этой ссылкой](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="f67d0-223">Vote on or review the status of this bug [with this link](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

<span data-ttu-id="f67d0-224">Приведенный выше код создает таблицу HTML с тремя столбцами.</span><span class="sxs-lookup"><span data-stu-id="f67d0-224">The preceding code creates an HTML table that has three columns.</span></span> <span data-ttu-id="f67d0-225">Каждый столбец содержит флажок и заголовок с номером и названием курса.</span><span class="sxs-lookup"><span data-stu-id="f67d0-225">Each column has a check box and a caption containing the course number and title.</span></span> <span data-ttu-id="f67d0-226">Все флажки имеют одинаковые имена ("selectedCourses").</span><span class="sxs-lookup"><span data-stu-id="f67d0-226">The check boxes all have the same name ("selectedCourses").</span></span> <span data-ttu-id="f67d0-227">Поскольку используется одно и то же имя, связыватель модели интерпретирует их как группу.</span><span class="sxs-lookup"><span data-stu-id="f67d0-227">Using the same name informs the model binder to treat them as a group.</span></span> <span data-ttu-id="f67d0-228">Атрибуту значения для каждого флажка присвоен `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-228">The value attribute of each check box is set to `CourseID`.</span></span> <span data-ttu-id="f67d0-229">При отправке страницы связыватель модели передает массив, содержащий значения `CourseID` только для выбранных флажков.</span><span class="sxs-lookup"><span data-stu-id="f67d0-229">When the page is posted, the model binder passes an array that consists of the `CourseID` values for only the check boxes that are selected.</span></span>

<span data-ttu-id="f67d0-230">При первичной отрисовке для курсов, назначенных преподавателю, отображаются установленные флажки.</span><span class="sxs-lookup"><span data-stu-id="f67d0-230">When the check boxes are initially rendered, courses assigned to the instructor have checked attributes.</span></span>

<span data-ttu-id="f67d0-231">Запустите приложение и протестируйте обновленную страницу редактирования преподавателя.</span><span class="sxs-lookup"><span data-stu-id="f67d0-231">Run the app and test the updated instructors Edit page.</span></span> <span data-ttu-id="f67d0-232">Измените некоторые назначения курсов.</span><span class="sxs-lookup"><span data-stu-id="f67d0-232">Change some course assignments.</span></span> <span data-ttu-id="f67d0-233">Изменения отражаются на странице указателя.</span><span class="sxs-lookup"><span data-stu-id="f67d0-233">The changes are reflected on the Index page.</span></span>

<span data-ttu-id="f67d0-234">Примечание. Описываемый здесь подход к редактированию данных курсов для преподавателя эффективен при ограниченном числе курсов.</span><span class="sxs-lookup"><span data-stu-id="f67d0-234">Note: The approach taken here to edit instructor course data works well when there's a limited number of courses.</span></span> <span data-ttu-id="f67d0-235">Для коллекций большего размера более практичным и эффективным было бы применение другого пользовательского интерфейса и другого метода обновления.</span><span class="sxs-lookup"><span data-stu-id="f67d0-235">For collections that are much larger, a different UI and a different updating method would be more useable and efficient.</span></span>

### <a name="update-the-instructors-create-page"></a><span data-ttu-id="f67d0-236">Обновление страницы создания преподавателей</span><span class="sxs-lookup"><span data-stu-id="f67d0-236">Update the instructors Create page</span></span>

<span data-ttu-id="f67d0-237">Обновите модель страницы создания преподавателя, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="f67d0-237">Update the instructor Create page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

<span data-ttu-id="f67d0-238">Приведенный выше код аналогичен коду в файле *Pages/Instructors/Edit.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="f67d0-238">The preceding code is similar to the *Pages/Instructors/Edit.cshtml.cs* code.</span></span>

<span data-ttu-id="f67d0-239">Обновите страницу создания преподавателя Razor Pages, используя следующую разметку:</span><span class="sxs-lookup"><span data-stu-id="f67d0-239">Update the instructor Create Razor page with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

<span data-ttu-id="f67d0-240">Протестируйте страницу создания преподавателя.</span><span class="sxs-lookup"><span data-stu-id="f67d0-240">Test the instructor Create page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="f67d0-241">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="f67d0-241">Update the Delete page</span></span>

<span data-ttu-id="f67d0-242">Обновите страницу "Delete" (Удаление) с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="f67d0-242">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

<span data-ttu-id="f67d0-243">Приведенный выше код вносит следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="f67d0-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="f67d0-244">Использует упреждающую загрузку для свойства навигации `CourseAssignments`.</span><span class="sxs-lookup"><span data-stu-id="f67d0-244">Uses eager loading for the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="f67d0-245">Требуется включить `CourseAssignments`, иначе они не будут удалены при удалении преподавателя.</span><span class="sxs-lookup"><span data-stu-id="f67d0-245">`CourseAssignments` must be included or they aren't deleted when the instructor is deleted.</span></span> <span data-ttu-id="f67d0-246">Чтобы избежать необходимости считывать их, настройте каскадное удаление в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f67d0-246">To avoid needing to read them, configure cascade delete in the database.</span></span>

* <span data-ttu-id="f67d0-247">Если преподаватель, которого требуется удалить, назначен в качестве администратора любой из кафедр, удаляется назначение преподавателя из таких кафедр.</span><span class="sxs-lookup"><span data-stu-id="f67d0-247">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f67d0-248">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f67d0-248">Additional resources</span></span>

* [<span data-ttu-id="f67d0-249">Версия руководства на YouTube (часть 1)</span><span class="sxs-lookup"><span data-stu-id="f67d0-249">YouTube version of this tutorial (Part 1)</span></span>](https://www.youtube.com/watch?v=Csh6gkmwc9E)
* [<span data-ttu-id="f67d0-250">Версия руководства на YouTube (часть 2)</span><span class="sxs-lookup"><span data-stu-id="f67d0-250">YouTube version of this tutorial (Part 2)</span></span>](https://www.youtube.com/watch?v=mOAankB_Zgc)

> [!div class="step-by-step"]
> <span data-ttu-id="f67d0-251">[Назад](xref:data/ef-rp/read-related-data)
> [Вперед](xref:data/ef-rp/concurrency)</span><span class="sxs-lookup"><span data-stu-id="f67d0-251">[Previous](xref:data/ef-rp/read-related-data)
[Next](xref:data/ef-rp/concurrency)</span></span>
