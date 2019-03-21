---
ms.openlocfilehash: 86cf1874677dc8b79e3223fb0819eb1881c69a11
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265601"
---
<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="a8d06-101">Добавление класса контекста для базы данных</span><span class="sxs-lookup"><span data-stu-id="a8d06-101">Add a database context class</span></span>

<span data-ttu-id="a8d06-102">Добавьте следующий класс `RazorPagesMovieContext` в папку *Models*:</span><span class="sxs-lookup"><span data-stu-id="a8d06-102">Add the following `RazorPagesMovieContext` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a8d06-103">Представленный выше код создает свойство `DbSet` для набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="a8d06-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="a8d06-104">В терминологии Entity Framework набор сущностей обычно соответствует таблице базы данных, а сущность — строке в этой таблице.</span><span class="sxs-lookup"><span data-stu-id="a8d06-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="a8d06-105">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="a8d06-105">Add a database connection string</span></span>

<span data-ttu-id="a8d06-106">Добавьте строку подключения в файл *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8d06-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="a8d06-107">Добавьте необходимые пакеты NuGet</span><span class="sxs-lookup"><span data-stu-id="a8d06-107">Add required NuGet packages</span></span>

<span data-ttu-id="a8d06-108">Выполните следующую команду .NET Core CLI, чтобы добавить в проект SQLite и CodeGeneration.Design:</span><span class="sxs-lookup"><span data-stu-id="a8d06-108">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design

```

<span data-ttu-id="a8d06-109">Пакет `Microsoft.VisualStudio.Web.CodeGeneration.Design` необходим для формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="a8d06-109">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="a8d06-110">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="a8d06-110">Register the database context</span></span>

<span data-ttu-id="a8d06-111">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="a8d06-111">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="a8d06-112">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8d06-112">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="a8d06-113">Соберите проект как проверку на ошибки.</span><span class="sxs-lookup"><span data-stu-id="a8d06-113">Build the project as a check for errors.</span></span>
