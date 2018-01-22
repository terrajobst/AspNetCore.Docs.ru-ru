---
title: "Данные - 7 из 10, относящиеся к ASP.NET Core MVC с основными EF - обновление"
author: tdykstra
description: "В этом учебнике будет обновить связанные данные, обновив внешних ключевых полей и свойств навигации."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 0e4df407a1ca15aa5baa2b7226be1cf91902a583
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a><span data-ttu-id="64db9-103">Обновление связанных данных - Core EF учебнику ASP.NET Core MVC (7, 10)</span><span class="sxs-lookup"><span data-stu-id="64db9-103">Updating related data - EF Core with ASP.NET Core MVC tutorial (7 of 10)</span></span>

<span data-ttu-id="64db9-104">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="64db9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="64db9-105">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC ядра с помощью основного Entity Framework и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="64db9-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="64db9-106">Сведения о учебника серии см [в первом учебнике ряда](intro.md).</span><span class="sxs-lookup"><span data-stu-id="64db9-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="64db9-107">В предыдущем учебнике отображаются связанные данные; в этом учебнике будет обновить связанные данные, обновив внешних ключевых полей и свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="64db9-107">In the previous tutorial you displayed related data; in this tutorial you'll update related data by updating foreign key fields and navigation properties.</span></span>

<span data-ttu-id="64db9-108">Некоторые страницы, которые вы будете работать с рисунках ниже.</span><span class="sxs-lookup"><span data-stu-id="64db9-108">The following illustrations show some of the pages that you'll work with.</span></span>

![Страница изменения курса](update-related-data/_static/course-edit.png)

![Страница изменения инструктора](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a><span data-ttu-id="64db9-111">Настроить создание и редактирование страниц для курсов</span><span class="sxs-lookup"><span data-stu-id="64db9-111">Customize the Create and Edit Pages for Courses</span></span>

<span data-ttu-id="64db9-112">При создании новой сущности курса, он должен иметь отношение к отдела.</span><span class="sxs-lookup"><span data-stu-id="64db9-112">When a new course entity is created, it must have a relationship to an existing department.</span></span> <span data-ttu-id="64db9-113">Чтобы упростить эту задачу, формирования шаблонов код включает создание и изменение представления, включающие раскрывающегося списка для выбора подразделение и методов контроллера.</span><span class="sxs-lookup"><span data-stu-id="64db9-113">To facilitate this, the scaffolded code includes controller methods and Create and Edit views that include a drop-down list for selecting the department.</span></span> <span data-ttu-id="64db9-114">Наборы раскрывающегося списка `Course.DepartmentID` свойство внешнего ключа, и это все платформы Entity Framework требуется для загрузки `Department` свойство навигации с соответствующие сущность Department.</span><span class="sxs-lookup"><span data-stu-id="64db9-114">The drop-down list sets the `Course.DepartmentID` foreign key property, and that's all the Entity Framework needs in order to load the `Department` navigation property with the appropriate Department entity.</span></span> <span data-ttu-id="64db9-115">Будет использовать формирования шаблонов кода, но изменить их немного на добавление обработки ошибок и сортировка раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="64db9-115">You'll use the scaffolded code, but change it slightly to add error handling and sort the drop-down list.</span></span>

<span data-ttu-id="64db9-116">В *CoursesController.cs*, удалить четыре метода Создание и изменение и замените их следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="64db9-116">In *CoursesController.cs*, delete the four Create and Edit methods and replace them with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

<span data-ttu-id="64db9-117">После `Edit` метод HttpPost, создайте новый метод, который загружает данные отдела для раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="64db9-117">After the `Edit` HttpPost method, create a new method that loads department info for the drop-down list.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

<span data-ttu-id="64db9-118">`PopulateDepartmentsDropDownList` Метод возвращает список всех отделов, отсортированных по имени, создает `SelectList` коллекции для раскрывающегося списка и передает в представление коллекции `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="64db9-118">The `PopulateDepartmentsDropDownList` method gets a list of all departments sorted by name, creates a `SelectList` collection for a drop-down list, and passes the collection to the view in `ViewBag`.</span></span> <span data-ttu-id="64db9-119">Этот метод принимает необязательный `selectedDepartment` параметр, который позволяет вызывающему коду указать элемент, который будет установлен при отображении раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="64db9-119">The method accepts the optional `selectedDepartment` parameter that allows the calling code to specify the item that will be selected when the drop-down list is rendered.</span></span> <span data-ttu-id="64db9-120">Представление будет передать имя «DepartmentID» `<select>` тег вспомогательный метод и вспомогательный объект для поиска в становится известен `ViewBag` для объекта `SelectList` с именем «DepartmentID».</span><span class="sxs-lookup"><span data-stu-id="64db9-120">The view will pass the name "DepartmentID" to the `<select>` tag helper, and the helper then knows to look in the `ViewBag` object for a `SelectList` named "DepartmentID".</span></span>

<span data-ttu-id="64db9-121">HttpGet `Create` вызовы метода `PopulateDepartmentsDropDownList` метод без установки выбранного элемента, так как для нового курса отдела не будет установлено еще:</span><span class="sxs-lookup"><span data-stu-id="64db9-121">The HttpGet `Create` method calls the `PopulateDepartmentsDropDownList` method without setting the selected item, because for a new course the department is not established yet:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

<span data-ttu-id="64db9-122">HttpGet `Edit` метод задает выбранный элемент, на основе идентификатора отдела, которые уже назначен редактируемого курса:</span><span class="sxs-lookup"><span data-stu-id="64db9-122">The HttpGet `Edit` method sets the selected item, based on the ID of the department that is already assigned to the course being edited:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

<span data-ttu-id="64db9-123">Для обоих методов HttpPost `Create` и `Edit` также включать код, который задает элемент, выбранный при их отображении страницы после возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="64db9-123">The HttpPost methods for both `Create` and `Edit` also include code that sets the selected item when they redisplay the page after an error.</span></span> <span data-ttu-id="64db9-124">Это гарантирует, что если отобразится страница для отображения сообщения об ошибках, был выбран любой отдел остается выбранным.</span><span class="sxs-lookup"><span data-stu-id="64db9-124">This ensures that when the page is redisplayed to show the error message, whatever department was selected stays selected.</span></span>

### <a name="add-asnotracking-to-details-and-delete-methods"></a><span data-ttu-id="64db9-125">Добавите. AsNoTracking подробности методы и Delete</span><span class="sxs-lookup"><span data-stu-id="64db9-125">Add .AsNoTracking to Details and Delete methods</span></span>

<span data-ttu-id="64db9-126">Для оптимизации производительности, сведения по курсу и удаление страниц, добавьте `AsNoTracking` вызывает в `Details` и HttpGet `Delete` методы.</span><span class="sxs-lookup"><span data-stu-id="64db9-126">To optimize performance of the Course Details and Delete pages, add `AsNoTracking` calls in the `Details` and HttpGet `Delete` methods.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a><span data-ttu-id="64db9-127">Изменение представлений курса</span><span class="sxs-lookup"><span data-stu-id="64db9-127">Modify the Course views</span></span>

<span data-ttu-id="64db9-128">В *Views/Courses/Create.cshtml*, добавьте параметр «Выберите отдела», чтобы **отдел** раскрывающемся списке, измените заголовок из **DepartmentID** для  **Отдел**и добавить сообщение проверки.</span><span class="sxs-lookup"><span data-stu-id="64db9-128">In *Views/Courses/Create.cshtml*, add a "Select Department" option to the **Department** drop-down list, change the caption from **DepartmentID** to **Department**, and add a validation message.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

<span data-ttu-id="64db9-129">В *Views/Courses/Edit.cshtml*, внести такое же изменение поля отдела, которые были в *Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="64db9-129">In *Views/Courses/Edit.cshtml*, make the same change for the Department field that you just did in *Create.cshtml*.</span></span>

<span data-ttu-id="64db9-130">Кроме того, в *Views/Courses/Edit.cshtml*, добавить числовое поле курс перед **заголовок** поля.</span><span class="sxs-lookup"><span data-stu-id="64db9-130">Also in *Views/Courses/Edit.cshtml*, add a course number field before the **Title** field.</span></span> <span data-ttu-id="64db9-131">Так как номер является первичным ключом, он отображается, но его нельзя изменить.</span><span class="sxs-lookup"><span data-stu-id="64db9-131">Because the course number is the primary key, it's displayed, but it can't be changed.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

<span data-ttu-id="64db9-132">Скрытое поле уже существует (`<input type="hidden">`) для номер в режиме редактирования.</span><span class="sxs-lookup"><span data-stu-id="64db9-132">There's already a hidden field (`<input type="hidden">`) for the course number in the Edit view.</span></span> <span data-ttu-id="64db9-133">Добавление `<label>` вспомогательный тег не устраняют необходимость в скрытое поле, так как оно не вызывает должны быть включены в данные, когда пользователь щелкает номер **Сохранить** на **изменить** страницы.</span><span class="sxs-lookup"><span data-stu-id="64db9-133">Adding a `<label>` tag helper doesn't eliminate the need for the hidden field because it doesn't cause the course number to be included in the posted data when the user clicks **Save** on the **Edit** page.</span></span>

<span data-ttu-id="64db9-134">В *Views/Courses/Delete.cshtml*добавьте курса числовое поле в верхней и сделать department, идентификатор и название отдела.</span><span class="sxs-lookup"><span data-stu-id="64db9-134">In *Views/Courses/Delete.cshtml*, add a course number field at the top and change department ID to department name.</span></span>

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

<span data-ttu-id="64db9-135">В *Views/Courses/Details.cshtml*, внести такое же изменение, которые были для *Delete.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="64db9-135">In *Views/Courses/Details.cshtml*, make the same change that you just did for *Delete.cshtml*.</span></span>

### <a name="test-the-course-pages"></a><span data-ttu-id="64db9-136">Тестирование страниц курса</span><span class="sxs-lookup"><span data-stu-id="64db9-136">Test the Course pages</span></span>

<span data-ttu-id="64db9-137">Запустите приложение, выберите **курсы** щелкните **создать новый**и введите данные для нового курса:</span><span class="sxs-lookup"><span data-stu-id="64db9-137">Run the app, select the **Courses** tab, click **Create New**, and enter data for a new course:</span></span>

![Страница создания курса](update-related-data/_static/course-create.png)

<span data-ttu-id="64db9-139">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="64db9-139">Click **Create**.</span></span> <span data-ttu-id="64db9-140">Страница индекса курсы отображается новый курс, добавляемый в список.</span><span class="sxs-lookup"><span data-stu-id="64db9-140">The Courses Index page is displayed with the new course added to the list.</span></span> <span data-ttu-id="64db9-141">Имя подразделения в список страниц индекса поступает из свойства навигации, показывающая связь была установлена правильно.</span><span class="sxs-lookup"><span data-stu-id="64db9-141">The department name in the Index page list comes from the navigation property, showing that the relationship was established correctly.</span></span>

<span data-ttu-id="64db9-142">Нажмите кнопку **изменить** на курс страницы индекса курсов.</span><span class="sxs-lookup"><span data-stu-id="64db9-142">Click **Edit** on a course in the Courses Index page.</span></span>

![Страница изменения курса](update-related-data/_static/course-edit.png)

<span data-ttu-id="64db9-144">Измените данные на странице и нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="64db9-144">Change data on the page and click **Save**.</span></span> <span data-ttu-id="64db9-145">Страница индекса курсы отображается с данными курс был обновлен.</span><span class="sxs-lookup"><span data-stu-id="64db9-145">The Courses Index page is displayed with the updated course data.</span></span>

## <a name="add-an-edit-page-for-instructors"></a><span data-ttu-id="64db9-146">Добавить страницу редактирования для преподавателей</span><span class="sxs-lookup"><span data-stu-id="64db9-146">Add an Edit Page for Instructors</span></span>

<span data-ttu-id="64db9-147">При редактировании запись инструктора, необходимо иметь возможность обновить назначение инструктора office.</span><span class="sxs-lookup"><span data-stu-id="64db9-147">When you edit an instructor record, you want to be able to update the instructor's office assignment.</span></span> <span data-ttu-id="64db9-148">Сущность инструктора имеет отношение "один к нулю или одному" с OfficeAssignment сущностью, которая означает, что код для обработки в следующих ситуациях:</span><span class="sxs-lookup"><span data-stu-id="64db9-148">The Instructor entity has a one-to-zero-or-one relationship with the OfficeAssignment entity, which means your code has to handle the following situations:</span></span>

* <span data-ttu-id="64db9-149">Если пользователь удаляет назначение office и было до значения, удалите OfficeAssignment сущности.</span><span class="sxs-lookup"><span data-stu-id="64db9-149">If the user clears the office assignment and it originally had a value, delete the OfficeAssignment entity.</span></span>

* <span data-ttu-id="64db9-150">Если пользователь вводит значение назначения office и он изначально был пуст, создайте новую сущность OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="64db9-150">If the user enters an office assignment value and it originally was empty, create a new OfficeAssignment entity.</span></span>

* <span data-ttu-id="64db9-151">Если пользователь изменяет значение назначения office, измените значение в существующей сущности OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="64db9-151">If the user changes the value of an office assignment, change the value in an existing OfficeAssignment entity.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="64db9-152">Обновление контроллера преподавателей</span><span class="sxs-lookup"><span data-stu-id="64db9-152">Update the Instructors controller</span></span>

<span data-ttu-id="64db9-153">В *InstructorsController.cs*, измените код в HttpGet `Edit` метода, так что он загружает сущность инструктора `OfficeAssignment` свойства навигации и вызывает `AsNoTracking`:</span><span class="sxs-lookup"><span data-stu-id="64db9-153">In *InstructorsController.cs*, change the code in the HttpGet `Edit` method so that it loads the Instructor entity's `OfficeAssignment` navigation property and calls `AsNoTracking`:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

<span data-ttu-id="64db9-154">Замените HttpPost `Edit` метод следующим кодом для обработки обновлений office назначения:</span><span class="sxs-lookup"><span data-stu-id="64db9-154">Replace the HttpPost `Edit` method with the following code to handle office assignment updates:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

<span data-ttu-id="64db9-155">Код выполняет следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="64db9-155">The code does the following:</span></span>

-  <span data-ttu-id="64db9-156">Изменяет имя метода для `EditPost` поскольку подпись теперь является таким же, как HttpGet `Edit` метод ( `ActionName` атрибут указывает, что `/Edit/` по-прежнему используется URL-адрес).</span><span class="sxs-lookup"><span data-stu-id="64db9-156">Changes the method name to `EditPost` because the signature is now the same as the HttpGet `Edit` method (the `ActionName` attribute specifies that the `/Edit/` URL is still used).</span></span>

-  <span data-ttu-id="64db9-157">Возвращает текущую сущность инструктора из базы данных с помощью упреждающая загрузка для `OfficeAssignment` свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="64db9-157">Gets the current Instructor entity from the database using eager loading for the `OfficeAssignment` navigation property.</span></span> <span data-ttu-id="64db9-158">Это то же самое, как выполнялись HttpGet `Edit` метод.</span><span class="sxs-lookup"><span data-stu-id="64db9-158">This is the same as what you did in the HttpGet `Edit` method.</span></span>

-  <span data-ttu-id="64db9-159">Обновляет полученную сущность инструктора значениями из связывателя модели.</span><span class="sxs-lookup"><span data-stu-id="64db9-159">Updates the retrieved Instructor entity with values from the model binder.</span></span> <span data-ttu-id="64db9-160">`TryUpdateModel` Перегрузка позволяет белого списка включаемых свойств.</span><span class="sxs-lookup"><span data-stu-id="64db9-160">The `TryUpdateModel` overload enables you to whitelist the properties you want to include.</span></span> <span data-ttu-id="64db9-161">Это предотвращает чрезмерное учета, как описано в статье [второй учебника](crud.md).</span><span class="sxs-lookup"><span data-stu-id="64db9-161">This prevents over-posting, as explained in the [second tutorial](crud.md).</span></span>

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   <span data-ttu-id="64db9-162">Если отсутствует в расположении офиса, задает свойство Instructor.OfficeAssignment значение null, чтобы связанные строки в таблице OfficeAssignment будут удалены.</span><span class="sxs-lookup"><span data-stu-id="64db9-162">If the office location is blank, sets the Instructor.OfficeAssignment property to null so that the related row in the OfficeAssignment table will be deleted.</span></span>

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- <span data-ttu-id="64db9-163">Сохраняет изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="64db9-163">Saves the changes to the database.</span></span>

### <a name="update-the-instructor-edit-view"></a><span data-ttu-id="64db9-164">Обновить представление изменить инструктора</span><span class="sxs-lookup"><span data-stu-id="64db9-164">Update the Instructor Edit view</span></span>

<span data-ttu-id="64db9-165">В *Views/Instructors/Edit.cshtml*, Добавление нового поля для редактирования в расположении офиса в конце перед **Сохранить** кнопки:</span><span class="sxs-lookup"><span data-stu-id="64db9-165">In *Views/Instructors/Edit.cshtml*, add a new field for editing the office location, at the end before the **Save** button:</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

<span data-ttu-id="64db9-166">Запустите приложение, выберите **инструкторов** , а затем щелкните **изменить** на инструктор.</span><span class="sxs-lookup"><span data-stu-id="64db9-166">Run the app, select the **Instructors** tab, and then click **Edit** on an instructor.</span></span> <span data-ttu-id="64db9-167">Изменение **офиса** и нажмите кнопку **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="64db9-167">Change the **Office Location** and click **Save**.</span></span>

![Страница изменения инструктора](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a><span data-ttu-id="64db9-169">Добавить назначения курса инструктора изменить страницу</span><span class="sxs-lookup"><span data-stu-id="64db9-169">Add Course assignments to the Instructor Edit page</span></span>

<span data-ttu-id="64db9-170">Преподаватели могут обучить любое количество курсов.</span><span class="sxs-lookup"><span data-stu-id="64db9-170">Instructors may teach any number of courses.</span></span> <span data-ttu-id="64db9-171">Теперь будет улучшить страница инструктора изменить, добавив возможность изменения курса назначения с помощью группы флажков, как показано на следующем снимке экрана:</span><span class="sxs-lookup"><span data-stu-id="64db9-171">Now you'll enhance the Instructor Edit page by adding the ability to change course assignments using a group of check boxes, as shown in the following screen shot:</span></span>

![Страница изменения инструктора курсы](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="64db9-173">Связь между сущностями Course и инструктора — многие ко многим.</span><span class="sxs-lookup"><span data-stu-id="64db9-173">The relationship between the Course and Instructor entities is many-to-many.</span></span> <span data-ttu-id="64db9-174">Чтобы добавить и удалить связи, можно добавлять и удалять сущности в и из набора сущностей CourseAssignments соединения.</span><span class="sxs-lookup"><span data-stu-id="64db9-174">To add and remove relationships, you add and remove entities to and from the CourseAssignments join entity set.</span></span>

<span data-ttu-id="64db9-175">Пользовательский Интерфейс, позволяющий изменить какие курсы инструктор назначен составляет несколько флажков.</span><span class="sxs-lookup"><span data-stu-id="64db9-175">The UI that enables you to change which courses an instructor is assigned to is a group of check boxes.</span></span> <span data-ttu-id="64db9-176">Отображается флажок для каждого курса в базе данных, а те, которые инструктора в данный момент назначен выбраны.</span><span class="sxs-lookup"><span data-stu-id="64db9-176">A check box for every course in the database is displayed, and the ones that the instructor is currently assigned to are selected.</span></span> <span data-ttu-id="64db9-177">Пользователь может установите или снимите флажки, чтобы изменить назначения курса.</span><span class="sxs-lookup"><span data-stu-id="64db9-177">The user can select or clear check boxes to change course assignments.</span></span> <span data-ttu-id="64db9-178">Если количество курсов были значительно больше, возможно, следует использовать другой метод представления данных в представлении, но используется один и тот же метод управления сущности объединения для создания и удаления связей.</span><span class="sxs-lookup"><span data-stu-id="64db9-178">If the number of courses were much greater, you would probably want to use a different method of presenting the data in the view, but you'd use the same method of manipulating a join entity to create or delete relationships.</span></span>

### <a name="update-the-instructors-controller"></a><span data-ttu-id="64db9-179">Обновление контроллера преподавателей</span><span class="sxs-lookup"><span data-stu-id="64db9-179">Update the Instructors controller</span></span>

<span data-ttu-id="64db9-180">Для предоставления данных представления для списка флажков, потребуется использовать класс модели представления.</span><span class="sxs-lookup"><span data-stu-id="64db9-180">To provide data to the view for the list of check boxes, you'll use a view model class.</span></span>

<span data-ttu-id="64db9-181">Создание *AssignedCourseData.cs* в *SchoolViewModels* папки и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="64db9-181">Create *AssignedCourseData.cs* in the *SchoolViewModels* folder and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

<span data-ttu-id="64db9-182">В *InstructorsController.cs*, замените HttpGet `Edit` метод следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="64db9-182">In *InstructorsController.cs*, replace the HttpGet `Edit` method with the following code.</span></span> <span data-ttu-id="64db9-183">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="64db9-183">The changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

<span data-ttu-id="64db9-184">Код добавляет упреждающую для `Courses` свойства навигации и вызывает новый `PopulateAssignedCourseData` метод для предоставления сведений о флажок массив с помощью `AssignedCourseData` просматривать класс модели.</span><span class="sxs-lookup"><span data-stu-id="64db9-184">The code adds eager loading for the `Courses` navigation property and calls the new `PopulateAssignedCourseData` method to provide information for the check box array using the `AssignedCourseData` view model class.</span></span>

<span data-ttu-id="64db9-185">Код в `PopulateAssignedCourseData` метод считывает все курса сущности для загрузки списка курсов, используя класс модели представления.</span><span class="sxs-lookup"><span data-stu-id="64db9-185">The code in the `PopulateAssignedCourseData` method reads through all Course entities in order to load a list of courses using the view model class.</span></span> <span data-ttu-id="64db9-186">Для каждого курса код проверяет, существует ли курса инструктора `Courses` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="64db9-186">For each course, the code checks whether the course exists in the instructor's `Courses` navigation property.</span></span> <span data-ttu-id="64db9-187">Чтобы создать эффективную подстановку при проверке, назначена ли курса инструктора, курсы, назначенные инструктора помещаются в `HashSet` коллекции.</span><span class="sxs-lookup"><span data-stu-id="64db9-187">To create efficient lookup when checking whether a course is assigned to the instructor, the courses assigned to the instructor are put into a `HashSet` collection.</span></span> <span data-ttu-id="64db9-188">`Assigned` Задано значение true для курсов инструктора назначается.</span><span class="sxs-lookup"><span data-stu-id="64db9-188">The `Assigned` property  is set to true for courses the instructor is assigned to.</span></span> <span data-ttu-id="64db9-189">Представление будет использовать это свойство для определения, какие проверки поля должны отображаться как выбранные.</span><span class="sxs-lookup"><span data-stu-id="64db9-189">The view will use this property to determine which check boxes must be displayed as selected.</span></span> <span data-ttu-id="64db9-190">Наконец, список передается в представление `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="64db9-190">Finally, the list is passed to the view in `ViewData`.</span></span>

<span data-ttu-id="64db9-191">Добавьте код, который выполняется, когда пользователь щелкает **Сохранить**.</span><span class="sxs-lookup"><span data-stu-id="64db9-191">Next, add the code that's executed when the user clicks **Save**.</span></span> <span data-ttu-id="64db9-192">Замените `EditPost` метод со следующими кода и добавьте новый метод, который обновляет `Courses` свойства навигации сущности инструктора.</span><span class="sxs-lookup"><span data-stu-id="64db9-192">Replace the `EditPost` method with the following code, and add a new method that updates the `Courses` navigation property of the Instructor entity.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

<span data-ttu-id="64db9-193">Сигнатура метода теперь отличается от HttpGet `Edit` метод, поэтому имя метода изменений из `EditPost` к `Edit`.</span><span class="sxs-lookup"><span data-stu-id="64db9-193">The method signature is now different from the HttpGet `Edit` method, so the method name changes from `EditPost` back to `Edit`.</span></span>

<span data-ttu-id="64db9-194">Так как представление не содержит коллекцию сущностей курса, связыватель модели не может автоматически обновлять `CourseAssignments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="64db9-194">Since the view doesn't have a collection of Course entities, the model binder can't automatically update the `CourseAssignments` navigation property.</span></span> <span data-ttu-id="64db9-195">Вместо использования связыватель модели для обновления `CourseAssignments` свойство навигации, выполните в новом `UpdateInstructorCourses` метод.</span><span class="sxs-lookup"><span data-stu-id="64db9-195">Instead of using the model binder to update the `CourseAssignments` navigation property, you do that in the new `UpdateInstructorCourses` method.</span></span> <span data-ttu-id="64db9-196">Поэтому необходимо исключить `CourseAssignments` свойству из привязки модели.</span><span class="sxs-lookup"><span data-stu-id="64db9-196">Therefore you need to exclude the `CourseAssignments` property from model binding.</span></span> <span data-ttu-id="64db9-197">Это не требует изменений в код, который вызывает `TryUpdateModel` так, как вы используете перегрузки список утвержденных и `CourseAssignments` отсутствует в списке include.</span><span class="sxs-lookup"><span data-stu-id="64db9-197">This doesn't require any change to the code that calls `TryUpdateModel` because you're using the whitelisting overload and `CourseAssignments` isn't in the include list.</span></span>

<span data-ttu-id="64db9-198">Если проверка не были выбраны поля, код в `UpdateInstructorCourses` инициализирует `CourseAssignments` свойство навигации с пустой коллекции и возвращает:</span><span class="sxs-lookup"><span data-stu-id="64db9-198">If no check boxes were selected, the code in `UpdateInstructorCourses` initializes the `CourseAssignments` navigation property with an empty collection and returns:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

<span data-ttu-id="64db9-199">Код реализован цикл просмотра всех курсов в базе данных и проверяет каждый курс от тех, которые в настоящее время назначен инструктора и те, которые были выбраны в представлении.</span><span class="sxs-lookup"><span data-stu-id="64db9-199">The code then loops through all courses in the database and checks each course against the ones currently assigned to the instructor versus the ones that were selected in the view.</span></span> <span data-ttu-id="64db9-200">Чтобы упростить эффективный уточняющие запросы, последние две коллекции хранятся в `HashSet` объектов.</span><span class="sxs-lookup"><span data-stu-id="64db9-200">To facilitate efficient lookups, the latter two collections are stored in `HashSet` objects.</span></span>

<span data-ttu-id="64db9-201">Если был установлен флажок для курса, но курса не находится в `Instructor.CourseAssignments` свойство навигации курса добавляется в коллекцию в свойстве навигации.</span><span class="sxs-lookup"><span data-stu-id="64db9-201">If the check box for a course was selected but the course isn't in the `Instructor.CourseAssignments` navigation property, the course is added to the collection in the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

<span data-ttu-id="64db9-202">Если флажок для курса не была выбрана, но курса в `Instructor.CourseAssignments` свойство навигации, курса удаляется из свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="64db9-202">If the check box for a course wasn't selected, but the course is in the `Instructor.CourseAssignments` navigation property, the course is removed from the navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a><span data-ttu-id="64db9-203">Обновление представлений инструктора</span><span class="sxs-lookup"><span data-stu-id="64db9-203">Update the Instructor views</span></span>

<span data-ttu-id="64db9-204">В *Views/Instructors/Edit.cshtml*, добавьте **курсы** поля с помощью массива флажки, добавив следующий код сразу после `div` элементы для **Office**  поля и перед `div` элемент для **Сохранить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="64db9-204">In *Views/Instructors/Edit.cshtml*, add a **Courses** field with an array of check boxes by adding the following code immediately after the `div` elements for the **Office** field and before the `div` element for the **Save** button.</span></span>

<a id="notepad"></a>
> [!NOTE] 
> <span data-ttu-id="64db9-205">При вставке кода в Visual Studio, разрывы строк будет изменяться в способом, нарушающим код.</span><span class="sxs-lookup"><span data-stu-id="64db9-205">When you paste the code in Visual Studio, line breaks will be changed in a way that breaks the code.</span></span>  <span data-ttu-id="64db9-206">Нажмите один раз сочетание клавиш Ctrl + Z для отмены автоматического форматирования.</span><span class="sxs-lookup"><span data-stu-id="64db9-206">Press Ctrl+Z one time to undo the automatic formatting.</span></span>  <span data-ttu-id="64db9-207">Это устранит разрывы, чтобы они выглядят как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="64db9-207">This will fix the line breaks so that they look like what you see here.</span></span> <span data-ttu-id="64db9-208">Отступы не должен быть идеальным, но `@</tr><tr>`, `@:<td>`, `@:</td>`, и `@:</tr>` строки должны быть в одной строке показанного или вы получите ошибку во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="64db9-208">The indentation doesn't have to be perfect, but the `@</tr><tr>`, `@:<td>`, `@:</td>`, and `@:</tr>` lines must each be on a single line as shown or you'll get a runtime error.</span></span> <span data-ttu-id="64db9-209">С блоком выбран новый код нажмите клавишу Tab трижды Чтобы выровнять новый код с существующим кодом.</span><span class="sxs-lookup"><span data-stu-id="64db9-209">With the block of new code selected, press Tab three times to line up the new code with the existing code.</span></span> <span data-ttu-id="64db9-210">Можно проверить состояние этой проблемы [здесь](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span><span class="sxs-lookup"><span data-stu-id="64db9-210">You can check the status of this problem [here](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

<span data-ttu-id="64db9-211">Этот код создает таблицу HTML, который содержит три столбца.</span><span class="sxs-lookup"><span data-stu-id="64db9-211">This code creates an HTML table that has three columns.</span></span> <span data-ttu-id="64db9-212">В каждом столбце является типа "флажок" следуют подпись, состоящая из номер и название.</span><span class="sxs-lookup"><span data-stu-id="64db9-212">In each column is a check box followed by a caption that consists of the course number and title.</span></span> <span data-ttu-id="64db9-213">Все флажки совпадают («selectedCourses»), который уведомляет связывателя модели, они должны рассматриваться как группа.</span><span class="sxs-lookup"><span data-stu-id="64db9-213">The check boxes all have the same name ("selectedCourses"), which informs the model binder that they are to be treated as a group.</span></span> <span data-ttu-id="64db9-214">Недопустимое значение атрибута каждого флажка присвоено значение `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="64db9-214">The value attribute of each check box is set to the value of `CourseID`.</span></span> <span data-ttu-id="64db9-215">При отправке страницы связывателя модели передает массив, состоящий из контроллера `CourseID` значения только флажки установлены.</span><span class="sxs-lookup"><span data-stu-id="64db9-215">When the page is posted, the model binder passes an array to the controller that consists of the `CourseID` values for only the check boxes which are selected.</span></span>

<span data-ttu-id="64db9-216">При первоначальном отображении флажки тех, которые назначены инструктора курсов проверки атрибутов, который выбирает их (открывается, когда их возвращен).</span><span class="sxs-lookup"><span data-stu-id="64db9-216">When the check boxes are initially rendered, those that are for courses assigned to the instructor have checked attributes, which selects them (displays them checked).</span></span>

<span data-ttu-id="64db9-217">Запустите приложение, выберите **инструкторов** и нажмите кнопку **изменить** на инструктор, чтобы увидеть **изменить** страницы.</span><span class="sxs-lookup"><span data-stu-id="64db9-217">Run the app, select the **Instructors** tab, and click **Edit** on an instructor to see the **Edit** page.</span></span>

![Страница изменения инструктора курсы](update-related-data/_static/instructor-edit-courses.png)

<span data-ttu-id="64db9-219">Некоторые курса назначения и нажмите кнопку Сохранить.</span><span class="sxs-lookup"><span data-stu-id="64db9-219">Change some course assignments and click Save.</span></span> <span data-ttu-id="64db9-220">Вносимые изменения отражаются на странице индекса.</span><span class="sxs-lookup"><span data-stu-id="64db9-220">The changes you make are reflected on the Index page.</span></span>

> [!NOTE] 
> <span data-ttu-id="64db9-221">Подходом, который используется здесь для изменения данных курса инструктора удобно использовать, когда имеется ограниченное число курсов.</span><span class="sxs-lookup"><span data-stu-id="64db9-221">The approach taken here to edit instructor course data works well when there is a limited number of courses.</span></span> <span data-ttu-id="64db9-222">Для коллекций, которые могут быть значительно большими другой пользовательский Интерфейс и другой метод обновления не требуются.</span><span class="sxs-lookup"><span data-stu-id="64db9-222">For collections that are much larger, a different UI and a different updating method would be required.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="64db9-223">Обновление страницы удаления</span><span class="sxs-lookup"><span data-stu-id="64db9-223">Update the Delete page</span></span>

<span data-ttu-id="64db9-224">В *InstructorsController.cs*, удалить `DeleteConfirmed` метод и вставьте следующий код на его месте.</span><span class="sxs-lookup"><span data-stu-id="64db9-224">In *InstructorsController.cs*, delete the `DeleteConfirmed` method and insert the following code in its place.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

<span data-ttu-id="64db9-225">Этот код выполняет следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="64db9-225">This code makes the following changes:</span></span>

* <span data-ttu-id="64db9-226">Загрузка для eager выполняет `CourseAssignments` свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="64db9-226">Does eager loading for the `CourseAssignments` navigation property.</span></span>  <span data-ttu-id="64db9-227">Необходимо включить это или EF не будет знать о связанных `CourseAssignment` сущности и не удалите их.</span><span class="sxs-lookup"><span data-stu-id="64db9-227">You have to include this or EF won't know about related `CourseAssignment` entities and won't delete them.</span></span>  <span data-ttu-id="64db9-228">Чтобы избежать необходимости читать их здесь выполняется настройка базы данных каскадное удаление.</span><span class="sxs-lookup"><span data-stu-id="64db9-228">To avoid needing to read them here you could configure cascade delete in the database.</span></span>

* <span data-ttu-id="64db9-229">Если администратор любые отделы назначается инструктора для удаления, удаляет инструктора назначения из тех отделов.</span><span class="sxs-lookup"><span data-stu-id="64db9-229">If the instructor to be deleted is assigned as administrator of any departments, removes the instructor assignment from those departments.</span></span>

## <a name="add-office-location-and-courses-to-the-create-page"></a><span data-ttu-id="64db9-230">Добавление страницы создания офиса и курсов</span><span class="sxs-lookup"><span data-stu-id="64db9-230">Add office location and courses to the Create page</span></span>

<span data-ttu-id="64db9-231">В *InstructorsController.cs*, удалите HttpGet и HttpPost `Create` методы, а затем добавьте следующий код вместо них:</span><span class="sxs-lookup"><span data-stu-id="64db9-231">In *InstructorsController.cs*, delete the HttpGet and HttpPost `Create` methods, and then add the following code in their place:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

<span data-ttu-id="64db9-232">Это тот же способом, что для `Edit` выбранных методов, за исключением того, которые изначально не курсов.</span><span class="sxs-lookup"><span data-stu-id="64db9-232">This code is similar to what you saw for the `Edit` methods except that initially no courses are selected.</span></span> <span data-ttu-id="64db9-233">HttpGet `Create` вызовы метода `PopulateAssignedCourseData` метод не потому, что может быть курсов, однако в выбранных заказ для пустой коллекции для обеспечения `foreach` цикла в представлении (в противном случае просмотр кода, вызывает исключение null reference).</span><span class="sxs-lookup"><span data-stu-id="64db9-233">The HttpGet `Create` method calls the `PopulateAssignedCourseData` method not because there might be courses selected but in order to provide an empty collection for the `foreach` loop in the view (otherwise the view code would throw a null reference exception).</span></span>

<span data-ttu-id="64db9-234">HttpPost `Create` метод добавляет каждого выбранного курса для `CourseAssignments` свойство навигации, прежде чем он проверяет наличие ошибок проверки и добавляет новый инструктор в базу данных.</span><span class="sxs-lookup"><span data-stu-id="64db9-234">The HttpPost `Create` method adds each selected course to the `CourseAssignments` navigation property before it checks for validation errors and adds the new instructor to the database.</span></span> <span data-ttu-id="64db9-235">Курсы добавляются, даже если есть какие-либо курс параметры, которые были внесены автоматическое восстановление при наличии ошибки модели (например, пользователь с ключами произошла недопустимая дата), и отобразится страница с сообщением об ошибке, ошибки модели.</span><span class="sxs-lookup"><span data-stu-id="64db9-235">Courses are added even if there are model errors so that when there are model errors (for an example, the user keyed an invalid date), and the page is redisplayed with an error message, any course selections that were made are automatically restored.</span></span>

<span data-ttu-id="64db9-236">Обратите внимание на то, чтобы иметь возможность добавить курсы для `CourseAssignments` свойство навигации было нужно инициализировать свойство как пустой коллекции:</span><span class="sxs-lookup"><span data-stu-id="64db9-236">Notice that in order to be able to add courses to the `CourseAssignments` navigation property you have to initialize the property as an empty collection:</span></span>

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

<span data-ttu-id="64db9-237">Вместо этого код контроллера вы могли сделать инструктора модели, изменив метод считывания свойства для автоматического создания коллекции, если он не существует, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="64db9-237">As an alternative to doing this in controller code, you could do it in the Instructor model by changing the property getter to automatically create the collection if it doesn't exist, as shown in the following example:</span></span>

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

<span data-ttu-id="64db9-238">При изменении `CourseAssignments` свойства таким образом, можно удалить код инициализации явное свойство в контроллере.</span><span class="sxs-lookup"><span data-stu-id="64db9-238">If you modify the `CourseAssignments` property in this way, you can remove the explicit property initialization code in the controller.</span></span>

<span data-ttu-id="64db9-239">В *Views/Instructor/Create.cshtml*, добавьте текстовое поле расположения office и установите флажки для курсов перед кнопки "Отправить".</span><span class="sxs-lookup"><span data-stu-id="64db9-239">In *Views/Instructor/Create.cshtml*, add an office location text box and check boxes for courses before the Submit button.</span></span> <span data-ttu-id="64db9-240">В случае страницы правки [форматировании Если Visual Studio переформатирует код при вставке](#notepad).</span><span class="sxs-lookup"><span data-stu-id="64db9-240">As in the case of the Edit page, [fix the formatting if Visual Studio reformats the code when you paste it](#notepad).</span></span>

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

<span data-ttu-id="64db9-241">Проверьте, выполнение приложения и создание инструктор.</span><span class="sxs-lookup"><span data-stu-id="64db9-241">Test by running the app and creating an instructor.</span></span> 

## <a name="handling-transactions"></a><span data-ttu-id="64db9-242">Обработка транзакций</span><span class="sxs-lookup"><span data-stu-id="64db9-242">Handling Transactions</span></span>

<span data-ttu-id="64db9-243">Как описано в статье [CRUD учебника](crud.md), Entity Framework неявно реализует транзакции.</span><span class="sxs-lookup"><span data-stu-id="64db9-243">As explained in the [CRUD tutorial](crud.md), the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="64db9-244">См. в сценариях, где требуется дополнительный контроль — например, если вы хотите включить действия, выполненные за пределами платформы Entity Framework транзакции-- [транзакции](https://docs.microsoft.com/ef/core/saving/transactions).</span><span class="sxs-lookup"><span data-stu-id="64db9-244">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="summary"></a><span data-ttu-id="64db9-245">Сводка</span><span class="sxs-lookup"><span data-stu-id="64db9-245">Summary</span></span>

<span data-ttu-id="64db9-246">Введение в работу со связанными данными завершена.</span><span class="sxs-lookup"><span data-stu-id="64db9-246">You have now completed the introduction to working with related data.</span></span> <span data-ttu-id="64db9-247">В следующем уроке вы увидите, как обрабатывать конфликты параллелизма.</span><span class="sxs-lookup"><span data-stu-id="64db9-247">In the next tutorial you'll see how to handle concurrency conflicts.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="64db9-248">[Назад](read-related-data.md)
[Вперед](concurrency.md)</span><span class="sxs-lookup"><span data-stu-id="64db9-248">[Previous](read-related-data.md)
[Next](concurrency.md)</span></span>  
