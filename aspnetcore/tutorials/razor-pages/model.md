---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 9/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 4f8b80cb51bd10eb3b136a780dc123c41d61c0a5
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519074"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="24e21-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24e21-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="24e21-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="24e21-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="24e21-105">В этом разделе добавляются классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="24e21-106">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="24e21-107">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая упрощает получение доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="24e21-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="24e21-108">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="24e21-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="24e21-109">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="24e21-110">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="24e21-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24e21-112">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="24e21-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="24e21-113">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-113">Name the folder *Models*.</span></span>

<span data-ttu-id="24e21-114">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-114">Right click the *Models* folder.</span></span> <span data-ttu-id="24e21-115">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="24e21-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="24e21-116">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="24e21-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="24e21-118">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="24e21-119">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="24e21-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-120">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="24e21-121">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="24e21-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="24e21-122">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="24e21-123">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="24e21-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="24e21-124">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="24e21-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="24e21-125">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="24e21-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="24e21-126">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="24e21-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="24e21-127">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="24e21-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="24e21-128">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="24e21-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="24e21-129">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="24e21-129">Scaffold the movie model</span></span>

<span data-ttu-id="24e21-130">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="24e21-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="24e21-131">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="24e21-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24e21-133">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="24e21-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="24e21-134">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="24e21-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="24e21-135">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="24e21-135">Name the folder *Movies*</span></span>

<span data-ttu-id="24e21-136">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="24e21-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="24e21-138">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="24e21-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="24e21-140">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="24e21-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="24e21-141">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="24e21-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="24e21-142">В строке **Класс контекста данных** щелкните значок плюса **+** и измените сгенерированное имя RazorPagesMovie.**Models**.RazorPagesMovieContext на RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="24e21-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="24e21-143">[Это изменение](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) не требуется.</span><span class="sxs-lookup"><span data-stu-id="24e21-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="24e21-144">Оно создает класс контекста базы данных с правильным пространством имен.</span><span class="sxs-lookup"><span data-stu-id="24e21-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="24e21-145">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="24e21-145">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/3/arp.png)

<span data-ttu-id="24e21-147">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="24e21-149">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="24e21-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="24e21-150">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="24e21-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="24e21-151">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="24e21-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="24e21-152">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="24e21-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-153">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="24e21-154">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="24e21-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="24e21-155">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="24e21-155">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="24e21-156">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="24e21-156">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="24e21-157">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="24e21-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24e21-159">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="24e21-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="24e21-160">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="24e21-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="24e21-161">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="24e21-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="24e21-162">Обновленные возможности</span><span class="sxs-lookup"><span data-stu-id="24e21-162">Updated</span></span>

* <span data-ttu-id="24e21-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="24e21-163">*Startup.cs*</span></span>

<span data-ttu-id="24e21-164">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="24e21-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="24e21-165">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="24e21-166">В процессе формирования шаблонов создаются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="24e21-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="24e21-167">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="24e21-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="24e21-168">В следующем разделе приводится описание созданных файлов.</span><span class="sxs-lookup"><span data-stu-id="24e21-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="24e21-169">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="24e21-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24e21-171">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="24e21-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="24e21-172">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="24e21-172">Add an initial migration.</span></span>
* <span data-ttu-id="24e21-173">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="24e21-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="24e21-174">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="24e21-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="24e21-176">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="24e21-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-178">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="24e21-179">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="24e21-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="24e21-180">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="24e21-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="24e21-181">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="24e21-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="24e21-182">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="24e21-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="24e21-183">Команда миграции формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-183">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="24e21-184">Схема создается на основе модели, указанной в `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="24e21-184">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="24e21-185">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="24e21-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="24e21-186">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="24e21-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="24e21-187">Команда `update` выполняет метод `Up` в миграциях, которые не были применены.</span><span class="sxs-lookup"><span data-stu-id="24e21-187">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="24e21-188">В этом случае `update` запускает метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-188">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="24e21-190">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="24e21-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="24e21-191">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="24e21-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="24e21-192">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="24e21-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="24e21-193">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="24e21-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="24e21-194">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="24e21-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="24e21-195">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="24e21-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="24e21-196">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24e21-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="24e21-197">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="24e21-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="24e21-198">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="24e21-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="24e21-199">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="24e21-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="24e21-200">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="24e21-201">Представленный выше код создает свойство [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="24e21-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="24e21-202">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="24e21-203">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="24e21-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="24e21-204">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="24e21-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="24e21-205">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="24e21-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="24e21-207">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="24e21-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-208">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="24e21-209">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="24e21-209">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="24e21-210">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="24e21-210">Test the app</span></span>

* <span data-ttu-id="24e21-211">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="24e21-211">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="24e21-212">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="24e21-212">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="24e21-213">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="24e21-213">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="24e21-214">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="24e21-214">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="24e21-216">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="24e21-216">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="24e21-217">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="24e21-217">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="24e21-218">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="24e21-218">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="24e21-219">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="24e21-219">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="24e21-220">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="24e21-220">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24e21-221">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="24e21-221">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="24e21-222">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="24e21-222">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="24e21-223">В этом разделе добавляются классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-223">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="24e21-224">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-224">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="24e21-225">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="24e21-225">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="24e21-226">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="24e21-226">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="24e21-227">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-227">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="24e21-228">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="24e21-228">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-229">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-229">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24e21-230">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="24e21-230">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="24e21-231">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-231">Name the folder *Models*.</span></span>

<span data-ttu-id="24e21-232">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-232">Right click the *Models* folder.</span></span> <span data-ttu-id="24e21-233">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="24e21-233">Select **Add** > **Class**.</span></span> <span data-ttu-id="24e21-234">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="24e21-234">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-235">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-235">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="24e21-236">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-236">Add a folder named *Models*.</span></span>
* <span data-ttu-id="24e21-237">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="24e21-237">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-238">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-238">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="24e21-239">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="24e21-239">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="24e21-240">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="24e21-240">Name the folder *Models*.</span></span>
* <span data-ttu-id="24e21-241">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="24e21-241">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="24e21-242">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="24e21-242">In the **New File** dialog:</span></span>

  * <span data-ttu-id="24e21-243">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="24e21-243">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="24e21-244">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="24e21-244">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="24e21-245">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="24e21-245">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="24e21-246">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="24e21-246">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="24e21-247">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="24e21-247">Scaffold the movie model</span></span>

<span data-ttu-id="24e21-248">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="24e21-248">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="24e21-249">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="24e21-249">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24e21-251">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="24e21-251">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="24e21-252">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="24e21-252">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="24e21-253">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="24e21-253">Name the folder *Movies*</span></span>

<span data-ttu-id="24e21-254">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="24e21-254">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="24e21-256">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="24e21-256">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="24e21-258">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="24e21-258">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="24e21-259">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="24e21-259">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="24e21-260">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="24e21-260">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="24e21-261">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="24e21-261">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<span data-ttu-id="24e21-263">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-263">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-264">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-264">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="24e21-265">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="24e21-265">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="24e21-266">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="24e21-266">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="24e21-267">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="24e21-267">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="24e21-268">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="24e21-268">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-269">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-269">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="24e21-270">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="24e21-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="24e21-271">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="24e21-271">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="24e21-272">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="24e21-272">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="24e21-273">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="24e21-273">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="24e21-274">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="24e21-274">Files created</span></span>

* <span data-ttu-id="24e21-275">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="24e21-275">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="24e21-276">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="24e21-276">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="24e21-277">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="24e21-277">File updated</span></span>

* <span data-ttu-id="24e21-278">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="24e21-278">*Startup.cs*</span></span>

<span data-ttu-id="24e21-279">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="24e21-279">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="24e21-280">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="24e21-280">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-281">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-281">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="24e21-282">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="24e21-282">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="24e21-283">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="24e21-283">Add an initial migration.</span></span>
* <span data-ttu-id="24e21-284">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="24e21-284">Update the database with the initial migration.</span></span>

<span data-ttu-id="24e21-285">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="24e21-285">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="24e21-287">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="24e21-287">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="24e21-288">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-288">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="24e21-289">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="24e21-289">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="24e21-290">Аргумент `InitialCreate` используется для присвоения имен миграции.</span><span class="sxs-lookup"><span data-stu-id="24e21-290">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="24e21-291">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="24e21-291">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="24e21-292">Дополнительные сведения можно найти по адресу: <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="24e21-292">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="24e21-293">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="24e21-293">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="24e21-294">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-294">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-295">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-295">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-296">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-296">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="24e21-297">В результате выполнения предыдущих команд выводится следующее предупреждение: "*Для десятичного столбца "Price" в типе сущности "Movie" не указан тип. Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию. С помощью метода "HasColumnType()" явно укажите тип столбца SQL Server, который может вместить все значения.* " Вы можете игнорировать это предупреждение, оно будет исправлено в следующем учебнике.</span><span class="sxs-lookup"><span data-stu-id="24e21-297">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="24e21-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="24e21-298">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="24e21-299">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="24e21-299">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="24e21-300">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="24e21-300">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="24e21-301">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="24e21-301">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="24e21-302">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="24e21-302">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="24e21-303">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="24e21-303">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="24e21-304">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="24e21-304">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="24e21-305">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24e21-305">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="24e21-306">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="24e21-306">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="24e21-307">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="24e21-307">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="24e21-308">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="24e21-308">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="24e21-309">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-309">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="24e21-310">Представленный выше код создает свойство [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="24e21-310">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="24e21-311">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="24e21-311">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="24e21-312">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="24e21-312">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="24e21-313">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="24e21-313">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="24e21-314">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="24e21-314">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="24e21-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="24e21-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="24e21-316">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="24e21-316">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="24e21-317">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="24e21-317">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="24e21-318">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="24e21-318">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="24e21-319">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="24e21-319">Test the app</span></span>

* <span data-ttu-id="24e21-320">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="24e21-320">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="24e21-321">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="24e21-321">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="24e21-322">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="24e21-322">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="24e21-323">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="24e21-323">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="24e21-325">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="24e21-325">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="24e21-326">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="24e21-326">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="24e21-327">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="24e21-327">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="24e21-328">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="24e21-328">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="24e21-329">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="24e21-329">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24e21-330">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="24e21-330">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="24e21-331">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="24e21-331">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
