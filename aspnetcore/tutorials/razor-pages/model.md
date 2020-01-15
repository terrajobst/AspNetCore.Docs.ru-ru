---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 0934c94236b507f2f57200ded4344a71c483d356
ms.sourcegitcommit: 5fe17e54f7e4267a2fdecc6f9aa1d41166cecc34
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737862"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="9606b-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9606b-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="9606b-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="9606b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="9606b-105">В этом разделе добавляются классы для управления фильмами в кроссплатформенной [базе данных SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="9606b-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="9606b-106">Приложения, созданные на основе шаблона ASP.NET Core, используют базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="9606b-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="9606b-107">Эти классы этой модели приложений используются в [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="9606b-108">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="9606b-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="9606b-109">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="9606b-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="9606b-110">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9606b-111">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="9606b-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9606b-113">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9606b-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9606b-114">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-114">Name the folder *Models*.</span></span>

<span data-ttu-id="9606b-115">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-115">Right click the *Models* folder.</span></span> <span data-ttu-id="9606b-116">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="9606b-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="9606b-117">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="9606b-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9606b-119">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="9606b-120">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="9606b-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-121">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9606b-122">На панели решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить**>**Новая папка**. Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="9606b-123">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить**>**Новый файл...**</span><span class="sxs-lookup"><span data-stu-id="9606b-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="9606b-124">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9606b-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="9606b-125">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="9606b-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="9606b-126">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="9606b-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="9606b-127">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9606b-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="9606b-128">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="9606b-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="9606b-129">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="9606b-129">Scaffold the movie model</span></span>

<span data-ttu-id="9606b-130">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="9606b-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="9606b-131">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="9606b-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9606b-133">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9606b-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9606b-134">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9606b-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9606b-135">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9606b-135">Name the folder *Movies*</span></span>

<span data-ttu-id="9606b-136">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить**>**New Scaffolded Item** (Создать шаблонный элемент).</span><span class="sxs-lookup"><span data-stu-id="9606b-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="9606b-138">В диалоговом окне **Добавление шаблона** выберите **Razor Pages на основе Entity Framework (CRUD)** >**Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="9606b-140">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9606b-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9606b-141">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="9606b-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9606b-142">В строке **Класс контекста данных** щелкните значок плюса **+** и измените сгенерированное имя RazorPagesMovie.**Models**.RazorPagesMovieContext на RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="9606b-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="9606b-143">[Это изменение](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) не требуется.</span><span class="sxs-lookup"><span data-stu-id="9606b-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="9606b-144">Оно создает класс контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="9606b-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="9606b-145">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-145">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/3/arp.png)

<span data-ttu-id="9606b-147">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="9606b-149">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="9606b-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="9606b-150">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="9606b-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="9606b-151">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9606b-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="9606b-152">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9606b-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-153">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9606b-154">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9606b-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9606b-155">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9606b-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9606b-156">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9606b-156">Name the folder *Movies*</span></span>

<span data-ttu-id="9606b-157">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **New Scaffolding...** (Создать шаблон...).</span><span class="sxs-lookup"><span data-stu-id="9606b-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/scaMac.png)

<span data-ttu-id="9606b-159">В диалоговом окне **New Scaffolding** (Новый шаблон) выберите **Razor Pages на основе Entity Framework (CRUD)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9606b-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="9606b-161">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9606b-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9606b-162">В раскрывающемся списке **Класс модели** выберите или введите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="9606b-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9606b-163">В строке **Data context class** (Класс контекста данных) введите имя нового класса RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="9606b-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="9606b-164">[Это изменение](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) не требуется.</span><span class="sxs-lookup"><span data-stu-id="9606b-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="9606b-165">Оно создает класс контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="9606b-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="9606b-166">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-166">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arpMac.png)

<span data-ttu-id="9606b-168">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="9606b-169">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="9606b-169">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9606b-171">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9606b-171">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="9606b-172">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9606b-172">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="9606b-173">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9606b-173">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="9606b-174">Обновленные возможности</span><span class="sxs-lookup"><span data-stu-id="9606b-174">Updated</span></span>

* <span data-ttu-id="9606b-175">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="9606b-175">*Startup.cs*</span></span>

<span data-ttu-id="9606b-176">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="9606b-176">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-177">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9606b-178">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9606b-178">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="9606b-179">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9606b-179">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="9606b-180">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9606b-180">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="9606b-181">Обновленные возможности</span><span class="sxs-lookup"><span data-stu-id="9606b-181">Updated</span></span>

* <span data-ttu-id="9606b-182">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="9606b-182">*Startup.cs*</span></span>

<span data-ttu-id="9606b-183">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="9606b-183">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-184">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-184">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9606b-185">В процессе формирования шаблонов создаются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9606b-185">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="9606b-186">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9606b-186">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="9606b-187">В следующем разделе приводится описание созданных файлов.</span><span class="sxs-lookup"><span data-stu-id="9606b-187">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="9606b-188">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="9606b-188">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-189">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9606b-190">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="9606b-190">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="9606b-191">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9606b-191">Add an initial migration.</span></span>
* <span data-ttu-id="9606b-192">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9606b-192">Update the database with the initial migration.</span></span>

<span data-ttu-id="9606b-193">В меню **Сервис**  последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9606b-193">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9606b-195">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9606b-195">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-197">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-197">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="9606b-198">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="9606b-198">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="9606b-199">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9606b-199">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="9606b-200">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="9606b-200">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="9606b-201">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="9606b-201">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="9606b-202">Команда миграции формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-202">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="9606b-203">Схема создается на основе модели, указанной в `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9606b-203">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="9606b-204">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="9606b-204">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="9606b-205">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="9606b-205">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="9606b-206">Команда `update` выполняет метод `Up` в миграциях, которые не были применены.</span><span class="sxs-lookup"><span data-stu-id="9606b-206">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="9606b-207">В этом случае `update` запускает метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-207">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-208">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="9606b-209">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="9606b-209">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="9606b-210">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9606b-210">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9606b-211">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9606b-211">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="9606b-212">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="9606b-212">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="9606b-213">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="9606b-213">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="9606b-214">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9606b-214">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="9606b-215">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9606b-215">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9606b-216">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="9606b-216">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="9606b-217">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9606b-217">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="9606b-218">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="9606b-218">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="9606b-219">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-219">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9606b-220">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9606b-220">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="9606b-221">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-221">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="9606b-222">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="9606b-222">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="9606b-223">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="9606b-223">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="9606b-224">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9606b-224">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-225">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-225">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9606b-226">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9606b-226">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-227">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9606b-228">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9606b-228">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="9606b-229">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="9606b-229">Test the app</span></span>

* <span data-ttu-id="9606b-230">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="9606b-230">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="9606b-231">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="9606b-231">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="9606b-232">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="9606b-232">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="9606b-233">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9606b-233">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="9606b-235">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="9606b-235">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="9606b-236">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="9606b-236">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="9606b-237">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="9606b-237">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="9606b-238">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-238">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="9606b-239">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9606b-239">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9606b-240">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9606b-240">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9606b-241">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9606b-241">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9606b-242">В этом разделе добавляются классы для управления фильмами в кроссплатформенной [базе данных SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="9606b-242">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="9606b-243">Приложения, созданные на основе шаблона ASP.NET Core, используют базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="9606b-243">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="9606b-244">Эти классы этой модели приложений используются в [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-244">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="9606b-245">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="9606b-245">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="9606b-246">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="9606b-246">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="9606b-247">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-247">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9606b-248">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="9606b-248">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-249">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-249">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9606b-250">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9606b-250">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9606b-251">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-251">Name the folder *Models*.</span></span>

<span data-ttu-id="9606b-252">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-252">Right click the *Models* folder.</span></span> <span data-ttu-id="9606b-253">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="9606b-253">Select **Add** > **Class**.</span></span> <span data-ttu-id="9606b-254">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="9606b-254">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-255">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-255">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9606b-256">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-256">Add a folder named *Models*.</span></span>
* <span data-ttu-id="9606b-257">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="9606b-257">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-258">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-258">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9606b-259">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9606b-259">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="9606b-260">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9606b-260">Name the folder *Models*.</span></span>
* <span data-ttu-id="9606b-261">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить**>**Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="9606b-261">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="9606b-262">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9606b-262">In the **New File** dialog:</span></span>

  * <span data-ttu-id="9606b-263">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="9606b-263">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="9606b-264">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="9606b-264">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="9606b-265">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9606b-265">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="9606b-266">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="9606b-266">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="9606b-267">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="9606b-267">Scaffold the movie model</span></span>

<span data-ttu-id="9606b-268">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="9606b-268">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="9606b-269">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="9606b-269">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-270">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-270">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9606b-271">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9606b-271">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9606b-272">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9606b-272">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9606b-273">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9606b-273">Name the folder *Movies*</span></span>

<span data-ttu-id="9606b-274">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить**>**New Scaffolded Item** (Создать шаблонный элемент).</span><span class="sxs-lookup"><span data-stu-id="9606b-274">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="9606b-276">В диалоговом окне **Добавление шаблона** выберите **Razor Pages на основе Entity Framework (CRUD)** >**Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-276">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="9606b-278">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9606b-278">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="9606b-279">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="9606b-279">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9606b-280">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="9606b-280">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="9606b-281">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-281">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<span data-ttu-id="9606b-283">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-283">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-284">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-284">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="9606b-285">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="9606b-285">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="9606b-286">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9606b-286">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="9606b-287">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9606b-287">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-288">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-288">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9606b-289">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9606b-289">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9606b-290">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9606b-290">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9606b-291">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9606b-291">Name the folder *Movies*</span></span>

<span data-ttu-id="9606b-292">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить**>**New Scaffolded Item** (Создать шаблонный элемент).</span><span class="sxs-lookup"><span data-stu-id="9606b-292">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/scaMac.png)

<span data-ttu-id="9606b-294">В диалоговом окне **Add New Scaffolding** (Добавление нового шаблона) выберите **Razor Pages на основе Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-294">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="9606b-296">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9606b-296">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9606b-297">В раскрывающемся списке **Класс модели** выберите или введите **Фильм**.</span><span class="sxs-lookup"><span data-stu-id="9606b-297">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="9606b-298">В строке **Data context class** (Класс контекста данных) введите или выберите **RazorPagesMovieContext**. Это приведет к созданию класса контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="9606b-298">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="9606b-299">В этом случае это будет **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="9606b-299">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="9606b-300">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-300">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arpMac.png)

<span data-ttu-id="9606b-302">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-302">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="9606b-303">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9606b-303">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="9606b-304">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="9606b-304">Files created</span></span>

* <span data-ttu-id="9606b-305">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9606b-305">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="9606b-306">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9606b-306">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="9606b-307">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="9606b-307">File updated</span></span>

* <span data-ttu-id="9606b-308">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="9606b-308">*Startup.cs*</span></span>

<span data-ttu-id="9606b-309">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="9606b-309">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="9606b-310">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="9606b-310">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9606b-312">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="9606b-312">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="9606b-313">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9606b-313">Add an initial migration.</span></span>
* <span data-ttu-id="9606b-314">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9606b-314">Update the database with the initial migration.</span></span>

<span data-ttu-id="9606b-315">В меню **Сервис**  последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9606b-315">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9606b-317">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9606b-317">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9606b-318">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-318">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9606b-319">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="9606b-319">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="9606b-320">Аргумент `InitialCreate` используется для присвоения имен миграции.</span><span class="sxs-lookup"><span data-stu-id="9606b-320">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="9606b-321">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="9606b-321">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="9606b-322">Для получения дополнительной информации см. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="9606b-322">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="9606b-323">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="9606b-323">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="9606b-324">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-324">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-326">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-326">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="9606b-327">В результате выполнения предыдущих команд выводится следующее предупреждение: "*Для десятичного столбца "Price" в типе сущности "Movie" не указан тип. Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию. С помощью метода "HasColumnType()" явно укажите тип столбца SQL Server, который может вместить все значения.* " Вы можете игнорировать это предупреждение, оно будет исправлено в следующем учебнике.</span><span class="sxs-lookup"><span data-stu-id="9606b-327">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9606b-328">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9606b-328">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="9606b-329">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="9606b-329">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="9606b-330">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9606b-330">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9606b-331">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9606b-331">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="9606b-332">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="9606b-332">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="9606b-333">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="9606b-333">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="9606b-334">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9606b-334">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="9606b-335">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9606b-335">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9606b-336">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="9606b-336">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="9606b-337">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9606b-337">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="9606b-338">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="9606b-338">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="9606b-339">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-339">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9606b-340">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9606b-340">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="9606b-341">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="9606b-341">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="9606b-342">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="9606b-342">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="9606b-343">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="9606b-343">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="9606b-344">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9606b-344">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9606b-345">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9606b-345">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9606b-346">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9606b-346">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9606b-347">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9606b-347">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9606b-348">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9606b-348">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="9606b-349">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="9606b-349">Test the app</span></span>

* <span data-ttu-id="9606b-350">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="9606b-350">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="9606b-351">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="9606b-351">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="9606b-352">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="9606b-352">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="9606b-353">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9606b-353">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="9606b-355">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="9606b-355">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="9606b-356">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="9606b-356">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="9606b-357">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="9606b-357">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="9606b-358">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="9606b-358">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="9606b-359">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9606b-359">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9606b-360">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9606b-360">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9606b-361">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9606b-361">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
