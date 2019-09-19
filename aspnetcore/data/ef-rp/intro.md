---
title: 'Razor Pages с Entity Framework Core в ASP.NET Core: учебник 1 из 8'
author: tdykstra
description: Сведения о том, как создать приложение Razor Pages с помощью Entity Framework Core
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 07/22/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 107b348b4484301b86eeb5528833914fe4c1eaf7
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080940"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="c56e0-103">Razor Pages с Entity Framework Core в ASP.NET Core: учебник 1 из 8</span><span class="sxs-lookup"><span data-stu-id="c56e0-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="c56e0-104">Авторы: [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra) и [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="c56e0-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c56e0-105">Это первый учебник из серии, посвященной использованию Entity Framework (EF) Core в приложении Razor Pages на основе ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c56e0-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an ASP.NET Core Razor Pages app.</span></span> <span data-ttu-id="c56e0-106">В учебниках создается веб-сайт для вымышленного университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="c56e0-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="c56e0-107">На сайте предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="c56e0-108">Скачайте или ознакомьтесь с готовым приложением.</span><span class="sxs-lookup"><span data-stu-id="c56e0-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="c56e0-109">[Указания по скачиванию](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="c56e0-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c56e0-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c56e0-110">Prerequisites</span></span>

* <span data-ttu-id="c56e0-111">Если у вас нет опыта работы с Razor Pages, ознакомьтесь с серией учебников [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), прежде чем приступать к изучению этого учебника.</span><span class="sxs-lookup"><span data-stu-id="c56e0-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-113">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="c56e0-114">Ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="c56e0-114">Database engines</span></span>

<span data-ttu-id="c56e0-115">В инструкциях для Visual Studio используется [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), версия SQL Server Express, которая работает только в Windows.</span><span class="sxs-lookup"><span data-stu-id="c56e0-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="c56e0-116">В инструкциях для Visual Studio Code используется [SQLite](https://www.sqlite.org/), кроссплатформенное ядро СУБД.</span><span class="sxs-lookup"><span data-stu-id="c56e0-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="c56e0-117">Если вы решили использовать SQLite, скачайте и установите стороннее средство для управления базой данных SQLite и ее просмотра, например [обозреватель базы данных для SQLite](https://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="c56e0-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c56e0-118">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="c56e0-118">Troubleshooting</span></span>

<span data-ttu-id="c56e0-119">Если вы столкнулись с проблемой, которую не можете решить, сравните свой код с кодом [готового проекта](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="c56e0-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="c56e0-120">Чтобы получить помощь, вы можете опубликовать вопрос на веб-сайте StackOverflow.com, используя [тег ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) или [тег EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="c56e0-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="c56e0-121">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="c56e0-121">The sample app</span></span>

<span data-ttu-id="c56e0-122">Приложение, создаваемое в этих руководствах, является простым веб-сайтом университета.</span><span class="sxs-lookup"><span data-stu-id="c56e0-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="c56e0-123">Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях.</span><span class="sxs-lookup"><span data-stu-id="c56e0-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="c56e0-124">Здесь приведено несколько экранов, создаваемых в руководстве.</span><span class="sxs-lookup"><span data-stu-id="c56e0-124">Here are a few of the screens created in the tutorial.</span></span>

![Страница указателя учащихся](intro/_static/students-index30.png)

![Страница редактирования учащихся](intro/_static/student-edit30.png)

<span data-ttu-id="c56e0-127">Стиль пользовательского интерфейса для этого сайта основан на встроенных шаблонах проектов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="c56e0-128">Это руководство посвящено использованию EF Core, а не настройке пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="c56e0-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="c56e0-129">Чтобы получить исходный код готового проекта, перейдите по ссылке в верхней части страницы.</span><span class="sxs-lookup"><span data-stu-id="c56e0-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="c56e0-130">В папке *cu30* содержится код для версии учебника по ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="c56e0-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="c56e0-131">Файлы, отражающие состояние кода для учебников 1–7, находятся в папке *cu30snapshots*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c56e0-133">Чтобы запустить приложение после скачивания готового проекта, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="c56e0-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="c56e0-134">Удалите три файла и одну папку, в именах которых есть слово *SQLite*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="c56e0-135">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-135">Build the project.</span></span>
* <span data-ttu-id="c56e0-136">В консоли диспетчера пакетов (PMC) выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="c56e0-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="c56e0-137">Запустите проект, чтобы заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-138">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c56e0-139">Чтобы запустить приложение после скачивания готового проекта, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="c56e0-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="c56e0-140">Удалите файл *ContosoUniversity.csproj*, а файл *ContosoUniversitySQLite.csproj* переименуйте в *ContosoUniversity.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="c56e0-141">Удалите файл *Startup.cs*, а файл *StartupSQLite.cs* переименуйте в *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="c56e0-142">Удалите файл *appSettings.json*, а файл *appSettingsSQLite.json* переименуйте в *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="c56e0-143">Удалите папку *Migrations*, а папку *MigrationsSQL* переименуйте в *Migrations*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="c56e0-144">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-144">Build the project.</span></span>
* <span data-ttu-id="c56e0-145">В командной строке в папке проекта выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="c56e0-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef --version 3.0.0-*
  dotnet ef database update
  ```

* <span data-ttu-id="c56e0-146">В средстве SQLite выполните следующую инструкцию SQL:</span><span class="sxs-lookup"><span data-stu-id="c56e0-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="c56e0-147">Запустите проект, чтобы заполнить базу данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="c56e0-148">Создание проекта веб-приложения</span><span class="sxs-lookup"><span data-stu-id="c56e0-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c56e0-150">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c56e0-151">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="c56e0-152">Назовите проект *ContosoUniversity*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="c56e0-153">Очень важно использовать именно такое имя с учетом регистра символов, чтобы пространства имен совпадали при копировании и вставке кода.</span><span class="sxs-lookup"><span data-stu-id="c56e0-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="c56e0-154">Выберите в раскрывающихся списках пункты **.NET Core** и **ASP.NET Core 3.0**, а затем выберите **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-155">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c56e0-156">В терминале перейдите в папку, в которой следует создать папку проекта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="c56e0-157">Выполните следующие команды, чтобы создать проект Razor Pages и перейти (`cd`) в новую папку проекта:</span><span class="sxs-lookup"><span data-stu-id="c56e0-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="c56e0-158">Настройка стиля сайта</span><span class="sxs-lookup"><span data-stu-id="c56e0-158">Set up the site style</span></span>

<span data-ttu-id="c56e0-159">Настройте заголовок сайта, нижний колонтитул и меню, внеся изменения в файл *Pages/Shared/_Layout.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="c56e0-160">Замените все вхождения "ContosoUniversity" на "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="c56e0-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="c56e0-161">Таких элементов будет три.</span><span class="sxs-lookup"><span data-stu-id="c56e0-161">There are three occurrences.</span></span>

* <span data-ttu-id="c56e0-162">Удалите пункты меню **Home** (Главная) и **Privacy** (Конфиденциальность). Добавьте пункты **About** (Сведения), **Students** (Учащиеся), **Courses** (Курсы), **Instructors** (Преподаватели) и **Departments** (Кафедры).</span><span class="sxs-lookup"><span data-stu-id="c56e0-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="c56e0-163">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="c56e0-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="c56e0-164">Замените содержимое файла *Pages/Index.cshtml* следующим кодом, который заменяет текст об ASP.NET Core описанием этого приложения:</span><span class="sxs-lookup"><span data-stu-id="c56e0-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="c56e0-165">Запустите приложение, чтобы проверить правильность отображения домашней страницы.</span><span class="sxs-lookup"><span data-stu-id="c56e0-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="c56e0-166">Модель данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-166">The data model</span></span>

<span data-ttu-id="c56e0-167">В следующих разделах создается модель данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-167">The following sections create a data model:</span></span>

![Схема модели данных "курс-регистрация-учащийся"](intro/_static/data-model-diagram.png)

<span data-ttu-id="c56e0-169">Учащийся может зарегистрироваться в любом количестве курсов, а в отдельном курсе может быть зарегистрировано любое количество учащихся.</span><span class="sxs-lookup"><span data-stu-id="c56e0-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="c56e0-170">Сущность Student</span><span class="sxs-lookup"><span data-stu-id="c56e0-170">The Student entity</span></span>

![Схема сущности Student](intro/_static/student-entity.png)

* <span data-ttu-id="c56e0-172">Создайте папку *Models* (Модели) в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="c56e0-173">Создайте файл *Models/Student.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c56e0-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="c56e0-174">Свойство `ID` используется в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу.</span><span class="sxs-lookup"><span data-stu-id="c56e0-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="c56e0-175">По умолчанию платформа EF Core интерпретирует в качестве первичного ключа свойство `ID` или `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="c56e0-176">Поэтому альтернативным автоматически распознаваемым именем для первичного ключа класса `Student` является `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="c56e0-177">Свойство `Enrollments` является [свойством навигации](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="c56e0-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="c56e0-178">Свойства навигации содержат другие сущности, связанные с этой сущностью.</span><span class="sxs-lookup"><span data-stu-id="c56e0-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="c56e0-179">В этом случае свойство `Enrollments` сущности `Student` содержит все сущности `Enrollment`, которые связаны с этим учащимся.</span><span class="sxs-lookup"><span data-stu-id="c56e0-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="c56e0-180">Например, если строка Student (Учащийся) в базе данных имеет две связанные строки Enrollment (Регистрация), свойство навигации `Enrollments` содержит две эти сущности Enrollment.</span><span class="sxs-lookup"><span data-stu-id="c56e0-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="c56e0-181">В базе данных строка Enrollment связана со строкой Student, если ее столбец StudentID содержит идентификатор учащегося.</span><span class="sxs-lookup"><span data-stu-id="c56e0-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="c56e0-182">Например, предположим, что строка Student содержит идентификатор 1.</span><span class="sxs-lookup"><span data-stu-id="c56e0-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="c56e0-183">Связанные строки Enrollment будут содержать значение StudentID (идентификатор учащегося), равное 1.</span><span class="sxs-lookup"><span data-stu-id="c56e0-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="c56e0-184">StudentID — это *внешний ключ* в таблице Enrollment.</span><span class="sxs-lookup"><span data-stu-id="c56e0-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="c56e0-185">Свойство `Enrollments` определено как `ICollection<Enrollment>`, так как может быть несколько связанных сущностей Enrollment.</span><span class="sxs-lookup"><span data-stu-id="c56e0-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="c56e0-186">Можно использовать и другие типы коллекций, например `List<Enrollment>` или `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="c56e0-187">Если используется `ICollection<Enrollment>`, платформа EF Core по умолчанию создает коллекцию `HashSet<Enrollment>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="c56e0-188">Сущность Enrollment</span><span class="sxs-lookup"><span data-stu-id="c56e0-188">The Enrollment entity</span></span>

![Схема сущности Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="c56e0-190">Создайте файл *Models/Enrollment.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c56e0-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="c56e0-191">Свойство `EnrollmentID` является первичным ключом. В этой сущности используется шаблон `classnameID` вместо `ID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="c56e0-192">Для рабочей модели данных выберите один шаблон и используйте только его.</span><span class="sxs-lookup"><span data-stu-id="c56e0-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="c56e0-193">В этом учебнике используются оба шаблона, чтобы проиллюстрировать их работу.</span><span class="sxs-lookup"><span data-stu-id="c56e0-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="c56e0-194">Использование `ID` без `classname` упрощает внесение некоторых изменений в модель данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="c56e0-195">Свойство `Grade` имеет тип `enum`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="c56e0-196">Знак вопроса после объявления типа `Grade` указывает, что свойство `Grade` [допускает значение NULL](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span><span class="sxs-lookup"><span data-stu-id="c56e0-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="c56e0-197">Оценка со значением NULL отличается от нулевой оценки тем, что при таком значении оценка еще не известна или не назначена.</span><span class="sxs-lookup"><span data-stu-id="c56e0-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="c56e0-198">Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="c56e0-199">Сущность `Enrollment` связана с одной сущностью `Student`, поэтому свойство содержит отдельную сущность `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="c56e0-200">Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="c56e0-201">Сущность `Enrollment` связана с одной сущностью `Course`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="c56e0-202">EF Core воспринимает свойство как внешний ключ, если он имеет имя `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="c56e0-203">Например, `StudentID` является внешним ключом для свойства навигации `Student`, так как сущность `Student` имеет первичный ключ `ID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="c56e0-204">Свойства внешнего ключа также могут называться `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="c56e0-205">Например, `CourseID`, так как сущность `Course` имеет первичный ключ `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="c56e0-206">Сущность Course</span><span class="sxs-lookup"><span data-stu-id="c56e0-206">The Course entity</span></span>

![Схема сущности Course](intro/_static/course-entity.png)

<span data-ttu-id="c56e0-208">Создайте файл *Models/Course.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c56e0-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="c56e0-209">Свойство `Enrollments` является свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="c56e0-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c56e0-210">Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="c56e0-211">Атрибут `DatabaseGenerated` позволяет приложению указать первичный ключ, а не использовать созданный базой данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="c56e0-212">Выполните сборку проекта и убедитесь в отсутствии ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="c56e0-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="c56e0-213">Формирование шаблона для страниц Student</span><span class="sxs-lookup"><span data-stu-id="c56e0-213">Scaffold Student pages</span></span>

<span data-ttu-id="c56e0-214">В этом разделе вы используете средство формирования шаблонов ASP.NET Core для создания указанных ниже компонентов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="c56e0-215">Класс *контекста* EF Core.</span><span class="sxs-lookup"><span data-stu-id="c56e0-215">An EF Core *context* class.</span></span> <span data-ttu-id="c56e0-216">Контекст —это основной класс, который координирует функциональные возможности Entity Framework для определенной модели данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="c56e0-217">Он является производным от класса `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="c56e0-218">Страницы Razor, обрабатывающие операции создания, чтения, обновления и удаления (CRUD) для сущности `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c56e0-220">В папке *Pages* создайте папку *Students*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="c56e0-221">В **обозревателе решений** щелкните правой кнопкой мыши папку *Pages/Students* и выберите пункты **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c56e0-222">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **ДОБАВИТЬ**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="c56e0-223">В диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="c56e0-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="c56e0-224">В раскрывающемся списке **Класс модели** выберите **Student (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="c56e0-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="c56e0-225">В строке **Класс контекста данных** щелкните знак плюса ( **+** ).</span><span class="sxs-lookup"><span data-stu-id="c56e0-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="c56e0-226">Измените имя контекста данных с *ContosoUniversity.Models.ContosoUniversityContext* на *ContosoUniversity.Data.SchoolContext*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="c56e0-227">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-227">Select **Add**.</span></span>

<span data-ttu-id="c56e0-228">Следующие пакеты устанавливаются автоматически:</span><span class="sxs-lookup"><span data-stu-id="c56e0-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-229">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c56e0-230">Выполните следующие команды интерфейса командной строки .NET Core, чтобы установить необходимые пакеты NuGet:</span><span class="sxs-lookup"><span data-stu-id="c56e0-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
  dotnet add package Microsoft.EntityFrameworkCore.Tools --version 3.0.0-*
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
  dotnet add package Microsoft.Extensions.Logging.Debug --version 3.0.0-*
  ```

  <span data-ttu-id="c56e0-231">Пакет Microsoft.VisualStudio.Web.CodeGeneration.Design требуется для формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="c56e0-232">Хотя приложение не будет использовать SQL Server, средству формирования шаблонов требуется пакет SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c56e0-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="c56e0-233">Создайте папку *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="c56e0-234">Выполните приведенную ниже команду, чтобы установить [средство формирования шаблонов aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="c56e0-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
  ```

* <span data-ttu-id="c56e0-235">Выполните приведенную ниже команду, чтобы сформировать шаблон для страниц Student.</span><span class="sxs-lookup"><span data-stu-id="c56e0-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="c56e0-236">**В Windows**</span><span class="sxs-lookup"><span data-stu-id="c56e0-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="c56e0-237">**В macOS или Linux**</span><span class="sxs-lookup"><span data-stu-id="c56e0-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="c56e0-238">Если в предыдущем шаге возникает проблема, выполните сборку проекта и повторите шаг формирования шаблона.</span><span class="sxs-lookup"><span data-stu-id="c56e0-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="c56e0-239">В процессе формирования шаблона выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="c56e0-239">The scaffolding process:</span></span>

* <span data-ttu-id="c56e0-240">создаются страницы Razor в папке *Pages/Students*:</span><span class="sxs-lookup"><span data-stu-id="c56e0-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="c56e0-241">*Create.cshtml* и *Create.cshtml.cs*;</span><span class="sxs-lookup"><span data-stu-id="c56e0-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="c56e0-242">*Delete.cshtml* и *Delete.cshtml.cs*;</span><span class="sxs-lookup"><span data-stu-id="c56e0-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="c56e0-243">*Details.cshtml* и *Details.cshtml.cs*;</span><span class="sxs-lookup"><span data-stu-id="c56e0-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="c56e0-244">*Edit.cshtml* и *Edit.cshtml.cs*;</span><span class="sxs-lookup"><span data-stu-id="c56e0-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="c56e0-245">*Index.cshtml* и *Index.cshtml.cs*;</span><span class="sxs-lookup"><span data-stu-id="c56e0-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="c56e0-246">создается файл *Data/SchoolContext.cs*;</span><span class="sxs-lookup"><span data-stu-id="c56e0-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="c56e0-247">добавляется контекст для внедрения зависимостей в файле *Startup.cs*;</span><span class="sxs-lookup"><span data-stu-id="c56e0-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="c56e0-248">добавляется строка подключения к базе данных в файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="c56e0-249">Строка подключения к базе данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c56e0-251">Строка подключения указывает базу данных [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="c56e0-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="c56e0-252">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки приложений и не ориентированная на использование в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="c56e0-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="c56e0-253">По умолчанию LocalDB создает файлы *MDF* в каталоге `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-254">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c56e0-255">Измените строку подключения так, чтобы она указывала на файл базы данных SQLite с именем *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="c56e0-256">Обновление класса контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-256">Update the database context class</span></span>

<span data-ttu-id="c56e0-257">Контекст базы данных — это основной класс, который координирует функциональные возможности EF Core для определенной модели данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="c56e0-258">Контекст наследуется от [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="c56e0-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="c56e0-259">Контекст указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="c56e0-260">В этом проекте соответствующий класс называется `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="c56e0-261">Обновите файл *SchoolContext.cs*, добавив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="c56e0-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="c56e0-262">Выделенный код создает свойство [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для каждого набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="c56e0-263">В терминологии EF Core:</span><span class="sxs-lookup"><span data-stu-id="c56e0-263">In EF Core terminology:</span></span>

* <span data-ttu-id="c56e0-264">Набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="c56e0-265">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="c56e0-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c56e0-266">Так как набор сущностей содержит несколько сущностей, свойства DBSet должны иметь имена во множественном числе.</span><span class="sxs-lookup"><span data-stu-id="c56e0-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="c56e0-267">Так как средство формирования шаблонов создало DBSet `Student`, в этом шаге его имя меняется на имя во множественном числе: `Students`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="c56e0-268">Чтобы код Razor Pages соответствовал новому имени DBSet, измените `_context.Student` на `_context.Students` в рамках всего проекта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="c56e0-269">Всего это имя встречается 8 раз.</span><span class="sxs-lookup"><span data-stu-id="c56e0-269">There are 8 occurrences.</span></span>

<span data-ttu-id="c56e0-270">Выполните сборку проекта и убедитесь в отсутствии ошибок компилятора.</span><span class="sxs-lookup"><span data-stu-id="c56e0-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="c56e0-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="c56e0-271">Startup.cs</span></span>

<span data-ttu-id="c56e0-272">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c56e0-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c56e0-273">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c56e0-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c56e0-274">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="c56e0-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c56e0-275">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="c56e0-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c56e0-276">Средство формирования шаблонов автоматически зарегистрировало класс контекста в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c56e0-278">Выделенные строки в `ConfigureServices` были добавлены средством формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="c56e0-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-279">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c56e0-280">Убедитесь в том, что код, добавленный средством формирования шаблонов в `ConfigureServices`, вызывает `UseSqlite`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="c56e0-281">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="c56e0-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="c56e0-282">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="c56e0-283">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-283">Create the database</span></span>

<span data-ttu-id="c56e0-284">Обновите файл *Program.cs*, чтобы создать базу данных, если она не существует.</span><span class="sxs-lookup"><span data-stu-id="c56e0-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="c56e0-285">Метод [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) не выполняет никаких действий, если база данных для контекста существует.</span><span class="sxs-lookup"><span data-stu-id="c56e0-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="c56e0-286">Если база данных не существует, она создается вместе со схемой.</span><span class="sxs-lookup"><span data-stu-id="c56e0-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="c56e0-287">`EnsureCreated` обеспечивает описанный ниже рабочий процесс для обработки изменений модели данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="c56e0-288">База данных удаляется.</span><span class="sxs-lookup"><span data-stu-id="c56e0-288">Delete the database.</span></span> <span data-ttu-id="c56e0-289">Все существующие данные теряются.</span><span class="sxs-lookup"><span data-stu-id="c56e0-289">Any existing data is lost.</span></span>
* <span data-ttu-id="c56e0-290">Модель данных изменяется.</span><span class="sxs-lookup"><span data-stu-id="c56e0-290">Change the data model.</span></span> <span data-ttu-id="c56e0-291">Например, добавляется поле `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="c56e0-292">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c56e0-292">Run the app.</span></span>
* <span data-ttu-id="c56e0-293">Метод `EnsureCreated` создает базу данных с новой схемой.</span><span class="sxs-lookup"><span data-stu-id="c56e0-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="c56e0-294">Этот рабочий процесс хорошо подходит для ранних стадий разработки, когда схема часто меняется, если данные сохранять не требуется.</span><span class="sxs-lookup"><span data-stu-id="c56e0-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="c56e0-295">Однако если данные, введенные в базу данных, необходимо сохранять, ситуация будет иной.</span><span class="sxs-lookup"><span data-stu-id="c56e0-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="c56e0-296">В таком случае используйте перенос.</span><span class="sxs-lookup"><span data-stu-id="c56e0-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="c56e0-297">Далее в этой серии учебников вы удалите базу данных, созданную методом `EnsureCreated`, и используете вместо этого перенос.</span><span class="sxs-lookup"><span data-stu-id="c56e0-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="c56e0-298">Созданную методом `EnsureCreated` базу данных нельзя обновить, используя перенос.</span><span class="sxs-lookup"><span data-stu-id="c56e0-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="c56e0-299">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="c56e0-299">Test the app</span></span>

* <span data-ttu-id="c56e0-300">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c56e0-300">Run the app.</span></span>
* <span data-ttu-id="c56e0-301">Щелкните ссылку **Students** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="c56e0-302">Протестируйте ссылки Edit, Details и Delete.</span><span class="sxs-lookup"><span data-stu-id="c56e0-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="c56e0-303">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-303">Seed the database</span></span>

<span data-ttu-id="c56e0-304">Метод `EnsureCreated` создает пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="c56e0-305">В этом разделе добавляется код, который заполняет базу данных тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="c56e0-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="c56e0-306">Создайте файл *Data/DbInitializer.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="c56e0-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="c56e0-307">Этот код проверяет наличие учащихся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="c56e0-308">Если учащихся нет, в базу данных добавляются тестовые данные.</span><span class="sxs-lookup"><span data-stu-id="c56e0-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="c56e0-309">Для повышения производительности тестовые данные создаются массивами, а не коллекциями `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="c56e0-310">В файле *Program.cs* замените вызов `EnsureCreated` вызовом `DbInitializer.Initialize`:</span><span class="sxs-lookup"><span data-stu-id="c56e0-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c56e0-312">Завершите работу приложения, если оно запущено, и выполните следующую команду в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="c56e0-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-313">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c56e0-314">Остановите приложение, если оно выполняется, и удалите файл *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="c56e0-315">Перезапустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c56e0-315">Restart the app.</span></span>

* <span data-ttu-id="c56e0-316">Выберите страницу учащихся, чтобы увидеть заполненные данные.</span><span class="sxs-lookup"><span data-stu-id="c56e0-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="c56e0-317">Просмотр базы данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c56e0-319">Откройте **обозреватель объектов SQL Server** (SSOX) из меню **Вид** в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c56e0-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="c56e0-320">В SSOX щелкните **(localdb)\MSSQLLocalDB > Базы данных > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="c56e0-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="c56e0-321">Имя базы данных создается на основе имени контекста, которое вы указали ранее, а также включает дефис и GUID.</span><span class="sxs-lookup"><span data-stu-id="c56e0-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="c56e0-322">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="c56e0-323">Щелкните правой кнопкой мыши таблицу **Student** (Учащийся) и выберите пункт **Просмотр данных**, чтобы просмотреть созданные столбцы и строки, вставленные в таблицу.</span><span class="sxs-lookup"><span data-stu-id="c56e0-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="c56e0-324">Щелкните правой кнопкой мыши таблицу **Student** (Учащийся) и выберите пункт **Просмотреть код**, чтобы увидеть, как модель `Student` соотносится со схемой таблицы `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-325">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c56e0-326">Используйте средство SQLite для просмотра схемы базы данных и заполненных данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="c56e0-327">Файл базы данных называется *CU.db* и находится в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="c56e0-328">Асинхронный код</span><span class="sxs-lookup"><span data-stu-id="c56e0-328">Asynchronous code</span></span>

<span data-ttu-id="c56e0-329">Асинхронное программирование по умолчанию применяется в ASP.NET Core и EF Core.</span><span class="sxs-lookup"><span data-stu-id="c56e0-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="c56e0-330">Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки.</span><span class="sxs-lookup"><span data-stu-id="c56e0-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="c56e0-331">В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки.</span><span class="sxs-lookup"><span data-stu-id="c56e0-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="c56e0-332">В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="c56e0-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="c56e0-333">В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="c56e0-334">Таким образом, асинхронный код позволяет более эффективно использовать ресурсы сервера, который может обрабатывать больше трафика без задержек.</span><span class="sxs-lookup"><span data-stu-id="c56e0-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="c56e0-335">Во время выполнения асинхронный код использует немного больше служебных ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="c56e0-336">Однако при низком объеме трафика этим можно пренебречь. Тем не менее в случае большого объема трафика это дает существенный выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="c56e0-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="c56e0-337">В следующем коде для асинхронного выполнения используются ключевое слово [async](/dotnet/csharp/language-reference/keywords/async), возвращаемое значение `Task<T>`, ключевое слово `await` и метод `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="c56e0-338">Ключевое слово `async` указывает компилятору:</span><span class="sxs-lookup"><span data-stu-id="c56e0-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="c56e0-339">создавать обратные вызовы для частей тела метода;</span><span class="sxs-lookup"><span data-stu-id="c56e0-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="c56e0-340">создавать возвращаемый объект [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="c56e0-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="c56e0-341">Тип возвращаемого значения `Task<T>` представляет текущую операцию.</span><span class="sxs-lookup"><span data-stu-id="c56e0-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="c56e0-342">Ключевое слово `await` предписывает компилятору разделить метод на две части.</span><span class="sxs-lookup"><span data-stu-id="c56e0-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="c56e0-343">Первая часть завершается операцией, которая запускается в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="c56e0-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="c56e0-344">Вторая часть помещается в метод обратного вызова, который вызывается при завершении операции.</span><span class="sxs-lookup"><span data-stu-id="c56e0-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="c56e0-345">`ToListAsync` является асинхронной версией метода расширения `ToList`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="c56e0-346">При написании асинхронного кода, который использует EF Core, нужно учитывать некоторые моменты:</span><span class="sxs-lookup"><span data-stu-id="c56e0-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="c56e0-347">Асинхронно выполняются только те инструкции, в результате которых в базу данных отправляются запросы или команды.</span><span class="sxs-lookup"><span data-stu-id="c56e0-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="c56e0-348">К ним относятся `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` и `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="c56e0-349">В их число не входят операторы, которые просто изменяют `IQueryable`, такие как `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="c56e0-350">Контекст EF Core не является потокобезопасным, поэтому не следует пытаться выполнять несколько операций параллельно.</span><span class="sxs-lookup"><span data-stu-id="c56e0-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="c56e0-351">Чтобы воспользоваться преимуществами повышенной производительности асинхронного кода, убедитесь в том, что пакеты библиотек (например, для разбиения на страницы) используют асинхронную модель, когда вызывают методы EF Core, которые направляют запросы в базу данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="c56e0-352">Дополнительные сведения об асинхронном программировании см. в разделах [Обзор асинхронной модели](/dotnet/standard/async) и [Асинхронное программирование с использованием ключевых слов Async и Await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="c56e0-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c56e0-353">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="c56e0-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c56e0-354">Следующий учебник</span><span class="sxs-lookup"><span data-stu-id="c56e0-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c56e0-355">На примере учебного веб-приложения "Университет Contoso" демонстрируется процесс создания веб-приложений Razor Pages ASP.NET Core с помощью Entity Framework (EF) Core.</span><span class="sxs-lookup"><span data-stu-id="c56e0-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="c56e0-356">В этом примере приложения реализуется веб-сайт вымышленного университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="c56e0-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="c56e0-357">На нем предусмотрены различные функции, в том числе прием учащихся, создание курсов и назначение преподавателей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="c56e0-358">Это первое руководство из серии, в котором описывается создание примера приложения для университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="c56e0-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="c56e0-359">Скачайте или ознакомьтесь с готовым приложением.</span><span class="sxs-lookup"><span data-stu-id="c56e0-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="c56e0-360">[Указания по скачиванию](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="c56e0-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c56e0-361">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="c56e0-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-363">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="c56e0-364">Знакомство с [Razor Pages](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="c56e0-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="c56e0-365">Новые программисты должны изучить статью [Начало работы с Razor Pages](xref:tutorials/razor-pages/razor-pages-start), прежде чем приступать к этой серии.</span><span class="sxs-lookup"><span data-stu-id="c56e0-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="c56e0-366">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="c56e0-366">Troubleshooting</span></span>

<span data-ttu-id="c56e0-367">Если вы столкнулись с проблемами, для их решения можно попробовать сравнить свой код с кодом [готового проекта](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="c56e0-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="c56e0-368">Чтобы получить помощь, вы можете опубликовать вопрос на веб-сайте [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) в разделе, посвященном [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) или [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="c56e0-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="c56e0-369">Веб-приложение университета Contoso</span><span class="sxs-lookup"><span data-stu-id="c56e0-369">The Contoso University web app</span></span>

<span data-ttu-id="c56e0-370">Приложение, создаваемое в этих руководствах, является простым веб-сайтом университета.</span><span class="sxs-lookup"><span data-stu-id="c56e0-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="c56e0-371">Пользователи приложения могут просматривать и обновлять сведения об учащихся, курсах и преподавателях.</span><span class="sxs-lookup"><span data-stu-id="c56e0-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="c56e0-372">Здесь приведено несколько экранов, создаваемых в руководстве.</span><span class="sxs-lookup"><span data-stu-id="c56e0-372">Here are a few of the screens created in the tutorial.</span></span>

![Страница указателя учащихся](intro/_static/students-index.png)

![Страница редактирования учащихся](intro/_static/student-edit.png)

<span data-ttu-id="c56e0-375">Стиль пользовательского интерфейса для этого сайта близок к обеспечиваемому встроенными шаблонами.</span><span class="sxs-lookup"><span data-stu-id="c56e0-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="c56e0-376">Это руководство посвящено EF Core с Razor Pages, а не пользовательскому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="c56e0-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="c56e0-377">Создание веб-приложения Razor Pages ContosoUniversity</span><span class="sxs-lookup"><span data-stu-id="c56e0-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c56e0-379">В меню **Файл** Visual Studio откройте **Создать** > **Проект**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="c56e0-380">Создайте новое веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c56e0-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="c56e0-381">Назовите проект **ContosoUniversity**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="c56e0-382">Очень важно, чтобы проект имел имя *ContosoUniversity*, чтобы совпадали пространства имен при копировании и вставке кода.</span><span class="sxs-lookup"><span data-stu-id="c56e0-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="c56e0-383">Выберите в раскрывающемся списке **ASP.NET Core 2.1**, а затем **Веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="c56e0-384">Изображения для приведенных выше шагов см. в разделе [Создание веб-приложения Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span><span class="sxs-lookup"><span data-stu-id="c56e0-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="c56e0-385">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c56e0-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-386">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="c56e0-387">Настройка стиля сайта</span><span class="sxs-lookup"><span data-stu-id="c56e0-387">Set up the site style</span></span>

<span data-ttu-id="c56e0-388">Выполните небольшую настройку меню, макета и домашней страницы сайта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="c56e0-389">Внесите следующие изменения в файл *Pages/Shared/_Layout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c56e0-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="c56e0-390">Замените все вхождения "ContosoUniversity" на "Contoso University".</span><span class="sxs-lookup"><span data-stu-id="c56e0-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="c56e0-391">Таких элементов будет три.</span><span class="sxs-lookup"><span data-stu-id="c56e0-391">There are three occurrences.</span></span>

* <span data-ttu-id="c56e0-392">Добавьте пункты меню **Students** (Учащиеся), **Courses** (Курсы), **Instructors** (Преподаватели) и **Departments** (Кафедры). Удалите пункт меню **Contact** (Контакты).</span><span class="sxs-lookup"><span data-stu-id="c56e0-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="c56e0-393">Изменения выделены.</span><span class="sxs-lookup"><span data-stu-id="c56e0-393">The changes are highlighted.</span></span> <span data-ttu-id="c56e0-394">(Вся разметка *не* отображается.)</span><span class="sxs-lookup"><span data-stu-id="c56e0-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="c56e0-395">Замените содержимое файла *Pages/Index.cshtml* следующим кодом, который заменяет текст о ASP.NET и MVC описанием этого приложения:</span><span class="sxs-lookup"><span data-stu-id="c56e0-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="c56e0-396">Создание модели данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-396">Create the data model</span></span>

<span data-ttu-id="c56e0-397">Создайте классы сущностей для приложения университета Contoso.</span><span class="sxs-lookup"><span data-stu-id="c56e0-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="c56e0-398">Начните со следующих трех сущностей:</span><span class="sxs-lookup"><span data-stu-id="c56e0-398">Start with the following three entities:</span></span>

![Схема модели данных "курс-регистрация-учащийся"](intro/_static/data-model-diagram.png)

<span data-ttu-id="c56e0-400">Между сущностями `Student` и `Enrollment` действует связь один ко многим.</span><span class="sxs-lookup"><span data-stu-id="c56e0-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="c56e0-401">Между сущностями `Course` и `Enrollment` действует связь один ко многим.</span><span class="sxs-lookup"><span data-stu-id="c56e0-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="c56e0-402">Студент может записаться на любое число курсов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="c56e0-403">На курс может быть зачислено любое количество учащихся.</span><span class="sxs-lookup"><span data-stu-id="c56e0-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="c56e0-404">В следующих разделах создается класс для каждой из этих сущностей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="c56e0-405">Сущность Student</span><span class="sxs-lookup"><span data-stu-id="c56e0-405">The Student entity</span></span>

![Схема сущности Student](intro/_static/student-entity.png)

<span data-ttu-id="c56e0-407">Создайте папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-407">Create a *Models* folder.</span></span> <span data-ttu-id="c56e0-408">В папке *Models* создайте файл класса с именем *Student.cs*, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="c56e0-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="c56e0-409">Свойство `ID` используется в качестве столбца первичного ключа в таблице базы данных, соответствующей этому классу.</span><span class="sxs-lookup"><span data-stu-id="c56e0-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="c56e0-410">По умолчанию платформа EF Core интерпретирует в качестве первичного ключа свойство `ID` или `classnameID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="c56e0-411">В `classnameID` `classname` — это имя класса.</span><span class="sxs-lookup"><span data-stu-id="c56e0-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="c56e0-412">Альтернативным автоматически распознаваемым первичным ключом является `StudentID` в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="c56e0-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="c56e0-413">Свойство `Enrollments` является [свойством навигации](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="c56e0-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="c56e0-414">Свойства навигации ссылаются на другие сущности, связанные с этой сущностью.</span><span class="sxs-lookup"><span data-stu-id="c56e0-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="c56e0-415">В этом случае свойство `Enrollments` сущности `Student entity` содержит все сущности `Enrollment`, которые связаны с этой сущностью `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="c56e0-416">Например, если строка "Student" (Учащийся) в базе данных имеет две связанные строки "Enrollment" (зачисление), свойство навигации `Enrollments` содержит две этих сущности `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="c56e0-417">Связанная строка `Enrollment` содержит значение первичного ключа для этого учащегося в столбце `StudentID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="c56e0-418">Предположим, например, что учащийся с идентификатором 1 имеет две строки в таблице `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="c56e0-419">Таблица `Enrollment` содержит две строки с `StudentID` = 1.</span><span class="sxs-lookup"><span data-stu-id="c56e0-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="c56e0-420">`StudentID` является внешним ключом в таблице `Enrollment`, указывающим учащегося в таблице `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="c56e0-421">Если свойство навигации может содержать несколько сущностей, оно должно иметь тип списка, такой как `ICollection<T>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="c56e0-422">Можно указать `ICollection<T>` либо тип, такой как `List<T>` или `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="c56e0-423">Если используется `ICollection<T>`, платформа EF Core по умолчанию создает коллекцию `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="c56e0-424">Свойства навигации, содержащие несколько сущностей, поступают по связям многие ко многим и один ко многим.</span><span class="sxs-lookup"><span data-stu-id="c56e0-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="c56e0-425">Сущность Enrollment</span><span class="sxs-lookup"><span data-stu-id="c56e0-425">The Enrollment entity</span></span>

![Схема сущности Enrollment](intro/_static/enrollment-entity.png)

<span data-ttu-id="c56e0-427">В папке *Models* создайте файл *Enrollment.cs*, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="c56e0-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="c56e0-428">Свойство `EnrollmentID` является первичным ключом.</span><span class="sxs-lookup"><span data-stu-id="c56e0-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="c56e0-429">Эта сущность использует шаблон `classnameID` вместо `ID`, как сущность `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="c56e0-430">Обычно разработчики выбирают один шаблон и используют его в рамках всей модели данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="c56e0-431">В одном из следующих руководств показано, за счет чего использование идентификатора без имени класса позволяет упростить реализацию наследования в модели данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="c56e0-432">Свойство `Grade` имеет тип `enum`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="c56e0-433">Знак вопроса после объявления типа `Grade` указывает, что свойство `Grade` допускает значение null.</span><span class="sxs-lookup"><span data-stu-id="c56e0-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="c56e0-434">Оценка со значением null отличается от нулевой оценки тем, что при таком значении оценка еще не известна или не назначена.</span><span class="sxs-lookup"><span data-stu-id="c56e0-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="c56e0-435">Свойство `StudentID` представляет собой внешний ключ. Ему соответствует свойство навигации `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="c56e0-436">Сущность `Enrollment` связана с одной сущностью `Student`, поэтому свойство содержит отдельную сущность `Student`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="c56e0-437">Сущность `Student` отличается от свойства навигации `Student.Enrollments`, которое содержит несколько сущностей `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="c56e0-438">Свойство `CourseID` представляет собой внешний ключ. Ему соответствует свойство навигации `Course`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="c56e0-439">Сущность `Enrollment` связана с одной сущностью `Course`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="c56e0-440">EF Core воспринимает свойство как внешний ключ, если он имеет имя `<navigation property name><primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="c56e0-441">Например, `StudentID` для свойства навигации `Student`, так как сущность `Student` имеет первичный ключ `ID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="c56e0-442">Свойства внешнего ключа также могут называться `<primary key property name>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="c56e0-443">Например, `CourseID`, так как сущность `Course` имеет первичный ключ `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="c56e0-444">Сущность Course</span><span class="sxs-lookup"><span data-stu-id="c56e0-444">The Course entity</span></span>

![Схема сущности Course](intro/_static/course-entity.png)

<span data-ttu-id="c56e0-446">В папке *Models* создайте файл *Course.cs*, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="c56e0-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="c56e0-447">Свойство `Enrollments` является свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="c56e0-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="c56e0-448">Сущность `Course` может быть связана с любым числом сущностей `Enrollment`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="c56e0-449">Атрибут `DatabaseGenerated` позволяет приложению указать первичный ключ, а не использовать созданный базой данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="c56e0-450">Формирование шаблона для модели Student</span><span class="sxs-lookup"><span data-stu-id="c56e0-450">Scaffold the student model</span></span>

<span data-ttu-id="c56e0-451">В этом разделе формируется шаблон для модели Student.</span><span class="sxs-lookup"><span data-stu-id="c56e0-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="c56e0-452">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели Student.</span><span class="sxs-lookup"><span data-stu-id="c56e0-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="c56e0-453">Выполните построение проекта.</span><span class="sxs-lookup"><span data-stu-id="c56e0-453">Build the project.</span></span>
* <span data-ttu-id="c56e0-454">Создайте папку *Pages/Students*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c56e0-456">В **обозревателе решений** щелкните правой кнопкой мыши папку *Pages/Students* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c56e0-457">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **ДОБАВИТЬ**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="c56e0-458">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="c56e0-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="c56e0-459">В раскрывающемся списке **Класс модели** выберите **Student (ContosoUniversity.Models)** .</span><span class="sxs-lookup"><span data-stu-id="c56e0-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="c56e0-460">В строке **Класс контекста данных** щелкните значок плюса ( **+** ) и измените автоматически присвоенное имя на **ContosoUniversity.Models.SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="c56e0-461">В раскрывающемся списке **Класс контекста данных** выберите **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="c56e0-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="c56e0-462">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-462">Select **Add**.</span></span>

![Диалоговое окно CRUD](intro/_static/s1.png)

<span data-ttu-id="c56e0-464">Если на предыдущем шаге у вас возникли проблемы, см. раздел [Создание модели фильма](xref:tutorials/razor-pages/model#scaffold-the-movie-model).</span><span class="sxs-lookup"><span data-stu-id="c56e0-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-465">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="c56e0-466">Выполните следующую команду, чтобы сформировать шаблон для модели Student.</span><span class="sxs-lookup"><span data-stu-id="c56e0-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="c56e0-467">В процессе формирования шаблонов были созданы и изменены следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="c56e0-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="c56e0-468">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="c56e0-468">Files created</span></span>

* <span data-ttu-id="c56e0-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span><span class="sxs-lookup"><span data-stu-id="c56e0-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="c56e0-470">*Data/SchoolContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="c56e0-471">Обновления файла</span><span class="sxs-lookup"><span data-stu-id="c56e0-471">File updates</span></span>

* <span data-ttu-id="c56e0-472">*Startup.cs*: изменения в этом файле подробно описываются в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="c56e0-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="c56e0-473">*appsettings.json*: добавляется строка подключения, используемая для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="c56e0-474">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="c56e0-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="c56e0-475">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c56e0-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c56e0-476">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="c56e0-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c56e0-477">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="c56e0-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c56e0-478">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="c56e0-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c56e0-479">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="c56e0-480">Проверьте метод `ConfigureServices` в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="c56e0-481">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="c56e0-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="c56e0-482">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="c56e0-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="c56e0-483">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="c56e0-484">Обновление метода Main</span><span class="sxs-lookup"><span data-stu-id="c56e0-484">Update main</span></span>

<span data-ttu-id="c56e0-485">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="c56e0-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="c56e0-486">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="c56e0-487">Вызовите [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span><span class="sxs-lookup"><span data-stu-id="c56e0-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="c56e0-488">Высвободите контекст после завершения работы метода `EnsureCreated`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="c56e0-489">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="c56e0-490">`EnsureCreated` позволяет проверить существование базы данных для контекста.</span><span class="sxs-lookup"><span data-stu-id="c56e0-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="c56e0-491">Если контекст существует, никаких действий не предпринимается.</span><span class="sxs-lookup"><span data-stu-id="c56e0-491">If it exists, no action is taken.</span></span> <span data-ttu-id="c56e0-492">Если контекст не существует, создаются база данных и вся ее схема.</span><span class="sxs-lookup"><span data-stu-id="c56e0-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="c56e0-493">`EnsureCreated` не использует миграции для создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="c56e0-494">Созданную с помощью `EnsureCreated` базу данных впоследствии нельзя обновить, используя миграции.</span><span class="sxs-lookup"><span data-stu-id="c56e0-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="c56e0-495">`EnsureCreated` вызывается при запуске приложения, что обеспечивает следующий рабочий процесс:</span><span class="sxs-lookup"><span data-stu-id="c56e0-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="c56e0-496">Удалите базу данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-496">Delete the DB.</span></span>
* <span data-ttu-id="c56e0-497">Измените схему базы данных (например, добавьте поле `EmailAddress`).</span><span class="sxs-lookup"><span data-stu-id="c56e0-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="c56e0-498">Запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="c56e0-498">Run the app.</span></span>
* <span data-ttu-id="c56e0-499">`EnsureCreated` создает базу данных со столбцом `EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="c56e0-500">`EnsureCreated` удобно использовать на ранних стадиях разработки, когда схема часто меняется.</span><span class="sxs-lookup"><span data-stu-id="c56e0-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="c56e0-501">Далее в этом учебнике база данных удаляется и используются миграции.</span><span class="sxs-lookup"><span data-stu-id="c56e0-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="c56e0-502">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="c56e0-502">Test the app</span></span>

<span data-ttu-id="c56e0-503">Запустите приложение и примите политику использования файлов cookie.</span><span class="sxs-lookup"><span data-stu-id="c56e0-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="c56e0-504">Это приложение не хранит персональные данные.</span><span class="sxs-lookup"><span data-stu-id="c56e0-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="c56e0-505">Сведения о политике использования файлов cookie можно прочитать в разделе [Поддержка общего регламента по защите данных ЕС (GDPR)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="c56e0-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="c56e0-506">Щелкните ссылку **Students** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="c56e0-507">Протестируйте ссылки Edit, Details и Delete.</span><span class="sxs-lookup"><span data-stu-id="c56e0-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="c56e0-508">Проверка контекста базы данных SchoolContext</span><span class="sxs-lookup"><span data-stu-id="c56e0-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="c56e0-509">Контекст базы данных — это класс main, который координирует функциональные возможности EF Core для заданной модели данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="c56e0-510">Контекст данных получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="c56e0-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="c56e0-511">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="c56e0-512">В этом проекте соответствующий класс называется `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="c56e0-513">Обновите файл *SchoolContext.cs*, добавив в него следующий код:</span><span class="sxs-lookup"><span data-stu-id="c56e0-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="c56e0-514">Выделенный код создает свойство [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для каждого набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="c56e0-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="c56e0-515">В терминологии EF Core:</span><span class="sxs-lookup"><span data-stu-id="c56e0-515">In EF Core terminology:</span></span>

* <span data-ttu-id="c56e0-516">Набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="c56e0-517">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="c56e0-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c56e0-518">`DbSet<Enrollment>` и `DbSet<Course>` можно опустить.</span><span class="sxs-lookup"><span data-stu-id="c56e0-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="c56e0-519">Платформа EF Core включает эти операторы неявно, так как сущность `Student` ссылается на сущность `Enrollment`, а `Enrollment` ссылается на сущность `Course`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="c56e0-520">В рамках данного руководства оставьте `DbSet<Enrollment>` и `DbSet<Course>` в `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="c56e0-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="c56e0-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="c56e0-522">Строка подключения указывает базу данных [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="c56e0-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="c56e0-523">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки приложений и не ориентированная на использование в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="c56e0-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="c56e0-524">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="c56e0-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="c56e0-525">По умолчанию LocalDB создает файлы базы данных *MDF* в каталоге `C:/Users/<user>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="c56e0-526">Добавление кода для инициализации базы данных с использованием тестовых данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="c56e0-527">EF Core создает пустую базу данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="c56e0-528">В этом разделе создается метод `Initialize`, предназначенный для заполнения тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="c56e0-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="c56e0-529">В папке *Data* создайте файл класса с именем *DbInitializer.cs* и добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="c56e0-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="c56e0-530">Примечание. Предыдущий код использует `Models` для пространства имен (`namespace ContosoUniversity.Models`) вместо `Data`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="c56e0-531">`Models` согласуется с кодом, созданным средством формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="c56e0-532">См. дополнительные сведения о [проблеме с формированием шаблонов на GitHub ](https://github.com/aspnet/Scaffolding/issues/822).</span><span class="sxs-lookup"><span data-stu-id="c56e0-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="c56e0-533">Код проверяет наличие учащихся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="c56e0-534">Если учащихся в базе данных нет, она заполняется тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="c56e0-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="c56e0-535">Для повышения производительности тестовые данные загружаются массивами, а не коллекциями `List<T>`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="c56e0-536">Метод `EnsureCreated` автоматически создает базу данных для контекста базы данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="c56e0-537">Если такая база данных существует, `EnsureCreated` возвращает управление без изменения базы данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="c56e0-538">В файле *Program.cs* измените метод `Main`, чтобы реализовать вызов `Initialize`:</span><span class="sxs-lookup"><span data-stu-id="c56e0-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c56e0-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c56e0-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c56e0-540">Завершите работу приложения, если оно запущено, и выполните следующую команду в **консоли диспетчера пакетов** (PMC):</span><span class="sxs-lookup"><span data-stu-id="c56e0-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c56e0-541">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c56e0-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="c56e0-542">Остановите приложение, если оно выполняется, и удалите файл *CU.db*.</span><span class="sxs-lookup"><span data-stu-id="c56e0-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="c56e0-543">Просмотр базы данных</span><span class="sxs-lookup"><span data-stu-id="c56e0-543">View the DB</span></span>

<span data-ttu-id="c56e0-544">Имя базы данных создается на основе имени контекста, которое вы указали ранее, а также включает дефис и GUID.</span><span class="sxs-lookup"><span data-stu-id="c56e0-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="c56e0-545">Таким образом, имя базы данных будет SchoolContext-{GUID}.</span><span class="sxs-lookup"><span data-stu-id="c56e0-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="c56e0-546">GUID будет отличаться для каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="c56e0-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="c56e0-547">Откройте **обозреватель объектов SQL Server** (SSOX) из меню **Вид** в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c56e0-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="c56e0-548">В SSOX щелкните **(localdb)\MSSQLLocalDB > Базы данных > SchoolContext-{GUID}** .</span><span class="sxs-lookup"><span data-stu-id="c56e0-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="c56e0-549">Разверните узел **Таблицы**.</span><span class="sxs-lookup"><span data-stu-id="c56e0-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="c56e0-550">Щелкните правой кнопкой мыши таблицу **Student** (Учащийся) и выберите пункт **Просмотр данных**, чтобы просмотреть созданные столбцы и строки, вставленные в таблицу.</span><span class="sxs-lookup"><span data-stu-id="c56e0-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="c56e0-551">Асинхронный код</span><span class="sxs-lookup"><span data-stu-id="c56e0-551">Asynchronous code</span></span>

<span data-ttu-id="c56e0-552">Асинхронное программирование по умолчанию применяется в ASP.NET Core и EF Core.</span><span class="sxs-lookup"><span data-stu-id="c56e0-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="c56e0-553">Веб-сервер имеет ограниченное число потоков, поэтому при высокой загрузке могут использоваться все доступные потоки.</span><span class="sxs-lookup"><span data-stu-id="c56e0-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="c56e0-554">В таких случаях сервер не может обрабатывать новые запросы до тех пор, пока не будут высвобождены потоки.</span><span class="sxs-lookup"><span data-stu-id="c56e0-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="c56e0-555">В синхронном коде многие потоки могут быть заняты, не выполняя при этом какие-либо операции и ожидая завершения ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="c56e0-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="c56e0-556">В асинхронном коде в то время, когда процесс ожидает завершения ввода-вывода, его поток высвобождается и может использоваться сервером для обработки других запросов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="c56e0-557">Таким образом, асинхронный код позволяет более эффективно использовать ресурсы сервера, который может обрабатывать больше трафика без задержек.</span><span class="sxs-lookup"><span data-stu-id="c56e0-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="c56e0-558">Во время выполнения асинхронный код использует немного больше служебных ресурсов.</span><span class="sxs-lookup"><span data-stu-id="c56e0-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="c56e0-559">Однако при низком объеме трафика этим можно пренебречь. Тем не менее в случае большого объема трафика это дает существенный выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="c56e0-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="c56e0-560">В следующем коде для асинхронного выполнения используются ключевое слово [async](/dotnet/csharp/language-reference/keywords/async), возвращаемое значение `Task<T>`, ключевое слово `await` и метод `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="c56e0-561">Ключевое слово `async` указывает компилятору:</span><span class="sxs-lookup"><span data-stu-id="c56e0-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="c56e0-562">создавать обратные вызовы для частей тела метода;</span><span class="sxs-lookup"><span data-stu-id="c56e0-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="c56e0-563">автоматически создавать возвращаемый объект [Task](/dotnet/api/system.threading.tasks.task).</span><span class="sxs-lookup"><span data-stu-id="c56e0-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="c56e0-564">Дополнительные сведения см. в разделе [Тип возвращаемых значений задач](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span><span class="sxs-lookup"><span data-stu-id="c56e0-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="c56e0-565">Неявный тип возвращаемого значения `Task` представляет текущую операцию.</span><span class="sxs-lookup"><span data-stu-id="c56e0-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="c56e0-566">Ключевое слово `await` предписывает компилятору разделить метод на две части.</span><span class="sxs-lookup"><span data-stu-id="c56e0-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="c56e0-567">Первая часть завершается операцией, которая запускается в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="c56e0-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="c56e0-568">Вторая часть помещается в метод обратного вызова, который вызывается при завершении операции.</span><span class="sxs-lookup"><span data-stu-id="c56e0-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="c56e0-569">`ToListAsync` является асинхронной версией метода расширения `ToList`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="c56e0-570">При написании асинхронного кода, который использует EF Core, нужно учитывать некоторые моменты:</span><span class="sxs-lookup"><span data-stu-id="c56e0-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="c56e0-571">Асинхронно выполняются только те операторы, в результате которых в базу данных отправляются запросы или команды.</span><span class="sxs-lookup"><span data-stu-id="c56e0-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="c56e0-572">К ним относятся `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` и `SaveChangesAsync`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="c56e0-573">В их число не входят операторы, которые просто изменяют `IQueryable`, такие как `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span><span class="sxs-lookup"><span data-stu-id="c56e0-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="c56e0-574">Контекст EF Core не является потокобезопасным, поэтому не следует пытаться выполнять несколько операций параллельно.</span><span class="sxs-lookup"><span data-stu-id="c56e0-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="c56e0-575">Чтобы воспользоваться преимуществами повышенной производительности асинхронного кода, убедитесь, что пакеты библиотек (например, для разбиения на страницы) используют асинхронную модель, когда вызывают методы EF Core, которые направляют запросы в базу данных.</span><span class="sxs-lookup"><span data-stu-id="c56e0-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="c56e0-576">Дополнительные сведения об асинхронном программировании см. в разделах [Обзор асинхронной модели](/dotnet/standard/async) и [Асинхронное программирование с использованием ключевых слов Async и Await](/dotnet/csharp/programming-guide/concepts/async/).</span><span class="sxs-lookup"><span data-stu-id="c56e0-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="c56e0-577">Следующее руководство посвящено базовым операциям CRUD (создание, чтение, обновление, удаление).</span><span class="sxs-lookup"><span data-stu-id="c56e0-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="c56e0-578">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c56e0-578">Additional resources</span></span>

* [<span data-ttu-id="c56e0-579">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="c56e0-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="c56e0-580">Вперед</span><span class="sxs-lookup"><span data-stu-id="c56e0-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
