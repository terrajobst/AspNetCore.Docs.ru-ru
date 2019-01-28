---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 5693fb833d19375ef53e0f367bc6064e239b01d3
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444108"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="58ff8-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="58ff8-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="58ff8-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="58ff8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="58ff8-105">В этом разделе добавляются классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="58ff8-106">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="58ff8-107">EF Core —это платформа объектно-реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="58ff8-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="58ff8-108">Эти классы моделей называются классами POCO (от plain old CLR objects — "старые добрые объекты CLR"), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="58ff8-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="58ff8-109">Эти классы определяют свойства данных, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="58ff8-110">[Просмотрите или скачайте](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) пример.</span><span class="sxs-lookup"><span data-stu-id="58ff8-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="58ff8-111">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="58ff8-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="58ff8-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58ff8-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="58ff8-113">Щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="58ff8-114">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="58ff8-114">Name the folder *Models*.</span></span>

<span data-ttu-id="58ff8-115">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="58ff8-115">Right click the *Models* folder.</span></span> <span data-ttu-id="58ff8-116">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="58ff8-117">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="58ff8-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="58ff8-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="58ff8-119">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="58ff8-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="58ff8-120">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="58ff8-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="58ff8-121">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="58ff8-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="58ff8-122">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="58ff8-123">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="58ff8-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="58ff8-124">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="58ff8-125">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="58ff8-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="58ff8-126">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="58ff8-127">Выберите в центральной области **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-127">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="58ff8-128">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="58ff8-129">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок компиляции.</span><span class="sxs-lookup"><span data-stu-id="58ff8-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="58ff8-130">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="58ff8-130">Scaffold the movie model</span></span>

<span data-ttu-id="58ff8-131">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="58ff8-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="58ff8-132">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="58ff8-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="58ff8-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58ff8-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="58ff8-134">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="58ff8-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="58ff8-135">Щелкните правой кнопкой мыши папку *Pages* и выберите **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="58ff8-136">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="58ff8-136">Name the folder *Movies*</span></span>

<span data-ttu-id="58ff8-137">Щелкните правой кнопкой мыши папку *Pages/Movies* и выберите **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="58ff8-139">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)**  >  **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="58ff8-141">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="58ff8-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="58ff8-142">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="58ff8-143">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="58ff8-144">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-144">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<span data-ttu-id="58ff8-146">Файл *appsettings.json* обновляется с указанием строки подключения, используемой для подключения к локальной базе данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="58ff8-147">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="58ff8-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="58ff8-148">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="58ff8-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="58ff8-149">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="58ff8-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="58ff8-150">**Для Windows**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="58ff8-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="58ff8-151">**Для macOS и Linux**: Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="58ff8-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="58ff8-152">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="58ff8-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="58ff8-153">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="58ff8-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="58ff8-154">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="58ff8-154">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```
* <span data-ttu-id="58ff8-155">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="58ff8-155">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="58ff8-156">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="58ff8-156">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="58ff8-157">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="58ff8-157">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="58ff8-158">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="58ff8-158">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="58ff8-159">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="58ff8-159">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="58ff8-160">В процессе формирования шаблонов создаются и обновляются указанные ниже файлы.</span><span class="sxs-lookup"><span data-stu-id="58ff8-160">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="58ff8-161">Создаваемые файлы</span><span class="sxs-lookup"><span data-stu-id="58ff8-161">Files created</span></span>

* <span data-ttu-id="58ff8-162">*Pages/Movies*: Create, Delete, Details, Edit и Index.</span><span class="sxs-lookup"><span data-stu-id="58ff8-162">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="58ff8-163">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="58ff8-163">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="58ff8-164">Обновляемые файлы</span><span class="sxs-lookup"><span data-stu-id="58ff8-164">File updated</span></span>

* <span data-ttu-id="58ff8-165">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="58ff8-165">*Startup.cs*</span></span>

<span data-ttu-id="58ff8-166">В следующем разделе приводится описание созданных и обновленных файлов.</span><span class="sxs-lookup"><span data-stu-id="58ff8-166">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="58ff8-167">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="58ff8-167">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="58ff8-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58ff8-168">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="58ff8-169">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="58ff8-169">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="58ff8-170">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="58ff8-170">Add an initial migration.</span></span>
* <span data-ttu-id="58ff8-171">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="58ff8-171">Update the database with the initial migration.</span></span>

<span data-ttu-id="58ff8-172">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-172">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="58ff8-174">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="58ff8-174">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="58ff8-175">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="58ff8-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="58ff8-176">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="58ff8-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="58ff8-177">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-177">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="58ff8-178">Схема создается на основе модели, указанной в `DbContext` (в файле *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="58ff8-178">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="58ff8-179">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="58ff8-179">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="58ff8-180">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="58ff8-180">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="58ff8-181">Команда `ef database update` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="58ff8-181">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="58ff8-182">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-182">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="58ff8-183">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="58ff8-183">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="58ff8-184">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="58ff8-184">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="58ff8-185">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="58ff8-185">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="58ff8-186">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="58ff8-186">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="58ff8-187">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="58ff8-187">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="58ff8-188">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="58ff8-188">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="58ff8-189">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="58ff8-189">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="58ff8-190">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="58ff8-190">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="58ff8-191">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="58ff8-191">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="58ff8-192">`RazorPagesMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="58ff8-192">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="58ff8-193">Контекст данных (`RazorPagesMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="58ff8-193">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="58ff8-194">Контекст данных указывает сущности, которые включаются в модель данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-194">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="58ff8-195">Представленный выше код создает свойство [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="58ff8-195">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="58ff8-196">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-196">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="58ff8-197">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="58ff8-197">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="58ff8-198">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="58ff8-198">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="58ff8-199">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="58ff8-199">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="58ff8-200">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="58ff8-200">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="58ff8-201">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="58ff8-201">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="58ff8-202">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-202">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="58ff8-203">Схема создается на основе модели, указанной в `RazorPagesMovieContext` (в файле *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="58ff8-203">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="58ff8-204">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="58ff8-204">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="58ff8-205">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="58ff8-205">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="58ff8-206">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="58ff8-206">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="58ff8-207">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="58ff8-207">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="58ff8-208">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="58ff8-208">Test the app</span></span>

* <span data-ttu-id="58ff8-209">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="58ff8-209">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="58ff8-210">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="58ff8-210">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="58ff8-211">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="58ff8-211">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="58ff8-212">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-212">Test the **Create** link.</span></span>

  ![Страница "Создать"](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="58ff8-214">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="58ff8-214">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="58ff8-215">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="58ff8-215">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="58ff8-216">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="58ff8-216">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="58ff8-217">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="58ff8-217">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="58ff8-218">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="58ff8-218">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="58ff8-219">[Предыдущая статья. Начало работы](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="58ff8-219">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
