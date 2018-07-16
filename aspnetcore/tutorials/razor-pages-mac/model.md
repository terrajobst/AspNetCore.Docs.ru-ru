---
title: Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac
author: rick-anderson
description: Узнайте, как добавить модель в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186762"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="54164-103">Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="54164-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="54164-104">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="54164-104">Add a data model</span></span>

* <span data-ttu-id="54164-105">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="54164-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="54164-106">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="54164-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="54164-107">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="54164-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="54164-108">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="54164-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="54164-109">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="54164-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="54164-110">Выберите в центральной области **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="54164-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="54164-111">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="54164-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="54164-112">Щелкните правой кнопкой мыши волнистую красную линию, например `MovieContext` в строке `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="54164-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="54164-113">Выберите **Быстрое исправление > С помощью RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="54164-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="54164-114">Visual Studio добавляет с оператор using.</span><span class="sxs-lookup"><span data-stu-id="54164-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="54164-115">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="54164-115">Build the project to verify you don't have any errors.</span></span>

![Страница "Создать"](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="54164-117">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="54164-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="54164-118">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="54164-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="54164-119">Щелкните ссылку [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet), чтобы получить номер версии.</span><span class="sxs-lookup"><span data-stu-id="54164-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="54164-120">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="54164-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="54164-121">**Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="54164-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="54164-122">Чтобы изменить файл *.csproj*, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="54164-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="54164-123">Выберите **Файл** > **Открыть**, а затем выберите файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="54164-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="54164-124">Выберите **Параметры**.</span><span class="sxs-lookup"><span data-stu-id="54164-124">Select **Options**.</span></span>
* <span data-ttu-id="54164-125">Изменение значение параметра **Открыть с помощью** на **Редактор исходного кода**.</span><span class="sxs-lookup"><span data-stu-id="54164-125">Change **Open with** to **Source Code Editor**.</span></span>

![Изменение файла csproj](model/csproj.png)

<span data-ttu-id="54164-127">Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**.</span><span class="sxs-lookup"><span data-stu-id="54164-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="54164-128">Номера версий, показанные в приведенном ниже коде, были правильными на момент написания статьи.</span><span class="sxs-lookup"><span data-stu-id="54164-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="54164-129">Добавление файлов страниц и фильмов в проект</span><span class="sxs-lookup"><span data-stu-id="54164-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="54164-130">В Visual Studio щелкните папку *Pages* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.</span><span class="sxs-lookup"><span data-stu-id="54164-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="54164-131">Выберите папку *Movies*.</span><span class="sxs-lookup"><span data-stu-id="54164-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="54164-132">В диалоговом окне *Выбор файлов для включения в проект* выберите **Включить все**.</span><span class="sxs-lookup"><span data-stu-id="54164-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="54164-133">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="54164-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="54164-134">[Предыдущая статья — "Начало работы"](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Следующая статья — "Сформированные страницы Razor Pages"](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="54164-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
