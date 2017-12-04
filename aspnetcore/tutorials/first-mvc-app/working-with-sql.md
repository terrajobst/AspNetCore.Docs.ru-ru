---
title: "Работа с SQL Server LocalDB"
author: rick-anderson
description: "Использование SQL Server LocalDB с простым приложением MVC"
keywords: ASP.NET Core, SQL Server LocalDB, SQL Server, LocalDB
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: ff8fd9b8-7c98-424d-8641-7524e23bf541
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 67894d05bfd44b0406d10cbbe30ddfdaf0d66636
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="d08b1-104">Работа с SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="d08b1-104">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="d08b1-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d08b1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d08b1-106">Объект `MvcMovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="d08b1-106">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d08b1-107">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d08b1-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="d08b1-108">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="d08b1-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="d08b1-109">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d08b1-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="d08b1-110">При развертывании приложения на тестовом или рабочем сервере вы можете использовать переменную среды или другой способ настройки строки подключения к реальному серверу SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d08b1-110">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="d08b1-111">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d08b1-111">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="d08b1-112">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d08b1-112">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d08b1-113">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="d08b1-113">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="d08b1-114">LocalDB запускается по требованию в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="d08b1-114">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="d08b1-115">По умолчанию база данных LocalDB создает файлы \*.mdf в каталоге *C:/Users/\<пользователь\>*.</span><span class="sxs-lookup"><span data-stu-id="d08b1-115">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="d08b1-116">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="d08b1-116">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](working-with-sql/_static/ssox.png)

* <span data-ttu-id="d08b1-118">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="d08b1-118">Right click on the `Movie` table **> View Designer**</span></span>

  ![Контекстное меню, открытое на таблице Movie](working-with-sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](working-with-sql/_static/dv.png)

<span data-ttu-id="d08b1-121">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="d08b1-121">Note the key icon next to `ID`.</span></span> <span data-ttu-id="d08b1-122">По умолчанию EF сделает свойство с именем `ID` первичным ключом.</span><span class="sxs-lookup"><span data-stu-id="d08b1-122">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="d08b1-123">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотр данных**.</span><span class="sxs-lookup"><span data-stu-id="d08b1-123">Right click on the `Movie` table **> View Data**</span></span>

  ![Контекстное меню, открытое на таблице Movie](working-with-sql/_static/ssox2.png)

  ![Открытая таблица Movie с отображением данных](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="d08b1-126">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="d08b1-126">Seed the database</span></span>

<span data-ttu-id="d08b1-127">Создайте класс `SeedData` в папке *Models*.</span><span class="sxs-lookup"><span data-stu-id="d08b1-127">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="d08b1-128">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="d08b1-128">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="d08b1-129">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="d08b1-129">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="d08b1-130">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="d08b1-130">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d08b1-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d08b1-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d08b1-132">Добавьте инициализатор заполнения в метод `Main` в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d08b1-132">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d08b1-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d08b1-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d08b1-134">Добавьте инициализатор заполнения в конец метода `Configure` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d08b1-134">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="d08b1-135">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="d08b1-135">Test the app</span></span>

* <span data-ttu-id="d08b1-136">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d08b1-136">Delete all the records in the DB.</span></span> <span data-ttu-id="d08b1-137">Это можно сделать с помощью ссылок удаления в браузере или из SSOX.</span><span class="sxs-lookup"><span data-stu-id="d08b1-137">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="d08b1-138">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="d08b1-138">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="d08b1-139">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d08b1-139">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="d08b1-140">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="d08b1-140">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="d08b1-141">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="d08b1-141">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Значок IIS Express в области уведомлений](working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="d08b1-144">Если среда VS была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="d08b1-144">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="d08b1-145">Если среда VS была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="d08b1-145">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="d08b1-146">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="d08b1-146">The app shows the seeded data.</span></span>

![Приложение MVC Movie, открытое в Microsoft Edge и отображающие данные о фильме](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="d08b1-148">[Назад](adding-model.md)
[Вперед](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="d08b1-148">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
