---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: Учебник. Создать представления для EF Database First с помощью приложения ASP.NET MVC
description: Эта статья посвящена Создание контроллеров и представлений с помощью формирования шаблонов ASP.NET.
author: Rick-Anderson
ms.author: riande
ms.date: 01/23/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e1f6646cdf10d293268b92f44b018709e70c0f86
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889786"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="503be-103">Учебник. Создать представления для EF Database First с помощью приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="503be-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="503be-104">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="503be-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="503be-105">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="503be-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="503be-106">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="503be-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="503be-107">Эта статья посвящена Создание контроллеров и представлений с помощью формирования шаблонов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="503be-107">This article focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="503be-108">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="503be-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="503be-109">Добавление элемента формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="503be-109">Add scaffold</span></span>
> * <span data-ttu-id="503be-110">Добавление ссылок на новые представления</span><span class="sxs-lookup"><span data-stu-id="503be-110">Add links to new views</span></span>
> * <span data-ttu-id="503be-111">Отображение представлений учащегося</span><span class="sxs-lookup"><span data-stu-id="503be-111">Display student views</span></span>
> * <span data-ttu-id="503be-112">Отображение представлений регистрации</span><span class="sxs-lookup"><span data-stu-id="503be-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="503be-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="503be-113">Prerequisite</span></span>

* [<span data-ttu-id="503be-114">Создание веб-приложение и модели данных</span><span class="sxs-lookup"><span data-stu-id="503be-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="503be-115">Добавление элемента формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="503be-115">Add scaffold</span></span>

<span data-ttu-id="503be-116">Все готово для создания кода, который предоставит операций стандартные данные о классах модели.</span><span class="sxs-lookup"><span data-stu-id="503be-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="503be-117">Добавьте код, добавив элемент формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="503be-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="503be-118">Существует много параметров для типа параметра формирования шаблонов, которые можно добавить; в этом руководстве каркаса будет включать контроллера и представлений, которые соответствуют Student и регистрации модели, для которых вы создали в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="503be-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="503be-119">Для поддержания согласованности в проекте, вы добавите новый контроллер для существующего **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="503be-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="503be-120">Щелкните правой кнопкой мыши **контроллеров** и выберите **добавить** > **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="503be-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="503be-121">Выберите **контроллер MVC 5 с представлениями, использующий Entity Framework** параметр.</span><span class="sxs-lookup"><span data-stu-id="503be-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="503be-122">Этот параметр будет создавать контроллера и представлений для обновления, удаления, создания и отображения данных в модели.</span><span class="sxs-lookup"><span data-stu-id="503be-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Добавьте контроллер mvc](generating-views/_static/image2.png)

<span data-ttu-id="503be-124">Выберите **учащегося (ContosoSite.Models)** класс модели и выберите команду **ContosoUniversityDataEntities (ContosoSite.Models)** для класса контекста.</span><span class="sxs-lookup"><span data-stu-id="503be-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="503be-125">Оставьте имя контроллера как **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="503be-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="503be-126">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="503be-126">Click **Add**.</span></span>

<span data-ttu-id="503be-127">Если произошла ошибка, возможно, так как вы не создавали проекта в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="503be-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="503be-128">Если это так, попробуйте выполнить сборку проекта и затем снова добавьте элемента формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="503be-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="503be-129">После завершения процесса создания кода вы увидите новый контроллер и представления в вашем проекте **контроллеров** и **представления** > **учащихся** папки .</span><span class="sxs-lookup"><span data-stu-id="503be-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>


<span data-ttu-id="503be-130">Снова выполните те же действия, но добавить элемент формирования шаблонов для **регистрации** класса.</span><span class="sxs-lookup"><span data-stu-id="503be-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="503be-131">По завершении у вас есть **EnrollmentsController.cs** файл и папку в узле **представления** с именем **регистрациями** с представлениями Create, Delete, сведений, редактирования и индекс.</span><span class="sxs-lookup"><span data-stu-id="503be-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="503be-132">Добавление ссылок на новые представления</span><span class="sxs-lookup"><span data-stu-id="503be-132">Add links to new views</span></span>

<span data-ttu-id="503be-133">Для упрощения перехода на новые представления, можно добавить несколько гиперссылок в индексе представления для учащихся и регистрации.</span><span class="sxs-lookup"><span data-stu-id="503be-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="503be-134">Откройте файл в **представления** > **домашней** > *Index.cshtml*, который является домашней страницей для вашего сайта.</span><span class="sxs-lookup"><span data-stu-id="503be-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="503be-135">Добавьте следующий код ниже jumbotron.</span><span class="sxs-lookup"><span data-stu-id="503be-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="503be-136">Для метода ActionLink первый параметр является текст, отображаемый в ссылке.</span><span class="sxs-lookup"><span data-stu-id="503be-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="503be-137">Второй параметр — действие, а третий параметр — имя контроллера.</span><span class="sxs-lookup"><span data-stu-id="503be-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="503be-138">Например первая ссылка указывает на действие индекса в StudentsController.</span><span class="sxs-lookup"><span data-stu-id="503be-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="503be-139">Фактический hyperlink создается на основе этих значений.</span><span class="sxs-lookup"><span data-stu-id="503be-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="503be-140">Первую ссылку в конечном счете пользователи открывают **Index.cshtml** файл включен в **представления/учащихся** папки.</span><span class="sxs-lookup"><span data-stu-id="503be-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="503be-141">Отображение представлений учащегося</span><span class="sxs-lookup"><span data-stu-id="503be-141">Display student views</span></span>

<span data-ttu-id="503be-142">Вы проверите, что код, добавленный в проект правильно отображает список учащихся и позволяет пользователям изменение, создание или удаление записей учащихся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="503be-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="503be-143">Щелкните правой кнопкой мыши **представления** > **Главная** > *Index.cshtml* файла и выберите **просмотреть в браузере**.</span><span class="sxs-lookup"><span data-stu-id="503be-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="503be-144">На домашней странице приложения выберите **список учащихся**.</span><span class="sxs-lookup"><span data-stu-id="503be-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="503be-145">На **индекс** странице, обратите внимание на список учащихся и ссылки, чтобы изменить эти данные.</span><span class="sxs-lookup"><span data-stu-id="503be-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="503be-146">Выберите **Create New** ссылку и ввести некоторые значения для нового учащегося.</span><span class="sxs-lookup"><span data-stu-id="503be-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="503be-147">Нажмите кнопку **создать**и обратите внимание, что нового учащегося добавляется в список.</span><span class="sxs-lookup"><span data-stu-id="503be-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="503be-148">Вернитесь на **индекс** выберите **изменить** связать и изменить некоторые значения для учащегося.</span><span class="sxs-lookup"><span data-stu-id="503be-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="503be-149">Нажмите кнопку **Сохранить**и обратите внимание, что запись учащегося была изменена.</span><span class="sxs-lookup"><span data-stu-id="503be-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="503be-150">Наконец, выберите **удалить** ссылку и убедитесь, что Вы действительно хотите удалить запись, щелкнув **удалить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="503be-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="503be-151">Без написания кода, вы добавили представления, которые выполняют общие операции с данными в таблице учащихся.</span><span class="sxs-lookup"><span data-stu-id="503be-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="503be-152">Вы заметите, что текстовую метку для поля на основе базы данных свойства (такие как **LastName**) который не обязательно нужно отобразить на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="503be-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="503be-153">Например, вы можете предпочесть метку, которая будет **Фамилия**.</span><span class="sxs-lookup"><span data-stu-id="503be-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="503be-154">Отображение проблема будет решена позже в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="503be-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="503be-155">Отображение представлений регистрации</span><span class="sxs-lookup"><span data-stu-id="503be-155">Display enrollment views</span></span>

<span data-ttu-id="503be-156">Базы данных включает в себя отношение "один ко многим" между таблицами Student и регистрации и отношение "один ко многим" между таблицами Course и Enrollment.</span><span class="sxs-lookup"><span data-stu-id="503be-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="503be-157">Представления для регистрации правильно обрабатывать эти связи.</span><span class="sxs-lookup"><span data-stu-id="503be-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="503be-158">Перейдите на домашнюю страницу сайта и нажмите кнопку **список регистраций** ссылку и затем **Create New** ссылку.</span><span class="sxs-lookup"><span data-stu-id="503be-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="503be-159">Представление отображает форму для создания новой записи регистрации.</span><span class="sxs-lookup"><span data-stu-id="503be-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="503be-160">В частности, обратите внимание, что форма содержит **CourseID** стрелку раскрывающегося списка и **StudentID** стрелку раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="503be-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="503be-161">Как заполняются значениями из связанных таблиц.</span><span class="sxs-lookup"><span data-stu-id="503be-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="503be-162">Кроме того предоставленных значений автоматически применяется проверка зависимости от типа данных поля.</span><span class="sxs-lookup"><span data-stu-id="503be-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="503be-163">**Оценка** требуется номер, поэтому сообщение об ошибке отображается при попытке предоставить несовместимое значение: *Поле корпоративного класса должно быть числом.*</span><span class="sxs-lookup"><span data-stu-id="503be-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="503be-164">Вы убедились, что автоматически создается представления позволяют пользователям работать с данными в базе данных.</span><span class="sxs-lookup"><span data-stu-id="503be-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="503be-165">В следующем руководстве этой серии будет обновлять базу данных и внесите соответствующие изменения в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="503be-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="503be-166">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="503be-166">Next steps</span></span>

<span data-ttu-id="503be-167">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="503be-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="503be-168">Добавлена каркаса</span><span class="sxs-lookup"><span data-stu-id="503be-168">Added scaffold</span></span>
> * <span data-ttu-id="503be-169">Добавлены ссылки на новые представления</span><span class="sxs-lookup"><span data-stu-id="503be-169">Added links to new views</span></span>
> * <span data-ttu-id="503be-170">Представления отображается учащегося</span><span class="sxs-lookup"><span data-stu-id="503be-170">Displayed student views</span></span>
> * <span data-ttu-id="503be-171">Представления отображается регистрации</span><span class="sxs-lookup"><span data-stu-id="503be-171">Displayed enrollment views</span></span>

<span data-ttu-id="503be-172">Перейдите к следующей статье, чтобы узнать, как изменение базы данных.</span><span class="sxs-lookup"><span data-stu-id="503be-172">Advance to the next article to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="503be-173">Изменение базы данных</span><span class="sxs-lookup"><span data-stu-id="503be-173">Change the database</span></span>](changing-the-database.md)