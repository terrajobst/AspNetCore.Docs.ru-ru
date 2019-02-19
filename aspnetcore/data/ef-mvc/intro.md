---
title: Учебник. Начало работы с EF Core в веб-приложении MVC ASP.NET
description: Это первый учебник из серии, в котором с самого начала описывается построение примера приложения для университета Contoso.
author: rick-anderson
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: f7b557c8e560393ae886c46fad95c48ccbcc65b4
ms.sourcegitcommit: 5e3797a02ff3c48bb8cb9ad4320bfd169ebe8aba
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2019
ms.locfileid: "56102972"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="33e31-103">Учебник. Начало работы с EF Core в веб-приложении MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="33e31-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="33e31-104">На примере веб-приложения Contoso University показано, как создавать веб-приложения MVC ASP.NET Core 2.2 с помощью Entity Framework (EF) Core 2.0 и Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="33e31-104">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="33e31-105">В этом примере приложения реализуется веб-сайт вымышленного университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="33e31-105">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="33e31-106">На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей.</span><span class="sxs-lookup"><span data-stu-id="33e31-106">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="33e31-107">Это первый учебник из серии, в котором с самого начала описывается построение примера приложения для университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="33e31-107">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="33e31-108">EF Core 2.0 — это последняя версия платформы EF, однако в ней реализованы еще не все возможности EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="33e31-108">EF Core 2.0 is the latest version of EF but doesn't yet have all the features of EF 6.x.</span></span> <span data-ttu-id="33e31-109">Дополнительные сведения о выборе между платформами EF 6.x и EF Core см. в [этой статье](/ef/efcore-and-ef6/).</span><span class="sxs-lookup"><span data-stu-id="33e31-109">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="33e31-110">Если вы используете платформу EF 6.x, ознакомьтесь с [предыдущей версией этой серии учебников](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="33e31-110">If you choose EF 6.x, see [the previous version of this tutorial series](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> <span data-ttu-id="33e31-111">Версия этого учебника для ASP.NET Core 1.1 и VS 2017 с обновлением 2 в формате PDF доступна [здесь](https://webpifeed.blob.core.windows.net/webpifeed/Partners/efmvc1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="33e31-111">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://webpifeed.blob.core.windows.net/webpifeed/Partners/efmvc1.1.pdf).</span></span>

<span data-ttu-id="33e31-112">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="33e31-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="33e31-113">Создание веб-приложения MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33e31-113">Create ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="33e31-114">Настройка стиля сайта</span><span class="sxs-lookup"><span data-stu-id="33e31-114">Set up the site style</span></span>
> * <span data-ttu-id="33e31-115">Дополнительные сведения о пакетах NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="33e31-115">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="33e31-116">Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="33e31-116">Create the data model</span></span>
> * <span data-ttu-id="33e31-117">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="33e31-117">Create the database context</span></span>
> * <span data-ttu-id="33e31-118">Регистрация SchoolContext</span><span class="sxs-lookup"><span data-stu-id="33e31-118">Register the SchoolContext</span></span>
> * <span data-ttu-id="33e31-119">Инициализация базы данных с тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="33e31-119">Initialize DB with test data</span></span>
> * <span data-ttu-id="33e31-120">Создание контроллера и представлений</span><span class="sxs-lookup"><span data-stu-id="33e31-120">Create controller and views</span></span>
> * <span data-ttu-id="33e31-121">Просмотр базы данных</span><span class="sxs-lookup"><span data-stu-id="33e31-121">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="33e31-122">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="33e31-122">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="troubleshooting"></a><span data-ttu-id="33e31-123">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="33e31-123">Troubleshooting</span></span>

<span data-ttu-id="33e31-124">Если вы столкнулись с проблемами, для их решения можно попробовать сравнить свой код с кодом [готового проекта](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="33e31-124">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="33e31-125">Список распространенных ошибок и способы их устранения см. в [разделе "Устранение неполадок" последнего руководства серии](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="33e31-125">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="33e31-126">Если вам не удалось найти нужную информацию, вы можете задать вопрос на сайте StackOverflow.com в разделах, посвященных [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) или [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="33e31-126">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="33e31-127">Эта серия включает в себя 10 учебников, содержание каждого из которых базируется на предыдущих учебниках.</span><span class="sxs-lookup"><span data-stu-id="33e31-127">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="33e31-128">После успешного завершения каждого руководства рекомендуется сохранять копию проекта.</span><span class="sxs-lookup"><span data-stu-id="33e31-128">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="33e31-129">Таким образом, при возникновении проблем вы сможете вернуться к предыдущему учебнику, а не к началу серии.</span><span class="sxs-lookup"><span data-stu-id="33e31-129">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="33e31-130">Веб-приложение Contoso University</span><span class="sxs-lookup"><span data-stu-id="33e31-130">Contoso University web app</span></span>

<span data-ttu-id="33e31-131">В рамках этих учебников вы будете создавать приложение, которое представляет собой простой веб-сайт университета.</span><span class="sxs-lookup"><span data-stu-id="33e31-131">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="33e31-132">Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях.</span><span class="sxs-lookup"><span data-stu-id="33e31-132">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="33e31-133">Будет создано несколько экранов.</span><span class="sxs-lookup"><span data-stu-id="33e31-133">Here are a few of the screens you'll create.</span></span>

![Страница указателя учащихся](intro/_static/students-index.png)

![Страница редактирования учащихся](intro/_static/student-edit.png)

<span data-ttu-id="33e31-136">Стиль пользовательского интерфейса этого сайта практически полностью основан на встроенных шаблонах, поскольку это позволяет сосредоточиться на изучении и использовании возможностей платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="33e31-136">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-aspnet-core-mvc-web-app"></a><span data-ttu-id="33e31-137">Создание веб-приложения MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33e31-137">Create ASP.NET Core MVC web app</span></span>

<span data-ttu-id="33e31-138">Откройте Visual Studio и создайте новый веб-проект ASP.NET Core C# с именем "ContosoUniversity".</span><span class="sxs-lookup"><span data-stu-id="33e31-138">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="33e31-139">В меню **Файл** выберите пункт **Создать > Проект**.</span><span class="sxs-lookup"><span data-stu-id="33e31-139">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="33e31-140">В области слева выберите **Установленные > Visual C# > Интернет**.</span><span class="sxs-lookup"><span data-stu-id="33e31-140">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="33e31-141">Выберите шаблон проекта **Веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="33e31-141">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="33e31-142">Введите имя **ContosoUniversity** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="33e31-142">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Диалоговое окно создания нового проекта](intro/_static/new-project2.png)

* <span data-ttu-id="33e31-144">Дождитесь появления диалогового окна **Создание веб-приложения ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="33e31-144">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

  ![Диалоговое окно "Создание проекта ASP.NET Core"](intro/_static/new-aspnet2.png)

* <span data-ttu-id="33e31-146">Выберите **ASP.NET Core 2.2** и шаблон **Веб-приложение (Модель — представление — контроллер)**.</span><span class="sxs-lookup"><span data-stu-id="33e31-146">Select **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="33e31-147">**Примечание.** В этом руководстве используются версии ASP.NET Core 2.2 и EF Core 2.0 или выше.</span><span class="sxs-lookup"><span data-stu-id="33e31-147">**Note:** This tutorial requires ASP.NET Core 2.2 and EF Core 2.0 or later.</span></span>

* <span data-ttu-id="33e31-148">Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="33e31-148">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="33e31-149">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="33e31-149">Click **OK**</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="33e31-150">Настройка стиля сайта</span><span class="sxs-lookup"><span data-stu-id="33e31-150">Set up the site style</span></span>

<span data-ttu-id="33e31-151">Выполните незначительную настройку меню, макета и домашней страницы сайта.</span><span class="sxs-lookup"><span data-stu-id="33e31-151">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="33e31-152">Откройте файл *Views/Shared/_Layout.cshtml* и внесите следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="33e31-152">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="33e31-153">Замените все вхождения "ContosoUniversity" на "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="33e31-153">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="33e31-154">Таких элементов будет три.</span><span class="sxs-lookup"><span data-stu-id="33e31-154">There are three occurrences.</span></span>

* <span data-ttu-id="33e31-155">Добавьте пункты меню **About** (Сведения), **Students** (Учащиеся), **Courses** (Курсы), **Instructors** (Преподаватели) и **Departments** (Кафедры). Удалите пункт меню **Privacy** (Конфиденциальность).</span><span class="sxs-lookup"><span data-stu-id="33e31-155">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="33e31-156">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="33e31-156">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,32-36,51)]

<span data-ttu-id="33e31-157">Замените содержимое файла *Views/Home/Index.cshtml* следующим кодом, который заменяет текст о ASP.NET и MVC описанием этого приложения:</span><span class="sxs-lookup"><span data-stu-id="33e31-157">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="33e31-158">Нажмите клавиши CTRL+F5, чтобы запустить проект, и выберите в меню **Отладка > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="33e31-158">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="33e31-159">Откроется домашняя страница, на которой будут представлены созданные в рамках этих учебников вкладки.</span><span class="sxs-lookup"><span data-stu-id="33e31-159">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Домашняя страница университета Contoso](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="33e31-161">Сведения о пакетах NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="33e31-161">About EF Core NuGet packages</span></span>

<span data-ttu-id="33e31-162">Чтобы реализовать поддержку EF Core в проекте, установите поставщик целевой базы данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-162">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="33e31-163">В этом учебнике используется SQL Server, для которого требуется пакет поставщика [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="33e31-163">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="33e31-164">Этот пакет входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), поэтому если приложение уже содержит ссылку на пакет `Microsoft.AspNetCore.App`, ссылаться на него не нужно.</span><span class="sxs-lookup"><span data-stu-id="33e31-164">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package if your app has a package reference for the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="33e31-165">Этот пакет и его зависимости (`Microsoft.EntityFrameworkCore` и `Microsoft.EntityFrameworkCore.Relational`) обеспечивают поддержку среды выполнения для платформы EF.</span><span class="sxs-lookup"><span data-stu-id="33e31-165">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="33e31-166">Пакет средств будет добавлен позднее в рамках учебника [Миграции](migrations.md).</span><span class="sxs-lookup"><span data-stu-id="33e31-166">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="33e31-167">Дополнительные сведения о других поставщиках баз данных, которые доступны для платформы Entity Framework Core, см. в разделе [Поставщики баз данных](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="33e31-167">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="33e31-168">Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="33e31-168">Create the data model</span></span>

<span data-ttu-id="33e31-169">Теперь необходимо создать классы сущностей для приложения университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="33e31-169">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="33e31-170">Для начала создаются следующие три сущности.</span><span class="sxs-lookup"><span data-stu-id="33e31-170">You'll start with the following three entities.</span></span>

![Схема модели данных "курс-регистрация-учащийся"](intro/_static/data-model-diagram.png)

<span data-ttu-id="33e31-172">Между сущностями `Student` и `Enrollment`, а также между сущностями `Course` и `Enrollment` существует отношение "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="33e31-172">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="33e31-173">Другими словами, учащийся может быть зарегистрирован в любом количестве курсов, а в отдельном курсе может быть зарегистрировано любое количество учащихся.</span><span class="sxs-lookup"><span data-stu-id="33e31-173">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="33e31-174">В следующих разделах создаются классы для каждой из этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="33e31-174">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="33e31-175">Сущность Student</span><span class="sxs-lookup"><span data-stu-id="33e31-175">The Student entity</span></span>

![Схема сущности Student](intro/_static/student-entity.png)

<span data-ttu-id="33e31-177">В папке *Models* создайте файл класса с именем *Student.cs* и замените код шаблона следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="33e31-177">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="33e31-178">Свойство `ID` будет использоваться в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу.</span><span class="sxs-lookup"><span data-stu-id="33e31-178">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="33e31-179">По умолчанию платформа Entity Framework интерпретирует в качестве первичного ключа свойство `ID` или `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="33e31-179">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="33e31-180">Свойство `Enrollments` является [свойством навигации](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="33e31-180">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="33e31-181">Свойства навигации содержат другие сущности, связанные с этой сущностью.</span><span class="sxs-lookup"><span data-stu-id="33e31-181">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="33e31-182">В этом случае свойство `Enrollments` сущности `Student entity` содержит все сущности `Enrollment`, которые связаны с этой сущностью `Student`.</span><span class="sxs-lookup"><span data-stu-id="33e31-182">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="33e31-183">Другими словами, если с указанной строкой Student в базе данных связаны две строки Enrollment (строки, в столбце внешнего ключа StudentID которых содержится первичный ключ этого учащегося), в этой сущности `Student` свойство навигации `Enrollments` будет содержать две этих сущности `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33e31-183">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="33e31-184">Если свойство навигации может содержать несколько сущностей (как в отношениях "многие ко многим" или "один ко многим"), оно должно иметь тип списка, допускающий добавление, удаление и обновление записей, такой как `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="33e31-184">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="33e31-185">Вы можете указать тип `ICollection<T>` либо, например, тип `List<T>` или `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="33e31-185">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="33e31-186">Если указан тип `ICollection<T>`, платформа EF по умолчанию создает коллекцию `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="33e31-186">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="33e31-187">Сущность Enrollment</span><span class="sxs-lookup"><span data-stu-id="33e31-187">The Enrollment entity</span></span>

![Схема сущности Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="33e31-189">В папке *Models* создайте файл *Enrollment.cs* и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="33e31-189">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="33e31-190">Свойство `EnrollmentID` будет использоваться в качестве первичного ключа. В этой сущности используется шаблон `classnameID` вместо `ID`, как в сущности `Student`.</span><span class="sxs-lookup"><span data-stu-id="33e31-190">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="33e31-191">Как правило, следует выбирать один шаблон, который будет использоваться в рамках всей модели данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-191">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="33e31-192">В этом случае демонстрируется возможность использования любого из шаблонов.</span><span class="sxs-lookup"><span data-stu-id="33e31-192">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="33e31-193">В [одном из следующих учебников](inheritance.md) вы узнаете, за счет чего использование идентификатора без имени класса позволяет упростить реализацию наследования в модели данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-193">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="33e31-194">Свойство `Grade` имеет тип `enum`.</span><span class="sxs-lookup"><span data-stu-id="33e31-194">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="33e31-195">Знак вопроса после объявления типа `Grade` указывает, что свойство `Grade` допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="33e31-195">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="33e31-196">Оценка со значением null отличается от нулевой оценки тем, что при таком значении оценка еще не известна или не назначена.</span><span class="sxs-lookup"><span data-stu-id="33e31-196">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="33e31-197">Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`.</span><span class="sxs-lookup"><span data-stu-id="33e31-197">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="33e31-198">Сущность `Enrollment` связана с одной сущностью `Student`, поэтому это свойство может содержать одну сущность `Student` (в отличие от представленного ранее свойства навигации `Student.Enrollments`, которое может содержать несколько сущностей `Enrollment`).</span><span class="sxs-lookup"><span data-stu-id="33e31-198">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="33e31-199">Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`.</span><span class="sxs-lookup"><span data-stu-id="33e31-199">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="33e31-200">Сущность `Enrollment` связана с одной сущностью `Course`.</span><span class="sxs-lookup"><span data-stu-id="33e31-200">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="33e31-201">Платформа Entity Framework интерпретирует свойство как свойство внешнего ключа, если оно имеет имя `<navigation property name><primary key property name>` (например, `StudentID` для свойства навигации `Student`, поскольку сущность `Student` имеет первичный ключ `ID`).</span><span class="sxs-lookup"><span data-stu-id="33e31-201">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="33e31-202">Свойства внешнего ключа также могут называться просто `<primary key property name>` (например, `CourseID` поскольку сущность `Course` имеет первичный ключ `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="33e31-202">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="33e31-203">Сущность Course</span><span class="sxs-lookup"><span data-stu-id="33e31-203">The Course entity</span></span>

![Схема сущности Course](intro/_static/course-entity.png)

<span data-ttu-id="33e31-205">В папке *Models* создайте файл *Course.cs* и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="33e31-205">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="33e31-206">Свойство `Enrollments` является свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="33e31-206">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="33e31-207">Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="33e31-207">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="33e31-208">Более подробное описание атрибута `DatabaseGenerated` будет приведено в [одном из следующих учебников](complex-data-model.md) этой серии.</span><span class="sxs-lookup"><span data-stu-id="33e31-208">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="33e31-209">Фактически, этот атрибут позволяет ввести первичный ключ для курса, а не использовать базу данных, чтобы создать его.</span><span class="sxs-lookup"><span data-stu-id="33e31-209">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="33e31-210">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="33e31-210">Create the database context</span></span>

<span data-ttu-id="33e31-211">Контекст базы данных — это основной класс, который координирует функциональные возможности Entity Framework для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-211">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="33e31-212">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="33e31-212">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="33e31-213">В коде указываются сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-213">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="33e31-214">Также вы можете настроить реакцию платформы Entity Framework на некоторые события.</span><span class="sxs-lookup"><span data-stu-id="33e31-214">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="33e31-215">В этом проекте соответствующий класс называется `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="33e31-215">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="33e31-216">В папке проекта создайте папку *Data*.</span><span class="sxs-lookup"><span data-stu-id="33e31-216">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="33e31-217">В папке *Data* создайте новый файл класса с именем *SchoolContext.cs* и замените код шаблона следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="33e31-217">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="33e31-218">Этот код создает свойство `DbSet` для каждого набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="33e31-218">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="33e31-219">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="33e31-219">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="33e31-220">Вы можете опустить инструкции `DbSet<Enrollment>` и `DbSet<Course>`. Это не нарушит функциональность.</span><span class="sxs-lookup"><span data-stu-id="33e31-220">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="33e31-221">Платформа Entity Framework включает эти инструкции неявно, поскольку сущность `Student` ссылается на сущность `Enrollment`, а `Enrollment` ссылается на сущность `Course`.</span><span class="sxs-lookup"><span data-stu-id="33e31-221">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="33e31-222">При создании базы данных платформа EF создает таблицы с именами, соответствующими именам свойств в `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="33e31-222">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="33e31-223">Имена свойств для коллекций, как правило, задаются во множественном числе (например, Students вместо Student), однако единого мнения по поводу присвоения имен во множественном числе таблицам среди разработчиков не существует.</span><span class="sxs-lookup"><span data-stu-id="33e31-223">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="33e31-224">В этих учебниках вместо принятого по умолчанию способа таблицам в DbContext присваиваются имена в единственном числе.</span><span class="sxs-lookup"><span data-stu-id="33e31-224">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="33e31-225">Чтобы сделать это, добавьте выделенный ниже код после последнего свойства DbSet.</span><span class="sxs-lookup"><span data-stu-id="33e31-225">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="33e31-226">Регистрация SchoolContext</span><span class="sxs-lookup"><span data-stu-id="33e31-226">Register the SchoolContext</span></span>

<span data-ttu-id="33e31-227">ASP.NET Core по умолчанию реализует технологию [внедрения зависимостей](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="33e31-227">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="33e31-228">С помощью внедрения зависимостей службы (например, контекст базы данных EF) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="33e31-228">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="33e31-229">Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="33e31-229">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="33e31-230">Код конструктора контроллера, который получает экземпляр контекста, будет приведен позднее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="33e31-230">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="33e31-231">Чтобы зарегистрировать `SchoolContext` как службу, откройте файл *Startup.cs* и добавьте выделенные строки в метод `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="33e31-231">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="33e31-232">Имя строки подключения передается в контекст путем вызова метода для объекта `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="33e31-232">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="33e31-233">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="33e31-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="33e31-234">Добавьте инструкции `using` для пространств имен `ContosoUniversity.Data` и `Microsoft.EntityFrameworkCore`, после чего выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="33e31-234">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="33e31-235">Откройте файл *appsettings.json* и добавьте строку подключения, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="33e31-235">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="33e31-236">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="33e31-236">SQL Server Express LocalDB</span></span>

<span data-ttu-id="33e31-237">Строка подключения указывает на базу данных SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="33e31-237">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="33e31-238">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки приложений и не ориентированная на использование в производственной среде.</span><span class="sxs-lookup"><span data-stu-id="33e31-238">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="33e31-239">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="33e31-239">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="33e31-240">По умолчанию база данных LocalDB создает файлы *.mdf* в каталоге `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="33e31-240">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="33e31-241">Инициализация базы данных с тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="33e31-241">Initialize DB with test data</span></span>

<span data-ttu-id="33e31-242">Платформа Entity Framework создает пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-242">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="33e31-243">В этом разделе вы напишете метод, который вызывается после создания базы данных и заполняет ее тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="33e31-243">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="33e31-244">Здесь будет использоваться метод `EnsureCreated` для автоматического создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-244">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="33e31-245">В [одном из следующих учебников](migrations.md) вы узнаете, как обрабатывать изменения модели с использованием Code First Migrations, что позволяет изменять схему базы данных вместо того, чтобы удалять и повторно создавать ее.</span><span class="sxs-lookup"><span data-stu-id="33e31-245">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="33e31-246">В папке *Data* создайте новый файл класса с именем *DbInitializer.cs* и замените код шаблона следующим кодом, который обеспечивает создание базы данных и загрузку в нее тестовых данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-246">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="33e31-247">Этот код проверяет, добавлены ли в базу данных учащиеся. Если нет, база данных считается новой и заполняется тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="33e31-247">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="33e31-248">Для повышения производительности тестовые данные загружаются массивами, а не коллекциями `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="33e31-248">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="33e31-249">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее поведение при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="33e31-249">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="33e31-250">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="33e31-250">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="33e31-251">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="33e31-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="33e31-252">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="33e31-252">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="33e31-253">Добавьте инструкции `using`:</span><span class="sxs-lookup"><span data-stu-id="33e31-253">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="33e31-254">В предыдущих учебниках аналогичный код использовался в методе `Configure` в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="33e31-254">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="33e31-255">Мы рекомендуем использовать метод `Configure` только для настройки конвейера запросов.</span><span class="sxs-lookup"><span data-stu-id="33e31-255">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="33e31-256">Код запуска приложения принадлежит методу `Main`.</span><span class="sxs-lookup"><span data-stu-id="33e31-256">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="33e31-257">Теперь при первом запуске приложения будет создана и заполнена тестовыми данными необходимая для работы база данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-257">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="33e31-258">При любом изменении модели данных вы можете удалить базу, обновить метод заполнения и начать работу с новой базой данных аналогичным способом.</span><span class="sxs-lookup"><span data-stu-id="33e31-258">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="33e31-259">В следующих учебниках вы узнаете, как изменить базу данных при изменении модели данных, не прибегая к ее удалению и повторному созданию.</span><span class="sxs-lookup"><span data-stu-id="33e31-259">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="33e31-260">Создание контроллера и представлений</span><span class="sxs-lookup"><span data-stu-id="33e31-260">Create controller and views</span></span>

<span data-ttu-id="33e31-261">Далее вы будете использовать механизм шаблонов Visual Studio для добавления контроллера и представлений MVC, которые будут использовать платформу EF для запроса данных и их сохранения.</span><span class="sxs-lookup"><span data-stu-id="33e31-261">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="33e31-262">Автоматическое создание методов и представлений операций CRUD (создание, чтение, обновление и удаление) называется формированием шаблонов.</span><span class="sxs-lookup"><span data-stu-id="33e31-262">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="33e31-263">Формирование шаблонов отличается от создания кода тем, что шаблонный код является отправной точкой и может изменяться в соответствии с потребностями, тогда как сформированный код обычно не изменяется.</span><span class="sxs-lookup"><span data-stu-id="33e31-263">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="33e31-264">В тех случаях, когда требуется настроить созданный код в соответствии с внесенными изменениями, вы можете использовать разделяемые классы или повторно создать код.</span><span class="sxs-lookup"><span data-stu-id="33e31-264">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="33e31-265">Щелкните правой кнопкой мыши папку **Контроллеры** в **обозревателе решений** и выберите **Добавить > Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="33e31-265">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

<span data-ttu-id="33e31-266">Если открывается диалоговое окно **Добавление зависимостей MVC**:</span><span class="sxs-lookup"><span data-stu-id="33e31-266">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="33e31-267">[Обновите Visual Studio до последней версии](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span><span class="sxs-lookup"><span data-stu-id="33e31-267">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).</span></span> <span data-ttu-id="33e31-268">Это диалоговое окно отображает версии Visual Studio, предшествующие 15.5.</span><span class="sxs-lookup"><span data-stu-id="33e31-268">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="33e31-269">Если обновить не удается, выберите **ДОБАВИТЬ** и снова выполните шаги по добавлению контроллера.</span><span class="sxs-lookup"><span data-stu-id="33e31-269">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

* <span data-ttu-id="33e31-270">В диалоговом окне **Добавление шаблона**:</span><span class="sxs-lookup"><span data-stu-id="33e31-270">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="33e31-271">Выберите **Контроллер MVC с представлениями, использующий Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="33e31-271">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="33e31-272">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="33e31-272">Click **Add**.</span></span> <span data-ttu-id="33e31-273">Откроется диалоговое окно **добавления контроллера MVC с представлениями с использованием Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="33e31-273">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![Шаблон Student](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="33e31-275">В разделе **Класс модели** выберите **Student**.</span><span class="sxs-lookup"><span data-stu-id="33e31-275">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="33e31-276">В разделе **Класс контекста данных** выберите **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="33e31-276">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="33e31-277">Оставьте предлагаемое по умолчанию имя **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="33e31-277">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="33e31-278">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="33e31-278">Click **Add**.</span></span>

  <span data-ttu-id="33e31-279">При нажатии кнопки **Добавить** подсистема формирования шаблонов Visual Studio создает файл *StudentsController.cs* и набор представлений (файлы с расширением *.cshtml*), которые будут работать с контроллером.</span><span class="sxs-lookup"><span data-stu-id="33e31-279">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="33e31-280">(Подсистема формирования шаблонов также может создать контекст базы данных, если вы не создали его вручную, как было показано ранее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="33e31-280">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="33e31-281">Вы можете задать новый класс контекста в диалоговом окне **Добавление контроллера**, нажав на значок плюса справа от раздела **Класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="33e31-281">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="33e31-282">В этом случае Visual Studio создаст класс `DbContext`, а также контроллеры и представления.)</span><span class="sxs-lookup"><span data-stu-id="33e31-282">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="33e31-283">Обратите внимание, что контроллер принимает `SchoolContext` в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="33e31-283">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="33e31-284">Технология внедрения зависимостей ASP.NET Core обеспечивает передачу экземпляра `SchoolContext` в контроллер.</span><span class="sxs-lookup"><span data-stu-id="33e31-284">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="33e31-285">Это поведение было настроено ранее в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="33e31-285">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="33e31-286">Контроллер содержит метод действия `Index`, который отображает всех учащихся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-286">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="33e31-287">Этот метод получает список учащихся из набора сущностей Students, считывая свойство `Students` экземпляра контекста базы данных:</span><span class="sxs-lookup"><span data-stu-id="33e31-287">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="33e31-288">Позднее в этом учебнике будут описаны элементы асинхронного программирования в этом коде.</span><span class="sxs-lookup"><span data-stu-id="33e31-288">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="33e31-289">Представление *Views/Students/Index.cshtml* отображает этот список в таблице:</span><span class="sxs-lookup"><span data-stu-id="33e31-289">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="33e31-290">Нажмите клавиши CTRL+F5, чтобы запустить проект, и выберите в меню **Отладка > Запуск без отладки**.</span><span class="sxs-lookup"><span data-stu-id="33e31-290">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="33e31-291">Перейдите на вкладку Students (Учащиеся), чтобы просмотреть тестовые данные, добавленные методом `DbInitializer.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="33e31-291">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="33e31-292">В зависимости от размеров окна браузера ссылку на вкладку `Student` можно найти вверху страницы или щелкнув значок навигации в правом верхнем углу.</span><span class="sxs-lookup"><span data-stu-id="33e31-292">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Уменьшенное окно с домашней страницей университета Contoso](intro/_static/home-page-narrow.png)

![Страница указателя учащихся](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="33e31-295">Просмотр базы данных</span><span class="sxs-lookup"><span data-stu-id="33e31-295">View the database</span></span>

<span data-ttu-id="33e31-296">При запуске приложения метод `DbInitializer.Initialize` вызывает метод `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="33e31-296">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="33e31-297">Платформа EF определяет, что база данных отсутствует, и создает ее, после чего код в оставшейся части метода `Initialize` заполняет базу данными.</span><span class="sxs-lookup"><span data-stu-id="33e31-297">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="33e31-298">Для просмотра базы данных в Visual Studio можно использовать **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="33e31-298">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="33e31-299">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="33e31-299">Close the browser.</span></span>

<span data-ttu-id="33e31-300">Если окно SSOX не открылось, выберите его в меню **Вид** Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="33e31-300">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="33e31-301">В окне SSOX щелкните **(localdb)\MSSQLLocalDB > Базы данных** и затем щелкните запись базы данных, имя которой указано в строке подключения в вашем файле *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="33e31-301">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="33e31-302">Разверните узел **Таблицы**, чтобы просмотреть представленные в базе таблицы.</span><span class="sxs-lookup"><span data-stu-id="33e31-302">Expand the **Tables** node to see the tables in your database.</span></span>

![Таблицы в SSOX](intro/_static/ssox-tables.png)

<span data-ttu-id="33e31-304">Щелкните правой кнопкой мыши таблицу **Student** и выберите пункт **Просмотр данных**, чтобы просмотреть созданные столбцы и строки, вставленные в базу данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-304">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Таблица Student в SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="33e31-306">Файлы базы данных с расширением <em>MDF</em> и <em>LDF</em> располагаются в папке <em>C:\Users\\<yourusername></em>.</span><span class="sxs-lookup"><span data-stu-id="33e31-306">The <em>.mdf</em> and <em>.ldf</em> database files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="33e31-307">Поскольку вы вызываете метод `EnsureCreated` в методе инициализатора, который выполняется при запуске приложения, теперь вы можете внести изменения в класс `Student`, удалить базу данных и снова запустить приложение. После этого база данных будет создана повторно в соответствии с внесенными изменениями.</span><span class="sxs-lookup"><span data-stu-id="33e31-307">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="33e31-308">Например, при добавлении свойства `EmailAddress` в класс `Student` во вновь созданной таблице появится столбец `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="33e31-308">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="33e31-309">Соглашения</span><span class="sxs-lookup"><span data-stu-id="33e31-309">Conventions</span></span>

<span data-ttu-id="33e31-310">Чтобы платформа Entity Framework автоматически создавала полную базу данных на основе принятых соглашений и допущений, потребуется написать минимальный объем кода.</span><span class="sxs-lookup"><span data-stu-id="33e31-310">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="33e31-311">В качестве имен таблиц используются имена свойств `DbSet`.</span><span class="sxs-lookup"><span data-stu-id="33e31-311">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="33e31-312">Для сущностей, на которые не ссылается свойство `DbSet`, в качестве имен таблиц используются имена классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="33e31-312">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="33e31-313">В качестве имен столбцов используются имена свойств сущностей.</span><span class="sxs-lookup"><span data-stu-id="33e31-313">Entity property names are used for column names.</span></span>

* <span data-ttu-id="33e31-314">Свойства сущностей с именем ID или classnameID распознаются как свойства первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="33e31-314">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="33e31-315">Свойство интерпретируется как свойство внешнего ключа, если оно имеет имя *<navigation property name><primary key property name>* (например, `StudentID` для свойства навигации `Student`, так как сущность `Student` имеет первичный ключ `ID`).</span><span class="sxs-lookup"><span data-stu-id="33e31-315">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="33e31-316">Свойства внешнего ключа также могут называться просто *<primary key property name>* (например `EnrollmentID`, поскольку сущность `Enrollment` имеет первичный ключ `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="33e31-316">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="33e31-317">Стандартное поведение можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="33e31-317">Conventional behavior can be overridden.</span></span> <span data-ttu-id="33e31-318">Например, можно явно задать имена таблиц, как было показано ранее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="33e31-318">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="33e31-319">Также можно указать имена столбцов и задать любое свойство в качестве первичного или внешнего ключа, как будет показано в [одном из следующих учебников](complex-data-model.md) этой серии.</span><span class="sxs-lookup"><span data-stu-id="33e31-319">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="33e31-320">Асинхронный код</span><span class="sxs-lookup"><span data-stu-id="33e31-320">Asynchronous code</span></span>

<span data-ttu-id="33e31-321">Асинхронное программирование по умолчанию применяется в ASP.NET Core и EF Core.</span><span class="sxs-lookup"><span data-stu-id="33e31-321">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="33e31-322">Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки.</span><span class="sxs-lookup"><span data-stu-id="33e31-322">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="33e31-323">В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки.</span><span class="sxs-lookup"><span data-stu-id="33e31-323">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="33e31-324">В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="33e31-324">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="33e31-325">В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов.</span><span class="sxs-lookup"><span data-stu-id="33e31-325">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="33e31-326">Таким образом, асинхронный код позволяет более эффективно использовать ресурсы сервера, который может обрабатывать больше трафика без задержек.</span><span class="sxs-lookup"><span data-stu-id="33e31-326">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="33e31-327">Во время выполнения асинхронный код использует немного больше служебных ресурсов, однако при низком объеме трафика этим можно пренебречь. Тем не менее в случае большого объема трафика это дает существенный выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="33e31-327">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="33e31-328">В следующем коде для асинхронного выполнения используются ключевое слово `async`, возвращаемое значение `Task<T>`, ключевое слово `await` и метод `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="33e31-328">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="33e31-329">Ключевое слово `async` указывает компилятору создавать обратные вызовы для частей тела метода и автоматически создавать возвращаемый объект `Task<IActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="33e31-329">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="33e31-330">Тип возвращаемого значения `Task<IActionResult>` представляет текущую операцию с помощью результата типа `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="33e31-330">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="33e31-331">Ключевое слово `await` предписывает компилятору разделить метод на две части.</span><span class="sxs-lookup"><span data-stu-id="33e31-331">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="33e31-332">Первая часть завершается операцией, которая запускается в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="33e31-332">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="33e31-333">Вторая часть помещается в метод обратного вызова, который вызывается при завершении операции.</span><span class="sxs-lookup"><span data-stu-id="33e31-333">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="33e31-334">`ToListAsync` является асинхронной версией метода расширения `ToList`.</span><span class="sxs-lookup"><span data-stu-id="33e31-334">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="33e31-335">При написании асинхронного кода, который использует Entity Framework, необходимо учитывать некоторые моменты:</span><span class="sxs-lookup"><span data-stu-id="33e31-335">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="33e31-336">Асинхронно выполняются только те инструкции, в результате которых в базу данных отправляются запросы или команды.</span><span class="sxs-lookup"><span data-stu-id="33e31-336">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="33e31-337">К ним относятся, например, `ToListAsync`, `SingleOrDefaultAsync` и `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="33e31-337">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="33e31-338">В их число не входят, например, инструкции, которые просто изменяют `IQueryable`, такие как `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="33e31-338">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="33e31-339">Контекст EF не является потокобезопасным, поэтому не следует пытаться выполнять несколько операций параллельно.</span><span class="sxs-lookup"><span data-stu-id="33e31-339">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="33e31-340">При вызове любого асинхронного метода EF всегда используйте ключевое слово `await`.</span><span class="sxs-lookup"><span data-stu-id="33e31-340">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="33e31-341">Если вы хотите использовать преимущества в производительности, которые обеспечивает асинхронный код, убедитесь, что все используемые пакеты библиотек (например, для разбиения на страницы) также используют асинхронный код при вызове любых методов Entity Framework, выполняющих запросы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="33e31-341">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="33e31-342">Дополнительные сведения об асинхронных методах программирования в .NET см. в разделе [Обзор асинхронной модели](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="33e31-342">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="33e31-343">Получение кода</span><span class="sxs-lookup"><span data-stu-id="33e31-343">Get the code</span></span>

[<span data-ttu-id="33e31-344">Скачайте или ознакомьтесь с готовым приложением.</span><span class="sxs-lookup"><span data-stu-id="33e31-344">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="33e31-345">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="33e31-345">Next steps</span></span>

<span data-ttu-id="33e31-346">В этом учебнике рассмотрены следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="33e31-346">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="33e31-347">Создание веб-приложения MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="33e31-347">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="33e31-348">Настройка стиля сайта</span><span class="sxs-lookup"><span data-stu-id="33e31-348">Set up the site style</span></span>
> * <span data-ttu-id="33e31-349">Дополнительные сведения о пакетах NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="33e31-349">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="33e31-350">Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="33e31-350">Created the data model</span></span>
> * <span data-ttu-id="33e31-351">Создание контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="33e31-351">Created the database context</span></span>
> * <span data-ttu-id="33e31-352">Регистрация SchoolContext</span><span class="sxs-lookup"><span data-stu-id="33e31-352">Registered the SchoolContext</span></span>
> * <span data-ttu-id="33e31-353">Инициализация базы данных с тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="33e31-353">Initialized DB with test data</span></span>
> * <span data-ttu-id="33e31-354">Создание контроллера и представлений</span><span class="sxs-lookup"><span data-stu-id="33e31-354">Created controller and views</span></span>
> * <span data-ttu-id="33e31-355">Просмотр базы данных</span><span class="sxs-lookup"><span data-stu-id="33e31-355">Viewed the database</span></span>

<span data-ttu-id="33e31-356">В следующем учебнике вы узнаете, как выполнять основные операции CRUD (создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="33e31-356">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="33e31-357">В следующем руководстве описано, как выполнять основные операции CRUD (создание, чтение, обновление и удаление).</span><span class="sxs-lookup"><span data-stu-id="33e31-357">Advance to the next article to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="33e31-358">Реализация базовых функций CRUD</span><span class="sxs-lookup"><span data-stu-id="33e31-358">Implement basic CRUD functionality</span></span>](crud.md)
