---
title: "Добавление модели в приложение MVC ASP.NET Core"
author: rick-anderson
description: "Добавление модели в простое приложение ASP.NET Core."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 12/8/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 03c16e523fe2f91cae5c71357835684d813e3a1f
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/14/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="54e23-104">Примечание. Шаблоны ASP.NET Core 2.0 содержат папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="54e23-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="54e23-105">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="54e23-105">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="54e23-106">Добавьте класс **Movie** и следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="54e23-106">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="54e23-107">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="54e23-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="54e23-108">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="54e23-108">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="54e23-109">Теперь в вашем приложении **M**VC есть **м**одель.</span><span class="sxs-lookup"><span data-stu-id="54e23-109">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="54e23-110">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="54e23-110">Scaffolding a controller</span></span>

<span data-ttu-id="54e23-111">В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="54e23-111">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller.png)

<span data-ttu-id="54e23-113">Если открывается диалоговое окно **Добавление зависимостей MVC**:</span><span class="sxs-lookup"><span data-stu-id="54e23-113">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="54e23-114">[Обновите Visual Studio до последней версии](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="54e23-114">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="54e23-115">Это диалоговое окно отображает версии Visual Studio, предшествующие 15.5.</span><span class="sxs-lookup"><span data-stu-id="54e23-115">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="54e23-116">Если обновить не удается, выберите **ДОБАВИТЬ** и снова выполните шаги по добавлению контроллера.</span><span class="sxs-lookup"><span data-stu-id="54e23-116">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="54e23-117">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="54e23-117">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="54e23-119">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="54e23-119">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="54e23-120">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="54e23-120">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="54e23-121">**Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54e23-121">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Добавление контекста данных](adding-model/_static/dc.png)

* <span data-ttu-id="54e23-123">**Представление:** оставьте флажки, установленные по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="54e23-123">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="54e23-124">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*).</span><span class="sxs-lookup"><span data-stu-id="54e23-124">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="54e23-125">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="54e23-125">Tap **Add**</span></span>

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

<span data-ttu-id="54e23-127">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="54e23-127">Visual Studio creates:</span></span>

* <span data-ttu-id="54e23-128">[класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);</span><span class="sxs-lookup"><span data-stu-id="54e23-128">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="54e23-129">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="54e23-129">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="54e23-130">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (*Views/Movies/&ast;.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="54e23-130">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="54e23-131">Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="54e23-131">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="54e23-132">Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="54e23-132">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="54e23-133">Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:</span><span class="sxs-lookup"><span data-stu-id="54e23-133">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="54e23-134">Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="54e23-134">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="54e23-135">С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="54e23-135">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="54e23-136">Добавление средств EF и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="54e23-136">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="54e23-137">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="54e23-137">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="54e23-138">Добавления пакета средств Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="54e23-138">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="54e23-139">Этот пакет необходим для добавления миграций и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="54e23-139">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="54e23-140">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="54e23-140">Add an initial migration.</span></span>
* <span data-ttu-id="54e23-141">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="54e23-141">Update the database with the initial migration.</span></span>

<span data-ttu-id="54e23-142">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="54e23-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Меню PMC](adding-model/_static/pmc.png)

<span data-ttu-id="54e23-144">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="54e23-144">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="54e23-145">**Примечание**. Если при выполнении команды `Install-Package` возникнет ошибка, откройте диспетчер пакетов NuGet и найдите пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="54e23-145">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="54e23-146">Это позволит установить пакет или убедиться, что он установлен.</span><span class="sxs-lookup"><span data-stu-id="54e23-146">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="54e23-147">Если же у вас возникают проблемы с PMC, вы можете [воспользоваться командной строкой](#cli).</span><span class="sxs-lookup"><span data-stu-id="54e23-147">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="54e23-148">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="54e23-148">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="54e23-149">Схема создается на основе модели, указанной в `DbContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="54e23-149">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="54e23-150">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="54e23-150">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="54e23-151">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="54e23-151">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="54e23-152">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="54e23-152">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="54e23-153">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<метка_времени>_Initial.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="54e23-153">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="54e23-154">Описанные выше действия можно выполнить с помощью интерфейса командной строки (CLI) вместо PMC.</span><span class="sxs-lookup"><span data-stu-id="54e23-154">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="54e23-155">Добавьте [средства EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="54e23-155">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="54e23-156">В консоли (в каталоге проекта) выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="54e23-156">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Контекстное меню Intellisense элемента модели со списком доступных свойств: идентификатор, цена, дата выпуска и название](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="54e23-158">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="54e23-158">Additional resources</span></span>

* [<span data-ttu-id="54e23-159">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="54e23-159">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="54e23-160">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="54e23-160">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="54e23-161">[Назад: добавление представления](adding-view.md)
[Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="54e23-161">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
