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
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 7b1b2d54e9c68b0a6f2b1355726d0d1cb484f69e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="f4019-103">Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="f4019-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f4019-104">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="f4019-104">Add a data model</span></span>

* <span data-ttu-id="f4019-105">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="f4019-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="f4019-106">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="f4019-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="f4019-107">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="f4019-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="f4019-108">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f4019-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="f4019-109">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="f4019-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="f4019-110">Выберите в центральной области **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="f4019-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="f4019-111">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="f4019-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="f4019-112">Щелкните правой кнопкой мыши волнистую красную линию, например `MovieContext` в строке `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="f4019-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="f4019-113">Выберите **Быстрое исправление > С помощью RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="f4019-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="f4019-114">Visual Studio добавляет с оператор using.</span><span class="sxs-lookup"><span data-stu-id="f4019-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="f4019-115">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="f4019-115">Build the project to verify you don't have any errors.</span></span>

![Страница "Создать"](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="f4019-117">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="f4019-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="f4019-118">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="f4019-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="f4019-119">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f4019-119">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="f4019-120">**Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="f4019-120">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="f4019-121">Чтобы изменить файл *.csproj*, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f4019-121">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="f4019-122">Выберите **Файл** > **Открыть**, а затем выберите файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f4019-122">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="f4019-123">Выберите **Параметры**.</span><span class="sxs-lookup"><span data-stu-id="f4019-123">Select **Options**.</span></span>
* <span data-ttu-id="f4019-124">Изменение значение параметра **Открыть с помощью** на **Редактор исходного кода**.</span><span class="sxs-lookup"><span data-stu-id="f4019-124">Change **Open with** to **Source Code Editor**.</span></span>

![Изменение файла csproj](model/csproj.png)

<span data-ttu-id="f4019-126">Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="f4019-126">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="f4019-127">Добавление файлов страниц и фильмов в проект</span><span class="sxs-lookup"><span data-stu-id="f4019-127">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="f4019-128">В Visual Studio щелкните папку *Pages* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.</span><span class="sxs-lookup"><span data-stu-id="f4019-128">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="f4019-129">Выберите папку *Movies*.</span><span class="sxs-lookup"><span data-stu-id="f4019-129">Select the *Movies* folder.</span></span>
* <span data-ttu-id="f4019-130">В диалоговом окне *Выберите файлы для добавления в проект* выберите параметр **Включить все**.</span><span class="sxs-lookup"><span data-stu-id="f4019-130">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="f4019-131">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="f4019-131">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f4019-132">[Назад: Начало работы](xref:tutorials/razor-pages-mac/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f4019-132">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
