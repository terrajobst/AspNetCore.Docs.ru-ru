---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 2fac37e7069fb2a464d4de1da8912197f7adf8a8
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2019
ms.locfileid: "73761091"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="0b9c0-103">Добавление модели в приложение MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b9c0-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="0b9c0-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)</span><span class="sxs-lookup"><span data-stu-id="0b9c0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0b9c0-105">В этом разделе мы добавим классы для управления фильмами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="0b9c0-106">Эти классы будут представлять уровень **м**одели в приложении **M**VC.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="0b9c0-107">Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="0b9c0-108">EF Core —это платформа объектно реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="0b9c0-109">Создаваемые вами классы моделей называются классами POCO (от **P**lain **O**ld **C**LR **O** — старые добрые объекты CLR), так как они не зависят от EF Core.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="0b9c0-110">Эти классы просто определяют свойства данных, которые будут храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="0b9c0-111">Работая с этим учебником, вы напишете классы моделей, а затем EF Core создаст базу данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="0b9c0-112">Другой вариант, который здесь не рассматривается: создание классов моделей из существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="0b9c0-113">Дополнительные сведения об этом подходе см. в разделе [ASP.NET Core — существующая база данных](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="0b9c0-114">Добавление класса модели данных</span><span class="sxs-lookup"><span data-stu-id="0b9c0-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-116">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="0b9c0-117">Назовите файл *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-118">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="0b9c0-119">Добавьте файл *Movie.cs* в папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

---

<span data-ttu-id="0b9c0-120">Обновите файл *Movie.cs*, используя следующий код:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-120">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="0b9c0-121">Класс `Movie` содержит поле `Id`, которое требуется базе данных в качестве первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-121">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="0b9c0-122">Атрибут [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) для `ReleaseDate` указывает тип данных (`Date`).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-122">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="0b9c0-123">С этим атрибутом:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-123">With this attribute:</span></span>

  * <span data-ttu-id="0b9c0-124">пользователю не требуется вводить сведения о времени в поле даты.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-124">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="0b9c0-125">Отображается только дата, а не время.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-125">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="0b9c0-126">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) рассматриваются в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-126">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="0b9c0-127">Добавление пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="0b9c0-127">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-128">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-129">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов** (PMC).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-129">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Меню PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0b9c0-131">В PMC выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-131">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="0b9c0-132">Приведенная выше команда добавляет поставщик EF Core для SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-132">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="0b9c0-133">Пакет поставщика устанавливает пакет EF Core в качестве зависимости.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-133">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="0b9c0-134">Дополнительные пакеты устанавливаются автоматически на этапе формирования шаблонов далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-134">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-135">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-135">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="0b9c0-136">Создание класса контекста для базы данных</span><span class="sxs-lookup"><span data-stu-id="0b9c0-136">Create a database context class</span></span>

<span data-ttu-id="0b9c0-137">Класс контекста базы данных необходим в целях координации функциональных возможностей EF Core (создание, чтение, обновление, удаление) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-137">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="0b9c0-138">Контекст базы данных наследуется от [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) и определяет сущности, которые необходимо включить в модель данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-138">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="0b9c0-139">Создайте папку *Data*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-139">Create a *Data* folder.</span></span>

<span data-ttu-id="0b9c0-140">Добавьте файл *Data/MvcMovieContext.cs* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-140">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="0b9c0-141">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-141">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="0b9c0-142">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-142">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="0b9c0-143">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-143">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="0b9c0-144">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="0b9c0-144">Register the database context</span></span>

<span data-ttu-id="0b9c0-145">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-145">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0b9c0-146">Службы (например, контекст базы данных EF Core) должны регистрироваться с помощью внедрения зависимостей во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-146">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="0b9c0-147">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-147">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="0b9c0-148">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-148">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="0b9c0-149">В этом разделе контекст базы данных регистрируется в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-149">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="0b9c0-150">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-150">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="0b9c0-151">Добавьте выделенный ниже код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-151">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-152">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-152">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-153">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-153">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="0b9c0-154">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-154">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="0b9c0-155">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-155">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="0b9c0-156">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="0b9c0-156">Add a database connection string</span></span>

<span data-ttu-id="0b9c0-157">Добавьте строку подключения в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-157">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-158">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-159">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-159">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="0b9c0-160">Выполните сборку проекта, чтобы проверить его на ошибки компиляции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-160">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="0b9c0-161">Формирования шаблона страниц фильмов</span><span class="sxs-lookup"><span data-stu-id="0b9c0-161">Scaffold movie pages</span></span>

<span data-ttu-id="0b9c0-162">Используйте средство формирования шаблонов, чтобы создать страницы для операций создания, чтения, обновления и удаления (CRUD) для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-162">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-163">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-164">В **Обозревателе решений** щелкните правой кнопкой мыши папку *Контроллеры* и выберите **Добавить > Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-164">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller21.png)

<span data-ttu-id="0b9c0-166">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-166">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="0b9c0-168">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-168">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="0b9c0-169">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="0b9c0-169">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="0b9c0-170">**Класс контекста данных:** *MvcMovieContext (MvcMovie.Data)*</span><span class="sxs-lookup"><span data-stu-id="0b9c0-170">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Добавление контекста данных](adding-model/_static/dc3.png)

* <span data-ttu-id="0b9c0-172">**Представления:** оставьте флажки, установленные по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0b9c0-172">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="0b9c0-173">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*)</span><span class="sxs-lookup"><span data-stu-id="0b9c0-173">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="0b9c0-174">Нажмите **Добавить**</span><span class="sxs-lookup"><span data-stu-id="0b9c0-174">Select **Add**</span></span>

<span data-ttu-id="0b9c0-175">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-175">Visual Studio creates:</span></span>

* <span data-ttu-id="0b9c0-176">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="0b9c0-176">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="0b9c0-177">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/\*.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-177">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="0b9c0-178">Автоматическое создание этих файлов называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-178">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0b9c0-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0b9c0-179">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="0b9c0-180">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-180">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="0b9c0-181">В Linux экспортируйте путь к средству формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-181">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="0b9c0-182">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-182">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0b9c0-183">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0b9c0-184">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-184">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="0b9c0-185">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-185">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="0b9c0-186">Сформированные страницы пока использовать нельзя, так как база данных не существует.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-186">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="0b9c0-187">Если запустить приложение и щелкнуть ссылку **Movie App**, появится сообщение об ошибке *Невозможно открыть базу данных* или *отсутствует таблица: Movie*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-187">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="0b9c0-188">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="0b9c0-188">Initial migration</span></span>

<span data-ttu-id="0b9c0-189">Создайте базу данных с помощью функции [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-189">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="0b9c0-190">Миграции — это набор средств, позволяющих создавать и обновлять базы данных в соответствии с моделью данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-190">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-191">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-192">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов** (PMC).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-192">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="0b9c0-193">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-193">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="0b9c0-194">`Add-Migration InitialCreate`. Создает файл миграции *Migrations/{метка_времени}_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-194">`Add-Migration InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="0b9c0-195">Аргумент `InitialCreate` — это имя миграции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-195">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="0b9c0-196">Можно использовать любое имя, но по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-196">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="0b9c0-197">Так как это первая миграция, созданный класс содержит код для создания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-197">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="0b9c0-198">Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-198">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="0b9c0-199">`Update-Database`. Обновляет базу данных до последней миграции, созданной предыдущей командой.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-199">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="0b9c0-200">Эта команда выполняет метод `Up` в файле *Migrations/{метка_времени}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-200">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="0b9c0-201">Команда обновления базы данных выдает следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-201">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="0b9c0-202">"Для десятичного столбца Price в типе сущности Movie не указан тип.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-202">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="0b9c0-203">Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-203">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="0b9c0-204">С помощью метода HasColumnType() явно укажите тип столбца SQL Server, который может вместить все значения".</span><span class="sxs-lookup"><span data-stu-id="0b9c0-204">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="0b9c0-205">Это предупреждение можно игнорировать. Оно будет устранено в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-205">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-206">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-206">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="0b9c0-207">Выполните следующие команды интерфейса командной строки .NET Core:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-207">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="0b9c0-208">`ef migrations add InitialCreate`. Создает файл миграции *Migrations/{метка_времени}_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-208">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="0b9c0-209">Аргумент `InitialCreate` — это имя миграции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-209">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="0b9c0-210">Можно использовать любое имя, но по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-210">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="0b9c0-211">Так как это первая миграция, созданный класс содержит код для создания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-211">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="0b9c0-212">Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-212">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="0b9c0-213">`ef database update`. Обновляет базу данных до последней миграции, созданной предыдущей командой.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-213">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="0b9c0-214">Эта команда выполняет метод `Up` в файле *Migrations/{метка_времени}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-214">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="0b9c0-215">Класс InitialCreate</span><span class="sxs-lookup"><span data-stu-id="0b9c0-215">The InitialCreate class</span></span>

<span data-ttu-id="0b9c0-216">Изучите файл миграции *Migrations/{метка_времени}_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-216">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="0b9c0-217">Метод `Up` создает таблицу Movie и настраивает `Id` в качестве первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-217">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="0b9c0-218">Метод `Down` отменяет изменения схемы, внесенные миграцией `Up`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-218">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="0b9c0-219">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="0b9c0-219">Test the app</span></span>

* <span data-ttu-id="0b9c0-220">Запустите приложение и щелкните ссылку **Movie App**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-220">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="0b9c0-221">Если возникнет исключение наподобие следующего:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-221">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-222">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-222">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-223">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="0b9c0-224">возможно, вы пропустили [шаг миграции](#migration).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-224">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="0b9c0-225">Протестируйте страницу **создания**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-225">Test the **Create** page.</span></span> <span data-ttu-id="0b9c0-226">Введите и отправьте данные.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-226">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="0b9c0-227">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-227">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="0b9c0-228">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-228">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="0b9c0-229">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-229">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="0b9c0-230">Протестируйте страницы **редактирования**, **сведений** и **удаления**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-230">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="0b9c0-231">Внедрение зависимостей в контроллере</span><span class="sxs-lookup"><span data-stu-id="0b9c0-231">Dependency injection in the controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-232">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-233">Откройте файл *Controllers/MoviesController.cs* и изучите его конструктор:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-233">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="0b9c0-234">Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`MvcMovieContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-234">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="0b9c0-235">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-235">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-236">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="0b9c0-237">Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`MvcMovieContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-237">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="0b9c0-238">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-238">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="0b9c0-239">Строго типизированные модели и ключевое слово @model</span><span class="sxs-lookup"><span data-stu-id="0b9c0-239">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="0b9c0-240">Ранее в этом руководстве вы ознакомились с передачей данных или объектов из контроллера в представление с использованием словаря `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-240">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="0b9c0-241">Словарь `ViewData` представляет собой динамический объект, который реализует удобный механизм позднего связывания для передачи информации в представление.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-241">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="0b9c0-242">Модель MVC также поддерживает передачу строго типизированных объектов модели в представление.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-242">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="0b9c0-243">Такой подход обеспечивает проверку кода во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-243">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="0b9c0-244">В этом подходе используется механизм формирования шаблонов (то есть передачи строго типизированной модели) с представлениями и классом `MoviesController`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-244">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="0b9c0-245">Изучите созданный метод `Details` в файле *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-245">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="0b9c0-246">Параметр `id` обычно передается в качестве данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-246">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="0b9c0-247">Например, `https://localhost:5001/movies/details/1` задает:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-247">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="0b9c0-248">Контроллер `movies` (первый сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-248">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="0b9c0-249">Действие `details` (второй сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-249">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="0b9c0-250">Идентификатор 1 (последний сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-250">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="0b9c0-251">Также можно передать `id` с помощью строки запроса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-251">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="0b9c0-252">Параметр `id` определяется как [тип, допускающий значение NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`), в случае, если не указано значение идентификатора.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-252">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="0b9c0-253">[Лямбда-выражение](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) передается в `FirstOrDefaultAsync` для выбора записей фильмов, которые соответствуют данным маршрута или значению строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-253">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="0b9c0-254">Если фильм найден, экземпляр модели `Movie` передается в представление `Details`:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-254">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="0b9c0-255">Изучите содержимое файла *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-255">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="0b9c0-256">Оператор `@model` в начале файла представления задает тип объекта, который будет ожидаться представлением.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-256">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="0b9c0-257">При создании контроллера movie был включен следующий оператор `@model`:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-257">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="0b9c0-258">Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-258">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="0b9c0-259">Объект `Model` является строго типизированным.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-259">The `Model` object is strongly typed.</span></span> <span data-ttu-id="0b9c0-260">Например, в представлении *Details.cshtml* код передает каждое поле фильма во вспомогательные функции HTML `DisplayNameFor` и `DisplayFor` со строго типизированным объектом `Model`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-260">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="0b9c0-261">Методы `Create` и `Edit` и представления также передают объект модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-261">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="0b9c0-262">Изучите представление *Index.cshtml* и метод `Index` в контроллере Movies.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-262">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="0b9c0-263">Обратите внимание на то, как в коде создается объект `List` при вызове метода `View`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-263">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="0b9c0-264">Код передает список `Movies` из метода действия `Index` в представление:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-264">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="0b9c0-265">При создании контроллера movies механизм формирования шаблонов включил следующий оператор `@model` в начало файла *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-265">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="0b9c0-266">Эта директива `@model` обеспечивает доступ к списку фильмов, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-266">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="0b9c0-267">Например, в представлении *Index.cshtml* код циклически перебирает фильмы с использованием оператора `foreach` для строго типизированного объекта `Model`:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-267">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="0b9c0-268">Поскольку объект `Model` является строго типизированным (как и объект `IEnumerable<Movie>`), каждый элемент в цикле получает тип `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-268">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="0b9c0-269">Помимо прочих преимуществ, это означает, что выполняется проверка кода во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-269">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b9c0-270">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0b9c0-270">Additional resources</span></span>

* [<span data-ttu-id="0b9c0-271">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="0b9c0-271">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="0b9c0-272">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="0b9c0-272">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="0b9c0-273">[Назад: добавление представления](adding-view.md)
> [Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="0b9c0-273">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="0b9c0-274">Добавление класса модели данных</span><span class="sxs-lookup"><span data-stu-id="0b9c0-274">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-275">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-275">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-276">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-276">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="0b9c0-277">Присвойте классу имя **Movie**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-277">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-278">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-278">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="0b9c0-279">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-279">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="0b9c0-280">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="0b9c0-280">Scaffold the movie model</span></span>

<span data-ttu-id="0b9c0-281">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-281">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="0b9c0-282">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-282">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-284">В **Обозревателе решений** щелкните правой кнопкой мыши папку *Контроллеры* и выберите **Добавить > Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-284">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller21.png)

<span data-ttu-id="0b9c0-286">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-286">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="0b9c0-288">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-288">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="0b9c0-289">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="0b9c0-289">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="0b9c0-290">**Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-290">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Добавление контекста данных](adding-model/_static/dc.png)

* <span data-ttu-id="0b9c0-292">**Представления:** оставьте флажки, установленные по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0b9c0-292">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="0b9c0-293">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*)</span><span class="sxs-lookup"><span data-stu-id="0b9c0-293">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="0b9c0-294">Нажмите **Добавить**</span><span class="sxs-lookup"><span data-stu-id="0b9c0-294">Select **Add**</span></span>

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

<span data-ttu-id="0b9c0-296">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-296">Visual Studio creates:</span></span>

* <span data-ttu-id="0b9c0-297">[класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);</span><span class="sxs-lookup"><span data-stu-id="0b9c0-297">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="0b9c0-298">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="0b9c0-298">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="0b9c0-299">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/\*.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-299">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="0b9c0-300">Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-300">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0b9c0-301">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0b9c0-301">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="0b9c0-302">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-302">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0b9c0-303">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-303">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="0b9c0-304">В Linux экспортируйте путь к средству формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-304">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="0b9c0-305">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-305">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="0b9c0-306">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-306">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="0b9c0-307">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-307">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0b9c0-308">Установка средства формирования шаблонов:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-308">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="0b9c0-309">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-309">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="0b9c0-310">Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-310">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-311">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-312">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-312">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="0b9c0-313">Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-313">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="0b9c0-314">С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-314">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="0b9c0-315">Первоначальная миграция</span><span class="sxs-lookup"><span data-stu-id="0b9c0-315">Initial migration</span></span>

<span data-ttu-id="0b9c0-316">В этом разделе выполняются следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-316">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="0b9c0-317">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-317">Add an initial migration.</span></span>
* <span data-ttu-id="0b9c0-318">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-318">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-319">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-319">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0b9c0-320">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов** (PMC).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-320">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Меню PMC](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="0b9c0-322">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-322">In the PMC, enter the following commands:</span></span>

   ```PMC
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="0b9c0-323">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-323">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="0b9c0-324">Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-324">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="0b9c0-325">Аргумент `Initial` — это имя миграции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-325">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="0b9c0-326">Можно использовать любое имя, но по соглашению используется имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-326">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="0b9c0-327">Дополнительные сведения можно найти по адресу: <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-327">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="0b9c0-328">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-328">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-329">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-329">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="0b9c0-330">Команда `ef migrations add InitialCreate` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-330">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="0b9c0-331">Схема базы данных создается на основе модели, указанной в классе `MvcMovieContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-331">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="0b9c0-332">Аргумент `InitialCreate` — это имя миграции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-332">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="0b9c0-333">Можно использовать любое имя, но по соглашению выбирается имя, которое описывает миграцию.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-333">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="0b9c0-334">Проверка контекста, зарегистрированного с помощью внедрения зависимостей</span><span class="sxs-lookup"><span data-stu-id="0b9c0-334">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="0b9c0-335">ASP.NET Core поддерживает [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-335">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="0b9c0-336">С помощью внедрения зависимостей службы (например, контекст базы данных EF Core) регистрируются во время запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-336">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="0b9c0-337">Затем компоненты, которые используют эти службы (например, Razor Pages), обращаются к ним через параметры конструктора.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-337">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="0b9c0-338">Код конструктора, который получает экземпляр контекста базы данных, приведен далее в этом руководстве.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-338">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0b9c0-339">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0b9c0-339">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="0b9c0-340">Средство формирования шаблонов автоматически создает контекст базы данных и регистрирует его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-340">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="0b9c0-341">Рассмотрим следующий метод `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-341">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="0b9c0-342">Средством формирования шаблонов была добавлена выделенная строка:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-342">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="0b9c0-343">`MvcMovieContext` координирует функции EF Core (Create, Read, Update, Delete и т. д.) для модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-343">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="0b9c0-344">Контекст данных (`MvcMovieContext`) получен из [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-344">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="0b9c0-345">Контекст данных указывает сущности, которые включаются в модель данных:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-345">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="0b9c0-346">Представленный выше код создает свойство [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-346">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="0b9c0-347">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-347">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="0b9c0-348">Сущность соответствует строке в таблице.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-348">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="0b9c0-349">Имя строки подключения передается в контекст путем вызова метода для объекта [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-349">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="0b9c0-350">При локальной разработке [система конфигурации ASP.NET Core](xref:fundamentals/configuration/index) считывает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-350">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="0b9c0-351">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="0b9c0-351">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="0b9c0-352">Вы создаете контекст базы данных и регистрируете его с использованием контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-352">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="0b9c0-353">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="0b9c0-353">Test the app</span></span>

* <span data-ttu-id="0b9c0-354">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-354">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="0b9c0-355">Если вы получите исключение базы данных, аналогичное следующему:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-355">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="0b9c0-356">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-356">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="0b9c0-357">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-357">Test the **Create** link.</span></span> <span data-ttu-id="0b9c0-358">Введите и отправьте данные.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-358">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="0b9c0-359">В поле `Price` нельзя вводить десятичные запятые.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-359">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="0b9c0-360">Чтобы обеспечить поддержку [проверки jQuery](https://jqueryvalidation.org/) для других языков, кроме английского, используйте вместо десятичной точки запятую (,), а для отображения данных в форматах для других языков, кроме английского, выполните глобализацию приложения.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-360">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="0b9c0-361">Инструкции по глобализации см. на [сайте GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-361">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="0b9c0-362">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-362">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="0b9c0-363">Проверьте класс `Startup`:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-363">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="0b9c0-364">Выделенный выше код демонстрирует добавление контекста базы данных фильмов в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="0b9c0-364">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="0b9c0-365">`services.AddDbContext<MvcMovieContext>(options =>` задает используемую базу данных и строку подключения.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-365">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="0b9c0-366">`=>` — это [лямбда-оператор](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="0b9c0-366">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="0b9c0-367">Откройте файл *Controllers/MoviesController.cs* и изучите его конструктор:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-367">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="0b9c0-368">Этот конструктор применяет [внедрение зависимостей](xref:fundamentals/dependency-injection) для внедрения контекста базы данных (`MvcMovieContext`) в контроллер.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-368">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="0b9c0-369">Контекст базы данных используется в каждом методе [создания, чтения, обновления и удаления](https://wikipedia.org/wiki/Create,_read,_update_and_delete) в контроллере.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-369">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="0b9c0-370">Строго типизированные модели и ключевое слово @model</span><span class="sxs-lookup"><span data-stu-id="0b9c0-370">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="0b9c0-371">Ранее в этом руководстве вы ознакомились с передачей данных или объектов из контроллера в представление с использованием словаря `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-371">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="0b9c0-372">Словарь `ViewData` представляет собой динамический объект, который реализует удобный механизм позднего связывания для передачи информации в представление.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-372">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="0b9c0-373">Модель MVC также поддерживает передачу строго типизированных объектов модели в представление.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-373">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="0b9c0-374">Такой подход обеспечивает более эффективную проверку кода во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-374">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="0b9c0-375">При создании методов и представлений в этом подходе используется механизм формирования шаблонов (то есть передачи строго типизированной модели) с представлениями и классами `MoviesController`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-375">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="0b9c0-376">Изучите созданный метод `Details` в файле *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-376">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="0b9c0-377">Параметр `id` обычно передается в качестве данных маршрута.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-377">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="0b9c0-378">Например, `https://localhost:5001/movies/details/1` задает:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-378">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="0b9c0-379">Контроллер `movies` (первый сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-379">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="0b9c0-380">Действие `details` (второй сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-380">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="0b9c0-381">Идентификатор 1 (последний сегмент URL-адреса).</span><span class="sxs-lookup"><span data-stu-id="0b9c0-381">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="0b9c0-382">Также можно передать `id` с помощью строки запроса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-382">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="0b9c0-383">Параметр `id` определяется как [тип, допускающий значение NULL](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`), в случае, если не указано значение идентификатора.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-383">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="0b9c0-384">[Лямбда-выражение](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) передается в `FirstOrDefaultAsync` для выбора записей фильмов, которые соответствуют данным маршрута или значению строки запроса.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-384">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="0b9c0-385">Если фильм найден, экземпляр модели `Movie` передается в представление `Details`:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-385">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="0b9c0-386">Изучите содержимое файла *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-386">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="0b9c0-387">Указав оператор `@model` в начале файла представления, вы можете задать тип объекта, который будет ожидаться представлением.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-387">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="0b9c0-388">При создании контроллера movie следующий оператор `@model` был автоматически добавлен в начало файла *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-388">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="0b9c0-389">Эта директива `@model` обеспечивает доступ к фильму, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-389">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="0b9c0-390">Например, в представлении *Details.cshtml* код передает каждое поле фильма во вспомогательные функции HTML `DisplayNameFor` и `DisplayFor` со строго типизированным объектом `Model`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-390">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="0b9c0-391">Методы `Create` и `Edit` и представления также передают объект модели `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-391">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="0b9c0-392">Изучите представление *Index.cshtml* и метод `Index` в контроллере Movies.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-392">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="0b9c0-393">Обратите внимание на то, как в коде создается объект `List` при вызове метода `View`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-393">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="0b9c0-394">Код передает список `Movies` из метода действия `Index` в представление:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-394">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="0b9c0-395">При создании контроллера movies механизм формирования шаблонов автоматически включает следующий оператор `@model` в начало файла *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-395">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="0b9c0-396">Эта директива `@model` обеспечивает доступ к списку фильмов, который контроллер передал в представление с использованием строго типизированного объекта `Model`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-396">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="0b9c0-397">Например, в представлении *Index.cshtml* код циклически перебирает фильмы с использованием оператора `foreach` для строго типизированного объекта `Model`:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-397">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="0b9c0-398">Поскольку объект `Model` является строго типизированным (как и объект `IEnumerable<Movie>`), каждый элемент в цикле получает тип `Movie`.</span><span class="sxs-lookup"><span data-stu-id="0b9c0-398">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="0b9c0-399">Помимо прочих преимуществ, это означает, что выполняется проверка кода во время компиляции:</span><span class="sxs-lookup"><span data-stu-id="0b9c0-399">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0b9c0-400">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0b9c0-400">Additional resources</span></span>

* [<span data-ttu-id="0b9c0-401">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="0b9c0-401">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="0b9c0-402">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="0b9c0-402">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="0b9c0-403">[Назад: добавление представления](adding-view.md)
> [Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="0b9c0-403">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end
