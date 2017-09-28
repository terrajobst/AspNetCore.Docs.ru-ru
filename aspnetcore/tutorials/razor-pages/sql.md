---
title: "Работа с SQL Server LocalDB и ASP.NET Core"
author: rick-anderson
description: "Описывается работа с SQL Server LocalDB и ASP.NET Core."
keywords: ASP.NET Core, Razor Pages, Razor, MVC, SQL, LocalDB
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 852bd2dff96c951f55a9b142d8e15b6ec5856921
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="60cba-104">Работа с SQL Server LocalDB и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60cba-104">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="60cba-105">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="60cba-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="60cba-106">Объект `MovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="60cba-106">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="60cba-107">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="60cba-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

<span data-ttu-id="60cba-108">Система [конфигурации](xref:fundamentals/configuration) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="60cba-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration) system reads the `ConnectionString`.</span></span> <span data-ttu-id="60cba-109">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="60cba-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-javascript[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="60cba-110">При развертывании приложения на тестовом или рабочем сервере вы можете использовать переменную среды или другой способ настройки строки подключения к реальному серверу SQL Server.</span><span class="sxs-lookup"><span data-stu-id="60cba-110">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="60cba-111">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration).</span><span class="sxs-lookup"><span data-stu-id="60cba-111">See [Configuration](xref:fundamentals/configuration) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="60cba-112">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="60cba-112">SQL Server Express LocalDB</span></span>

<span data-ttu-id="60cba-113">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="60cba-113">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="60cba-114">LocalDB запускается по требованию в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="60cba-114">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="60cba-115">По умолчанию база данных LocalDB создает файлы \*.mdf в каталоге *C:/Users/\<пользователь\>*.</span><span class="sxs-lookup"><span data-stu-id="60cba-115">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="60cba-116">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="60cba-116">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="60cba-118">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="60cba-118">Right click on the `Movie` table **> View Designer**</span></span>

  ![Контекстное меню, открытое на таблице Movie](sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](sql/_static/dv.png)

<span data-ttu-id="60cba-121">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="60cba-121">Note the key icon next to `ID`.</span></span> <span data-ttu-id="60cba-122">По умолчанию EF сделает свойство с именем `ID` первичным ключом.</span><span class="sxs-lookup"><span data-stu-id="60cba-122">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="60cba-123">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотр данных**.</span><span class="sxs-lookup"><span data-stu-id="60cba-123">Right click on the `Movie` table **> View Data**</span></span>

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="60cba-125">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="60cba-125">Seed the database</span></span>

<span data-ttu-id="60cba-126">Создайте класс `SeedData` в папке *Models*.</span><span class="sxs-lookup"><span data-stu-id="60cba-126">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="60cba-127">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="60cba-127">Replace the generated code with the following:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="60cba-128">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="60cba-128">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="60cba-129">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="60cba-129">Add the seed initializer</span></span>

<span data-ttu-id="60cba-130">Добавьте инициализатор заполнения в конец метода `Main` в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="60cba-130">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs?highlight=6,17-32)]

<span data-ttu-id="60cba-131">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="60cba-131">Test the app</span></span>

* <span data-ttu-id="60cba-132">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="60cba-132">Delete all the records in the DB.</span></span> <span data-ttu-id="60cba-133">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="60cba-133">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="60cba-134">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="60cba-134">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="60cba-135">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="60cba-135">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="60cba-136">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="60cba-136">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="60cba-137">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="60cba-137">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

   * <span data-ttu-id="60cba-140">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="60cba-140">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="60cba-141">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="60cba-141">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="60cba-142">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="60cba-142">The app shows the seeded data.</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

<span data-ttu-id="60cba-144">В следующем учебнике будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="60cba-144">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="60cba-145">[Назад: сформированные страницы Razor Pages](xref:tutorials/razor-pages/page) </span><span class="sxs-lookup"><span data-stu-id="60cba-145">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page) </span></span>  
[<span data-ttu-id="60cba-146">Далее: обновление страниц</span><span class="sxs-lookup"><span data-stu-id="60cba-146">Next: Updating the pages</span></span>](xref:tutorials/razor-pages/da1)
