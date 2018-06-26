---
title: Добавление модели в приложение MVC ASP.NET Core
author: rick-anderson
description: Добавление модели в простое приложение ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 802cb458cb05579b97256022b56d6f97a03d2f1a
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687796"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="226f5-103">Добавление модели в приложение MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="226f5-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="226f5-104">Щелкните правой кнопкой мыши папку *Models* и выберите пункт **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="226f5-104">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="226f5-105">Добавьте класс **Movie** и следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="226f5-105">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="226f5-106">Поле `ID` является обязательным для первичного ключа базы данных.</span><span class="sxs-lookup"><span data-stu-id="226f5-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="226f5-107">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="226f5-107">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="226f5-108">Теперь в вашем приложении **M**VC есть **м**одель.</span><span class="sxs-lookup"><span data-stu-id="226f5-108">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="226f5-109">Формирование шаблонов контроллера</span><span class="sxs-lookup"><span data-stu-id="226f5-109">Scaffolding a controller</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="226f5-110">В **Обозревателе решений** щелкните правой кнопкой мыши папку *Контроллеры* и выберите **Добавить > Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="226f5-110">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller21.png)

<span data-ttu-id="226f5-112">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="226f5-112">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold21.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="226f5-114">В **обозревателе решений** щелкните папку *Контроллеры* правой кнопкой мыши и выберите пункт **Добавить > Контроллер**.</span><span class="sxs-lookup"><span data-stu-id="226f5-114">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![представление указанного выше шага](adding-model/_static/add_controller.png)

<span data-ttu-id="226f5-116">Если открывается диалоговое окно **Добавление зависимостей MVC**:</span><span class="sxs-lookup"><span data-stu-id="226f5-116">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="226f5-117">[Обновите Visual Studio до последней версии](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="226f5-117">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="226f5-118">Это диалоговое окно отображает версии Visual Studio, предшествующие 15.5.</span><span class="sxs-lookup"><span data-stu-id="226f5-118">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="226f5-119">Если обновить не удается, выберите **ДОБАВИТЬ** и снова выполните шаги по добавлению контроллера.</span><span class="sxs-lookup"><span data-stu-id="226f5-119">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="226f5-120">В диалоговом окне **Добавление шаблона** выберите **Контроллер MVC с представлениями, использующий Entity Framework > Добавить**.</span><span class="sxs-lookup"><span data-stu-id="226f5-120">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Диалоговое окно "Добавление шаблона"](adding-model/_static/add_scaffold2.png)

::: moniker-end

<span data-ttu-id="226f5-122">Выполните необходимые действия в диалоговом окне **Добавление контроллера**:</span><span class="sxs-lookup"><span data-stu-id="226f5-122">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="226f5-123">**Класс модели:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="226f5-123">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="226f5-124">**Класс контекста данных:** щелкните значок **+** и добавьте класс **MvcMovie.Models.MvcMovieContext** по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="226f5-124">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Добавление контекста данных](adding-model/_static/dc.png)

* <span data-ttu-id="226f5-126">**Представление:** оставьте флажки, установленные по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="226f5-126">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="226f5-127">**Имя контроллера:** оставьте имя по умолчанию (*MoviesController*).</span><span class="sxs-lookup"><span data-stu-id="226f5-127">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="226f5-128">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="226f5-128">Tap **Add**</span></span>

![Диалоговое окно "Добавление контроллера"](adding-model/_static/add_controller2.png)

<span data-ttu-id="226f5-130">Visual Studio создаст следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="226f5-130">Visual Studio creates:</span></span>

* <span data-ttu-id="226f5-131">[класс контекста базы данных](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*);</span><span class="sxs-lookup"><span data-stu-id="226f5-131">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="226f5-132">контроллер фильмов (*Controllers/MoviesController.cs*);</span><span class="sxs-lookup"><span data-stu-id="226f5-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="226f5-133">файлы представления Razor для страниц Create, Delete, Details, Edit и Index (<em>Views/Movies/&ast;.cshtml</em>).</span><span class="sxs-lookup"><span data-stu-id="226f5-133">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="226f5-134">Автоматическое создание контекста базы данных и методов и представлений действий [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (создание, чтение, обновление и удаление) называется *формированием шаблонов*.</span><span class="sxs-lookup"><span data-stu-id="226f5-134">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="226f5-135">Вскоре вы получите полнофункциональное веб-приложение, позволяющее управлять базой данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="226f5-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="226f5-136">Если запустить приложение и щелкнуть ссылку **Mvc Movie**, возникает ошибка наподобие следующей:</span><span class="sxs-lookup"><span data-stu-id="226f5-136">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="226f5-137">Вам необходимо создать базу данных. Для этого вы используете функцию [миграций](xref:data/ef-mvc/migrations) EF Core.</span><span class="sxs-lookup"><span data-stu-id="226f5-137">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="226f5-138">С помощью миграций можно создать базу данных, соответствующую модели данных, и обновлять схему базы данных при изменении модели.</span><span class="sxs-lookup"><span data-stu-id="226f5-138">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="226f5-139">Добавление средств EF и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="226f5-139">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="226f5-140">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий:</span><span class="sxs-lookup"><span data-stu-id="226f5-140">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="226f5-141">Добавления пакета средств Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="226f5-141">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="226f5-142">Этот пакет необходим для добавления миграций и обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="226f5-142">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="226f5-143">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="226f5-143">Add an initial migration.</span></span>
* <span data-ttu-id="226f5-144">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="226f5-144">Update the database with the initial migration.</span></span>

<span data-ttu-id="226f5-145">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="226f5-145">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Меню PMC](adding-model/_static/pmc.png)

<span data-ttu-id="226f5-147">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="226f5-147">In the PMC, enter the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"
``` PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="226f5-148">Не обращайте внимание на сообщение об ошибке, мы исправим это в следующем руководстве:</span><span class="sxs-lookup"><span data-stu-id="226f5-148">Ignore the following error message, we fix it in the next tutorial:</span></span>

<span data-ttu-id="226f5-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span><span class="sxs-lookup"><span data-stu-id="226f5-149">*Microsoft.EntityFrameworkCore.Model.Validation[30000]*</span></span>  
      <span data-ttu-id="226f5-150">*Для десятичного столбца Price в типе сущности Movie не указан тип. Это приведет к тому, что значения будут усекаться без вмешательства пользователя, если они не помещаются в значения точности и масштаба по умолчанию. Явно укажите тип столбца SQL Server, который может вместить все значения, с помощью 'ForHasColumnType()'.*</span><span class="sxs-lookup"><span data-stu-id="226f5-150">*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*</span></span>

::: moniker-end
::: moniker range="<= aspnetcore-2.0"

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="226f5-151">**Примечание**. Если при выполнении команды `Install-Package` возникнет ошибка, откройте диспетчер пакетов NuGet и найдите пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="226f5-151">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="226f5-152">Это позволит установить пакет или убедиться, что он установлен.</span><span class="sxs-lookup"><span data-stu-id="226f5-152">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="226f5-153">Если же у вас возникают проблемы с PMC, вы можете [воспользоваться командной строкой](#cli).</span><span class="sxs-lookup"><span data-stu-id="226f5-153">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

::: moniker-end

<span data-ttu-id="226f5-154">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="226f5-154">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="226f5-155">Схема создается на основе модели, указанной в `DbContext` (в файле *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="226f5-155">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="226f5-156">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="226f5-156">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="226f5-157">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="226f5-157">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="226f5-158">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="226f5-158">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="226f5-159">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/\<метка_времени>_Initial.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="226f5-159">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="226f5-160">Описанные выше действия можно выполнить с помощью интерфейса командной строки (CLI) вместо PMC.</span><span class="sxs-lookup"><span data-stu-id="226f5-160">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="226f5-161">Добавьте [средства EF Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="226f5-161">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="226f5-162">В консоли (в каталоге проекта) выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="226f5-162">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```

  <span data-ttu-id="226f5-163">Если вы запускаете приложение и возникает ошибка:</span><span class="sxs-lookup"><span data-stu-id="226f5-163">If you run the app and get the error:</span></span>

  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="226f5-164">Возможно, вы не выполнили `dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="226f5-164">You probably have not run `dotnet ef database update`.</span></span>

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model3.md)]

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="226f5-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="226f5-165">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="226f5-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="226f5-166">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/Includes/mvc-intro/adding-model4.md)]

![Контекстное меню Intellisense элемента модели со списком доступных свойств: идентификатор, цена, дата выпуска и название](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="226f5-168">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="226f5-168">Additional resources</span></span>

* [<span data-ttu-id="226f5-169">Вспомогательные функции тегов</span><span class="sxs-lookup"><span data-stu-id="226f5-169">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="226f5-170">Глобализация и локализация</span><span class="sxs-lookup"><span data-stu-id="226f5-170">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="226f5-171">[Назад: добавление представления](adding-view.md)
> [Далее: работа с SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="226f5-171">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
