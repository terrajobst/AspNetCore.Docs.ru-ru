---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 6132f7b907014b4f57bb9ae0300e00b6ecb23f1a
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820065"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="fb8ac-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fb8ac-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="fb8ac-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="fb8ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="fb8ac-105">В этом разделе добавляются классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="fb8ac-106">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="fb8ac-107">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="fb8ac-108">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="fb8ac-109">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="fb8ac-110">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="fb8ac-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fb8ac-112">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="fb8ac-113">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-113">Name the folder *Models*.</span></span>

<span data-ttu-id="fb8ac-114">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-114">Right click the *Models* folder.</span></span> <span data-ttu-id="fb8ac-115">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="fb8ac-116">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-117">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fb8ac-118">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="fb8ac-119">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-120">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fb8ac-121">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fb8ac-122">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="fb8ac-123">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fb8ac-124">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fb8ac-125">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fb8ac-126">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="fb8ac-127">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="fb8ac-128">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="fb8ac-129">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="fb8ac-129">Scaffold the movie model</span></span>

<span data-ttu-id="fb8ac-130">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="fb8ac-131">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fb8ac-133">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="fb8ac-134">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="fb8ac-135">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="fb8ac-135">Name the folder *Movies*</span></span>

<span data-ttu-id="fb8ac-136">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="fb8ac-138">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="fb8ac-140">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="fb8ac-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="fb8ac-141">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="fb8ac-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="fb8ac-142">В строке **Класс контекста данных** щелкните значок плюса **+** и измените сгенерированное имя RazorPagesMovie.**Models**.RazorPagesMovieContext на RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="fb8ac-143">[Это изменение](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) не требуется.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="fb8ac-144">Оно создает класс контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="fb8ac-145">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-145">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/3/arp.png)

<span data-ttu-id="fb8ac-147">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-148">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="fb8ac-149">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fb8ac-150">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="fb8ac-151">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="fb8ac-152">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-153">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fb8ac-154">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fb8ac-155">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="fb8ac-156">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="fb8ac-157">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="fb8ac-158">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="fb8ac-158">Files created</span></span>

* <span data-ttu-id="fb8ac-159">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="fb8ac-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="fb8ac-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="fb8ac-161">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="fb8ac-161">File updated</span></span>

* <span data-ttu-id="fb8ac-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="fb8ac-162">*Startup.cs*</span></span>

<span data-ttu-id="fb8ac-163">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="fb8ac-164">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="fb8ac-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fb8ac-166">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="fb8ac-167">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-167">Add an initial migration.</span></span>
* <span data-ttu-id="fb8ac-168">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="fb8ac-169">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="fb8ac-171">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-172">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-173">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="fb8ac-174">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="fb8ac-175">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="fb8ac-176">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="fb8ac-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="fb8ac-177">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="fb8ac-178">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fb8ac-179">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fb8ac-180">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="fb8ac-181">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="fb8ac-182">Команда `ef database update` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="fb8ac-183">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="fb8ac-185">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="fb8ac-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="fb8ac-186">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="fb8ac-187">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="fb8ac-188">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="fb8ac-189">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="fb8ac-190">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="fb8ac-191">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fb8ac-192">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="fb8ac-193">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="fb8ac-194">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="fb8ac-195">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="fb8ac-196">Представленный выше код создает свойство [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="fb8ac-197">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="fb8ac-198">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="fb8ac-199">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="fb8ac-200">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-201">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fb8ac-202">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-203">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fb8ac-204">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="fb8ac-205">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fb8ac-206">Схема создается на основе модели, указанной в `RazorPagesMovieContext` (в файле *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fb8ac-207">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="fb8ac-208">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="fb8ac-209">Дополнительные сведения можно найти по адресу: <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="fb8ac-210">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="fb8ac-211">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="fb8ac-211">Test the app</span></span>

* <span data-ttu-id="fb8ac-212">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="fb8ac-213">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="fb8ac-214">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="fb8ac-215">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-215">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="fb8ac-217">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="fb8ac-218">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="fb8ac-219">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="fb8ac-220">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="fb8ac-221">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb8ac-222">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fb8ac-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fb8ac-223">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="fb8ac-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="fb8ac-224">В этом разделе добавляются классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="fb8ac-225">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="fb8ac-226">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="fb8ac-227">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="fb8ac-228">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="fb8ac-229">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="fb8ac-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fb8ac-231">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="fb8ac-232">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-232">Name the folder *Models*.</span></span>

<span data-ttu-id="fb8ac-233">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-233">Right click the *Models* folder.</span></span> <span data-ttu-id="fb8ac-234">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="fb8ac-235">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-236">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="fb8ac-237">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="fb8ac-238">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-239">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fb8ac-240">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fb8ac-241">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="fb8ac-242">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fb8ac-243">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fb8ac-244">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fb8ac-245">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="fb8ac-246">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="fb8ac-247">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="fb8ac-248">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="fb8ac-248">Scaffold the movie model</span></span>

<span data-ttu-id="fb8ac-249">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="fb8ac-250">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fb8ac-252">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="fb8ac-253">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="fb8ac-254">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="fb8ac-254">Name the folder *Movies*</span></span>

<span data-ttu-id="fb8ac-255">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="fb8ac-257">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="fb8ac-259">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="fb8ac-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="fb8ac-260">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="fb8ac-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="fb8ac-261">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="fb8ac-262">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-262">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<span data-ttu-id="fb8ac-264">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-265">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="fb8ac-266">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fb8ac-267">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="fb8ac-268">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="fb8ac-269">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-270">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="fb8ac-271">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="fb8ac-272">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="fb8ac-273">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="fb8ac-274">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="fb8ac-275">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="fb8ac-275">Files created</span></span>

* <span data-ttu-id="fb8ac-276">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="fb8ac-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="fb8ac-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="fb8ac-278">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="fb8ac-278">File updated</span></span>

* <span data-ttu-id="fb8ac-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="fb8ac-279">*Startup.cs*</span></span>

<span data-ttu-id="fb8ac-280">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="fb8ac-281">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="fb8ac-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="fb8ac-283">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="fb8ac-284">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-284">Add an initial migration.</span></span>
* <span data-ttu-id="fb8ac-285">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="fb8ac-286">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="fb8ac-288">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-289">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-290">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="fb8ac-291">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="fb8ac-292">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="fb8ac-293">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="fb8ac-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="fb8ac-294">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="fb8ac-295">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fb8ac-296">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fb8ac-297">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="fb8ac-298">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="fb8ac-299">Команда `ef database update` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="fb8ac-300">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="fb8ac-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="fb8ac-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="fb8ac-302">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="fb8ac-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="fb8ac-303">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="fb8ac-304">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="fb8ac-305">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="fb8ac-306">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="fb8ac-307">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="fb8ac-308">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="fb8ac-309">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="fb8ac-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="fb8ac-310">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="fb8ac-311">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="fb8ac-312">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="fb8ac-313">Представленный выше код создает свойство [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="fb8ac-314">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="fb8ac-315">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="fb8ac-316">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="fb8ac-317">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="fb8ac-318">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="fb8ac-319">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="fb8ac-320">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb8ac-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="fb8ac-321">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="fb8ac-322">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="fb8ac-323">Схема создается на основе модели, указанной в `RazorPagesMovieContext` (в файле *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="fb8ac-324">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="fb8ac-325">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="fb8ac-326">Дополнительные сведения можно найти по адресу: <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="fb8ac-327">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="fb8ac-328">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="fb8ac-328">Test the app</span></span>

* <span data-ttu-id="fb8ac-329">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="fb8ac-330">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="fb8ac-331">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="fb8ac-332">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-332">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="fb8ac-334">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="fb8ac-335">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="fb8ac-336">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="fb8ac-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="fb8ac-337">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="fb8ac-338">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="fb8ac-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fb8ac-339">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fb8ac-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fb8ac-340">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="fb8ac-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
