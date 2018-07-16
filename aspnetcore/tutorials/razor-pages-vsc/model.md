---
title: Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code
author: rick-anderson
description: Узнайте, как добавить модель в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38152975"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="0f0c0-103">Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0f0c0-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="0f0c0-104">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="0f0c0-104">Add a data model</span></span>

* <span data-ttu-id="0f0c0-105">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="0f0c0-106">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="0f0c0-107">Добавьте в файл *Models/Movie.cs* следующий код:</span><span class="sxs-lookup"><span data-stu-id="0f0c0-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="0f0c0-108">Добавьте следующие инструкции `using` в начало файла *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-108">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="0f0c0-109">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-109">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="0f0c0-110">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="0f0c0-110">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="0f0c0-111">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="0f0c0-111">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="0f0c0-112">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-112">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="0f0c0-113">**Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-113">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="0f0c0-114">Измените файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-114">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="0f0c0-115">Выберите **Файл** > **Открыть файл**, а затем выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-115">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="0f0c0-116">Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="0f0c0-116">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="0f0c0-117">Модель создания фильма</span><span class="sxs-lookup"><span data-stu-id="0f0c0-117">Scaffold the Movie model</span></span>

* <span data-ttu-id="0f0c0-118">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="0f0c0-118">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0f0c0-119">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="0f0c0-119">Run the following command:</span></span>

<span data-ttu-id="0f0c0-120">**Примечание. Выполните в Windows следующую команду. Команда для MacOS и Linux указана ниже**.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-120">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="0f0c0-121">На MacOS и Linux выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-121">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="0f0c0-122">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-122">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="0f0c0-123">Выйдите из Visual Studio и выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-123">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="0f0c0-124">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="0f0c0-124">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0f0c0-125">[Предыдущая статья — "Начало работы"](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Следующая статья — "Сформированные страницы Razor Pages"](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="0f0c0-125">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
