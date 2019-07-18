<a name="dc"></a>

<span data-ttu-id="ab74f-101">Добавьте следующий класс `MvcMovieContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="ab74f-101">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="ab74f-102">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="ab74f-102">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="ab74f-103">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="ab74f-103">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="ab74f-104">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="ab74f-104">Add a database connection string</span></span>

<span data-ttu-id="ab74f-105">Добавьте строку подключения в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="ab74f-105">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="ab74f-106">Добавьте необходимые пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="ab74f-106">Add required NuGet packages</span></span>

<span data-ttu-id="ab74f-107">Выполните следующую команду .NET Core CLI, чтобы добавить в проект SQLite и CodeGeneration.Design:</span><span class="sxs-lookup"><span data-stu-id="ab74f-107">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="ab74f-108">Пакет `Microsoft.VisualStudio.Web.CodeGeneration.Design` необходим для формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="ab74f-108">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="ab74f-109">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="ab74f-109">Register the database context</span></span>

<span data-ttu-id="ab74f-110">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="ab74f-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="ab74f-111">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="ab74f-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="ab74f-112">Соберите проект как проверку на ошибки.</span><span class="sxs-lookup"><span data-stu-id="ab74f-112">Build the project as a check for errors.</span></span>