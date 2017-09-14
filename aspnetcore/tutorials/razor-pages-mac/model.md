---
title: "Добавление модели в приложение Razor Pages с помощью Visual Studio для Mac"
author: rick-anderson
description: "Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac"
keywords: "ASP.NET Core,Razor Pages,Razor,MVC,модель"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: b234eb93fbca1f4c83712990712b86e9941968fd
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="52f40-104">Добавление модели в приложение Razor Pages в ASP.NET Core с помощью Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="52f40-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="52f40-105">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="52f40-105">Add a data model</span></span>

* <span data-ttu-id="52f40-106">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши, выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="52f40-106">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="52f40-107">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="52f40-107">Name the folder *Models*.</span></span>
* <span data-ttu-id="52f40-108">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="52f40-108">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="52f40-109">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="52f40-109">In the **New File** dialog:</span></span>

  * <span data-ttu-id="52f40-110">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="52f40-110">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="52f40-111">Выберите в центральной области **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="52f40-111">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="52f40-112">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="52f40-112">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="52f40-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="52f40-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="52f40-114">Щелкните правой кнопкой мыши волнистую красную линию, например `MovieContext` в строке `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="52f40-114">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="52f40-115">Выберите **Быстрое исправление > С помощью RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="52f40-115">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="52f40-116">Visual Studio добавляет с оператор using.</span><span class="sxs-lookup"><span data-stu-id="52f40-116">Visual studio adds the using statement.</span></span>

<span data-ttu-id="52f40-117">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="52f40-117">Build the project to verify you don't have any errors.</span></span>

![Страница "Создать"](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="52f40-119">Пакеты Entity Framework Core NuGet для миграций</span><span class="sxs-lookup"><span data-stu-id="52f40-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="52f40-120">Средства EF для интерфейса командной строки (CLI) доступны в [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="52f40-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="52f40-121">Чтобы установить этот пакет, добавьте его в коллекцию `DotNetCliToolReference` в файле *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="52f40-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="52f40-122">**Примечание**. Необходимо установить этот пакет, отредактировав файл *.csproj*; использовать команду `install-package` или графический пользовательский интерфейс диспетчера пакетов нельзя.</span><span class="sxs-lookup"><span data-stu-id="52f40-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="52f40-123">Чтобы изменить файл *.csproj*, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="52f40-123">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="52f40-124">Выберите **Файл > Открыть** и выберите файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="52f40-124">Select **File > Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="52f40-125">Выберите **Параметры**.</span><span class="sxs-lookup"><span data-stu-id="52f40-125">Select **Options**.</span></span>
* <span data-ttu-id="52f40-126">Изменение значение параметра **Открыть с помощью** на **Редактор исходного кода**.</span><span class="sxs-lookup"><span data-stu-id="52f40-126">Change **Open with** to **Source Code Editor**.</span></span>

![Изменение файла csproj](model/csproj.png)

<span data-ttu-id="52f40-128">В следующем примере кода показан обновленный файл *csproj*.</span><span class="sxs-lookup"><span data-stu-id="52f40-128">The following code shows the updated *csproj* file.</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="52f40-129">Добавление файлов страниц и фильмов в проект</span><span class="sxs-lookup"><span data-stu-id="52f40-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="52f40-130">В Visual Studio щелкните папку *Pages* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.</span><span class="sxs-lookup"><span data-stu-id="52f40-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="52f40-131">Выберите папку *Movies*.</span><span class="sxs-lookup"><span data-stu-id="52f40-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="52f40-132">В диалоговом окне *Выберите файлы для добавления в проект* выберите параметр **Включить все**.</span><span class="sxs-lookup"><span data-stu-id="52f40-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="52f40-133">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="52f40-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="52f40-134">[Назад: Начало работы](xref:tutorials/razor-pages-mac/razor-pages-start)
[Далее: Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="52f40-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
