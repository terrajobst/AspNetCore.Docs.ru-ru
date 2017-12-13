---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: "EF базы данных сначала с ASP.NET MVC: создание представлений | Документы Microsoft"
author: tfitzmac
description: "С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных. Этот учебник seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: 5fccb3c56af0945ec448becff777a3e92dc160d7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a><span data-ttu-id="2ad54-104">EF базы данных сначала с ASP.NET MVC: создание представлений</span><span class="sxs-lookup"><span data-stu-id="2ad54-104">EF Database First with ASP.NET MVC: Generating Views</span></span>
====================
<span data-ttu-id="2ad54-105">по [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2ad54-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2ad54-106">С помощью MVC, Entity Framework и формирование шаблонов ASP.NET, можно создать веб-приложения, который предоставляет интерфейс для существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="2ad54-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="2ad54-107">Этот ряд учебника показано, как автоматически создают код, который дает пользователям возможность отображения, изменения, создания и удаления данных, которые хранятся в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="2ad54-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="2ad54-108">Созданный код соответствует столбцам в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="2ad54-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="2ad54-109">Эта часть ряда внимание уделяется использованию формирование шаблонов ASP.NET для формирования контроллеров и представлений.</span><span class="sxs-lookup"><span data-stu-id="2ad54-109">This part of the series focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>


## <a name="add-scaffold"></a><span data-ttu-id="2ad54-110">Добавление формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="2ad54-110">Add scaffold</span></span>

<span data-ttu-id="2ad54-111">Все готово для создания кода, предоставляющий операций стандартные данные для классов модели.</span><span class="sxs-lookup"><span data-stu-id="2ad54-111">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="2ad54-112">Добавьте код, добавив элемент формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="2ad54-112">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="2ad54-113">Существует много параметров для типа формирование шаблонов, которые можно добавить; в этом учебнике представление формирования будет содержать контроллера и представления, которые соответствуют студента и регистрации модели, созданный в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="2ad54-113">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="2ad54-114">Для поддержания согласованности в проекте, добавит новый контроллер для существующего **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="2ad54-114">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="2ad54-115">Щелкните правой кнопкой мыши **контроллеров** папки, а затем выберите **добавить** — **новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="2ad54-115">Right-click the **Controllers** folder, and select **Add** – **New Scaffolded Item**.</span></span>

![Добавление формирования шаблонов](generating-views/_static/image1.png)

<span data-ttu-id="2ad54-117">Выберите **контроллер MVC 5 с представлениями, использующий Entity Framework** параметр.</span><span class="sxs-lookup"><span data-stu-id="2ad54-117">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="2ad54-118">Этот параметр создаст контроллера и представления для обновления, удаления, создания и отображения данных в модели.</span><span class="sxs-lookup"><span data-stu-id="2ad54-118">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![добавить контроллер mvc](generating-views/_static/image2.png)

<span data-ttu-id="2ad54-120">Выберите **студента** класс модели, а затем выберите **ContosoUniversityEntities** для класса контекста.</span><span class="sxs-lookup"><span data-stu-id="2ad54-120">Select **Student** for the model class, and select the **ContosoUniversityEntities** for the context class.</span></span> <span data-ttu-id="2ad54-121">Оставьте имя контроллера, как **StudentsController**,</span><span class="sxs-lookup"><span data-stu-id="2ad54-121">Keep the controller name as **StudentsController**,</span></span>

![Укажите контроллер](generating-views/_static/image3.png)

<span data-ttu-id="2ad54-123">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="2ad54-123">Click **Add**.</span></span>

<span data-ttu-id="2ad54-124">Если появляется ошибка, возможно, так как не удалось построения проекта в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="2ad54-124">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="2ad54-125">В этом случае попробуйте построить проект и затем снова добавить элемент формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="2ad54-125">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="2ad54-126">После завершения процесса создания кода в проекте появится новый контроллер и представления.</span><span class="sxs-lookup"><span data-stu-id="2ad54-126">After the code generation process is complete, you will see a new controller and views in your project.</span></span>

![Отображение представления](generating-views/_static/image4.png)

<span data-ttu-id="2ad54-128">Снова выполните те же действия, но добавить элемент формирования шаблонов для регистрации класса.</span><span class="sxs-lookup"><span data-stu-id="2ad54-128">Perform the same steps again, but add a scaffold for the Enrollment class.</span></span> <span data-ttu-id="2ad54-129">После завершения, должны иметь **EnrollmentsController.cs** файл и папку в папке **представления** с именем **регистраций** с Create, Delete, сведения, редактирования и индекса Представления.</span><span class="sxs-lookup"><span data-stu-id="2ad54-129">When finished, you should have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

![Отображение представления](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a><span data-ttu-id="2ad54-131">Добавьте ссылки на новые представления</span><span class="sxs-lookup"><span data-stu-id="2ad54-131">Add links to new views</span></span>

<span data-ttu-id="2ad54-132">Для упрощения перехода на новые представления, можно добавить несколько гиперссылки к представлениям индекс для студентов и регистрации.</span><span class="sxs-lookup"><span data-stu-id="2ad54-132">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="2ad54-133">Открыть файл в **Views/Home/Index.cshtml**, который является домашней страницей для веб-узла.</span><span class="sxs-lookup"><span data-stu-id="2ad54-133">Open the file at **Views/Home/Index.cshtml**, which is the home page for your site.</span></span> <span data-ttu-id="2ad54-134">Добавьте следующий код под jumbotron.</span><span class="sxs-lookup"><span data-stu-id="2ad54-134">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="2ad54-135">Метод ActionLink первый параметр является текст, отображаемый в ссылке.</span><span class="sxs-lookup"><span data-stu-id="2ad54-135">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="2ad54-136">Второй параметр — действие, а третий параметр — имя контроллера.</span><span class="sxs-lookup"><span data-stu-id="2ad54-136">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="2ad54-137">Например первая ссылка указывает на действие индекса в StudentsController.</span><span class="sxs-lookup"><span data-stu-id="2ad54-137">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="2ad54-138">Фактический гиперссылки состоит из следующих значений.</span><span class="sxs-lookup"><span data-stu-id="2ad54-138">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="2ad54-139">Первую ссылку в конечном счете принимает пользователям **Index.cshtml** файл включен в **представления/учащихся** папки.</span><span class="sxs-lookup"><span data-stu-id="2ad54-139">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="2ad54-140">Отображение представлений студента</span><span class="sxs-lookup"><span data-stu-id="2ad54-140">Display student views</span></span>

<span data-ttu-id="2ad54-141">Вы проверите код добавлен в проект правильно отображает список студентов что позволяет пользователям редактировать, создавать или удалять записи студентов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2ad54-141">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="2ad54-142">Щелкните правой кнопкой мыши **Views/Home/Index.cshtml** правой кнопкой мыши и выберите **Просмотр в браузере**.</span><span class="sxs-lookup"><span data-stu-id="2ad54-142">Right-click the **Views/Home/Index.cshtml** file, and select **View in Browser**.</span></span> <span data-ttu-id="2ad54-143">На этой странице щелкните ссылку для списка учащихся.</span><span class="sxs-lookup"><span data-stu-id="2ad54-143">On this page, click the link for the list of students.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="2ad54-144">На этой странице Обратите внимание на список студентов и ссылки для изменения этих данных.</span><span class="sxs-lookup"><span data-stu-id="2ad54-144">On this page, notice the list of the students and links to modify this data.</span></span>

![список учащихся](generating-views/_static/image7.png)

<span data-ttu-id="2ad54-146">Нажмите кнопку **создать новый** ссылку и предоставляют некоторые значения для нового студента.</span><span class="sxs-lookup"><span data-stu-id="2ad54-146">Click the **Create New** link and provide some values for a new student.</span></span>

![Создание нового студента](generating-views/_static/image8.png)

<span data-ttu-id="2ad54-148">Нажмите кнопку **создать**и обратите внимание нового студента будет добавлен в список.</span><span class="sxs-lookup"><span data-stu-id="2ad54-148">Click **Create**, and notice the new student is added to your list.</span></span>

![список с нового студента](generating-views/_static/image9.png)

<span data-ttu-id="2ad54-150">Выберите **изменить** ссылку и изменить некоторые значения для учащихся.</span><span class="sxs-lookup"><span data-stu-id="2ad54-150">Select the **Edit** link, and change some of the values for a student.</span></span>

![изменить студента](generating-views/_static/image10.png)

<span data-ttu-id="2ad54-152">Нажмите кнопку **Сохранить**и обратите внимание запись студента была изменена.</span><span class="sxs-lookup"><span data-stu-id="2ad54-152">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="2ad54-153">Наконец, выберите **удаление** ссылку и подтвердить, что хотите удалить запись, щелкнув **удалить** кнопки.</span><span class="sxs-lookup"><span data-stu-id="2ad54-153">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

![удалить студента](generating-views/_static/image11.png)

<span data-ttu-id="2ad54-155">Без написания кода, добавления представления, которые выполнения общих операций с данными в таблице учащихся.</span><span class="sxs-lookup"><span data-stu-id="2ad54-155">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="2ad54-156">Можно заметить, что текстовую метку для поля зависит от свойства базы данных (таких как **LastName**) которого не обязательно требуется отображать на веб-странице.</span><span class="sxs-lookup"><span data-stu-id="2ad54-156">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="2ad54-157">Например, можно указать метку, которая должна быть **Фамилия**.</span><span class="sxs-lookup"><span data-stu-id="2ad54-157">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="2ad54-158">Данная проблема отображения будет исправлено позже в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="2ad54-158">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="2ad54-159">Отображение представлений регистрации</span><span class="sxs-lookup"><span data-stu-id="2ad54-159">Display enrollment views</span></span>

<span data-ttu-id="2ad54-160">Базы данных включает в себя отношение "один ко многим" между таблицами Student и регистрации и один ко многим связи между таблицами курс и регистрации.</span><span class="sxs-lookup"><span data-stu-id="2ad54-160">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="2ad54-161">Представления для регистрации правильно обрабатывать эти связи.</span><span class="sxs-lookup"><span data-stu-id="2ad54-161">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="2ad54-162">Перейдите на домашнюю страницу сайта и нажмите кнопку **список регистраций** ссылку и затем **создать новый** ссылку.</span><span class="sxs-lookup"><span data-stu-id="2ad54-162">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span> <span data-ttu-id="2ad54-163">Представление отображает форму для создания новой записи регистрации.</span><span class="sxs-lookup"><span data-stu-id="2ad54-163">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="2ad54-164">В частности Обратите внимание, что форма содержит два раскрывающихся списков, которые заполняются значениями из связанных таблиц.</span><span class="sxs-lookup"><span data-stu-id="2ad54-164">In particular, notice that the form contains two drop-down lists that are populated with values from the related tables.</span></span>

![Создание регистрации](generating-views/_static/image12.png)

<span data-ttu-id="2ad54-166">Кроме того проверки предоставленных значений применяется автоматически на основе типа данных поля.</span><span class="sxs-lookup"><span data-stu-id="2ad54-166">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="2ad54-167">Уровень с числовым, поэтому сообщение об ошибке отображается при попытке предоставить несовместимое значение.</span><span class="sxs-lookup"><span data-stu-id="2ad54-167">Grade requires a number, so an error message is displayed if you try to provide an incompatible value.</span></span>

![Проверка сообщений](generating-views/_static/image13.png)

<span data-ttu-id="2ad54-169">Вы убедились, что автоматически создается представления позволяют пользователям работать с данными в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2ad54-169">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="2ad54-170">В следующем учебнике этой серии будет обновления базы данных и внести соответствующие изменения в веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="2ad54-170">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2ad54-171">[Назад](creating-the-web-application.md)
[Вперед](changing-the-database.md)</span><span class="sxs-lookup"><span data-stu-id="2ad54-171">[Previous](creating-the-web-application.md)
[Next](changing-the-database.md)</span></span>
