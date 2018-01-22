---
title: "Добавление модели в приложение MVC ASP.NET Core"
author: rick-anderson
description: "Добавление модели в простое приложение ASP.NET Core."
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 3a7db5e435fe72150feb1e0ec905b6f6adc16f2c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="3b0dc-103">Щелкните папку *Models* правой кнопкой мыши и выберите пункт **Добавить** > **Новый файл**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-103">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="3b0dc-104">В диалоговом окне **Новый файл** выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-104">In the **New File** dialog:</span></span>

  * <span data-ttu-id="3b0dc-105">Выберите на левой панели пункт **Общие**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-105">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="3b0dc-106">Выберите в центральной области **Пустой класс**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-106">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="3b0dc-107">Назовите класс **Movie** и выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-107">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="3b0dc-108">Добавьте в класс `Movie` следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="3b0dc-108">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="3b0dc-109">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-109">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="3b0dc-110">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="3b0dc-111">Теперь в вашем приложении **M**VC есть **M**одель.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="3b0dc-112">Подготовка проекта для формирования шаблонов</span><span class="sxs-lookup"><span data-stu-id="3b0dc-112">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="3b0dc-113">Щелкните файл проекта правой кнопкой мыши и последовательно выберите параметры **Сервис > Изменить файл**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-113">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![представление указанного выше шага](adding-model/_static/1.png)

- <span data-ttu-id="3b0dc-115">Добавьте в файл *MvcMovie.csproj* следующие выделенные пакеты NuGet:</span><span class="sxs-lookup"><span data-stu-id="3b0dc-115">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="3b0dc-116">Сохраните файл.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-116">Save the file.</span></span>

- <span data-ttu-id="3b0dc-117">Создайте файл *Models/MvcMovieContext.cs* и добавьте следующий класс `MvcMovieContext`: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)].</span><span class="sxs-lookup"><span data-stu-id="3b0dc-117">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="3b0dc-118">Откройте файл *Startup.cs* и добавьте две директивы using: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)].</span><span class="sxs-lookup"><span data-stu-id="3b0dc-118">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="3b0dc-119">Добавьте контекст базы данных в файл *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3b0dc-119">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="3b0dc-120">За счет этого Entity Framework будет знать, какие классы модели входят в модель данных.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-120">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="3b0dc-121">Вы определяете один *набор сущностей* объектов Movie, который будут представлен в базу данных как таблица Movie.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-121">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="3b0dc-122">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-122">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="3b0dc-123">Формирование шаблонов для MovieController</span><span class="sxs-lookup"><span data-stu-id="3b0dc-123">Scaffold the MovieController</span></span>

<span data-ttu-id="3b0dc-124">Откройте окно терминала в папке проекта и выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-124">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="3b0dc-125">Если возникает ошибка `No executable found matching command "dotnet-aspnet-codegenerator", verify`.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-125">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="3b0dc-126">Вы находитесь в папке проекта.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-126">You are in the project directory.</span></span> <span data-ttu-id="3b0dc-127">В папке проекта имеются файлы *Program.cs*, *Startup.cs* и *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-127">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="3b0dc-128">Ваша версия — 1.1 или более поздняя.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-128">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="3b0dc-129">Запустите `dotnet`, чтобы узнать версию.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-129">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="3b0dc-130">Вы добавили элемент `<DotNetCliToolReference>` в [файл MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="3b0dc-130">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="3b0dc-131">Ядро формирование шаблонов создает следующее.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-131">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="3b0dc-132">Контроллер фильмов (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="3b0dc-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="3b0dc-133">Файлы представления Razor для страниц Create, Delete, Edit и Index (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="3b0dc-133">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="3b0dc-134">Автоматическое создание методов и представлений действия [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-134">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="3b0dc-135">Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="3b0dc-136">Добавление файлов в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3b0dc-136">Add the files to Visual Studio</span></span>

* <span data-ttu-id="3b0dc-137">Добавьте файл *MovieController.cs* в проект Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-137">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="3b0dc-138">Щелкните папку *Controllers* правой кнопкой мыши и выберите пункт **Добавить > Добавить файлы**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-138">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="3b0dc-139">Выберите файл *MovieController.cs*.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-139">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="3b0dc-140">Добавьте папку *Movies* и представления.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-140">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="3b0dc-141">Щелкните папку *Views* правой кнопкой мыши и выберите пункт **Добавить > Добавить существующую папку**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-141">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="3b0dc-142">Перейдите к папку *Views*, выберите *Views\Movies* и нажмите кнопку **Открыть**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-142">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="3b0dc-143">В диалоговом окне **Выберите файлы для добавления из папки Movies** выберите вариант **Включить все** и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-143">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="3b0dc-144">Теперь у вас есть база данных и страницы для отображения, редактирования, обновления и удаления данных.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-144">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="3b0dc-145">В следующем руководстве мы будем работать с базой данных.</span><span class="sxs-lookup"><span data-stu-id="3b0dc-145">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3b0dc-146">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3b0dc-146">Additional resources</span></span>

* [<span data-ttu-id="3b0dc-147">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="3b0dc-147">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="3b0dc-148">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="3b0dc-148">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="3b0dc-149">[Назад: добавление представления](adding-view.md)
[Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="3b0dc-149">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
