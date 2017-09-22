---
title: "Ядро ASP.NET MVC с Entity Framework Core - учебник № 1 по 10"
author: tdykstra
description: 
keywords: "ASP.NET Core, Entity Framework Core, учебник"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: b67c3d4a-f2bf-4132-a48b-4b0d599d7981
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: a4e9ab26fa49720aa2334101ee12916fc797d944
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a><span data-ttu-id="583a6-103">Приступая к работе с ASP.NET MVC ядра и Entity Framework Core, с помощью Visual Studio (1, 10)</span><span class="sxs-lookup"><span data-stu-id="583a6-103">Getting started with ASP.NET Core MVC and Entity Framework Core using Visual Studio (1 of 10)</span></span>

<span data-ttu-id="583a6-104">По [Tom Dykstra](https://github.com/tdykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="583a6-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="583a6-105">Contoso университета примера веб-приложения показано, как создавать веб-приложения ASP.NET MVC 2.0 ядра с помощью основных Entity Framework (EF) 2.0 и Visual Studio 2017 г.</span><span class="sxs-lookup"><span data-stu-id="583a6-105">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="583a6-106">Образец приложения является веб-сайт для вымышленной компании Contoso университета.</span><span class="sxs-lookup"><span data-stu-id="583a6-106">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="583a6-107">Он включает функции, такие как допуском студента, создание курса и инструктора назначения.</span><span class="sxs-lookup"><span data-stu-id="583a6-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="583a6-108">Это первое в серию учебников, в которых объясняется, как создать пример приложения университета Contoso с нуля.</span><span class="sxs-lookup"><span data-stu-id="583a6-108">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

[<span data-ttu-id="583a6-109">Загрузить или просмотреть завершенное приложение.</span><span class="sxs-lookup"><span data-stu-id="583a6-109">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

<span data-ttu-id="583a6-110">EF Core 2.0 — это последняя версия EF, но еще не все возможности EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="583a6-110">EF Core 2.0 is the latest version of EF but does not yet have all the features of EF 6.x.</span></span> <span data-ttu-id="583a6-111">Сведения о том, как выбрать между EF 6.x и EF Core см [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span><span class="sxs-lookup"><span data-stu-id="583a6-111">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="583a6-112">При выборе EF 6.x. в разделе [предыдущей версии этого учебника ряда](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="583a6-112">If you choose EF 6.x, see [the previous version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> * <span data-ttu-id="583a6-113">Версии ASP.NET Core 1.1 этого учебника, см. [VS 2017 г. с обновлением 2 версии этого учебника в формате PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="583a6-113">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span></span>
> * <span data-ttu-id="583a6-114">Версию этого учебника для Visual Studio 2015 см. в статье [документации по ASP.NET Core для VS 2015 в формате PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span><span class="sxs-lookup"><span data-stu-id="583a6-114">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="583a6-115">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="583a6-115">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="583a6-116">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="583a6-116">Troubleshooting</span></span>

<span data-ttu-id="583a6-117">Если возникли проблемы, не удается устранить, обычно можно найти решения путем сравнения код, чтобы [завершенного проекта](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span><span class="sxs-lookup"><span data-stu-id="583a6-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="583a6-118">Список распространенных ошибок и способы их устранения см. в разделе [раздел "Устранение неполадок" последнего учебника в серии](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="583a6-118">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="583a6-119">Если вы не нашли нужную информацию существует, можно разместить вопрос StackOverflow.com для [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) или [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="583a6-119">If you don't find what you need there, you can post a question to StackOverflow.com for  [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP] 
> <span data-ttu-id="583a6-120">Это серию учебников, 10, каждый из которых строится на выполняемые операции в предыдущих учебниках.</span><span class="sxs-lookup"><span data-stu-id="583a6-120">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span>  <span data-ttu-id="583a6-121">Рекомендуется сохранить копию проекта после каждого успешного завершения учебника.</span><span class="sxs-lookup"><span data-stu-id="583a6-121">Consider saving a copy of the project after each successful tutorial completion.</span></span>  <span data-ttu-id="583a6-122">Затем, если возникли проблемы, можно начать сначала из предыдущего учебника вместо вернемся к началу весь ряд.</span><span class="sxs-lookup"><span data-stu-id="583a6-122">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="the-contoso-university-web-application"></a><span data-ttu-id="583a6-123">Веб-приложение Contoso университета</span><span class="sxs-lookup"><span data-stu-id="583a6-123">The Contoso University web application</span></span>

<span data-ttu-id="583a6-124">Приложение, которое вы будете построения в этих учебниках является простой университета веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="583a6-124">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="583a6-125">Пользователи могут просматривать и обновлять студента курса и инструктора сведения.</span><span class="sxs-lookup"><span data-stu-id="583a6-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="583a6-126">Ниже представлены несколько экранов, которые будут созданы.</span><span class="sxs-lookup"><span data-stu-id="583a6-126">Here are a few of the screens you'll create.</span></span>

![Страница индекса студентов](intro/_static/students-index.png)

![Страница изменения студентов](intro/_static/student-edit.png)

<span data-ttu-id="583a6-129">Стиль пользовательского интерфейса этого сайта была сохранена близко к создаваемой с помощью встроенных шаблонов учебника можно сосредоточиться на том, как использовать платформу Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="583a6-129">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-an-aspnet-core-mvc-web-application"></a><span data-ttu-id="583a6-130">Создать веб-приложение ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="583a6-130">Create an ASP.NET Core MVC web application</span></span>

<span data-ttu-id="583a6-131">Откройте Visual Studio и создайте новый веб-проекта ASP.NET Core C#, с именем «ContosoUniversity».</span><span class="sxs-lookup"><span data-stu-id="583a6-131">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="583a6-132">Из **файл** последовательно выберите пункты **Создать > проект**.</span><span class="sxs-lookup"><span data-stu-id="583a6-132">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="583a6-133">В левой области выберите **установленные > Visual C# > Web**.</span><span class="sxs-lookup"><span data-stu-id="583a6-133">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="583a6-134">Выберите **веб-приложения ASP.NET Core** шаблона проекта.</span><span class="sxs-lookup"><span data-stu-id="583a6-134">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="583a6-135">Введите **ContosoUniversity** как имя и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="583a6-135">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Диалоговое окно создания нового проекта](intro/_static/new-project.png)

* <span data-ttu-id="583a6-137">Дождитесь **новый основной веб-приложения ASP.NET (.NET Core)** диалоговое окно для отображения</span><span class="sxs-lookup"><span data-stu-id="583a6-137">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

* <span data-ttu-id="583a6-138">Выберите **ASP.NET Core 2.0** и **веб-приложения (Model-View-Controller)** шаблона.</span><span class="sxs-lookup"><span data-stu-id="583a6-138">Select **ASP.NET Core 2.0** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="583a6-139">**Примечание:** упражнений этого учебника требуется ASP.NET 2.0 основных компонентов и Core EF 2.0 или более поздней — убедитесь, что **ASP.NET Core 1.1** не выбран.</span><span class="sxs-lookup"><span data-stu-id="583a6-139">**Note:** This tutorial requires ASP.NET Core 2.0 and EF Core 2.0 or later -- make sure that **ASP.NET Core 1.1** is not selected.</span></span>

* <span data-ttu-id="583a6-140">Убедитесь, что **проверки подлинности** равно **без проверки подлинности**.</span><span class="sxs-lookup"><span data-stu-id="583a6-140">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="583a6-141">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="583a6-141">Click **OK**</span></span>

  ![Диалоговое окно "Новый проект ASP.NET"](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="583a6-143">Настройка стиля узла</span><span class="sxs-lookup"><span data-stu-id="583a6-143">Set up the site style</span></span>

<span data-ttu-id="583a6-144">Несколько простых изменений будет установлен сайт меню, макета и домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="583a6-144">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="583a6-145">Откройте *Views/Shared/_Layout.cshtml* и внесите следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="583a6-145">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="583a6-146">Измените все вхождения «ContosoUniversity» на «Contoso University».</span><span class="sxs-lookup"><span data-stu-id="583a6-146">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="583a6-147">Нет трех экземпляров.</span><span class="sxs-lookup"><span data-stu-id="583a6-147">There are three occurrences.</span></span>

* <span data-ttu-id="583a6-148">Добавление пунктов меню для **учащихся**, **курсы**, **инструкторов**, и **отделы**и удалите **контакт** меню.</span><span class="sxs-lookup"><span data-stu-id="583a6-148">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="583a6-149">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="583a6-149">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

<span data-ttu-id="583a6-150">В *Views/Home/Index.cshtml*, замените содержимое файла следующим кодом, чтобы заменить текст о ASP.NET и MVC на текст об этом приложении:</span><span class="sxs-lookup"><span data-stu-id="583a6-150">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-html[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="583a6-151">Нажмите CTRL + F5, чтобы запустить проект или **Отладка > Начать без отладки** в меню.</span><span class="sxs-lookup"><span data-stu-id="583a6-151">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="583a6-152">Появится домашняя страница с вкладками для страниц, которые можно создать с помощью этих учебников.</span><span class="sxs-lookup"><span data-stu-id="583a6-152">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Домашняя страница университета Contoso](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="583a6-154">Entity Framework Core NuGet пакетов</span><span class="sxs-lookup"><span data-stu-id="583a6-154">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="583a6-155">Добавление поддержка ядра EF в проект, установите поставщик базы данных, которую требуется использовать.</span><span class="sxs-lookup"><span data-stu-id="583a6-155">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="583a6-156">В этом учебнике используется SQL Server и поставщик пакет [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="583a6-156">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="583a6-157">Этот пакет включен в [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, поэтому устанавливать его не требуется.</span><span class="sxs-lookup"><span data-stu-id="583a6-157">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="583a6-158">Этот пакет и его зависимости (`Microsoft.EntityFrameworkCore` и `Microsoft.EntityFrameworkCore.Relational`) обеспечивают поддержку времени выполнения EF.</span><span class="sxs-lookup"><span data-stu-id="583a6-158">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="583a6-159">Вы добавите пакет средств далее в [миграций](migrations.md) учебника.</span><span class="sxs-lookup"><span data-stu-id="583a6-159">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span> 

<span data-ttu-id="583a6-160">Сведения о других поставщиков базы данных, которые доступны для Entity Framework Core см. в разделе [базы данных поставщиков](https://docs.microsoft.com/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="583a6-160">For information about other database providers that are available for Entity Framework Core, see [Database providers](https://docs.microsoft.com/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="583a6-161">Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="583a6-161">Create the data model</span></span>

<span data-ttu-id="583a6-162">Затем вы создадите классы сущностей для приложения Contoso университета.</span><span class="sxs-lookup"><span data-stu-id="583a6-162">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="583a6-163">Вы начнете с следующие три сущности.</span><span class="sxs-lookup"><span data-stu-id="583a6-163">You'll start with the following three entities.</span></span>

![Схема модели курса регистрации студента данных](intro/_static/data-model-diagram.png)

<span data-ttu-id="583a6-165">Имеется отношение "один ко многим" между `Student` и `Enrollment` сущностями, и имеется отношение "один ко многим" между `Course` и `Enrollment` сущности.</span><span class="sxs-lookup"><span data-stu-id="583a6-165">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="583a6-166">Другими словами студент может быть зарегистрированы в любое количество курсы и курс может иметь любое количество студентов, зарегистрированных в нем.</span><span class="sxs-lookup"><span data-stu-id="583a6-166">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="583a6-167">В следующих разделах вы создадите класса для каждого из этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="583a6-167">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="583a6-168">Сущность Student</span><span class="sxs-lookup"><span data-stu-id="583a6-168">The Student entity</span></span>

![Схема entity студента](intro/_static/student-entity.png)

<span data-ttu-id="583a6-170">В *моделей* папки, создайте файл класса с именем *Student.cs* и замените код шаблона с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="583a6-170">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="583a6-171">`ID` Свойство станет столбец первичного ключа таблицы базы данных, соответствующий этому классу.</span><span class="sxs-lookup"><span data-stu-id="583a6-171">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="583a6-172">По умолчанию платформа Entity Framework интерпретирует свойство, которое называется `ID` или `classnameID` как первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="583a6-172">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="583a6-173">`Enrollments` Свойство является свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="583a6-173">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="583a6-174">Свойства навигации хранения других сущностей, которые относятся к этой сущности.</span><span class="sxs-lookup"><span data-stu-id="583a6-174">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="583a6-175">В этом случае `Enrollments` свойство `Student entity` будет содержать все `Enrollment` сущностей, которые относятся к, `Student` сущности.</span><span class="sxs-lookup"><span data-stu-id="583a6-175">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="583a6-176">Другими словами, если данная строка студента в базе данных имеет две связанные регистрации строк (которые содержат этого студента значение первичного ключа в столбец внешнего ключа их идентификатором StudentID), который `Student` сущности `Enrollments` свойство навигации будет содержать те два `Enrollment` сущностей.</span><span class="sxs-lookup"><span data-stu-id="583a6-176">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="583a6-177">Если свойство навигации может содержать несколько сущностей (как отношения "многие ко многим" или "один ко многим"), его тип должен быть список, в котором записи могут быть добавлены, удаленные и обновления, такие как `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="583a6-177">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span>  <span data-ttu-id="583a6-178">Можно указать `ICollection<T>` или тип, такой как `List<T>` или `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="583a6-178">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="583a6-179">При указании `ICollection<T>`, создает EF `HashSet<T>` коллекции по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="583a6-179">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="583a6-180">Сущность регистрации</span><span class="sxs-lookup"><span data-stu-id="583a6-180">The Enrollment entity</span></span>

![Схема entity регистрации](intro/_static/enrollment-entity.png)

<span data-ttu-id="583a6-182">В *моделей* папке создайте *«Enrollment.cs» в качестве* и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="583a6-182">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="583a6-183">`EnrollmentID` Свойство будет иметь первичный ключ, использует эту сущность `classnameID` шаблона вместо `ID` сам по себе, как вы видели в `Student` сущности.</span><span class="sxs-lookup"><span data-stu-id="583a6-183">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="583a6-184">Обычно следует выбрать один шаблон и использовать его во всей модели данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-184">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="583a6-185">Здесь значение варианта иллюстрирует, которые можно использовать либо шаблон.</span><span class="sxs-lookup"><span data-stu-id="583a6-185">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="583a6-186">В [более поздней версии учебника](inheritance.md), вы увидите, как с помощью идентификатора без classname для упрощения реализации наследования в модели данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-186">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="583a6-187">`Grade` Свойство `enum`.</span><span class="sxs-lookup"><span data-stu-id="583a6-187">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="583a6-188">Знак вопроса после `Grade` объявление типа указывает, что `Grade` свойство имеет значение NULL.</span><span class="sxs-lookup"><span data-stu-id="583a6-188">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="583a6-189">Степень, которую имеет значение null отличается от нуля оценку — значение null означает оценку не известен или еще не назначена.</span><span class="sxs-lookup"><span data-stu-id="583a6-189">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="583a6-190">`StudentID` Свойство внешнего ключа, и соответствующее свойство навигации `Student`.</span><span class="sxs-lookup"><span data-stu-id="583a6-190">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="583a6-191">`Enrollment` Сущности связан с одним `Student` сущности, поэтому свойство может содержать только один `Student` сущности (в отличие от `Student.Enrollments` свойство навигации вы видели ранее, который может содержать несколько `Enrollment` сущностей).</span><span class="sxs-lookup"><span data-stu-id="583a6-191">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="583a6-192">`CourseID` Свойство внешнего ключа, и соответствующее свойство навигации `Course`.</span><span class="sxs-lookup"><span data-stu-id="583a6-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="583a6-193">`Enrollment` Сущности связан с одним `Course` сущности.</span><span class="sxs-lookup"><span data-stu-id="583a6-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="583a6-194">Entity Framework интерпретирует свойство как свойство внешнего ключа, если он называется `<navigation property name><primary key property name>` (например, `StudentID` для `Student` свойство навигации с момента `Student` первичного ключа сущности является `ID`).</span><span class="sxs-lookup"><span data-stu-id="583a6-194">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="583a6-195">Свойства внешнего ключа может также называться просто `<primary key property name>` (например, `CourseID` с момента `Course` первичного ключа сущности является `CourseID`).</span><span class="sxs-lookup"><span data-stu-id="583a6-195">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="583a6-196">Сущность курса</span><span class="sxs-lookup"><span data-stu-id="583a6-196">The Course entity</span></span>

![Схема entity курса](intro/_static/course-entity.png)

<span data-ttu-id="583a6-198">В *моделей* папке создайте *Course.cs* и замените существующий код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="583a6-198">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="583a6-199">`Enrollments` Свойство является свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="583a6-199">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="583a6-200">Объект `Course` сущности могут быть связаны с любым количеством `Enrollment` сущностей.</span><span class="sxs-lookup"><span data-stu-id="583a6-200">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="583a6-201">Допустим, что больше о `DatabaseGenerated` атрибута в [более поздней версии учебника](complex-data-model.md) этой серии.</span><span class="sxs-lookup"><span data-stu-id="583a6-201">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="583a6-202">По сути этот атрибут позволяет ввести первичный ключ для курса вместо формирования базы данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-202">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="583a6-203">Создать контекст базы данных</span><span class="sxs-lookup"><span data-stu-id="583a6-203">Create the Database Context</span></span>

<span data-ttu-id="583a6-204">Основной класс, который координирует функции в заданной модели данных Entity Framework является класс контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-204">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="583a6-205">Этот класс создается путем наследования от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="583a6-205">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="583a6-206">В коде указывается, какие сущности включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-206">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="583a6-207">Можно также настроить некоторые параметры поведения Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="583a6-207">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="583a6-208">В этом проекте класс с именем `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="583a6-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="583a6-209">В папку проекта, создайте папку с именем *данные*.</span><span class="sxs-lookup"><span data-stu-id="583a6-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="583a6-210">В *данные* папке создайте новый файл класса с именем *SchoolContext.cs*и замените код шаблона с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="583a6-210">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="583a6-211">Этот код создает `DbSet` свойство для каждого набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="583a6-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="583a6-212">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="583a6-212">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="583a6-213">Может опущен `DbSet<Enrollment>` и `DbSet<Course>` инструкций и он будет работать так же.</span><span class="sxs-lookup"><span data-stu-id="583a6-213">You could have omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="583a6-214">Платформа Entity Framework будет включать их неявно поскольку `Student` ссылки на сущности `Enrollment` сущности и `Enrollment` ссылки на сущности `Course` сущности.</span><span class="sxs-lookup"><span data-stu-id="583a6-214">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="583a6-215">При создании базы данных, EF создает таблицы, которые имеют имена, то же, что `DbSet` имена свойств.</span><span class="sxs-lookup"><span data-stu-id="583a6-215">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="583a6-216">Имена свойств для коллекций, обычно являются множественного числа (учащихся вместо студента), но разработчики не соглашаться ли имена таблиц должны быть имена во множественном числе или нет.</span><span class="sxs-lookup"><span data-stu-id="583a6-216">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="583a6-217">Для этих учебников вы переопределить поведение по умолчанию, указав имена таблиц в единственном числе в класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="583a6-217">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="583a6-218">Чтобы сделать это, добавьте следующий выделенный код после последнего DbSet свойства.</span><span class="sxs-lookup"><span data-stu-id="583a6-218">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="583a6-219">Регистрация контекста с помощью внедрения зависимости</span><span class="sxs-lookup"><span data-stu-id="583a6-219">Register the context with dependency injection</span></span>

<span data-ttu-id="583a6-220">Реализует ASP.NET Core [внедрения зависимостей](../../fundamentals/dependency-injection.md) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="583a6-220">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="583a6-221">Службы (такие как контекст базы данных EF) регистрируются с помощью внедрения зависимости во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="583a6-221">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="583a6-222">Компоненты, которые требуют этих служб (например, контроллеров MVC) предоставляются этим службам через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="583a6-222">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="583a6-223">Вы увидите код конструктора контроллера, который возвращает экземпляр контекста далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="583a6-223">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="583a6-224">Чтобы зарегистрировать `SchoolContext` как служба, откройте *файла Startup.cs*и добавьте выделенные строки к `ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="583a6-224">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="583a6-225">Имя строки подключения передается в контекст путем вызова метода на `DbContextOptionsBuilder` объекта.</span><span class="sxs-lookup"><span data-stu-id="583a6-225">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="583a6-226">Для локальной разработки [система конфигурации ASP.NET Core](../../fundamentals/configuration.md) считывает строку подключения из *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="583a6-226">For local development, the [ASP.NET Core configuration system](../../fundamentals/configuration.md) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="583a6-227">Добавить `using` инструкции для `ContosoUniversity.Data` и `Microsoft.EntityFrameworkCore` пространства имен, а затем постройте проект.</span><span class="sxs-lookup"><span data-stu-id="583a6-227">Add `using` statements for `ContosoUniversity.Data`  and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="583a6-228">Откройте *appsettings.json* и добавьте строку подключения, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="583a6-228">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="583a6-229">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="583a6-229">SQL Server Express LocalDB</span></span>

<span data-ttu-id="583a6-230">Строка подключения указывает на базу данных SQL Server LocalDB.</span><span class="sxs-lookup"><span data-stu-id="583a6-230">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="583a6-231">LocalDB — это облегченная версия SQL Server Express Database Engine и предназначен для разработки приложений, а не использования в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="583a6-231">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="583a6-232">LocalDB запускается по требованию в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="583a6-232">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="583a6-233">По умолчанию создает LocalDB *.mdf* файлов баз данных в `C:/Users/<user>` каталога.</span><span class="sxs-lookup"><span data-stu-id="583a6-233">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-database-with-test-data"></a><span data-ttu-id="583a6-234">Добавьте код для инициализации базы данных с тестовыми данными</span><span class="sxs-lookup"><span data-stu-id="583a6-234">Add code to initialize the database with test data</span></span>

<span data-ttu-id="583a6-235">Платформа Entity Framework будет создать пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-235">The Entity Framework will create an empty database for you.</span></span>  <span data-ttu-id="583a6-236">В этом разделе необходимо написать метод, вызываемый после создания базы данных, чтобы заполнить его данными теста.</span><span class="sxs-lookup"><span data-stu-id="583a6-236">In this section, you write a method that is called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="583a6-237">Здесь вы воспользуетесь `EnsureCreated` метод для автоматического создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-237">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="583a6-238">В [более поздней версии учебника](migrations.md) вы научитесь обрабатывать изменения модели с помощью Code First Migrations для изменения схемы базы данных вместо удаления и повторного создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-238">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="583a6-239">В *данные* папки, создайте новый файл класса с именем *DbInitializer.cs* и замените следующий код, который вызывает базы данных, создаваемый при необходимости код шаблона и загружает данные в новый теста База данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-239">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="583a6-240">Этот код проверяет, если существует любой студентов в базе данных, а в противном случае предполагается новые базы данных и требуется инициализировать с тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="583a6-240">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span>  <span data-ttu-id="583a6-241">Он загружает тестовых данных в массивы вместо `List<T>` коллекций для оптимизации производительности.</span><span class="sxs-lookup"><span data-stu-id="583a6-241">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="583a6-242">В *Program.cs*, изменить `Main` метод для выполнения следующих при запуске приложения:</span><span class="sxs-lookup"><span data-stu-id="583a6-242">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="583a6-243">Получите экземпляр контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="583a6-243">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="583a6-244">Вызовите метод инициализации, передав ему контекст.</span><span class="sxs-lookup"><span data-stu-id="583a6-244">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="583a6-245">Освободить контекст, когда выполняется метод начальное значение.</span><span class="sxs-lookup"><span data-stu-id="583a6-245">Dispose the context when the seed method is done.</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="583a6-246">Добавить `using` инструкции:</span><span class="sxs-lookup"><span data-stu-id="583a6-246">Add `using` statements:</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="583a6-247">В более старых учебники могут возникать похожий код в `Configure` метод в *файла Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="583a6-247">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="583a6-248">Мы рекомендуем использовать `Configure` метод только для настройки конвейера запросов.</span><span class="sxs-lookup"><span data-stu-id="583a6-248">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="583a6-249">Принадлежит кода запуска приложения в `Main` метод.</span><span class="sxs-lookup"><span data-stu-id="583a6-249">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="583a6-250">Теперь первый раз, при запуске приложения, базы данных создана и заполнена с тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="583a6-250">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="583a6-251">При каждом изменении модели данных можно удалить базу данных, обновите seed-метод и начать заново с новой базы данных так же, как.</span><span class="sxs-lookup"><span data-stu-id="583a6-251">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="583a6-252">На следующих занятиях рассматривается вы увидите, как изменить базу данных при изменений, без удаления и повторного создания в модели данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-252">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="583a6-253">Создание контроллера и представления</span><span class="sxs-lookup"><span data-stu-id="583a6-253">Create a controller and views</span></span>

<span data-ttu-id="583a6-254">Далее будет использоваться механизм формирования шаблонов в Visual Studio добавить контроллер MVC и представления, которые будут использовать EF для запроса и сохранения данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-254">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="583a6-255">Автоматическое создание CRUD методы действий и представления называется формирование шаблонов.</span><span class="sxs-lookup"><span data-stu-id="583a6-255">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="583a6-256">Формирование шаблонов отличается от создания кода, что код формирования шаблонов является отправной точкой, можно изменить в соответствии с требованиями, тогда как обычно не изменить созданный код.</span><span class="sxs-lookup"><span data-stu-id="583a6-256">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="583a6-257">При необходимости настройки автоматически созданного кода использовать разделяемые классы или повторном создании кода об изменениях.</span><span class="sxs-lookup"><span data-stu-id="583a6-257">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="583a6-258">Щелкните правой кнопкой мыши **контроллеров** папки в **обозревателе решений** и выберите **Добавить > новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="583a6-258">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="583a6-259">В диалоговом окне **Добавление зависимостей MVC** установите переключатель в положение **Минимальный набор зависимостей** и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="583a6-259">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

  ![Добавление зависимостей](intro/_static/add-depend.png)

  <span data-ttu-id="583a6-261">Visual Studio добавляет зависимости, необходимые для формировании шаблона контроллера.</span><span class="sxs-lookup"><span data-stu-id="583a6-261">Visual Studio adds the dependencies needed to scaffold a controller.</span></span> <span data-ttu-id="583a6-262">Единственное изменение в файле проекта заключается в добавлении `Microsoft.VisualStudio.Web.CodeGeneration.Design` пакета.</span><span class="sxs-lookup"><span data-stu-id="583a6-262">The only change in the project file is the addition of the `Microsoft.VisualStudio.Web.CodeGeneration.Design` package.</span></span>

  <span data-ttu-id="583a6-263">Объект *ScaffoldingReadMe.txt* создается файл, который можно удалить.</span><span class="sxs-lookup"><span data-stu-id="583a6-263">A *ScaffoldingReadMe.txt* file is created which you can delete.</span></span>

* <span data-ttu-id="583a6-264">Еще раз щелкните правой кнопкой мыши **контроллеров** папки в **обозревателе решений** и выберите **Добавить > новый элемент формирования шаблонов**.</span><span class="sxs-lookup"><span data-stu-id="583a6-264">Once again, right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="583a6-265">В **Добавление формирования шаблонов** диалоговое окно:</span><span class="sxs-lookup"><span data-stu-id="583a6-265">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="583a6-266">Выберите **контроллер MVC с представлениями, использующий Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="583a6-266">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="583a6-267">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="583a6-267">Click **Add**.</span></span>

* <span data-ttu-id="583a6-268">В **добавить контроллер** диалоговое окно:</span><span class="sxs-lookup"><span data-stu-id="583a6-268">In the **Add Controller** dialog box:</span></span>

  * <span data-ttu-id="583a6-269">В **класс модели** выберите **студента**.</span><span class="sxs-lookup"><span data-stu-id="583a6-269">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="583a6-270">В **класс контекста данных** выберите **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="583a6-270">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="583a6-271">Примите имя по умолчанию **StudentsController** как имя.</span><span class="sxs-lookup"><span data-stu-id="583a6-271">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="583a6-272">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="583a6-272">Click **Add**.</span></span>

  ![Ученик формирования шаблонов](intro/_static/scaffold-student.png)

  <span data-ttu-id="583a6-274">При нажатии кнопки **добавить**, подсистему формирования шаблонов Visual Studio создает *StudentsController.cs* файл и ряд представлений (*.cshtml* файлы), работающих с контроллером.</span><span class="sxs-lookup"><span data-stu-id="583a6-274">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="583a6-275">(Ядро формирование шаблонов также можно создать контекст базы данных автоматически Если вы не создаете вручную сначала, как это было ранее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="583a6-275">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="583a6-276">Можно указать новый класс контекста в **добавления контроллера** , щелкнув знак «плюс» справа от **класс контекста данных**.</span><span class="sxs-lookup"><span data-stu-id="583a6-276">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="583a6-277">Visual Studio создаст вашей `DbContext` класса, а также контроллеров и представления.)</span><span class="sxs-lookup"><span data-stu-id="583a6-277">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="583a6-278">Обратите внимание, что контроллер принимает `SchoolContext` как параметр конструктора.</span><span class="sxs-lookup"><span data-stu-id="583a6-278">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="583a6-279">Внедрение зависимостей ASP.NET берет на себя передать экземпляр `SchoolContext` в контроллер.</span><span class="sxs-lookup"><span data-stu-id="583a6-279">ASP.NET dependency injection will take care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="583a6-280">Вы настроили в *файла Startup.cs* файлов более ранних версий.</span><span class="sxs-lookup"><span data-stu-id="583a6-280">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="583a6-281">Контроллер содержит `Index` метод действия, который отображает всех студентов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-281">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="583a6-282">Метод возвращает список учащихся из набора, считывая сущностей учащихся `Students` свойства экземпляра контекста базы данных:</span><span class="sxs-lookup"><span data-stu-id="583a6-282">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="583a6-283">Вы узнаете о элементы асинхронного программирования в этом коде далее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="583a6-283">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="583a6-284">*Views/Students/Index.cshtml* представление отображает этот список в таблице:</span><span class="sxs-lookup"><span data-stu-id="583a6-284">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="583a6-285">Нажмите CTRL + F5, чтобы запустить проект или **Отладка > Начать без отладки** в меню.</span><span class="sxs-lookup"><span data-stu-id="583a6-285">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="583a6-286">Перейдите на вкладку учащихся для просмотра тестовых данных, `DbInitializer.Initialize` метод вставки.</span><span class="sxs-lookup"><span data-stu-id="583a6-286">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="583a6-287">В зависимости от того как узких окно браузера, вы увидите `Student` ссылку вкладку в верхней части страницы или придется щелкнуть значок навигации в правом верхнем углу, чтобы увидеть эту ссылку.</span><span class="sxs-lookup"><span data-stu-id="583a6-287">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Домашняя страница Contoso университета узкий](intro/_static/home-page-narrow.png)

![Страница индекса студентов](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="583a6-290">Просмотр базы данных</span><span class="sxs-lookup"><span data-stu-id="583a6-290">View the Database</span></span>

<span data-ttu-id="583a6-291">При запуске приложения, `DbInitializer.Initialize` вызовы метода `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="583a6-291">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="583a6-292">EF узнали, что база данных не была, и поэтому он создан один, а затем остальная часть `Initialize` код метода заполнения базы данных с данными.</span><span class="sxs-lookup"><span data-stu-id="583a6-292">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="583a6-293">Можно использовать **обозреватель объектов SQL Server** (SSOX) для просмотра базы данных в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="583a6-293">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="583a6-294">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="583a6-294">Close the browser.</span></span>

<span data-ttu-id="583a6-295">Если окно SSOX еще не открыт, выберите его из **представление** меню в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="583a6-295">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="583a6-296">В SSOX, щелкните **(localdb) \MSSQLLocalDB > баз данных**, затем щелкните запись для имени базы данных, который находится в строке подключения в вашей *appsettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="583a6-296">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that is in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="583a6-297">Разверните **таблиц** узел, чтобы просмотреть таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-297">Expand the **Tables** node to see the tables in your database.</span></span>

![Таблицы в SSOX](intro/_static/ssox-tables.png)

<span data-ttu-id="583a6-299">Щелкните правой кнопкой мыши **студента** и нажмите кнопку **данные представления** столбцы, которые были созданы и строк, которые были вставлены в таблицу.</span><span class="sxs-lookup"><span data-stu-id="583a6-299">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Таблица студента в SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="583a6-301">*.Mdf* и *.ldf* файлы базы данных находятся в *C:\Users\<имя_пользователя >* папки.</span><span class="sxs-lookup"><span data-stu-id="583a6-301">The *.mdf* and *.ldf* database files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="583a6-302">Так как выполняется вызов `EnsureCreated` в методе инициализатора, который выполняется при запуске приложения, теперь может внести изменения в `Student` класса, удалите базу данных, снова запустите приложение и базы данных автоматически будет создан повторно для сопоставления внесенные изменения.</span><span class="sxs-lookup"><span data-stu-id="583a6-302">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="583a6-303">Например, при добавлении `EmailAddress` свойства `Student` класса, вы увидите новую `EmailAddress` столбца в таблице будет создана повторно.</span><span class="sxs-lookup"><span data-stu-id="583a6-303">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="583a6-304">Соглашения</span><span class="sxs-lookup"><span data-stu-id="583a6-304">Conventions</span></span>

<span data-ttu-id="583a6-305">Объем кода, который раньше нужно было написать в порядке иметь возможность создавать всей базы данных автоматически Entity Framework сводится к минимуму из-за использования соглашения или предположения, что позволяет платформе Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="583a6-305">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="583a6-306">Имена `DbSet` свойства используются в качестве имен таблиц.</span><span class="sxs-lookup"><span data-stu-id="583a6-306">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="583a6-307">Для сущностей, не ссылается `DbSet` свойство класса сущности, имена используются в качестве имен таблиц.</span><span class="sxs-lookup"><span data-stu-id="583a6-307">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="583a6-308">Имена свойств сущности используются для имен столбцов.</span><span class="sxs-lookup"><span data-stu-id="583a6-308">Entity property names are used for column names.</span></span>

* <span data-ttu-id="583a6-309">Свойства сущности, которые называются Идентификатором или classnameID распознаются как свойства основного ключа.</span><span class="sxs-lookup"><span data-stu-id="583a6-309">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="583a6-310">Свойство интерпретируется как свойство внешнего ключа, если он называется * <navigation property name> <primary key property name> * (например, `StudentID` для `Student` свойство навигации с момента `Student` — первичный ключ сущности `ID`).</span><span class="sxs-lookup"><span data-stu-id="583a6-310">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="583a6-311">Свойства внешнего ключа может также называться просто * <primary key property name> * (например, `EnrollmentID` с момента `Enrollment` первичного ключа сущности является `EnrollmentID`).</span><span class="sxs-lookup"><span data-stu-id="583a6-311">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="583a6-312">Можно переопределить обычным образом.</span><span class="sxs-lookup"><span data-stu-id="583a6-312">Conventional behavior can be overridden.</span></span> <span data-ttu-id="583a6-313">Например можно явно указать имена таблиц, как было показано ранее в этом учебнике.</span><span class="sxs-lookup"><span data-stu-id="583a6-313">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="583a6-314">И можно задать имена столбцов и настройте желаемые свойства как первичного или внешнего ключа, как можно будет увидеть в [более поздней версии учебника](complex-data-model.md) этой серии.</span><span class="sxs-lookup"><span data-stu-id="583a6-314">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="583a6-315">Асинхронный код</span><span class="sxs-lookup"><span data-stu-id="583a6-315">Asynchronous code</span></span>

<span data-ttu-id="583a6-316">Асинхронное программирование является режимом по умолчанию для ASP.NET Core и EF Core.</span><span class="sxs-lookup"><span data-stu-id="583a6-316">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="583a6-317">Ограниченное число потоков, доступных на веб-сервере, и в случае высокой загрузки все доступные потоки быть используется.</span><span class="sxs-lookup"><span data-stu-id="583a6-317">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="583a6-318">В этом случае сервер может обработать новые запросы, пока не освободятся потоков.</span><span class="sxs-lookup"><span data-stu-id="583a6-318">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="583a6-319">С синхронным кодом большое количество потоков может блокировали они не выполняя фактически действия из-за ожидания завершения ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="583a6-319">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="583a6-320">Асинхронный код когда процесс ожидает ввода-вывода завершить, свой поток освобождается для сервера, используемые для обработки других запросов.</span><span class="sxs-lookup"><span data-stu-id="583a6-320">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="583a6-321">В результате асинхронного кода обеспечивает более эффективное использование ресурсов сервера и что сервер включен для обработки большего объема трафика без задержек.</span><span class="sxs-lookup"><span data-stu-id="583a6-321">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="583a6-322">Асинхронного кода привести небольшой объем служебных данных во время выполнения, но в ситуациях сниженным трафиком, снижение производительности будет незначительным, при в ситуациях с высоким трафиком, является существенным потенциальное улучшение производительности.</span><span class="sxs-lookup"><span data-stu-id="583a6-322">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="583a6-323">В следующем коде `async` ключевое слово, `Task<T>` возвращаемое значение, `await` ключевое слово, и `ToListAsync` метод делает код более асинхронного выполнения.</span><span class="sxs-lookup"><span data-stu-id="583a6-323">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="583a6-324">`async` Ключевое слово указывает компилятору создавать обратные вызовы для части тела метода и автоматически создавать `Task<IActionResult>` возвращаемый объект.</span><span class="sxs-lookup"><span data-stu-id="583a6-324">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that is returned.</span></span>

* <span data-ttu-id="583a6-325">Тип возвращаемого значения `Task<IActionResult>` представляет выполняющуюся работу с результат типа `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="583a6-325">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="583a6-326">`await` Ключевое слово указывает компилятору на необходимость разделить на две части метода.</span><span class="sxs-lookup"><span data-stu-id="583a6-326">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="583a6-327">Первая часть завершается с операцией, которая запускается в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="583a6-327">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="583a6-328">Вторая часть переводится в метод обратного вызова, вызываемый после завершения операции.</span><span class="sxs-lookup"><span data-stu-id="583a6-328">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="583a6-329">`ToListAsync`асинхронная версия `ToList` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="583a6-329">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="583a6-330">Некоторые аспекты, которые следует учитывать при написании асинхронного кода, использующего платформу Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="583a6-330">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="583a6-331">Только инструкции, вызывающие запросов или команд, отправляемых в базе данных выполняются асинхронно.</span><span class="sxs-lookup"><span data-stu-id="583a6-331">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="583a6-332">Содержащий, например, `ToListAsync`, `SingleOrDefaultAsync`, и `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="583a6-332">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span>  <span data-ttu-id="583a6-333">Он содержит, например, инструкции, которые просто изменить `IQueryable`, такие как `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="583a6-333">It does not include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="583a6-334">Контекст EF не является потокобезопасным: не пытайтесь выполнить несколько операций параллельно.</span><span class="sxs-lookup"><span data-stu-id="583a6-334">An EF context is not thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="583a6-335">При вызове любого метода async EF, всегда используйте `await` ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="583a6-335">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="583a6-336">Если вы хотите использовать преимущества производительности асинхронного кода, убедитесь, что ни одну библиотеку пакеты, которые вы используете (например, для разбиения на страницы), также используют асинхронный, вызывающие методы Entity Framework, которые запросы, отправляемые в базу данных.</span><span class="sxs-lookup"><span data-stu-id="583a6-336">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="583a6-337">Дополнительные сведения об асинхронном программировании в .NET см. в разделе [Обзор Async](https://docs.microsoft.com/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="583a6-337">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

## <a name="summary"></a><span data-ttu-id="583a6-338">Сводка</span><span class="sxs-lookup"><span data-stu-id="583a6-338">Summary</span></span>

<span data-ttu-id="583a6-339">В результате создается простое приложение, которое используется для хранения и отображения данных Entity Framework Core и SQL Server Express LocalDB.</span><span class="sxs-lookup"><span data-stu-id="583a6-339">You've now created a simple application that uses the Entity Framework Core and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="583a6-340">В этом руководстве вы узнаете, как выполнять основные CRUD (Создание, чтение, обновление и удаление) операции.</span><span class="sxs-lookup"><span data-stu-id="583a6-340">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="583a6-341">Вперед</span><span class="sxs-lookup"><span data-stu-id="583a6-341">Next</span></span>](crud.md)  
