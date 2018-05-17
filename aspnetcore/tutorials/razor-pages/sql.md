---
title: Работа с SQL Server LocalDB и ASP.NET Core
author: rick-anderson
description: Описывается работа с SQL Server LocalDB и ASP.NET Core.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/sql
ms.openlocfilehash: d1a345fe8c61f6e07ebbe53de6d53e18d6f4c851
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="b99b5-103">Работа с SQL Server LocalDB и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b99b5-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="b99b5-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="b99b5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="b99b5-105">Объект `MovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="b99b5-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b99b5-106">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b99b5-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

<span data-ttu-id="b99b5-107">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b99b5-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b99b5-108">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b99b5-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="b99b5-109">При развертывании приложения на тестовом или рабочем сервере вы можете использовать переменную среды или другой способ настройки строки подключения к реальному серверу SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b99b5-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="b99b5-110">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b99b5-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b99b5-111">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b99b5-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b99b5-112">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="b99b5-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="b99b5-113">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="b99b5-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b99b5-114">По умолчанию база данных LocalDB создает файлы \*.mdf в каталоге *C:/Users/\<пользователь\>*.</span><span class="sxs-lookup"><span data-stu-id="b99b5-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="b99b5-115">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b99b5-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="b99b5-117">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="b99b5-117">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Контекстное меню, открытое на таблице Movie](sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](sql/_static/dv.png)

<span data-ttu-id="b99b5-120">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="b99b5-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="b99b5-121">По умолчанию EF создает свойство с именем `ID` для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="b99b5-121">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="b99b5-122">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="b99b5-122">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="b99b5-124">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="b99b5-124">Seed the database</span></span>

<span data-ttu-id="b99b5-125">Создайте класс `SeedData` в папке *Models*.</span><span class="sxs-lookup"><span data-stu-id="b99b5-125">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="b99b5-126">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="b99b5-126">Replace the generated code with the following:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="b99b5-127">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="b99b5-127">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="b99b5-128">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="b99b5-128">Add the seed initializer</span></span>

<span data-ttu-id="b99b5-129">Добавьте инициализатор заполнения в конец метода `Main` в файле *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b99b5-129">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

<span data-ttu-id="b99b5-130">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="b99b5-130">Test the app</span></span>

* <span data-ttu-id="b99b5-131">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="b99b5-131">Delete all the records in the DB.</span></span> <span data-ttu-id="b99b5-132">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="b99b5-132">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="b99b5-133">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="b99b5-133">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="b99b5-134">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b99b5-134">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="b99b5-135">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="b99b5-135">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="b99b5-136">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="b99b5-136">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

    * <span data-ttu-id="b99b5-139">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="b99b5-139">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="b99b5-140">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="b99b5-140">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="b99b5-141">В приложении отображаются заполненные данные.</span><span class="sxs-lookup"><span data-stu-id="b99b5-141">The app shows the seeded data:</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

<span data-ttu-id="b99b5-143">В следующем учебнике будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="b99b5-143">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b99b5-144">[Предыдущая тема. Шаблонные страницы Razor](xref:tutorials/razor-pages/page)
> [Следующая тема. Обновление страниц](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="b99b5-144">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
