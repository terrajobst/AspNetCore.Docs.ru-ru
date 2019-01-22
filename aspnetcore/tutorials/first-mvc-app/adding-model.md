---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 062a248ffdf8e30ed01a72e0a555c1c9a1ab1b6d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341616"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="dc477-103">Добавление модели в приложение MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dc477-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="dc477-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="dc477-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="dc477-105">В этом разделе мы добавим классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="dc477-106">Эти классы будут представлять уровень **м**одели в приложении **M**VC.</span><span class="sxs-lookup"><span data-stu-id="dc477-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="dc477-107">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="dc477-108">EF Core —это платформа объектно реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="dc477-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="dc477-109">Создаваемые вами классы моделей называются классами POCO (от **P**lain **O**ld **C**LR **O** — старые добрые объекты CLR), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc477-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="dc477-110">Эти классы просто определяют свойства данных, которые будут храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="dc477-111">Работая с этим учебником, вы напишете классы моделей, а затем EF Core создаст базу данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="dc477-112">Другой вариант, который здесь не рассматривается: создание классов моделей из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="dc477-113">Дополнительные сведения об этом подходе см. в разделе [ASP.NET Core — существующая база данных](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="dc477-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="dc477-114">Добавление класса модели данных</span><span class="sxs-lookup"><span data-stu-id="dc477-114">Add a data model class</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc477-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc477-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc477-116">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="dc477-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="dc477-117">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="dc477-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="dc477-118">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dc477-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="dc477-119">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc477-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---  
<!-- End of VS tabs -->

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="dc477-120">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="dc477-120">Scaffold the movie model</span></span>

<span data-ttu-id="dc477-121">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="dc477-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="dc477-122">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="dc477-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc477-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc477-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc477-124">В **Обозревателе решений** щелкните правой кнопкой мыши папку *Контроллеры* и выберите **Добавить > Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="dc477-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller21.png)

<span data-ttu-id="dc477-126">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="dc477-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="dc477-128">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="dc477-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="dc477-129">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="dc477-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="dc477-130">**Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="dc477-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Добавление контекста данных](adding-model/_static/dc.png)

* <span data-ttu-id="dc477-132">**Представления:** оставьте флажки, установленные по умолчанию</span><span class="sxs-lookup"><span data-stu-id="dc477-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="dc477-133">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*)</span><span class="sxs-lookup"><span data-stu-id="dc477-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="dc477-134">Нажмите **Добавить**</span><span class="sxs-lookup"><span data-stu-id="dc477-134">Select **Add**</span></span>

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

<span data-ttu-id="dc477-136">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="dc477-136">Visual Studio creates:</span></span>

* <span data-ttu-id="dc477-137">[класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);</span><span class="sxs-lookup"><span data-stu-id="dc477-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="dc477-138">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="dc477-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="dc477-139">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (<em>Views/Movies/&ast;.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="dc477-139">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="dc477-140">Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="dc477-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="dc477-141">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="dc477-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="dc477-142">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="dc477-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="dc477-143">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="dc477-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="dc477-144">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="dc477-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="dc477-145">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dc477-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="dc477-146">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="dc477-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="dc477-147">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="dc477-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="dc477-148">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="dc477-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="dc477-149">Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:</span><span class="sxs-lookup"><span data-stu-id="dc477-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="dc477-150">Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="dc477-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="dc477-151">С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="dc477-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="dc477-152">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="dc477-152">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc477-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc477-153">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="dc477-154">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="dc477-154">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="dc477-155">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="dc477-155">Add an initial migration.</span></span>
* <span data-ttu-id="dc477-156">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="dc477-156">Update the database with the initial migration.</span></span>

<span data-ttu-id="dc477-157">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="dc477-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="dc477-159">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="dc477-159">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="dc477-160">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="dc477-161">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dc477-161">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]
<span data-ttu-id="dc477-162">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-162">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="dc477-163">В результате выполнения предыдущих команд выводится следующее предупреждение: "Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="dc477-163">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="dc477-164">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="dc477-164">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="dc477-165">С помощью метода 'HasColumnType()' явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="dc477-165">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="dc477-166">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="dc477-166">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="dc477-167">Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="dc477-167">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="dc477-168">Аргумент `InitialCreate` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="dc477-168">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="dc477-169">Можно использовать любое имя, однако по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="dc477-169">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="dc477-170">Команда `ef database update` выполняет метод `Up` в файле *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="dc477-170">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="dc477-171">Метод `Up` создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-171">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="dc477-172">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="dc477-172">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="dc477-173">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="dc477-173">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="dc477-174">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dc477-174">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="dc477-175">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="dc477-175">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="dc477-176">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="dc477-176">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="dc477-177">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="dc477-177">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="dc477-178">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="dc477-178">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="dc477-179">Проверьте метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="dc477-179">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="dc477-180">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="dc477-180">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="dc477-181">`MvcMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="dc477-181">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="dc477-182">Контекст данных (`MvcMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="dc477-182">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="dc477-183">Контекст данных указывает сущности, которые включаются в модель данных:</span><span class="sxs-lookup"><span data-stu-id="dc477-183">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="dc477-184">Представленный выше код создает свойство [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="dc477-184">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="dc477-185">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-185">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="dc477-186">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="dc477-186">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="dc477-187">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="dc477-187">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="dc477-188">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="dc477-188">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="dc477-189">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="dc477-189">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="dc477-190">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="dc477-190">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="dc477-191">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="dc477-191">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="dc477-192">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="dc477-192">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="dc477-193">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="dc477-193">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="dc477-194">Вы создаете контекст базы данных и регистрируете его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="dc477-194">You created a DB context and registered it with the dependency injection container.</span></span>

---

<span data-ttu-id="dc477-195">Схема создается на основе модели, указанной в `MvcMovieContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="dc477-195">The schema is based on the model specified in the `MvcMovieContext` (In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="dc477-196">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="dc477-196">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="dc477-197">Можно использовать любое имя, однако по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="dc477-197">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="dc477-198">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="dc477-198">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="dc477-199">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="dc477-199">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="dc477-200">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="dc477-200">Test the app</span></span>

* <span data-ttu-id="dc477-201">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="dc477-201">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="dc477-202">Если вы получите исключение базы данных, аналогичное следующему:</span><span class="sxs-lookup"><span data-stu-id="dc477-202">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="dc477-203">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="dc477-203">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="dc477-204">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="dc477-204">Test the **Create** link.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dc477-205">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="dc477-205">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="dc477-206">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="dc477-206">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="dc477-207">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="dc477-207">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="dc477-208">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="dc477-208">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="dc477-209">Проверьте класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="dc477-209">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="dc477-210">Выделенный выше код демонстрирует добавление контекста базы данных фильмов в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="dc477-210">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="dc477-211">`services.AddDbContext<MvcMovieContext>(options =>` задает используемую базу данных и строку подключения.</span><span class="sxs-lookup"><span data-stu-id="dc477-211">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="dc477-212">`=>` — это [лямбда-оператор](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="dc477-212">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="dc477-213">Откройте файл *Controllers/MoviesController.cs* и изучите его конструктор:</span><span class="sxs-lookup"><span data-stu-id="dc477-213">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="dc477-214">Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`MvcMovieContext `) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="dc477-214">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="dc477-215">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="dc477-215">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="dc477-216">Строго типизированные модели и ключевое слово @model</span><span class="sxs-lookup"><span data-stu-id="dc477-216">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="dc477-217">Ранее в этом руководстве вы ознакомились с передачей данных или объектов из контроллера в представление с использованием словаря `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="dc477-217">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="dc477-218">Словарь `ViewData` представляет собой динамический объект, который реализует удобный механизм позднего связывания для передачи информации в представление.</span><span class="sxs-lookup"><span data-stu-id="dc477-218">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="dc477-219">Модель MVC также поддерживает передачу строго типизированных объектов модели в представление.</span><span class="sxs-lookup"><span data-stu-id="dc477-219">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="dc477-220">Такой подход обеспечивает более эффективную проверку кода во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="dc477-220">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="dc477-221">При создании методов и представлений в этом подходе используется механизм формирования шаблонов (то есть передачи строго типизированной модели) с представлениями и классами `MoviesController`.</span><span class="sxs-lookup"><span data-stu-id="dc477-221">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="dc477-222">Изучите созданный метод `Details` в файле *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="dc477-222">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="dc477-223">Параметр `id` обычно передается в качестве данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="dc477-223">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="dc477-224">Например, `https://localhost:5001/movies/details/1` задает:</span><span class="sxs-lookup"><span data-stu-id="dc477-224">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="dc477-225">Контроллер `movies` (первый сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="dc477-225">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="dc477-226">Действие `details` (второй сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="dc477-226">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="dc477-227">Идентификатор 1 (последний сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="dc477-227">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="dc477-228">Также можно передать `id` с помощью строки запроса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="dc477-228">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="dc477-229">Параметр `id` определяется как [тип, допускающий значение NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`), в случае, если не указано значение идентификатора.</span><span class="sxs-lookup"><span data-stu-id="dc477-229">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="dc477-230">[Лямбда-выражение](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) передается в `FirstOrDefaultAsync` для выбора записей фильмов, которые соответствуют данным маршрута или значению строки запроса.</span><span class="sxs-lookup"><span data-stu-id="dc477-230">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="dc477-231">Если фильм найден, экземпляр модели `Movie` передается в представление `Details`:</span><span class="sxs-lookup"><span data-stu-id="dc477-231">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="dc477-232">Изучите содержимое файла *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="dc477-232">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="dc477-233">Указав оператор `@model` в начале файла представления, вы можете задать тип объекта, который будет ожидаться представлением.</span><span class="sxs-lookup"><span data-stu-id="dc477-233">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="dc477-234">При создании контроллера movie следующий оператор `@model` был автоматически добавлен в начало файла *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="dc477-234">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="dc477-235">Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="dc477-235">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="dc477-236">Например, в представлении *Details.cshtml* код передает каждое поле фильма во вспомогательные функции HTML `DisplayNameFor` и `DisplayFor` со строго типизированным объектом `Model`.</span><span class="sxs-lookup"><span data-stu-id="dc477-236">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="dc477-237">Методы `Create` и `Edit` и представления также передают объект модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="dc477-237">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="dc477-238">Изучите представление *Index.cshtml* и метод `Index` в контроллере Movies.</span><span class="sxs-lookup"><span data-stu-id="dc477-238">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="dc477-239">Обратите внимание на то, как в коде создается объект `List` при вызове метода `View`.</span><span class="sxs-lookup"><span data-stu-id="dc477-239">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="dc477-240">Код передает список `Movies` из метода действия `Index` в представление:</span><span class="sxs-lookup"><span data-stu-id="dc477-240">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="dc477-241">При создании контроллера movies механизм формирования шаблонов автоматически включает следующий оператор `@model` в начало файла *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="dc477-241">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="dc477-242">Эта директива `@model` обеспечивает доступ к списку фильмов, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="dc477-242">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="dc477-243">Например, в представлении *Index.cshtml* код циклически перебирает фильмы с использованием оператора `foreach` для строго типизированного объекта `Model`:</span><span class="sxs-lookup"><span data-stu-id="dc477-243">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="dc477-244">Поскольку объект `Model` является строго типизированным (как и объект `IEnumerable<Movie>`), каждый элемент в цикле получает тип `Movie`.</span><span class="sxs-lookup"><span data-stu-id="dc477-244">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="dc477-245">Помимо прочих преимуществ, это означает, что выполняется проверка кода во время компиляции:</span><span class="sxs-lookup"><span data-stu-id="dc477-245">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dc477-246">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="dc477-246">Additional resources</span></span>

* [<span data-ttu-id="dc477-247">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="dc477-247">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="dc477-248">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="dc477-248">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="dc477-249">[Назад: добавление представления](adding-view.md)
> [Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="dc477-249">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
