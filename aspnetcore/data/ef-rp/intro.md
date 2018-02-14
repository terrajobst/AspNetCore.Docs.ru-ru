---
title: "Razor Pages с Entity Framework Core: учебник 1 из 8"
author: rick-anderson
description: "Сведения о том, как создать приложение Razor Pages с помощью Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="c774d-103">Начало работы с Razor Pages и Entity Framework Core в Visual Studio (1 из 8)</span><span class="sxs-lookup"><span data-stu-id="c774d-103">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="c774d-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="c774d-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c774d-105">На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений ASP.NET Core 2.0 MVC с помощью Entity Framework (EF) Core 2.0 и Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c774d-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="c774d-106">В этом примере приложения реализуется веб-сайт вымышленного университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="c774d-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="c774d-107">На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей.</span><span class="sxs-lookup"><span data-stu-id="c774d-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="c774d-108">Это первое руководство из серии, в котором описывается создание примера приложения для университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="c774d-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="c774d-109">Скачайте или ознакомьтесь с готовым приложением.</span><span class="sxs-lookup"><span data-stu-id="c774d-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="c774d-110">[Указания по скачиванию](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="c774d-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c774d-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c774d-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="c774d-112">Знакомство с [Razor Pages](xref:mvc/razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="c774d-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="c774d-113">Новые программисты должны изучить статью [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), прежде чем приступать к этой серии.</span><span class="sxs-lookup"><span data-stu-id="c774d-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c774d-114">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="c774d-114">Troubleshooting</span></span>

<span data-ttu-id="c774d-115">Если вы столкнулись с проблемами, для их решения можно попробовать сравнить свой код с кодом [готового этапа](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) или [готового проекта](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="c774d-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="c774d-116">Список распространенных ошибок и способы их устранения см. в [разделе "Устранение неполадок" последнего руководства серии](xref:data/ef-mvc/advanced#common-errors).</span><span class="sxs-lookup"><span data-stu-id="c774d-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="c774d-117">Если вам не удалось найти нужную информацию, вы можете задать вопрос на сайте [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) в разделах, посвященных [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) или [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="c774d-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="c774d-118">Эта серия включает в себя руководства, содержание каждого из которых базируется на предыдущих руководствах.</span><span class="sxs-lookup"><span data-stu-id="c774d-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="c774d-119">После успешного завершения каждого руководства рекомендуется сохранять копию проекта.</span><span class="sxs-lookup"><span data-stu-id="c774d-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="c774d-120">При возникновении проблем вы сможете вернуться к предыдущему руководству, а не к самому началу.</span><span class="sxs-lookup"><span data-stu-id="c774d-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="c774d-121">Кроме того, вы можете скачать [завершенный этап](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) и начать с него.</span><span class="sxs-lookup"><span data-stu-id="c774d-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="c774d-122">Веб-приложение университета Contoso</span><span class="sxs-lookup"><span data-stu-id="c774d-122">The Contoso University web app</span></span>

<span data-ttu-id="c774d-123">Приложение, создаваемое в этих руководствах, является простым веб-сайтом университета.</span><span class="sxs-lookup"><span data-stu-id="c774d-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="c774d-124">Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях.</span><span class="sxs-lookup"><span data-stu-id="c774d-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="c774d-125">Здесь приведено несколько экранов, создаваемых в руководстве.</span><span class="sxs-lookup"><span data-stu-id="c774d-125">Here are a few of the screens created in the tutorial.</span></span>

![Страница указателя учащихся](intro/_static/students-index.png)

![Страница редактирования учащихся](intro/_static/student-edit.png)

<span data-ttu-id="c774d-128">Стиль пользовательского интерфейса для этого сайта близок к обеспечиваемому встроенными шаблонами.</span><span class="sxs-lookup"><span data-stu-id="c774d-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="c774d-129">Это руководство посвящено EF Core с Razor Pages, а не пользовательскому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="c774d-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="c774d-130">Создание веб-приложения Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c774d-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="c774d-131">В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="c774d-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c774d-132">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c774d-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="c774d-133">Назовите проект **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="c774d-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="c774d-134">Очень важно, чтобы проект имел имя *ContosoUniversity*, чтобы совпадали пространства имен при копировании и вставке кода.</span><span class="sxs-lookup"><span data-stu-id="c774d-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="c774d-135">![Новое веб-приложение ASP.NET Core](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="c774d-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="c774d-136">Выберите в раскрывающемся списке **ASP.NET Core 2.0**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="c774d-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="c774d-137">![Веб-приложение (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="c774d-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="c774d-138">Нажмите клавишу **F5**, чтобы запустить приложение в режиме отладки, или **CTRL-F5**, чтобы запустить его без подключения отладчика.</span><span class="sxs-lookup"><span data-stu-id="c774d-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="c774d-139">Настройка стиля сайта</span><span class="sxs-lookup"><span data-stu-id="c774d-139">Set up the site style</span></span>

<span data-ttu-id="c774d-140">Выполните небольшую настройку меню, макета и домашней страницы сайта.</span><span class="sxs-lookup"><span data-stu-id="c774d-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="c774d-141">Откройте файл *Pages/_Layout.cshtml* и внесите следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="c774d-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="c774d-142">Замените все вхождения "ContosoUniversity" на "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="c774d-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="c774d-143">Таких элементов будет три.</span><span class="sxs-lookup"><span data-stu-id="c774d-143">There are three occurrences.</span></span>

* <span data-ttu-id="c774d-144">Добавьте пункты меню **Students** (Учащиеся), **Courses** (Курсы), **Instructors** (Преподаватели) и **Departments** (Кафедры). Удалите пункт меню **Contact** (Контакты).</span><span class="sxs-lookup"><span data-stu-id="c774d-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="c774d-145">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="c774d-145">The changes are highlighted.</span></span> <span data-ttu-id="c774d-146">(Вся разметка *не* отображается.)</span><span class="sxs-lookup"><span data-stu-id="c774d-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="c774d-147">Замените содержимое файла *Pages/Index.cshtml* следующим кодом, который заменяет текст о ASP.NET и MVC описанием этого приложения:</span><span class="sxs-lookup"><span data-stu-id="c774d-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="c774d-148">Нажмите клавиши CTRL+F5, чтобы запустить проект.</span><span class="sxs-lookup"><span data-stu-id="c774d-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="c774d-149">Домашняя страница отображается с вкладками, созданными в следующих руководствах:</span><span class="sxs-lookup"><span data-stu-id="c774d-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Домашняя страница университета Contoso](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="c774d-151">Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="c774d-151">Create the data model</span></span>

<span data-ttu-id="c774d-152">Создайте классы сущностей для приложения университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="c774d-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="c774d-153">Начните со следующих трех сущностей:</span><span class="sxs-lookup"><span data-stu-id="c774d-153">Start with the following three entities:</span></span>

![Схема модели данных "курс-регистрация-учащийся"](intro/_static/data-model-diagram.png)

<span data-ttu-id="c774d-155">Между сущностями `Student` и `Enrollment` действует связь один ко многим.</span><span class="sxs-lookup"><span data-stu-id="c774d-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="c774d-156">Между сущностями `Course` и `Enrollment` действует связь один ко многим.</span><span class="sxs-lookup"><span data-stu-id="c774d-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="c774d-157">Студент может записаться на любое число курсов.</span><span class="sxs-lookup"><span data-stu-id="c774d-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="c774d-158">На курс может быть зачислено любое количество учащихся.</span><span class="sxs-lookup"><span data-stu-id="c774d-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="c774d-159">В следующих разделах создается класс для каждой из этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="c774d-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="c774d-160">Сущность Student</span><span class="sxs-lookup"><span data-stu-id="c774d-160">The Student entity</span></span>

![Схема сущности Student](intro/_static/student-entity.png)

<span data-ttu-id="c774d-162">Создайте папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="c774d-162">Create a *Models* folder.</span></span> <span data-ttu-id="c774d-163">В папке *Models* создайте файл класса с именем *Student.cs*, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="c774d-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="c774d-164">Свойство `ID` используется в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу.</span><span class="sxs-lookup"><span data-stu-id="c774d-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="c774d-165">По умолчанию платформа EF Core интерпретирует в качестве первичного ключа свойство `ID` или `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="c774d-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="c774d-166">Свойство `Enrollments` является свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="c774d-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c774d-167">Свойства навигации ссылаются на другие сущности, связанные с этой сущностью.</span><span class="sxs-lookup"><span data-stu-id="c774d-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="c774d-168">В этом случае свойство `Enrollments` сущности `Student entity` содержит все сущности `Enrollment`, которые связаны с этой сущностью `Student`.</span><span class="sxs-lookup"><span data-stu-id="c774d-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="c774d-169">Например, если строка "Student" (Учащийся) в базе данных имеет две связанные строки "Enrollment" (зачисление), свойство навигации `Enrollments` содержит две этих сущности `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c774d-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="c774d-170">Связанная строка `Enrollment` содержит значение первичного ключа для этого учащегося в столбце `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="c774d-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="c774d-171">Предположим, например, что учащийся с идентификатором 1 имеет две строки в таблице `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c774d-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="c774d-172">Таблица `Enrollment` содержит две строки с `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="c774d-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="c774d-173">`StudentID` является внешним ключом в таблице `Enrollment`, указывающим учащегося в таблице `Student`.</span><span class="sxs-lookup"><span data-stu-id="c774d-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="c774d-174">Если свойство навигации может содержать несколько сущностей, оно должно иметь тип списка, такой как `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="c774d-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="c774d-175">Можно указать `ICollection<T>` либо тип, такой как `List<T>` или `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="c774d-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="c774d-176">Если используется `ICollection<T>`, платформа EF Core по умолчанию создает коллекцию `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="c774d-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="c774d-177">Свойства навигации, содержащие несколько сущностей, поступают по связям многие ко многим и один ко многим.</span><span class="sxs-lookup"><span data-stu-id="c774d-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="c774d-178">Сущность Enrollment</span><span class="sxs-lookup"><span data-stu-id="c774d-178">The Enrollment entity</span></span>

![Схема сущности Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="c774d-180">В папке *Models* создайте файл *Enrollment.cs*, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="c774d-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="c774d-181">Свойство `EnrollmentID` является первичным ключом.</span><span class="sxs-lookup"><span data-stu-id="c774d-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="c774d-182">Эта сущность использует шаблон `classnameID` вместо `ID`, как сущность `Student`.</span><span class="sxs-lookup"><span data-stu-id="c774d-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="c774d-183">Обычно разработчики выбирают один шаблон и используют его в рамках всей модели данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="c774d-184">В одном из следующих руководств показано, за счет чего использование идентификатора без имени класса позволяет упростить реализацию наследования в модели данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="c774d-185">Свойство `Grade` имеет тип `enum`.</span><span class="sxs-lookup"><span data-stu-id="c774d-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="c774d-186">Знак вопроса после объявления типа `Grade` указывает, что свойство `Grade` допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="c774d-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="c774d-187">Оценка со значением null отличается от нулевой оценки тем, что при таком значении оценка еще не известна или не назначена.</span><span class="sxs-lookup"><span data-stu-id="c774d-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="c774d-188">Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`.</span><span class="sxs-lookup"><span data-stu-id="c774d-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="c774d-189">Сущность `Enrollment` связана с одной сущностью `Student`, поэтому свойство содержит отдельную сущность `Student`.</span><span class="sxs-lookup"><span data-stu-id="c774d-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="c774d-190">Сущность `Student` отличается от свойства навигации `Student.Enrollments`, которое содержит несколько сущностей `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c774d-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="c774d-191">Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`.</span><span class="sxs-lookup"><span data-stu-id="c774d-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="c774d-192">Сущность `Enrollment` связана с одной сущностью `Course`.</span><span class="sxs-lookup"><span data-stu-id="c774d-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="c774d-193">EF Core воспринимает свойство как внешний ключ, если он имеет имя `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c774d-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="c774d-194">Например, `StudentID` для свойства навигации `Student`, так как сущность `Student` имеет первичный ключ `ID`.</span><span class="sxs-lookup"><span data-stu-id="c774d-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="c774d-195">Свойства внешнего ключа также могут называться `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c774d-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="c774d-196">Например, `CourseID`, так как сущность `Course` имеет первичный ключ `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="c774d-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="c774d-197">Сущность Course</span><span class="sxs-lookup"><span data-stu-id="c774d-197">The Course entity</span></span>

![Схема сущности Course](intro/_static/course-entity.png)

<span data-ttu-id="c774d-199">В папке *Models* создайте файл *Course.cs*, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="c774d-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="c774d-200">Свойство `Enrollments` является свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="c774d-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c774d-201">Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c774d-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="c774d-202">Атрибут `DatabaseGenerated` позволяет приложению указать первичный ключ, а не использовать созданный базой данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="c774d-203">Создание контекста базы данных SchoolContext</span><span class="sxs-lookup"><span data-stu-id="c774d-203">Create the SchoolContext DB context</span></span>

<span data-ttu-id="c774d-204">Контекст базы данных — это класс main, который координирует функциональные возможности EF Core для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-204">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="c774d-205">Контекст данных является производным от `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c774d-205">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="c774d-206">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-206">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="c774d-207">В этом проекте соответствующий класс называется `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c774d-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="c774d-208">В папке проекта создайте папку *Data*.</span><span class="sxs-lookup"><span data-stu-id="c774d-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="c774d-209">В папке *Data* создайте файл *SchoolContext.cs*, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="c774d-209">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="c774d-210">Этот код создает свойство `DbSet` для каждого набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="c774d-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="c774d-211">В терминологии EF Core:</span><span class="sxs-lookup"><span data-stu-id="c774d-211">In EF Core terminology:</span></span>

* <span data-ttu-id="c774d-212">Набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-212">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="c774d-213">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="c774d-213">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c774d-214">`DbSet<Enrollment>` и `DbSet<Course>` можно опустить.</span><span class="sxs-lookup"><span data-stu-id="c774d-214">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="c774d-215">Платформа EF Core включает эти операторы неявно, так как сущность `Student` ссылается на сущность `Enrollment`, а `Enrollment` ссылается на сущность `Course`.</span><span class="sxs-lookup"><span data-stu-id="c774d-215">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="c774d-216">В рамках данного руководства оставьте `DbSet<Enrollment>` и `DbSet<Course>` в `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c774d-216">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="c774d-217">При создании базы данных платформа EF Core создает таблицы с именами, соответствующими именам свойств в `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="c774d-217">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="c774d-218">Имена свойств для коллекций обычно указаны во множественном числе (Students вместо Student).</span><span class="sxs-lookup"><span data-stu-id="c774d-218">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="c774d-219">В среде разработчиков нет единого мнения о том, следует ли использовать имена таблиц во множественном числе.</span><span class="sxs-lookup"><span data-stu-id="c774d-219">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="c774d-220">В этих учебниках вместо принятого по умолчанию способа таблицам в DbContext присваиваются имена в единственном числе.</span><span class="sxs-lookup"><span data-stu-id="c774d-220">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="c774d-221">Чтобы указать имена таблиц в единственном числе, добавьте следующий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="c774d-221">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="c774d-222">Регистрация контекста с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="c774d-222">Register the context with dependency injection</span></span>

<span data-ttu-id="c774d-223">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c774d-223">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c774d-224">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c774d-224">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c774d-225">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="c774d-225">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c774d-226">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="c774d-226">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c774d-227">Чтобы зарегистрировать `SchoolContext` как службу, откройте файл *Startup.cs* и добавьте выделенные строки в метод `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c774d-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="c774d-228">Имя строки подключения передается в контекст путем вызова метода для объекта `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="c774d-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="c774d-229">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c774d-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="c774d-230">Добавьте операторы `using` для пространств имен `ContosoUniversity.Data` и `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="c774d-230">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="c774d-231">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="c774d-231">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="c774d-232">Откройте файл *appsettings.json* и добавьте строку подключения, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="c774d-232">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="c774d-233">Предыдущая строка подключения использует `ConnectRetryCount=0` для предотвращения зависания [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework).</span><span class="sxs-lookup"><span data-stu-id="c774d-233">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="c774d-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="c774d-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="c774d-235">Строка подключения указывает базу данных SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="c774d-235">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="c774d-236">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки приложений и не ориентированная на использование в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="c774d-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="c774d-237">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="c774d-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="c774d-238">По умолчанию LocalDB создает файлы базы данных *MDF* в каталоге `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="c774d-238">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="c774d-239">Добавление кода для инициализации базы данных с использованием тестовых данных</span><span class="sxs-lookup"><span data-stu-id="c774d-239">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="c774d-240">EF Core создает пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-240">EF Core creates an empty DB.</span></span> <span data-ttu-id="c774d-241">В этом разделе создается метод *Seed*, предназначенный для заполнения тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="c774d-241">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="c774d-242">В папке *Data* создайте файл класса с именем *DbInitializer.cs* и добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="c774d-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="c774d-243">Код проверяет наличие учащихся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-243">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="c774d-244">Если учащихся в базе данных нет, она заполняется тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="c774d-244">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="c774d-245">Для повышения производительности тестовые данные загружаются массивами, а не коллекциями `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="c774d-245">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="c774d-246">Метод `EnsureCreated` автоматически создает базу данных для контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-246">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="c774d-247">Если такая база данных существует, `EnsureCreated` возвращает управление без изменения базы данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-247">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="c774d-248">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="c774d-248">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="c774d-249">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c774d-249">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="c774d-250">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="c774d-250">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="c774d-251">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="c774d-251">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="c774d-252">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="c774d-252">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="c774d-253">При первом запуске приложения создается база данных, которая заполняется тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="c774d-253">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="c774d-254">При обновлении модели данных:</span><span class="sxs-lookup"><span data-stu-id="c774d-254">When the data model is updated:</span></span>
* <span data-ttu-id="c774d-255">Удалите базу данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-255">Delete the DB.</span></span>
* <span data-ttu-id="c774d-256">Обновите метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="c774d-256">Update the seed method.</span></span>
* <span data-ttu-id="c774d-257">Запустите приложение, чтобы создать базу данных, заполненную начальными значениями.</span><span class="sxs-lookup"><span data-stu-id="c774d-257">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="c774d-258">Последующие руководства описывают, как обновить базу данных при изменении модели данных, не прибегая к ее удалению и повторному созданию.</span><span class="sxs-lookup"><span data-stu-id="c774d-258">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="c774d-259">Добавление средств для формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="c774d-259">Add scaffold tooling</span></span>

<span data-ttu-id="c774d-260">В этом разделе с помощью консоли диспетчера пакетов (PMC) добавляется пакет создания веб-кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c774d-260">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="c774d-261">Этот пакет необходим для работы ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c774d-261">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="c774d-262">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="c774d-262">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="c774d-263">Введите в консоли диспетчера пакетов (PMC) следующие команды:</span><span class="sxs-lookup"><span data-stu-id="c774d-263">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="c774d-264">Предыдущая команда добавляет в файл \*.csproj пакеты NuGet:</span><span class="sxs-lookup"><span data-stu-id="c774d-264">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="c774d-265">Формирование шаблонов для модели</span><span class="sxs-lookup"><span data-stu-id="c774d-265">Scaffold the model</span></span>

* <span data-ttu-id="c774d-266">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="c774d-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c774d-267">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="c774d-267">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="c774d-268">При возникновении следующей ошибки:</span><span class="sxs-lookup"><span data-stu-id="c774d-268">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="c774d-269">Выполните команду еще раз и оставьте комментарий в нижней части страницы.</span><span class="sxs-lookup"><span data-stu-id="c774d-269">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="c774d-270">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="c774d-270">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="c774d-271">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="c774d-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="c774d-272">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="c774d-272">Build the project.</span></span> <span data-ttu-id="c774d-273">Эта сборка выдает ошибки, например следующего характера:</span><span class="sxs-lookup"><span data-stu-id="c774d-273">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="c774d-274">Выполните глобальную замену `_context.Student` на `_context.Students` (она заключается в добавлении "s" к `Student`).</span><span class="sxs-lookup"><span data-stu-id="c774d-274">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="c774d-275">Обнаруживаются и изменяются 7 экземпляров.</span><span class="sxs-lookup"><span data-stu-id="c774d-275">7 occurrences are found and updated.</span></span> <span data-ttu-id="c774d-276">Мы постараемся исправить [эту ошибку](https://github.com/aspnet/Scaffolding/issues/633) в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="c774d-276">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="c774d-277">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="c774d-277">Test the app</span></span>

<span data-ttu-id="c774d-278">Запустите приложение и выберите ссылку **Students** (Учащиеся).</span><span class="sxs-lookup"><span data-stu-id="c774d-278">Run the app and select the **Students** link.</span></span> <span data-ttu-id="c774d-279">В зависимости от ширины браузера ссылка **Students** (Учащиеся) появляется в верхней части страницы.</span><span class="sxs-lookup"><span data-stu-id="c774d-279">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="c774d-280">Если ссылка **Students** (Учащиеся) не отображается, щелкните значок навигации в правом верхнем углу.</span><span class="sxs-lookup"><span data-stu-id="c774d-280">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Уменьшенное окно с домашней страницей университета Contoso](intro/_static/home-page-narrow.png)

<span data-ttu-id="c774d-282">Проверьте работу ссылок **Create** (Создать), **Edit** (Изменить) и **Details** (Сведения).</span><span class="sxs-lookup"><span data-stu-id="c774d-282">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="c774d-283">Просмотр базы данных</span><span class="sxs-lookup"><span data-stu-id="c774d-283">View the DB</span></span>

<span data-ttu-id="c774d-284">При запуске приложения `DbInitializer.Initialize` вызывает `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="c774d-284">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="c774d-285">`EnsureCreated` определяет, существует ли база данных, и при необходимости создает ее.</span><span class="sxs-lookup"><span data-stu-id="c774d-285">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="c774d-286">Если учащихся в базе данных нет, метод `Initialize` добавляет их.</span><span class="sxs-lookup"><span data-stu-id="c774d-286">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="c774d-287">Откройте **обозреватель объектов SQL Server** (SSOX) из меню **Вид** в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c774d-287">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="c774d-288">В SSOX щелкните **(localdb)\MSSQLLocalDB > Базы данных > ContosoUniversity1**.</span><span class="sxs-lookup"><span data-stu-id="c774d-288">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="c774d-289">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="c774d-289">Expand the **Tables** node.</span></span>

<span data-ttu-id="c774d-290">Щелкните правой кнопкой мыши таблицу **Student** (Учащийся) и выберите пункт **Просмотр данных**, чтобы просмотреть созданные столбцы и строки, вставленные в таблицу.</span><span class="sxs-lookup"><span data-stu-id="c774d-290">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="c774d-291">Файлы базы данных с расширением *MDF* и *LDF* располагаются в папке *C:\Users\\<yourusername>*.</span><span class="sxs-lookup"><span data-stu-id="c774d-291">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="c774d-292">`EnsureCreated` вызывается при запуске приложения, что обеспечивает следующий рабочий процесс:</span><span class="sxs-lookup"><span data-stu-id="c774d-292">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="c774d-293">Удалите базу данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-293">Delete the DB.</span></span>
* <span data-ttu-id="c774d-294">Измените схему базы данных (например, добавьте поле `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="c774d-294">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="c774d-295">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c774d-295">Run the app.</span></span>

<span data-ttu-id="c774d-296">`EnsureCreated` создает базу данных со столбцом `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="c774d-296">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="c774d-297">Соглашения</span><span class="sxs-lookup"><span data-stu-id="c774d-297">Conventions</span></span>

<span data-ttu-id="c774d-298">Объем кода, написанного для создания полной базы данных платформой EF Core, сведен к минимуму благодаря использованию соглашений или предположений, задаваемых EF Core.</span><span class="sxs-lookup"><span data-stu-id="c774d-298">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="c774d-299">В качестве имен таблиц используются имена свойств `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="c774d-299">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="c774d-300">Для сущностей, на которые не ссылается свойство `DbSet`, в качестве имен таблиц используются имена классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="c774d-300">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="c774d-301">В качестве имен столбцов используются имена свойств сущностей.</span><span class="sxs-lookup"><span data-stu-id="c774d-301">Entity property names are used for column names.</span></span>

* <span data-ttu-id="c774d-302">Свойства сущностей с именем ID или classnameID распознаются как свойства первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="c774d-302">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="c774d-303">Свойство интерпретируется как свойство внешнего ключа, если оно имеет имя *<navigation property name><primary key property name>* (например, `StudentID` для свойства навигации `Student`, так как сущность `Student` имеет первичный ключ `ID`).</span><span class="sxs-lookup"><span data-stu-id="c774d-303">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="c774d-304">Свойства внешнего ключа могут называться *<primary key property name>* (например, `EnrollmentID`, так как сущность `Enrollment` имеет первичный ключ `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="c774d-304">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="c774d-305">Стандартное поведение можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="c774d-305">Conventional behavior can be overridden.</span></span> <span data-ttu-id="c774d-306">Например, можно явно задать имена таблиц, как было показано ранее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="c774d-306">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="c774d-307">Можно явно задать имена столбцов.</span><span class="sxs-lookup"><span data-stu-id="c774d-307">The column names can be explicitly set.</span></span> <span data-ttu-id="c774d-308">Можно явно задать первичные и внешние ключи.</span><span class="sxs-lookup"><span data-stu-id="c774d-308">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="c774d-309">Асинхронный код</span><span class="sxs-lookup"><span data-stu-id="c774d-309">Asynchronous code</span></span>

<span data-ttu-id="c774d-310">Асинхронное программирование по умолчанию применяется в ASP.NET Core и EF Core.</span><span class="sxs-lookup"><span data-stu-id="c774d-310">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="c774d-311">Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки.</span><span class="sxs-lookup"><span data-stu-id="c774d-311">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="c774d-312">В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки.</span><span class="sxs-lookup"><span data-stu-id="c774d-312">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="c774d-313">В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="c774d-313">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="c774d-314">В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов.</span><span class="sxs-lookup"><span data-stu-id="c774d-314">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="c774d-315">Таким образом, асинхронный код позволяет более эффективно использовать ресурсы сервера, который может обрабатывать больше трафика без задержек.</span><span class="sxs-lookup"><span data-stu-id="c774d-315">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="c774d-316">Во время выполнения асинхронный код использует немного больше служебных ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c774d-316">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="c774d-317">Однако при низком объеме трафика этим можно пренебречь. Тем не менее в случае большого объема трафика это дает существенный выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="c774d-317">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="c774d-318">В следующем коде для асинхронного выполнения используются ключевое слово `async`, возвращаемое значение `Task<T>`, ключевое слово `await` и метод `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="c774d-318">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="c774d-319">Ключевое слово `async` указывает компилятору:</span><span class="sxs-lookup"><span data-stu-id="c774d-319">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="c774d-320">создавать обратные вызовы для частей тела метода;</span><span class="sxs-lookup"><span data-stu-id="c774d-320">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="c774d-321">автоматически создавать возвращаемый объект [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7).</span><span class="sxs-lookup"><span data-stu-id="c774d-321">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="c774d-322">Дополнительные сведения см. в разделе [Тип возвращаемых значений задач](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="c774d-322">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="c774d-323">Неявный тип возвращаемого значения `Task` представляет текущую операцию.</span><span class="sxs-lookup"><span data-stu-id="c774d-323">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="c774d-324">Ключевое слово `await` предписывает компилятору разделить метод на две части.</span><span class="sxs-lookup"><span data-stu-id="c774d-324">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="c774d-325">Первая часть завершается операцией, которая запускается в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="c774d-325">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="c774d-326">Вторая часть помещается в метод обратного вызова, который вызывается при завершении операции.</span><span class="sxs-lookup"><span data-stu-id="c774d-326">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="c774d-327">`ToListAsync` является асинхронной версией метода расширения `ToList`.</span><span class="sxs-lookup"><span data-stu-id="c774d-327">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="c774d-328">При написании асинхронного кода, который использует EF Core, нужно учитывать некоторые моменты:</span><span class="sxs-lookup"><span data-stu-id="c774d-328">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="c774d-329">Асинхронно выполняются только те операторы, в результате которых в базу данных отправляются запросы или команды.</span><span class="sxs-lookup"><span data-stu-id="c774d-329">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="c774d-330">К ним относятся `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` и `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="c774d-330">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="c774d-331">В их число не входят операторы, которые просто изменяют `IQueryable`, такие как `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="c774d-331">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="c774d-332">Контекст EF Core не является потокобезопасным, поэтому не следует пытаться выполнять несколько операций параллельно.</span><span class="sxs-lookup"><span data-stu-id="c774d-332">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="c774d-333">Чтобы воспользоваться преимуществами повышенной производительности асинхронного кода, убедитесь, что пакеты библиотек (например, для разбиения на страницы) используют асинхронную модель, когда вызывают методы EF Core, которые направляют запросы в базу данных.</span><span class="sxs-lookup"><span data-stu-id="c774d-333">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="c774d-334">Дополнительные сведения об асинхронных методах программирования в .NET см. в разделе [Обзор асинхронной модели](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="c774d-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="c774d-335">Следующее руководство посвящено базовым операциям CRUD (создание, чтение, обновление, удаление).</span><span class="sxs-lookup"><span data-stu-id="c774d-335">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="c774d-336">Вперед</span><span class="sxs-lookup"><span data-stu-id="c774d-336">Next</span></span>](xref:data/ef-rp/crud)
