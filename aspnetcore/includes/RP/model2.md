<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="9adc3-101">Добавление класса контекста для базы данных</span><span class="sxs-lookup"><span data-stu-id="9adc3-101">Add a database context class</span></span>

<span data-ttu-id="9adc3-102">В проекте RazorPagesMovie создайте новую папку с именем *Data*.</span><span class="sxs-lookup"><span data-stu-id="9adc3-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="9adc3-103">Добавьте следующий класс `RazorPagesMovieContext` в папку *Data*:</span><span class="sxs-lookup"><span data-stu-id="9adc3-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9adc3-104">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="9adc3-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="9adc3-105">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="9adc3-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="9adc3-106">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="9adc3-106">Add a database connection string</span></span>

<span data-ttu-id="9adc3-107">Добавьте строку подключения в файл *appsettings.json*, как показано в следующем выделенном коде:</span><span class="sxs-lookup"><span data-stu-id="9adc3-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="9adc3-108">Добавление пакетов NuGet и средств EF</span><span class="sxs-lookup"><span data-stu-id="9adc3-108">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="9adc3-109">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="9adc3-109">Register the database context</span></span>

<span data-ttu-id="9adc3-110">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9adc3-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="9adc3-111">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9adc3-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="9adc3-112">Добавьте необходимые пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="9adc3-112">Add required NuGet packages</span></span>

<span data-ttu-id="9adc3-113">Выполните следующую команду .NET Core CLI, чтобы добавить в проект SQLite и CodeGeneration.Design:</span><span class="sxs-lookup"><span data-stu-id="9adc3-113">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="9adc3-114">Пакет `Microsoft.VisualStudio.Web.CodeGeneration.Design` необходим для формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="9adc3-114">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="9adc3-115">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="9adc3-115">Register the database context</span></span>

<span data-ttu-id="9adc3-116">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="9adc3-116">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="9adc3-117">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9adc3-117">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="9adc3-118">Соберите проект как проверку на ошибки.</span><span class="sxs-lookup"><span data-stu-id="9adc3-118">Build the project as a check for errors.</span></span>
::: moniker-end
