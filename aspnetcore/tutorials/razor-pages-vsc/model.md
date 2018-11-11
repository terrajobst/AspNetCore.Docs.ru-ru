---
title: Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code
author: rick-anderson
description: Узнайте, как добавить модель в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244727"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="6aae0-103">Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6aae0-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6aae0-104">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="6aae0-104">Add a data model</span></span>

* <span data-ttu-id="6aae0-105">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="6aae0-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="6aae0-106">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="6aae0-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="6aae0-107">Добавьте в файл *Models/Movie.cs* следующий код:</span><span class="sxs-lookup"><span data-stu-id="6aae0-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="6aae0-108">Пакет Entity Framework Core NuGet для SQLite</span><span class="sxs-lookup"><span data-stu-id="6aae0-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="6aae0-109">В командной строке выполните следующую команду .NET Core:</span><span class="sxs-lookup"><span data-stu-id="6aae0-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="6aae0-110">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="6aae0-110">Register the database context</span></span>

<span data-ttu-id="6aae0-111">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в файле *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6aae0-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

<span data-ttu-id="6aae0-112">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="6aae0-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="6aae0-113">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="6aae0-113">Build the project to verify you don't have any errors.</span></span>

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="6aae0-114">Модель создания фильма</span><span class="sxs-lookup"><span data-stu-id="6aae0-114">Scaffold the Movie model</span></span>

* <span data-ttu-id="6aae0-115">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="6aae0-115">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6aae0-116">**Для Windows**: выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="6aae0-116">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="6aae0-117">**Для MacOS и Linux**: выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="6aae0-117">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="6aae0-118">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="6aae0-118">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6aae0-119">[Предыдущая статья — "Начало работы"](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Следующая статья — "Сформированные страницы Razor Pages"](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="6aae0-119">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
