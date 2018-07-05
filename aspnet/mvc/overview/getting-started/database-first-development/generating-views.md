---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'EF Database First с ASP.NET MVC: создание представлений | Документация Майкрософт'
author: tfitzmac
description: С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных. Этот учебник seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 58f8be181f80f5b27353e9ef5cafe48bcb370e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399614"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="8e34c-104">EF Database First с ASP.NET MVC: создание представлений</span><span class="sxs-lookup"><span data-stu-id="8e34c-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="8e34c-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8e34c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8e34c-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложение, которое предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="8e34c-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="8e34c-107">В этой серии руководств показано, как автоматически создавать код, позволяющий пользователям для отображения, изменения и создавать и удалять данные, находящиеся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="8e34c-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="8e34c-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="8e34c-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="8e34c-109">Части этой серии рассматривается создание контроллеров и представлений с помощью формирования шаблонов ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8e34c-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="8e34c-110">Добавление элемента формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="8e34c-110">Add scaffold</span></span>

<span data-ttu-id="8e34c-111">Все готово для создания кода, который предоставит операций стандартные данные о классах модели.</span><span class="sxs-lookup"><span data-stu-id="8e34c-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="8e34c-112">Добавьте код, добавив элемент формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="8e34c-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="8e34c-113">Существует много параметров для типа параметра формирования шаблонов, которые можно добавить; в этом руководстве каркаса будет включать контроллера и представлений, которые соответствуют Student и регистрации модели, для которых вы создали в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="8e34c-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="8e34c-114">Для поддержания согласованности в проекте, вы добавите новый контроллер для существующего **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="8e34c-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="8e34c-115">Щелкните правой кнопкой мыши **контроллеров** и выберите **добавить** — **создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="8e34c-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Добавление элемента формирования шаблонов](generating-views/_static/image1.png)

<span data-ttu-id="8e34c-117">Выберите **контроллер MVC 5 с представлениями, использующий Entity Framework** параметр.</span><span class="sxs-lookup"><span data-stu-id="8e34c-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="8e34c-118">Этот параметр будет создавать контроллера и представлений для обновления, удаления, создания и отображения данных в модели.</span><span class="sxs-lookup"><span data-stu-id="8e34c-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Добавьте контроллер mvc](generating-views/_static/image2.png)

<span data-ttu-id="8e34c-120">Выберите **учащихся** для класс модели и выберите **ContosoUniversityEntities** для класса контекста.</span><span class="sxs-lookup"><span data-stu-id="8e34c-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="8e34c-121">Оставьте имя контроллера как **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="8e34c-121">Keep the controller name as **StudentsController**,</span></span>

![Укажите контроллер](generating-views/_static/image3.png)

<span data-ttu-id="8e34c-123">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="8e34c-123">Click **Add**.</span></span>

<span data-ttu-id="8e34c-124">Если произошла ошибка, возможно, так как вы не создавали проекта в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="8e34c-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="8e34c-125">Если это так, попробуйте выполнить сборку проекта и затем снова добавьте элемента формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="8e34c-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="8e34c-126">После завершения процесса создания кода в проекте появится новый контроллер и представления.</span><span class="sxs-lookup"><span data-stu-id="8e34c-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Показать представления](generating-views/_static/image4.png)

<span data-ttu-id="8e34c-128">Снова выполните те же действия, но добавить элемент формирования шаблонов для регистрации класса.</span><span class="sxs-lookup"><span data-stu-id="8e34c-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="8e34c-129">После завершения вы получите **EnrollmentsController.cs** файл и папку в узле **представления** с именем **регистрациями** с Create, Delete, сведений, редактирования и индекса Представления.</span><span class="sxs-lookup"><span data-stu-id="8e34c-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Показать представления](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="8e34c-131">Добавление ссылок на новые представления</span><span class="sxs-lookup"><span data-stu-id="8e34c-131">Add links to new views</span></span>

<span data-ttu-id="8e34c-132">Для упрощения перехода на новые представления, можно добавить несколько гиперссылок в индексе представления для учащихся и регистрации.</span><span class="sxs-lookup"><span data-stu-id="8e34c-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="8e34c-133">Откройте файл в **Views/Home/Index.cshtml**, который является домашней страницей для вашего сайта.</span><span class="sxs-lookup"><span data-stu-id="8e34c-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="8e34c-134">Добавьте следующий код ниже jumbotron.</span><span class="sxs-lookup"><span data-stu-id="8e34c-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="8e34c-135">Для метода ActionLink первый параметр является текст, отображаемый в ссылке.</span><span class="sxs-lookup"><span data-stu-id="8e34c-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="8e34c-136">Второй параметр — действие, а третий параметр — имя контроллера.</span><span class="sxs-lookup"><span data-stu-id="8e34c-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="8e34c-137">Например первая ссылка указывает на действие индекса в StudentsController.</span><span class="sxs-lookup"><span data-stu-id="8e34c-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="8e34c-138">Фактический hyperlink создается на основе этих значений.</span><span class="sxs-lookup"><span data-stu-id="8e34c-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="8e34c-139">Первую ссылку в конечном счете пользователи открывают **Index.cshtml** файл включен в **представления/учащихся** папки.</span><span class="sxs-lookup"><span data-stu-id="8e34c-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="8e34c-140">Отображение представлений учащегося</span><span class="sxs-lookup"><span data-stu-id="8e34c-140">Display student views</span></span>

<span data-ttu-id="8e34c-141">Вы проверите, что код, добавленный в проект правильно отображает список учащихся и позволяет пользователям изменение, создание или удаление записей учащихся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8e34c-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="8e34c-142">Щелкните правой кнопкой мыши **Views/Home/Index.cshtml** файла и выберите **просмотреть в браузере**.</span><span class="sxs-lookup"><span data-stu-id="8e34c-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="8e34c-143">На этой странице щелкните ссылку для списка учащихся.</span><span class="sxs-lookup"><span data-stu-id="8e34c-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="8e34c-144">На этой странице Обратите внимание на список учащихся и ссылки, чтобы изменить эти данные.</span><span class="sxs-lookup"><span data-stu-id="8e34c-144">On this page, notice the list of the students and links to modify this data.</span></span>

![список учащихся](generating-views/_static/image7.png)

<span data-ttu-id="8e34c-146">Нажмите кнопку **Create New** ссылку и ввести некоторые значения для нового учащегося.</span><span class="sxs-lookup"><span data-stu-id="8e34c-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Создание нового учащегося](generating-views/_static/image8.png)

<span data-ttu-id="8e34c-148">Нажмите кнопку **создать**и обратите внимание, что нового учащегося добавляется в список.</span><span class="sxs-lookup"><span data-stu-id="8e34c-148">Click **Create**, and notice the new student is added to your list.</span></span>

![список с помощью нового учащегося](generating-views/_static/image9.png)

<span data-ttu-id="8e34c-150">Выберите **изменить** связать и изменить некоторые значения для учащегося.</span><span class="sxs-lookup"><span data-stu-id="8e34c-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![Изменение параметров учащегося](generating-views/_static/image10.png)

<span data-ttu-id="8e34c-152">Нажмите кнопку **Сохранить**и обратите внимание, что запись учащегося была изменена.</span><span class="sxs-lookup"><span data-stu-id="8e34c-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="8e34c-153">Наконец, выберите **удалить** ссылку и убедитесь, что Вы действительно хотите удалить запись, щелкнув **удалить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="8e34c-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![удалить учащихся](generating-views/_static/image11.png)

<span data-ttu-id="8e34c-155">Без написания кода, вы добавили представления, которые выполняют общие операции с данными в таблице учащихся.</span><span class="sxs-lookup"><span data-stu-id="8e34c-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="8e34c-156">Вы заметите, что текстовую метку для поля на основе базы данных свойства (такие как **LastName**) который не обязательно нужно отобразить на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="8e34c-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="8e34c-157">Например, вы можете предпочесть метку, которая будет **Фамилия**.</span><span class="sxs-lookup"><span data-stu-id="8e34c-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="8e34c-158">Отображение проблема будет решена позже в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="8e34c-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="8e34c-159">Отображение представлений регистрации</span><span class="sxs-lookup"><span data-stu-id="8e34c-159">Display enrollment views</span></span>

<span data-ttu-id="8e34c-160">Базы данных включает в себя отношение "один ко многим" между таблицами Student и регистрации и отношение "один ко многим" между таблицами Course и Enrollment.</span><span class="sxs-lookup"><span data-stu-id="8e34c-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="8e34c-161">Представления для регистрации правильно обрабатывать эти связи.</span><span class="sxs-lookup"><span data-stu-id="8e34c-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="8e34c-162">Перейдите на домашнюю страницу сайта и нажмите кнопку **список регистраций** ссылку и затем **Create New** ссылку.</span><span class="sxs-lookup"><span data-stu-id="8e34c-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="8e34c-163">Представление отображает форму для создания новой записи регистрации.</span><span class="sxs-lookup"><span data-stu-id="8e34c-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="8e34c-164">В частности Обратите внимание на то, что форма содержит два раскрывающихся списков, которые заполняются значениями из связанных таблиц.</span><span class="sxs-lookup"><span data-stu-id="8e34c-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Создание регистрации](generating-views/_static/image12.png)

<span data-ttu-id="8e34c-166">Кроме того предоставленных значений автоматически применяется проверка зависимости от типа данных поля.</span><span class="sxs-lookup"><span data-stu-id="8e34c-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="8e34c-167">Корпоративного уровня с числовым, поэтому сообщение об ошибке отображается при попытке предоставить несовместимое значение.</span><span class="sxs-lookup"><span data-stu-id="8e34c-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![сообщение проверки](generating-views/_static/image13.png)

<span data-ttu-id="8e34c-169">Вы убедились, что автоматически создается представления позволяют пользователям работать с данными в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8e34c-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="8e34c-170">В следующем руководстве этой серии будет обновлять базу данных и внесите соответствующие изменения в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="8e34c-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8e34c-171">[Назад](creating-the-web-application.md)
> [Вперед](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="8e34c-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
