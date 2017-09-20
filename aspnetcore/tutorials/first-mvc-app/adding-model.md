---
title: "Добавление модели в приложение MVC ASP.NET Core"
author: rick-anderson
description: "Добавление модели в простое приложение ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="5a44c-104">Примечание. Шаблоны ASP.NET Core 2.0 содержат папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="5a44c-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="5a44c-105">В обозревателе решений щелкните проект **MvcMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="5a44c-106">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="5a44c-106">Name the folder *Models*.</span></span>

<span data-ttu-id="5a44c-107">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="5a44c-108">Добавьте класс **Movie** и следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="5a44c-108">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="5a44c-109">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="5a44c-109">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="5a44c-110">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="5a44c-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="5a44c-111">Теперь в вашем приложении **M**VC есть **м**одель.</span><span class="sxs-lookup"><span data-stu-id="5a44c-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="5a44c-112">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="5a44c-112">Scaffolding a controller</span></span>

<span data-ttu-id="5a44c-113">В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller.png)

<span data-ttu-id="5a44c-115">В диалоговом окне **Добавление зависимостей MVC** установите переключатель в положение **Минимальный набор зависимостей** и нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-115">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![представление указанного выше шага](adding-model/_static/add_depend.png)

<span data-ttu-id="5a44c-117">В Visual Studio добавляются зависимости, необходимые для формирования шаблонов контроллера, но сам контроллер не создается.</span><span class="sxs-lookup"><span data-stu-id="5a44c-117">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="5a44c-118">Контроллер будет создан при следующем выполнении команды **Добавить > Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-118">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="5a44c-119">В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-119">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller.png)

<span data-ttu-id="5a44c-121">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-121">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="5a44c-123">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="5a44c-123">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="5a44c-124">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="5a44c-124">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="5a44c-125">**Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5a44c-125">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Добавление контекста данных](adding-model/_static/dc.png)

* <span data-ttu-id="5a44c-127">**Представление:** оставьте флажки, установленные по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5a44c-127">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="5a44c-128">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*).</span><span class="sxs-lookup"><span data-stu-id="5a44c-128">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="5a44c-129">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-129">Tap **Add**</span></span>

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

<span data-ttu-id="5a44c-131">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="5a44c-131">Visual Studio creates:</span></span>

* <span data-ttu-id="5a44c-132">[класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);</span><span class="sxs-lookup"><span data-stu-id="5a44c-132">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="5a44c-133">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="5a44c-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="5a44c-134">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/&ast;.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="5a44c-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="5a44c-135">Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="5a44c-135">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="5a44c-136">Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="5a44c-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="5a44c-137">Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникнет ошибка наподобие следующей:</span><span class="sxs-lookup"><span data-stu-id="5a44c-137">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="5a44c-138">Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a44c-138">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="5a44c-139">С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="5a44c-139">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="5a44c-140">Добавление средств EF и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="5a44c-140">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="5a44c-141">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="5a44c-141">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="5a44c-142">Добавления пакета средств Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="5a44c-142">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="5a44c-143">Этот пакет необходим для добавления миграций и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="5a44c-143">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="5a44c-144">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="5a44c-144">Add an initial migration.</span></span>
* <span data-ttu-id="5a44c-145">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="5a44c-145">Update the database with the initial migration.</span></span>

<span data-ttu-id="5a44c-146">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="5a44c-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Меню PMC](adding-model/_static/pmc.png)

<span data-ttu-id="5a44c-148">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="5a44c-148">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="5a44c-149">**Примечание**. Если при выполнении команды `Install-Package` возникнет ошибка, откройте диспетчер пакетов NuGet и найдите пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="5a44c-149">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="5a44c-150">Это позволит установить пакет или убедиться, что он установлен.</span><span class="sxs-lookup"><span data-stu-id="5a44c-150">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="5a44c-151">Если же у вас возникают проблемы с PMC, вы можете [воспользоваться командной строкой](#cli).</span><span class="sxs-lookup"><span data-stu-id="5a44c-151">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="5a44c-152">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="5a44c-152">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="5a44c-153">Схема создается на основе модели, указанной в `DbContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="5a44c-153">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="5a44c-154">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="5a44c-154">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="5a44c-155">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="5a44c-155">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="5a44c-156">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="5a44c-156">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="5a44c-157">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<метка-времени>_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="5a44c-157">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="5a44c-158"><a name="cli"></a> Описанные выше действия можно выполнить с помощью интерфейса командной строки (CLI) вместо PMC.</span><span class="sxs-lookup"><span data-stu-id="5a44c-158"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="5a44c-159">Добавьте [средства EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="5a44c-159">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="5a44c-160">В консоли (в каталоге проекта) выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="5a44c-160">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Контекстное меню Intellisense элемента модели со списком доступных свойств: идентификатор, цена, дата выпуска и название](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="5a44c-162">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5a44c-162">Additional resources</span></span>

* [<span data-ttu-id="5a44c-163">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="5a44c-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="5a44c-164">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="5a44c-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="5a44c-165">[Назад: добавление представления](adding-view.md)
[Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="5a44c-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
