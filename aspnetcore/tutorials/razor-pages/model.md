---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 95b6d3e016edcd2e13207c8e658cf0d2fb21f945
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959088"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="7fa55-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7fa55-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="7fa55-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7fa55-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7fa55-105">В этом разделе добавляются классы для управления фильмами в кроссплатформенной [базе данных SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="7fa55-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="7fa55-106">Приложения, созданные на основе шаблона ASP.NET Core, используют базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="7fa55-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="7fa55-107">Эти классы этой модели приложений используются в [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="7fa55-108">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="7fa55-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="7fa55-109">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="7fa55-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="7fa55-110">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="7fa55-111">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="7fa55-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7fa55-113">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="7fa55-114">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-114">Name the folder *Models*.</span></span>

<span data-ttu-id="7fa55-115">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-115">Right click the *Models* folder.</span></span> <span data-ttu-id="7fa55-116">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="7fa55-117">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7fa55-119">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="7fa55-120">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-121">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7fa55-122">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="7fa55-123">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="7fa55-124">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="7fa55-125">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="7fa55-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="7fa55-126">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="7fa55-127">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="7fa55-128">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="7fa55-129">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="7fa55-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="7fa55-130">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="7fa55-130">Scaffold the movie model</span></span>

<span data-ttu-id="7fa55-131">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="7fa55-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="7fa55-132">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="7fa55-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7fa55-134">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="7fa55-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="7fa55-135">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="7fa55-136">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="7fa55-136">Name the folder *Movies*</span></span>

<span data-ttu-id="7fa55-137">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="7fa55-139">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="7fa55-141">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="7fa55-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="7fa55-142">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="7fa55-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="7fa55-143">В строке **Класс контекста данных** щелкните значок плюса **+** и измените сгенерированное имя RazorPagesMovie.**Models**.RazorPagesMovieContext на RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="7fa55-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="7fa55-144">[Это изменение](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) не требуется.</span><span class="sxs-lookup"><span data-stu-id="7fa55-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="7fa55-145">Оно создает класс контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="7fa55-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="7fa55-146">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-146">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/3/arp.png)

<span data-ttu-id="7fa55-148">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="7fa55-150">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="7fa55-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7fa55-151">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="7fa55-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="7fa55-152">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7fa55-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="7fa55-153">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7fa55-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-154">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7fa55-155">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="7fa55-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7fa55-156">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="7fa55-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="7fa55-157">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7fa55-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="7fa55-158">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="7fa55-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7fa55-160">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="7fa55-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="7fa55-161">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="7fa55-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="7fa55-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="7fa55-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="7fa55-163">Обновленные возможности</span><span class="sxs-lookup"><span data-stu-id="7fa55-163">Updated</span></span>

* <span data-ttu-id="7fa55-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="7fa55-164">*Startup.cs*</span></span>

<span data-ttu-id="7fa55-165">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="7fa55-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7fa55-166">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="7fa55-167">В процессе формирования шаблонов создаются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="7fa55-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="7fa55-168">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="7fa55-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="7fa55-169">В следующем разделе приводится описание созданных файлов.</span><span class="sxs-lookup"><span data-stu-id="7fa55-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="7fa55-170">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="7fa55-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7fa55-172">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="7fa55-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="7fa55-173">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="7fa55-173">Add an initial migration.</span></span>
* <span data-ttu-id="7fa55-174">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="7fa55-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="7fa55-175">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="7fa55-177">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="7fa55-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-179">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="7fa55-180">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="7fa55-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="7fa55-181">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7fa55-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="7fa55-182">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="7fa55-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="7fa55-183">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="7fa55-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="7fa55-184">Команда миграции формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="7fa55-185">Схема создается на основе модели, указанной в `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="7fa55-186">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="7fa55-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="7fa55-187">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="7fa55-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="7fa55-188">Команда `update` выполняет метод `Up` в миграциях, которые не были применены.</span><span class="sxs-lookup"><span data-stu-id="7fa55-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="7fa55-189">В этом случае `update` запускает метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="7fa55-191">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="7fa55-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="7fa55-192">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7fa55-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7fa55-193">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7fa55-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7fa55-194">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="7fa55-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7fa55-195">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="7fa55-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7fa55-196">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7fa55-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="7fa55-197">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7fa55-198">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="7fa55-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="7fa55-199">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="7fa55-200">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="7fa55-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="7fa55-201">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="7fa55-202">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="7fa55-202">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="7fa55-203">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="7fa55-204">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="7fa55-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7fa55-205">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="7fa55-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="7fa55-206">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7fa55-208">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-209">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7fa55-210">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="7fa55-211">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="7fa55-211">Test the app</span></span>

* <span data-ttu-id="7fa55-212">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="7fa55-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="7fa55-213">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="7fa55-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="7fa55-214">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="7fa55-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="7fa55-215">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-215">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="7fa55-217">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="7fa55-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="7fa55-218">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="7fa55-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="7fa55-219">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="7fa55-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="7fa55-220">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="7fa55-221">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="7fa55-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7fa55-222">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7fa55-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7fa55-223">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="7fa55-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7fa55-224">В этом разделе добавляются классы для управления фильмами в кроссплатформенной [базе данных SQLite](https://www.sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="7fa55-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="7fa55-225">Приложения, созданные на основе шаблона ASP.NET Core, используют базу данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="7fa55-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="7fa55-226">Эти классы этой модели приложений используются в [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="7fa55-227">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="7fa55-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="7fa55-228">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="7fa55-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="7fa55-229">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="7fa55-230">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="7fa55-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7fa55-232">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="7fa55-233">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-233">Name the folder *Models*.</span></span>

<span data-ttu-id="7fa55-234">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-234">Right click the *Models* folder.</span></span> <span data-ttu-id="7fa55-235">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="7fa55-236">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="7fa55-238">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="7fa55-239">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-240">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7fa55-241">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="7fa55-242">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="7fa55-243">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="7fa55-244">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="7fa55-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="7fa55-245">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="7fa55-246">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="7fa55-247">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="7fa55-248">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="7fa55-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="7fa55-249">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="7fa55-249">Scaffold the movie model</span></span>

<span data-ttu-id="7fa55-250">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="7fa55-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="7fa55-251">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="7fa55-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7fa55-253">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="7fa55-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="7fa55-254">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="7fa55-255">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="7fa55-255">Name the folder *Movies*</span></span>

<span data-ttu-id="7fa55-256">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="7fa55-258">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="7fa55-260">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="7fa55-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="7fa55-261">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="7fa55-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="7fa55-262">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="7fa55-263">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-263">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<span data-ttu-id="7fa55-265">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="7fa55-267">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="7fa55-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="7fa55-268">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7fa55-268">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="7fa55-269">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7fa55-269">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-270">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="7fa55-271">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="7fa55-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7fa55-272">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="7fa55-272">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="7fa55-273">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="7fa55-273">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="7fa55-274">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="7fa55-274">Files created</span></span>

* <span data-ttu-id="7fa55-275">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="7fa55-275">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="7fa55-276">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="7fa55-276">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="7fa55-277">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="7fa55-277">File updated</span></span>

* <span data-ttu-id="7fa55-278">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="7fa55-278">*Startup.cs*</span></span>

<span data-ttu-id="7fa55-279">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="7fa55-279">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="7fa55-280">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="7fa55-280">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-281">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-281">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7fa55-282">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="7fa55-282">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="7fa55-283">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="7fa55-283">Add an initial migration.</span></span>
* <span data-ttu-id="7fa55-284">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="7fa55-284">Update the database with the initial migration.</span></span>

<span data-ttu-id="7fa55-285">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-285">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="7fa55-287">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="7fa55-287">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="7fa55-288">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-288">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="7fa55-289">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="7fa55-289">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="7fa55-290">Аргумент `InitialCreate` используется для присвоения имен миграции.</span><span class="sxs-lookup"><span data-stu-id="7fa55-290">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="7fa55-291">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="7fa55-291">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="7fa55-292">Дополнительные сведения можно найти по адресу: <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="7fa55-292">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="7fa55-293">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-293">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="7fa55-294">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-294">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-295">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-295">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-296">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-296">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="7fa55-297">В результате выполнения предыдущих команд выводится следующее предупреждение: "*Для десятичного столбца "Price" в типе сущности "Movie" не указан тип. Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию. С помощью метода "HasColumnType()" явно укажите тип столбца SQL Server, который может вместить все значения.* " Вы можете игнорировать это предупреждение, оно будет исправлено в следующем учебнике.</span><span class="sxs-lookup"><span data-stu-id="7fa55-297">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7fa55-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7fa55-298">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="7fa55-299">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="7fa55-299">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="7fa55-300">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7fa55-300">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7fa55-301">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="7fa55-301">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7fa55-302">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="7fa55-302">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7fa55-303">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="7fa55-303">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7fa55-304">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7fa55-304">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="7fa55-305">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-305">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="7fa55-306">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="7fa55-306">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="7fa55-307">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-307">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="7fa55-308">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="7fa55-308">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="7fa55-309">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-309">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="7fa55-310">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="7fa55-310">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="7fa55-311">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="7fa55-311">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="7fa55-312">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="7fa55-312">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7fa55-313">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="7fa55-313">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="7fa55-314">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7fa55-314">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7fa55-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7fa55-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7fa55-316">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-316">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7fa55-317">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7fa55-317">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7fa55-318">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="7fa55-318">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="7fa55-319">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="7fa55-319">Test the app</span></span>

* <span data-ttu-id="7fa55-320">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="7fa55-320">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="7fa55-321">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="7fa55-321">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="7fa55-322">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="7fa55-322">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="7fa55-323">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-323">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="7fa55-325">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="7fa55-325">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="7fa55-326">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="7fa55-326">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="7fa55-327">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="7fa55-327">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="7fa55-328">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="7fa55-328">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="7fa55-329">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="7fa55-329">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7fa55-330">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7fa55-330">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7fa55-331">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="7fa55-331">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
