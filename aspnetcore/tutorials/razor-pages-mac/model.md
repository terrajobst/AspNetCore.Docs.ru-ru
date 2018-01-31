---
title: "Добавление модели в приложение Razor Pages с помощью Visual Studio для Mac"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac"
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: b8e5d65e195f9824602ec15d05dc013faa2a8dc9
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="fb18a-103">Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="fb18a-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="fb18a-104">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="fb18a-104">Add a data model</span></span>

* <span data-ttu-id="fb18a-105">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="fb18a-106">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="fb18a-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="fb18a-107">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="fb18a-108">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb18a-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="fb18a-109">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="fb18a-110">Выберите в центральной области **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="fb18a-111">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="fb18a-112">Щелкните правой кнопкой мыши волнистую красную линию, например `MovieContext` в строке `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="fb18a-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="fb18a-113">Выберите **Быстрое исправление > С помощью RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="fb18a-114">Visual Studio добавляет с оператор using.</span><span class="sxs-lookup"><span data-stu-id="fb18a-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="fb18a-115">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="fb18a-115">Build the project to verify you don't have any errors.</span></span>

![Страница "Создать"](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="fb18a-117">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="fb18a-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="fb18a-118">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="fb18a-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="fb18a-119">Щелкните ссылку [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet), чтобы получить номер версии.</span><span class="sxs-lookup"><span data-stu-id="fb18a-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="fb18a-120">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="fb18a-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="fb18a-121">**Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="fb18a-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="fb18a-122">Чтобы изменить файл *.csproj*, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="fb18a-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="fb18a-123">Выберите **Файл** > **Открыть**, а затем выберите файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="fb18a-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="fb18a-124">Выберите **Параметры**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-124">Select **Options**.</span></span>
* <span data-ttu-id="fb18a-125">Изменение значение параметра **Открыть с помощью** на **Редактор исходного кода**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-125">Change **Open with** to **Source Code Editor**.</span></span>

![Изменение файла csproj](model/csproj.png)

<span data-ttu-id="fb18a-127">Добавьте ссылку на инструмент `Microsoft.EntityFrameworkCore.Tools.DotNet` во вторую группу **\<ItemGroup>**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="fb18a-128">Номера версий, показанные в приведенном ниже коде, были правильными на момент написания статьи.</span><span class="sxs-lookup"><span data-stu-id="fb18a-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="fb18a-129">Добавление файлов страниц и фильмов в проект</span><span class="sxs-lookup"><span data-stu-id="fb18a-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="fb18a-130">В Visual Studio щелкните папку *Pages* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="fb18a-131">Выберите папку *Movies*.</span><span class="sxs-lookup"><span data-stu-id="fb18a-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="fb18a-132">В диалоговом окне *Выберите файлы для добавления в проект* выберите параметр **Включить все**.</span><span class="sxs-lookup"><span data-stu-id="fb18a-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="fb18a-133">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="fb18a-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fb18a-134">[Назад: Начало работы](xref:tutorials/razor-pages-mac/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="fb18a-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
