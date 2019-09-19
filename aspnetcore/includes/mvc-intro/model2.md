::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

<span data-ttu-id="c2b38-101">Создайте папку *Data*.</span><span class="sxs-lookup"><span data-stu-id="c2b38-101">Create a *Data* folder.</span></span>

<span data-ttu-id="c2b38-102">Добавьте следующий класс `MvcMovieContext` в папку *Data*:</span><span class="sxs-lookup"><span data-stu-id="c2b38-102">Add the following `MvcMovieContext` class to the *Data* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="c2b38-103">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="c2b38-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="c2b38-104">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="c2b38-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="c2b38-105">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="c2b38-105">Add a database connection string</span></span>

<span data-ttu-id="c2b38-106">Добавьте строку подключения в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="c2b38-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="c2b38-107">Добавление пакетов NuGet и средств EF</span><span class="sxs-lookup"><span data-stu-id="c2b38-107">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="c2b38-108">Выполните следующие команды интерфейса командной строки .NET Core:</span><span class="sxs-lookup"><span data-stu-id="c2b38-108">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="c2b38-109">Приведенные выше команды добавляют в проект средства Entity Framework Core для .NET CLI и несколько пакетов.</span><span class="sxs-lookup"><span data-stu-id="c2b38-109">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="c2b38-110">Пакет `Microsoft.VisualStudio.Web.CodeGeneration.Design` необходим для формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c2b38-110">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="c2b38-111">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="c2b38-111">Register the database context</span></span>

<span data-ttu-id="c2b38-112">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c2b38-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="c2b38-113">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c2b38-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="c2b38-114">Выполните сборку проекта, чтобы проверить его на ошибки компиляции.</span><span class="sxs-lookup"><span data-stu-id="c2b38-114">Build the project as a check for compiler errors.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c2b38-115">Добавьте следующий класс `MvcMovieContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="c2b38-115">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="c2b38-116">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="c2b38-116">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="c2b38-117">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="c2b38-117">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="c2b38-118">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="c2b38-118">Add a database connection string</span></span>

<span data-ttu-id="c2b38-119">Добавьте строку подключения в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="c2b38-119">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="c2b38-120">Добавьте необходимые пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="c2b38-120">Add required NuGet packages</span></span>

<span data-ttu-id="c2b38-121">Выполните следующую команду .NET Core CLI, чтобы добавить в проект SQLite и CodeGeneration.Design:</span><span class="sxs-lookup"><span data-stu-id="c2b38-121">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="c2b38-122">Пакет `Microsoft.VisualStudio.Web.CodeGeneration.Design` необходим для формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c2b38-122">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="c2b38-123">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="c2b38-123">Register the database context</span></span>

<span data-ttu-id="c2b38-124">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c2b38-124">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="c2b38-125">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c2b38-125">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="c2b38-126">Соберите проект как проверку на ошибки.</span><span class="sxs-lookup"><span data-stu-id="c2b38-126">Build the project as a check for errors.</span></span>
::: moniker-end