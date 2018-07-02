---
title: Добавление модели в приложение Razor Pages в ASP.NET Core
author: rick-anderson
description: Узнайте, как добавлять классы для управления фильмами в базе данных с использованием Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327556"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="364d3-103">Добавление модели в приложение Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="364d3-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="364d3-104">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="364d3-104">Add a data model</span></span>

<span data-ttu-id="364d3-105">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="364d3-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="364d3-106">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="364d3-106">Name the folder *Models*.</span></span>

<span data-ttu-id="364d3-107">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="364d3-107">Right click the *Models* folder.</span></span> <span data-ttu-id="364d3-108">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="364d3-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="364d3-109">Добавьте класс **Movie** и следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="364d3-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="364d3-110">Замените содержимое класса `Movie` следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="364d3-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="364d3-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="364d3-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="364d3-112">Создание модели фильма</span><span class="sxs-lookup"><span data-stu-id="364d3-112">Scaffold the movie model</span></span>

<span data-ttu-id="364d3-113">В этом разделе создается модель фильма.</span><span class="sxs-lookup"><span data-stu-id="364d3-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="364d3-114">То есть средство формирования шаблонов создает страницы для операций создания, чтения, обновления и удаления для модели фильма.</span><span class="sxs-lookup"><span data-stu-id="364d3-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="364d3-115">Создайте папку *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="364d3-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="364d3-116">В **Обозревателе решений** щелкните правой кнопкой мыши на папку *Pages* > **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="364d3-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="364d3-117">Назовите папку *Movies*</span><span class="sxs-lookup"><span data-stu-id="364d3-117">Name the folder *Movies*</span></span>

<span data-ttu-id="364d3-118">В **Обозревателе решений** щелкните правой кнопкой мыши на папку *Pages/Movies* > **Добавить** > **Создать шаблонный элемент**.</span><span class="sxs-lookup"><span data-stu-id="364d3-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/sca.png)

<span data-ttu-id="364d3-120">В диалоговом окне **Добавить шаблон** выберите **Razor Pages с помощью Entity Framework (CRUD)** > **ДОБАВИТЬ**.</span><span class="sxs-lookup"><span data-stu-id="364d3-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/add_scaffold.png)

<span data-ttu-id="364d3-122">Заполните поля в диалоговом окне **Добавить Razor Pages с помощью Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="364d3-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="364d3-123">В раскрывающемся списке **Класс модели** выберите **Фильм (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="364d3-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="364d3-124">В строке **Класс контекста данных** нажмите на значок плюса **+** и примите сгенерированное имя **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="364d3-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="364d3-125">В раскрывающемся списке **Класс контекста данных**  выберите **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="364d3-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="364d3-126">Нажмите **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="364d3-126">Select **Add**.</span></span>

![Изображение из предыдущих инструкций.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="364d3-128">Выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="364d3-128">Perform initial migration</span></span>

<span data-ttu-id="364d3-129">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.</span><span class="sxs-lookup"><span data-stu-id="364d3-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="364d3-130">Добавления первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="364d3-130">Add an initial migration.</span></span>
* <span data-ttu-id="364d3-131">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="364d3-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="364d3-132">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="364d3-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="364d3-134">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="364d3-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="364d3-135">Кроме того, можно использовать следующие команды .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="364d3-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="364d3-136">Не обращайте внимание на следующее предупреждающее сообщение, эту проблемы мы решим в следующем руководстве:</span><span class="sxs-lookup"><span data-stu-id="364d3-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="364d3-137">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="364d3-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="364d3-138">Схема создается на основе модели, указанной в `RazorPagesMovieContext` (в файле *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="364d3-138">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="364d3-139">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="364d3-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="364d3-140">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="364d3-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="364d3-141">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="364d3-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="364d3-142">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="364d3-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="364d3-143">Если возникает ошибка.</span><span class="sxs-lookup"><span data-stu-id="364d3-143">If you get the error:</span></span>

<span data-ttu-id="364d3-144">SqlException: не удается открыть базу данных RazorPagesMovieContext-GUID, запрошенную при входе.</span><span class="sxs-lookup"><span data-stu-id="364d3-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="364d3-145">Сбой при входе.</span><span class="sxs-lookup"><span data-stu-id="364d3-145">The login failed.</span></span>
<span data-ttu-id="364d3-146">Сбой при входе в систему пользователя user name.</span><span class="sxs-lookup"><span data-stu-id="364d3-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="364d3-147">Вы пропустили [шаг миграции](#pmc).</span><span class="sxs-lookup"><span data-stu-id="364d3-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="364d3-148">Добавление модели данных</span><span class="sxs-lookup"><span data-stu-id="364d3-148">Add a data model</span></span>

<span data-ttu-id="364d3-149">В обозревателе решений щелкните проект **RazorPagesMovie** правой кнопкой мыши и выберите пункт **Добавить** > **Новая папка**.</span><span class="sxs-lookup"><span data-stu-id="364d3-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="364d3-150">Присвойте папке имя *Models*.</span><span class="sxs-lookup"><span data-stu-id="364d3-150">Name the folder *Models*.</span></span>

<span data-ttu-id="364d3-151">Щелкните правой кнопкой мыши папку *Models*.</span><span class="sxs-lookup"><span data-stu-id="364d3-151">Right click the *Models* folder.</span></span> <span data-ttu-id="364d3-152">Выберите **Добавить** > **Класс**.</span><span class="sxs-lookup"><span data-stu-id="364d3-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="364d3-153">Добавьте класс **Movie** и следующие свойства:</span><span class="sxs-lookup"><span data-stu-id="364d3-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="364d3-154">Добавление строки подключения базы данных</span><span class="sxs-lookup"><span data-stu-id="364d3-154">Add a database connection string</span></span>

<span data-ttu-id="364d3-155">Добавьте строку подключения в файл *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="364d3-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="364d3-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="364d3-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="364d3-157">Регистрация контекста базы данных</span><span class="sxs-lookup"><span data-stu-id="364d3-157">Register the database context</span></span>

<span data-ttu-id="364d3-158">Зарегистрируйте контекст базы данных в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) в [методе ConfigureServices класса Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="364d3-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="364d3-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="364d3-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="364d3-160">Выполните сборку проекта, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="364d3-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="364d3-161">Добавление средств формирования шаблонов и выполнение первоначальной миграции</span><span class="sxs-lookup"><span data-stu-id="364d3-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="364d3-162">В этом разделе консоль диспетчера пакетов (PMC) используется для выполнения следующих действий.</span><span class="sxs-lookup"><span data-stu-id="364d3-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="364d3-163">Добавление веб-пакета создания кода для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="364d3-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="364d3-164">Этот пакет необходим для работы ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="364d3-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="364d3-165">Добавление первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="364d3-165">Add an initial migration.</span></span>
* <span data-ttu-id="364d3-166">Обновления базы данных с помощью первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="364d3-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="364d3-167">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet** > **Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="364d3-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Меню PMC](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="364d3-169">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="364d3-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="364d3-170">Кроме того, можно использовать следующие команды .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="364d3-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="364d3-171">Не обращайте внимание на следующее сообщение:</span><span class="sxs-lookup"><span data-stu-id="364d3-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="364d3-172">Мы исправим это в следующем руководстве.</span><span class="sxs-lookup"><span data-stu-id="364d3-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="364d3-173">Команда `Install-Package` устанавливает средства, необходимые для запуска ядра формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="364d3-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="364d3-174">Команда `Add-Migration` формирует код для создания схемы исходной базы данных.</span><span class="sxs-lookup"><span data-stu-id="364d3-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="364d3-175">Схема создается на основе модели, указанной в `DbContext` (в файле *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="364d3-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="364d3-176">Аргумент `Initial` используется для присвоения имен миграциям.</span><span class="sxs-lookup"><span data-stu-id="364d3-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="364d3-177">Можно использовать любое имя, но обычно выбирается имя, описывающее миграцию.</span><span class="sxs-lookup"><span data-stu-id="364d3-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="364d3-178">Дополнительные сведения см. в статье [Введение в миграции](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="364d3-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="364d3-179">Команда `Update-Database` выполняет метод `Up` в файле *Migrations/{time-stamp}_InitialCreate.cs*, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="364d3-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="364d3-180">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="364d3-180">Test the app</span></span>

* <span data-ttu-id="364d3-181">Запустите приложение и добавьте `/Movies` к URL-адресу в браузере (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="364d3-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="364d3-182">Протестируйте ссылку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="364d3-182">Test the **Create** link.</span></span>

  ![Страница "Создать"](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="364d3-184">Протестируйте ссылки **Изменить**, **Сведения** и **Удалить**.</span><span class="sxs-lookup"><span data-stu-id="364d3-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="364d3-185">Если появляется исключение SQL, выполните миграции и обновите базу данных.</span><span class="sxs-lookup"><span data-stu-id="364d3-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="364d3-186">В следующем учебнике рассматриваются файлы, созданные с помощью формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="364d3-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="364d3-187">[Предыдущая статья — "Начало работы"](xref:tutorials/razor-pages/razor-pages-start)
> [Следующая статья — "Сформированные страницы Razor Pages"](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="364d3-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
