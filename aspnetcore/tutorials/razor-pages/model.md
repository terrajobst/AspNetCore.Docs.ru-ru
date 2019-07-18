---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 02/12/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: be9f515178d0169a69487f917c7d39c6f11f1292
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815054"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="ef801-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef801-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="ef801-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="ef801-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="ef801-105">В этом разделе добавляются классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="ef801-106">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="ef801-107">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="ef801-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="ef801-108">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="ef801-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="ef801-109">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="ef801-110">[Просмотрите или скачайте](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) пример.</span><span class="sxs-lookup"><span data-stu-id="ef801-110">[View or download](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="ef801-111">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="ef801-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef801-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef801-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ef801-113">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="ef801-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="ef801-114">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="ef801-114">Name the folder *Models*.</span></span>

<span data-ttu-id="ef801-115">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="ef801-115">Right click the *Models* folder.</span></span> <span data-ttu-id="ef801-116">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="ef801-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="ef801-117">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="ef801-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef801-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ef801-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ef801-119">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="ef801-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="ef801-120">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="ef801-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef801-121">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ef801-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ef801-122">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="ef801-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="ef801-123">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="ef801-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="ef801-124">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="ef801-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="ef801-125">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="ef801-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ef801-126">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="ef801-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="ef801-127">В центральной области выберите **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="ef801-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="ef801-128">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ef801-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="ef801-129">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="ef801-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="ef801-130">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="ef801-130">Scaffold the movie model</span></span>

<span data-ttu-id="ef801-131">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="ef801-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="ef801-132">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="ef801-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef801-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef801-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ef801-134">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="ef801-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="ef801-135">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="ef801-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="ef801-136">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="ef801-136">Name the folder *Movies*</span></span>

<span data-ttu-id="ef801-137">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="ef801-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="ef801-139">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)**  >  **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="ef801-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="ef801-141">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="ef801-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="ef801-142">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="ef801-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="ef801-143">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="ef801-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="ef801-144">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="ef801-144">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<span data-ttu-id="ef801-146">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef801-147">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ef801-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="ef801-148">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="ef801-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ef801-149">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="ef801-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="ef801-150">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ef801-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="ef801-151">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ef801-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef801-152">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ef801-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ef801-153">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="ef801-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ef801-154">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="ef801-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="ef801-155">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="ef801-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="ef801-156">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="ef801-156">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="ef801-157">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="ef801-157">Files created</span></span>

* <span data-ttu-id="ef801-158">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="ef801-158">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="ef801-159">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="ef801-159">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="ef801-160">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="ef801-160">File updated</span></span>

* <span data-ttu-id="ef801-161">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="ef801-161">*Startup.cs*</span></span>

<span data-ttu-id="ef801-162">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="ef801-162">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="ef801-163">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="ef801-163">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef801-164">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef801-164">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="ef801-165">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="ef801-165">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="ef801-166">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="ef801-166">Add an initial migration.</span></span>
* <span data-ttu-id="ef801-167">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="ef801-167">Update the database with the initial migration.</span></span>

<span data-ttu-id="ef801-168">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="ef801-168">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="ef801-170">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="ef801-170">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef801-171">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ef801-171">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef801-172">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ef801-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="ef801-173">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="ef801-173">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="ef801-174">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ef801-174">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="ef801-175">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="ef801-175">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="ef801-176">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="ef801-176">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="ef801-177">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-177">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="ef801-178">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="ef801-178">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="ef801-179">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="ef801-179">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="ef801-180">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="ef801-180">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="ef801-181">Команда `ef database update` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="ef801-181">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="ef801-182">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-182">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef801-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef801-183">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="ef801-184">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="ef801-184">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="ef801-185">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="ef801-185">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="ef801-186">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="ef801-186">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="ef801-187">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="ef801-187">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="ef801-188">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="ef801-188">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="ef801-189">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="ef801-189">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="ef801-190">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ef801-190">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="ef801-191">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="ef801-191">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="ef801-192">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="ef801-192">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="ef801-193">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="ef801-193">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="ef801-194">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-194">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="ef801-195">Представленный выше код создает свойство [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="ef801-195">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="ef801-196">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-196">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="ef801-197">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="ef801-197">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="ef801-198">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="ef801-198">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="ef801-199">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ef801-199">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef801-200">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ef801-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ef801-201">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="ef801-201">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef801-202">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="ef801-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="ef801-203">Проверьте метод `Up`.</span><span class="sxs-lookup"><span data-stu-id="ef801-203">Examine the `Up` method.</span></span>

---

<span data-ttu-id="ef801-204">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-204">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="ef801-205">Схема создается на основе модели, указанной в `RazorPagesMovieContext` (в файле *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="ef801-205">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="ef801-206">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="ef801-206">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="ef801-207">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="ef801-207">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="ef801-208">Дополнительные сведения можно найти по адресу: <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="ef801-208">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="ef801-209">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="ef801-209">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="ef801-210">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="ef801-210">Test the app</span></span>

* <span data-ttu-id="ef801-211">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="ef801-211">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="ef801-212">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="ef801-212">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="ef801-213">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="ef801-213">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="ef801-214">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="ef801-214">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="ef801-216">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="ef801-216">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="ef801-217">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="ef801-217">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="ef801-218">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="ef801-218">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="ef801-219">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="ef801-219">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ef801-220">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ef801-220">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef801-221">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ef801-221">Additional resources</span></span>

* [<span data-ttu-id="ef801-222">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="ef801-222">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=sFVIsdR_RcM)

> [!div class="step-by-step"]
> <span data-ttu-id="ef801-223">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="ef801-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
