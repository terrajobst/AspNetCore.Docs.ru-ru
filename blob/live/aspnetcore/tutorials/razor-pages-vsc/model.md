---
title: "Добавление модели в приложение Razor Pages с помощью Visual Studio для Mac"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 2a0c21ddc9ff44143ab828f8962946138a7ca0fa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="705a6-103">Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="705a6-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="705a6-104">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="705a6-104">Add a data model</span></span>

* <span data-ttu-id="705a6-105">Добавьте папку с именем *Models*.</span><span class="sxs-lookup"><span data-stu-id="705a6-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="705a6-106">Добавьте класс в папку *Models* с именем *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="705a6-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="705a6-107">Добавьте в файл *ToUpperPlugin.cs* следующий код:</span><span class="sxs-lookup"><span data-stu-id="705a6-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="705a6-108">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="705a6-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="705a6-109">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="705a6-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="705a6-110">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="705a6-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="705a6-111">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="705a6-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="705a6-112">**Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="705a6-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="705a6-113">Измените файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="705a6-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="705a6-114">Выберите **Файл** > **Открыть файл**, а затем выберите файл *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="705a6-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="705a6-115">Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="705a6-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="705a6-116">Модель создания фильма</span><span class="sxs-lookup"><span data-stu-id="705a6-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="705a6-117">Откройте окно командной строки в папке проекта (папке, где находятся файлы *Program.cs*, *Startup.cs* и *.csproj* файлов).</span><span class="sxs-lookup"><span data-stu-id="705a6-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="705a6-118">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="705a6-118">Run the following command:</span></span>

<span data-ttu-id="705a6-119">**Примечание. Выполните в Windows следующую команду. Команда для MacOS и Linux указана ниже**.</span><span class="sxs-lookup"><span data-stu-id="705a6-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="705a6-120">На MacOS и Linux выполните следующую команду.</span><span class="sxs-lookup"><span data-stu-id="705a6-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="705a6-121">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="705a6-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="705a6-122">Выйдите из Visual Studio и выполните команду еще раз.</span><span class="sxs-lookup"><span data-stu-id="705a6-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="705a6-123"> В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="705a6-123"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="705a6-124">[Назад: Начало работы](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="705a6-124">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
