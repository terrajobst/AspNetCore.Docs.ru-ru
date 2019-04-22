---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
ms.author: riande
ms.date: 02/25/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 08d8e9679bfee11f03e61cb4b9ae9b5b36186049
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59012829"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="60b4b-103">Добавление модели в приложение MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60b4b-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="60b4b-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="60b4b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="60b4b-105">В этом разделе мы добавим классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="60b4b-106">Эти классы будут представлять уровень **м**одели в приложении **M**VC.</span><span class="sxs-lookup"><span data-stu-id="60b4b-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="60b4b-107">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="60b4b-108">EF Core —это платформа объектно реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="60b4b-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="60b4b-109">Создаваемые вами классы моделей называются классами POCO (от **P**lain **O**ld **C**LR **O** — старые добрые объекты CLR), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="60b4b-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="60b4b-110">Эти классы просто определяют свойства данных, которые будут храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="60b4b-111">Работая с этим учебником, вы напишете классы моделей, а затем EF Core создаст базу данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="60b4b-112">Другой вариант, который здесь не рассматривается: создание классов моделей из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="60b4b-113">Дополнительные сведения об этом подходе см. в разделе [ASP.NET Core — существующая база данных](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="60b4b-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="60b4b-114">Добавление класса модели данных</span><span class="sxs-lookup"><span data-stu-id="60b4b-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60b4b-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60b4b-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60b4b-116">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="60b4b-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="60b4b-117">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="60b4b-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60b4b-118">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="60b4b-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="60b4b-119">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="60b4b-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="60b4b-120">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="60b4b-120">Scaffold the movie model</span></span>

<span data-ttu-id="60b4b-121">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="60b4b-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="60b4b-122">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="60b4b-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60b4b-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60b4b-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60b4b-124">В **Обозревателе решений** щелкните правой кнопкой мыши папку *Контроллеры* и выберите **Добавить > Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="60b4b-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller21.png)

<span data-ttu-id="60b4b-126">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="60b4b-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="60b4b-128">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="60b4b-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="60b4b-129">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="60b4b-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="60b4b-130">**Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="60b4b-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Добавление контекста данных](adding-model/_static/dc.png)

* <span data-ttu-id="60b4b-132">**Представления:** оставьте флажки, установленные по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60b4b-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="60b4b-133">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*)</span><span class="sxs-lookup"><span data-stu-id="60b4b-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="60b4b-134">Нажмите **Добавить**</span><span class="sxs-lookup"><span data-stu-id="60b4b-134">Select **Add**</span></span>

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

<span data-ttu-id="60b4b-136">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="60b4b-136">Visual Studio creates:</span></span>

* <span data-ttu-id="60b4b-137">[класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);</span><span class="sxs-lookup"><span data-stu-id="60b4b-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="60b4b-138">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="60b4b-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="60b4b-139">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/\*.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="60b4b-139">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="60b4b-140">Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="60b4b-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="60b4b-141">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="60b4b-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="60b4b-142">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="60b4b-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="60b4b-143">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="60b4b-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="60b4b-144">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="60b4b-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="60b4b-145">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="60b4b-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="60b4b-146">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="60b4b-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="60b4b-147">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="60b4b-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="60b4b-148">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="60b4b-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="60b4b-149">Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:</span><span class="sxs-lookup"><span data-stu-id="60b4b-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="60b4b-150">Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="60b4b-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="60b4b-151">С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="60b4b-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="60b4b-152">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="60b4b-152">Initial migration</span></span>

<span data-ttu-id="60b4b-153">В этом разделе выполняются следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="60b4b-153">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="60b4b-154">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="60b4b-154">Add an initial migration.</span></span>
* <span data-ttu-id="60b4b-155">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="60b4b-155">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60b4b-156">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60b4b-156">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="60b4b-157">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов** (PMC).</span><span class="sxs-lookup"><span data-stu-id="60b4b-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Меню PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="60b4b-159">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="60b4b-159">In the PMC, enter the following commands:</span></span>

   ```console
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="60b4b-160">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="60b4b-161">Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="60b4b-161">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="60b4b-162">Аргумент `Initial` — это имя миграции.</span><span class="sxs-lookup"><span data-stu-id="60b4b-162">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="60b4b-163">Можно использовать любое имя, но по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="60b4b-163">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="60b4b-164">Для получения дополнительной информации см. <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="60b4b-164">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="60b4b-165">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-165">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60b4b-166">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="60b4b-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="60b4b-167">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-167">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="60b4b-168">Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="60b4b-168">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="60b4b-169">Аргумент `InitialCreate` — это имя миграции.</span><span class="sxs-lookup"><span data-stu-id="60b4b-169">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="60b4b-170">Можно использовать любое имя, но по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="60b4b-170">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="60b4b-171">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="60b4b-171">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="60b4b-172">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="60b4b-172">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="60b4b-173">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="60b4b-173">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="60b4b-174">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="60b4b-174">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="60b4b-175">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="60b4b-175">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="60b4b-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60b4b-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="60b4b-177">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="60b4b-177">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="60b4b-178">Рассмотрим следующий метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-178">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="60b4b-179">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="60b4b-179">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="60b4b-180">`MvcMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-180">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="60b4b-181">Контекст данных (`MvcMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="60b4b-181">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="60b4b-182">Контекст данных указывает сущности, которые включаются в модель данных:</span><span class="sxs-lookup"><span data-stu-id="60b4b-182">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="60b4b-183">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="60b4b-183">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="60b4b-184">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="60b4b-184">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="60b4b-185">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="60b4b-185">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="60b4b-186">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="60b4b-186">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="60b4b-187">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="60b4b-187">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="60b4b-188">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="60b4b-188">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="60b4b-189">Вы создаете контекст базы данных и регистрируете его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="60b4b-189">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="60b4b-190">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="60b4b-190">Test the app</span></span>

* <span data-ttu-id="60b4b-191">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="60b4b-191">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="60b4b-192">Если вы получите исключение базы данных, аналогичное следующему:</span><span class="sxs-lookup"><span data-stu-id="60b4b-192">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="60b4b-193">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="60b4b-193">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="60b4b-194">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="60b4b-194">Test the **Create** link.</span></span> <span data-ttu-id="60b4b-195">Введите и отправьте данные.</span><span class="sxs-lookup"><span data-stu-id="60b4b-195">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="60b4b-196">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="60b4b-196">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="60b4b-197">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="60b4b-197">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="60b4b-198">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="60b4b-198">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="60b4b-199">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="60b4b-199">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="60b4b-200">Проверьте класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="60b4b-200">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="60b4b-201">Выделенный выше код демонстрирует добавление контекста базы данных фильмов в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="60b4b-201">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="60b4b-202">`services.AddDbContext<MvcMovieContext>(options =>` задает используемую базу данных и строку подключения.</span><span class="sxs-lookup"><span data-stu-id="60b4b-202">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="60b4b-203">`=>` — это [лямбда-оператор](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="60b4b-203">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="60b4b-204">Откройте файл *Controllers/MoviesController.cs* и изучите его конструктор:</span><span class="sxs-lookup"><span data-stu-id="60b4b-204">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="60b4b-205">Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`MvcMovieContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="60b4b-205">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="60b4b-206">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="60b4b-206">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="60b4b-207">Строго типизированные модели и ключевое слово @model</span><span class="sxs-lookup"><span data-stu-id="60b4b-207">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="60b4b-208">Ранее в этом руководстве вы ознакомились с передачей данных или объектов из контроллера в представление с использованием словаря `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-208">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="60b4b-209">Словарь `ViewData` представляет собой динамический объект, который реализует удобный механизм позднего связывания для передачи информации в представление.</span><span class="sxs-lookup"><span data-stu-id="60b4b-209">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="60b4b-210">Модель MVC также поддерживает передачу строго типизированных объектов модели в представление.</span><span class="sxs-lookup"><span data-stu-id="60b4b-210">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="60b4b-211">Такой подход обеспечивает более эффективную проверку кода во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="60b4b-211">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="60b4b-212">При создании методов и представлений в этом подходе используется механизм формирования шаблонов (то есть передачи строго типизированной модели) с представлениями и классами `MoviesController`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-212">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="60b4b-213">Изучите созданный метод `Details` в файле *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="60b4b-213">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="60b4b-214">Параметр `id` обычно передается в качестве данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="60b4b-214">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="60b4b-215">Например, `https://localhost:5001/movies/details/1` задает:</span><span class="sxs-lookup"><span data-stu-id="60b4b-215">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="60b4b-216">Контроллер `movies` (первый сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="60b4b-216">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="60b4b-217">Действие `details` (второй сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="60b4b-217">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="60b4b-218">Идентификатор 1 (последний сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="60b4b-218">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="60b4b-219">Также можно передать `id` с помощью строки запроса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="60b4b-219">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="60b4b-220">Параметр `id` определяется как [тип, допускающий значение NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`), в случае, если не указано значение идентификатора.</span><span class="sxs-lookup"><span data-stu-id="60b4b-220">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="60b4b-221">[Лямбда-выражение](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) передается в `FirstOrDefaultAsync` для выбора записей фильмов, которые соответствуют данным маршрута или значению строки запроса.</span><span class="sxs-lookup"><span data-stu-id="60b4b-221">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="60b4b-222">Если фильм найден, экземпляр модели `Movie` передается в представление `Details`:</span><span class="sxs-lookup"><span data-stu-id="60b4b-222">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="60b4b-223">Изучите содержимое файла *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="60b4b-223">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="60b4b-224">Указав оператор `@model` в начале файла представления, вы можете задать тип объекта, который будет ожидаться представлением.</span><span class="sxs-lookup"><span data-stu-id="60b4b-224">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="60b4b-225">При создании контроллера movie следующий оператор `@model` был автоматически добавлен в начало файла *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="60b4b-225">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="60b4b-226">Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-226">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="60b4b-227">Например, в представлении *Details.cshtml* код передает каждое поле фильма во вспомогательные функции HTML `DisplayNameFor` и `DisplayFor` со строго типизированным объектом `Model`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-227">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="60b4b-228">Методы `Create` и `Edit` и представления также передают объект модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-228">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="60b4b-229">Изучите представление *Index.cshtml* и метод `Index` в контроллере Movies.</span><span class="sxs-lookup"><span data-stu-id="60b4b-229">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="60b4b-230">Обратите внимание на то, как в коде создается объект `List` при вызове метода `View`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-230">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="60b4b-231">Код передает список `Movies` из метода действия `Index` в представление:</span><span class="sxs-lookup"><span data-stu-id="60b4b-231">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="60b4b-232">При создании контроллера movies механизм формирования шаблонов автоматически включает следующий оператор `@model` в начало файла *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="60b4b-232">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="60b4b-233">Эта директива `@model` обеспечивает доступ к списку фильмов, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-233">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="60b4b-234">Например, в представлении *Index.cshtml* код циклически перебирает фильмы с использованием оператора `foreach` для строго типизированного объекта `Model`:</span><span class="sxs-lookup"><span data-stu-id="60b4b-234">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="60b4b-235">Поскольку объект `Model` является строго типизированным (как и объект `IEnumerable<Movie>`), каждый элемент в цикле получает тип `Movie`.</span><span class="sxs-lookup"><span data-stu-id="60b4b-235">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="60b4b-236">Помимо прочих преимуществ, это означает, что выполняется проверка кода во время компиляции:</span><span class="sxs-lookup"><span data-stu-id="60b4b-236">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="60b4b-237">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="60b4b-237">Additional resources</span></span>

* [<span data-ttu-id="60b4b-238">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="60b4b-238">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="60b4b-239">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="60b4b-239">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="60b4b-240">[Назад: добавление представления](adding-view.md)
> [Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="60b4b-240">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>
