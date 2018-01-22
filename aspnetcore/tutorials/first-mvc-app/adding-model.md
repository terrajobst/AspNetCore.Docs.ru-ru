---
title: "Добавление модели в приложение MVC ASP.NET Core"
author: rick-anderson
description: "Добавление модели в простое приложение ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/8/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: c2cd3cc81221c146dec70e487a17b33360eb6112
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="6095e-103">Примечание. Шаблоны ASP.NET Core 2.0 содержат папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="6095e-103">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="6095e-104">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="6095e-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="6095e-105">Добавьте класс **Movie** и следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="6095e-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="6095e-106">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="6095e-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="6095e-107">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="6095e-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="6095e-108">Теперь в вашем приложении **M**VC есть **м**одель.</span><span class="sxs-lookup"><span data-stu-id="6095e-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="6095e-109">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="6095e-109">Scaffolding a controller</span></span>

<span data-ttu-id="6095e-110">В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="6095e-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller.png)

<span data-ttu-id="6095e-112">Если открывается диалоговое окно **Добавление зависимостей MVC**:</span><span class="sxs-lookup"><span data-stu-id="6095e-112">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="6095e-113">[Обновите Visual Studio до последней версии](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6095e-113">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="6095e-114">Это диалоговое окно отображает версии Visual Studio, предшествующие 15.5.</span><span class="sxs-lookup"><span data-stu-id="6095e-114">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="6095e-115">Если обновить не удается, выберите **ДОБАВИТЬ** и снова выполните шаги по добавлению контроллера.</span><span class="sxs-lookup"><span data-stu-id="6095e-115">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="6095e-116">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6095e-116">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="6095e-118">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="6095e-118">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="6095e-119">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="6095e-119">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="6095e-120">**Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6095e-120">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Добавление контекста данных](adding-model/_static/dc.png)

* <span data-ttu-id="6095e-122">**Представление:** оставьте флажки, установленные по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6095e-122">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="6095e-123">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*).</span><span class="sxs-lookup"><span data-stu-id="6095e-123">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="6095e-124">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="6095e-124">Tap **Add**</span></span>

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

<span data-ttu-id="6095e-126">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="6095e-126">Visual Studio creates:</span></span>

* <span data-ttu-id="6095e-127">[класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);</span><span class="sxs-lookup"><span data-stu-id="6095e-127">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="6095e-128">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="6095e-128">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="6095e-129">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/&ast;.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6095e-129">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="6095e-130">Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="6095e-130">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="6095e-131">Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="6095e-131">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="6095e-132">Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:</span><span class="sxs-lookup"><span data-stu-id="6095e-132">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="6095e-133">Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="6095e-133">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="6095e-134">С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="6095e-134">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="6095e-135">Добавление средств EF и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="6095e-135">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="6095e-136">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="6095e-136">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="6095e-137">Добавления пакета средств Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="6095e-137">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="6095e-138">Этот пакет необходим для добавления миграций и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="6095e-138">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="6095e-139">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="6095e-139">Add an initial migration.</span></span>
* <span data-ttu-id="6095e-140">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="6095e-140">Update the database with the initial migration.</span></span>

<span data-ttu-id="6095e-141">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="6095e-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Меню PMC](adding-model/_static/pmc.png)

<span data-ttu-id="6095e-143">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="6095e-143">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="6095e-144">**Примечание**. Если при выполнении команды `Install-Package` возникнет ошибка, откройте диспетчер пакетов NuGet и найдите пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="6095e-144">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="6095e-145">Это позволит установить пакет или убедиться, что он установлен.</span><span class="sxs-lookup"><span data-stu-id="6095e-145">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="6095e-146">Если же у вас возникают проблемы с PMC, вы можете [воспользоваться командной строкой](#cli).</span><span class="sxs-lookup"><span data-stu-id="6095e-146">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="6095e-147">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="6095e-147">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="6095e-148">Схема создается на основе модели, указанной в `DbContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="6095e-148">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="6095e-149">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="6095e-149">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="6095e-150">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="6095e-150">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="6095e-151">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="6095e-151">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="6095e-152">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<метка_времени>_Initial.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="6095e-152">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="6095e-153">Описанные выше действия можно выполнить с помощью интерфейса командной строки (CLI) вместо PMC.</span><span class="sxs-lookup"><span data-stu-id="6095e-153">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="6095e-154">Добавьте [средства EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="6095e-154">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="6095e-155">В консоли (в каталоге проекта) выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="6095e-155">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Контекстное меню Intellisense элемента модели со списком доступных свойств: идентификатор, цена, дата выпуска и название](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="6095e-157">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6095e-157">Additional resources</span></span>

* [<span data-ttu-id="6095e-158">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="6095e-158">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="6095e-159">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="6095e-159">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="6095e-160">[Назад: добавление представления](adding-view.md)
[Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="6095e-160">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
