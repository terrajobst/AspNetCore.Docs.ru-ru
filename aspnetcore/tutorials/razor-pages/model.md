---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "78647176"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="9320c-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9320c-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="9320c-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="9320c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="9320c-105">В этом разделе добавляются классы для управления фильмами в кроссплатформенной [базе данных SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="9320c-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="9320c-106">Приложения, созданные на основе шаблона ASP.NET Core, используют базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="9320c-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="9320c-107">Эти классы этой модели приложений используются в [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="9320c-108">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="9320c-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="9320c-109">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="9320c-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="9320c-110">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9320c-111">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="9320c-111">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9320c-113">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9320c-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9320c-114">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-114">Name the folder *Models*.</span></span>

<span data-ttu-id="9320c-115">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-115">Right click the *Models* folder.</span></span> <span data-ttu-id="9320c-116">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="9320c-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="9320c-117">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="9320c-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9320c-119">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="9320c-120">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="9320c-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-121">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9320c-122">На панели решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить**>**Новая папка**. Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="9320c-123">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить**>**Новый файл...**</span><span class="sxs-lookup"><span data-stu-id="9320c-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="9320c-124">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9320c-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="9320c-125">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="9320c-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="9320c-126">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="9320c-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="9320c-127">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9320c-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="9320c-128">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="9320c-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="9320c-129">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="9320c-129">Scaffold the movie model</span></span>

<span data-ttu-id="9320c-130">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="9320c-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="9320c-131">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="9320c-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9320c-133">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9320c-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9320c-134">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9320c-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9320c-135">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9320c-135">Name the folder *Movies*</span></span>

<span data-ttu-id="9320c-136">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить**>**New Scaffolded Item** (Создать шаблонный элемент).</span><span class="sxs-lookup"><span data-stu-id="9320c-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="9320c-138">В диалоговом окне **Добавление шаблона** выберите **Razor Pages на основе Entity Framework (CRUD)** >**Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="9320c-140">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9320c-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9320c-141">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="9320c-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9320c-142">В строке **Класс контекста данных** щелкните значок плюса **+** и измените сгенерированное имя RazorPagesMovie.**Models**.RazorPagesMovieContext на RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="9320c-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="9320c-143">[Это изменение](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) не требуется.</span><span class="sxs-lookup"><span data-stu-id="9320c-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="9320c-144">Оно создает класс контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="9320c-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="9320c-145">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-145">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/3/arp.png)

<span data-ttu-id="9320c-147">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="9320c-149">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="9320c-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="9320c-150">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="9320c-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="9320c-151">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9320c-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="9320c-152">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9320c-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-153">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9320c-154">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9320c-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9320c-155">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9320c-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9320c-156">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9320c-156">Name the folder *Movies*</span></span>

<span data-ttu-id="9320c-157">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **New Scaffolding...** (Создать шаблон...).</span><span class="sxs-lookup"><span data-stu-id="9320c-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/scaMac.png)

<span data-ttu-id="9320c-159">В диалоговом окне **New Scaffolding** (Новый шаблон) выберите **Razor Pages на основе Entity Framework (CRUD)** > **Далее**.</span><span class="sxs-lookup"><span data-stu-id="9320c-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="9320c-161">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9320c-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9320c-162">В раскрывающемся списке **Класс модели** выберите или введите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="9320c-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9320c-163">В строке **Data context class** (Класс контекста данных) введите имя нового класса RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="9320c-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="9320c-164">[Это изменение](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) не требуется.</span><span class="sxs-lookup"><span data-stu-id="9320c-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="9320c-165">Оно создает класс контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="9320c-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="9320c-166">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-166">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arpMac.png)

<span data-ttu-id="9320c-168">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="9320c-169">Добавление средств EF</span><span class="sxs-lookup"><span data-stu-id="9320c-169">Add EF tools</span></span>

<span data-ttu-id="9320c-170">Выполните следующую команду .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="9320c-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="9320c-171">Приведенная выше команда добавляет средства Entity Framework Core для .NET Core CLI.</span><span class="sxs-lookup"><span data-stu-id="9320c-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="9320c-172">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="9320c-172">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9320c-174">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9320c-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="9320c-175">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9320c-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="9320c-176">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9320c-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="9320c-177">Обновленные возможности</span><span class="sxs-lookup"><span data-stu-id="9320c-177">Updated</span></span>

* <span data-ttu-id="9320c-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="9320c-178">*Startup.cs*</span></span>

<span data-ttu-id="9320c-179">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="9320c-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-180">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9320c-181">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9320c-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="9320c-182">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9320c-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="9320c-183">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9320c-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="9320c-184">Обновленные возможности</span><span class="sxs-lookup"><span data-stu-id="9320c-184">Updated</span></span>

* <span data-ttu-id="9320c-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="9320c-185">*Startup.cs*</span></span>

<span data-ttu-id="9320c-186">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="9320c-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9320c-188">В процессе формирования шаблонов создаются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9320c-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="9320c-189">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9320c-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="9320c-190">В следующем разделе приводится описание созданных файлов.</span><span class="sxs-lookup"><span data-stu-id="9320c-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="9320c-191">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="9320c-191">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9320c-193">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="9320c-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="9320c-194">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9320c-194">Add an initial migration.</span></span>
* <span data-ttu-id="9320c-195">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9320c-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="9320c-196">В меню **Сервис**  последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9320c-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9320c-198">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9320c-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-200">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="9320c-201">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="9320c-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="9320c-202">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9320c-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="9320c-203">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="9320c-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="9320c-204">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="9320c-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="9320c-205">Команда миграции формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="9320c-206">Схема создается на основе модели, указанной в `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="9320c-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="9320c-207">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="9320c-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="9320c-208">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="9320c-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="9320c-209">Команда `update` выполняет метод `Up` в миграциях, которые не были применены.</span><span class="sxs-lookup"><span data-stu-id="9320c-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="9320c-210">В этом случае `update` запускает метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="9320c-212">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="9320c-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="9320c-213">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9320c-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9320c-214">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9320c-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="9320c-215">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="9320c-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="9320c-216">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="9320c-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="9320c-217">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9320c-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="9320c-218">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9320c-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9320c-219">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="9320c-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="9320c-220">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9320c-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="9320c-221">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="9320c-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="9320c-222">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9320c-223">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9320c-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="9320c-224">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="9320c-225">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="9320c-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="9320c-226">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="9320c-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="9320c-227">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9320c-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9320c-229">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9320c-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-230">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9320c-231">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9320c-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="9320c-232">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="9320c-232">Test the app</span></span>

* <span data-ttu-id="9320c-233">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="9320c-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="9320c-234">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="9320c-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="9320c-235">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="9320c-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="9320c-236">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9320c-236">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="9320c-238">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="9320c-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="9320c-239">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="9320c-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="9320c-240">Инструкции по глобализации см. на [сайте GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="9320c-240">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="9320c-241">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="9320c-242">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9320c-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9320c-243">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9320c-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9320c-244">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9320c-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9320c-245">В этом разделе добавляются классы для управления фильмами в кроссплатформенной [базе данных SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="9320c-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="9320c-246">Приложения, созданные на основе шаблона ASP.NET Core, используют базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="9320c-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="9320c-247">Эти классы этой модели приложений используются в [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="9320c-248">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="9320c-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="9320c-249">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="9320c-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="9320c-250">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9320c-251">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="9320c-251">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9320c-253">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9320c-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9320c-254">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-254">Name the folder *Models*.</span></span>

<span data-ttu-id="9320c-255">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-255">Right click the *Models* folder.</span></span> <span data-ttu-id="9320c-256">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="9320c-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="9320c-257">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="9320c-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9320c-259">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="9320c-260">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="9320c-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-261">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9320c-262">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9320c-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="9320c-263">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="9320c-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="9320c-264">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить**>**Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="9320c-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="9320c-265">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9320c-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="9320c-266">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="9320c-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="9320c-267">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="9320c-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="9320c-268">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9320c-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="9320c-269">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="9320c-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="9320c-270">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="9320c-270">Scaffold the movie model</span></span>

<span data-ttu-id="9320c-271">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="9320c-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="9320c-272">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="9320c-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9320c-274">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9320c-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9320c-275">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9320c-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9320c-276">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9320c-276">Name the folder *Movies*</span></span>

<span data-ttu-id="9320c-277">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить**>**New Scaffolded Item** (Создать шаблонный элемент).</span><span class="sxs-lookup"><span data-stu-id="9320c-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="9320c-279">В диалоговом окне **Добавление шаблона** выберите **Razor Pages на основе Entity Framework (CRUD)** >**Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="9320c-281">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9320c-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="9320c-282">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="9320c-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9320c-283">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="9320c-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="9320c-284">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-284">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<span data-ttu-id="9320c-286">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="9320c-288">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="9320c-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="9320c-289">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9320c-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="9320c-290">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9320c-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-291">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9320c-292">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9320c-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9320c-293">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="9320c-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9320c-294">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="9320c-294">Name the folder *Movies*</span></span>

<span data-ttu-id="9320c-295">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить**>**New Scaffolded Item** (Создать шаблонный элемент).</span><span class="sxs-lookup"><span data-stu-id="9320c-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/scaMac.png)

<span data-ttu-id="9320c-297">В диалоговом окне **Add New Scaffolding** (Добавление нового шаблона) выберите **Razor Pages на основе Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="9320c-299">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="9320c-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9320c-300">В раскрывающемся списке **Класс модели** выберите или введите **Фильм**.</span><span class="sxs-lookup"><span data-stu-id="9320c-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="9320c-301">В строке **Data context class** (Класс контекста данных) введите или выберите **RazorPagesMovieContext**. Это приведет к созданию класса контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="9320c-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="9320c-302">В этом случае это будет **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="9320c-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="9320c-303">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-303">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arpMac.png)

<span data-ttu-id="9320c-305">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="9320c-306">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="9320c-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="9320c-307">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="9320c-307">Files created</span></span>

* <span data-ttu-id="9320c-308">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="9320c-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="9320c-309">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9320c-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="9320c-310">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="9320c-310">File updated</span></span>

* <span data-ttu-id="9320c-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="9320c-311">*Startup.cs*</span></span>

<span data-ttu-id="9320c-312">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="9320c-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="9320c-313">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="9320c-313">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9320c-315">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="9320c-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="9320c-316">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9320c-316">Add an initial migration.</span></span>
* <span data-ttu-id="9320c-317">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="9320c-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="9320c-318">В меню **Сервис**  последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9320c-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9320c-320">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="9320c-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9320c-321">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9320c-322">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="9320c-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="9320c-323">Аргумент `InitialCreate` используется для присвоения имен миграции.</span><span class="sxs-lookup"><span data-stu-id="9320c-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="9320c-324">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="9320c-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="9320c-325">Для получения дополнительной информации см. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="9320c-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="9320c-326">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="9320c-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="9320c-327">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-329">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="9320c-330">В результате выполнения предыдущих команд выводится следующее предупреждение: "*Для десятичного столбца "Price" в типе сущности "Movie" не указан тип. Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию. С помощью метода "HasColumnType()" явно укажите тип столбца SQL Server, который может вместить все значения.* " Вы можете игнорировать это предупреждение, оно будет исправлено в следующем учебнике.</span><span class="sxs-lookup"><span data-stu-id="9320c-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="9320c-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9320c-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="9320c-332">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="9320c-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="9320c-333">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9320c-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9320c-334">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="9320c-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="9320c-335">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="9320c-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="9320c-336">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="9320c-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="9320c-337">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="9320c-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="9320c-338">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9320c-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9320c-339">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="9320c-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="9320c-340">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="9320c-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="9320c-341">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="9320c-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="9320c-342">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9320c-343">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9320c-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="9320c-344">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="9320c-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="9320c-345">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="9320c-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="9320c-346">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="9320c-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="9320c-347">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9320c-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="9320c-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9320c-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9320c-349">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9320c-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="9320c-350">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="9320c-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9320c-351">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="9320c-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="9320c-352">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="9320c-352">Test the app</span></span>

* <span data-ttu-id="9320c-353">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="9320c-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="9320c-354">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="9320c-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="9320c-355">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="9320c-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="9320c-356">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="9320c-356">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="9320c-358">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="9320c-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="9320c-359">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="9320c-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="9320c-360">Инструкции по глобализации см. на [сайте GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="9320c-360">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="9320c-361">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="9320c-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="9320c-362">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9320c-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9320c-363">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9320c-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9320c-364">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9320c-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
