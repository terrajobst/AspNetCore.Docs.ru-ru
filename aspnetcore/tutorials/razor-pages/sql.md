---
title: Работа с SQL Server LocalDB и ASP.NET Core
author: rick-anderson
description: Описывается работа с SQL Server LocalDB и ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: ef4e1fb3bf1ac1b3695ff89d6692ac6fa1641e31
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2018
ms.locfileid: "41820163"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="e1e00-103">Работа с SQL Server LocalDB и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1e00-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="e1e00-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="e1e00-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="e1e00-105">Объект `MovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1e00-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e1e00-106">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1e00-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="e1e00-107">Дополнительные сведения о методах, которые используются в `ConfigureServices`, см.:</span><span class="sxs-lookup"><span data-stu-id="e1e00-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="e1e00-108">[Общий регламент по защите данных (GDPR), принятый в ЕС, в ASP.NET Core](xref:security/gdpr) для `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="e1e00-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="e1e00-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="e1e00-109">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

::: moniker-end

<span data-ttu-id="e1e00-110">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e1e00-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e1e00-111">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e1e00-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="e1e00-112">Значение имени для базы данных (`Database={Database name}`) будет отличаться для созданного кода.</span><span class="sxs-lookup"><span data-stu-id="e1e00-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="e1e00-113">Значение имени является произвольным.</span><span class="sxs-lookup"><span data-stu-id="e1e00-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="e1e00-114">При развертывании приложения на тестовом или рабочем сервере вы можете использовать переменную среды или другой способ настройки строки подключения к реальному серверу SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e1e00-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="e1e00-115">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e1e00-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e1e00-116">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e1e00-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e1e00-117">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="e1e00-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="e1e00-118">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="e1e00-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="e1e00-119">По умолчанию база данных LocalDB создает файлы \*.mdf в каталоге *C:/Users/\<пользователь\>*.</span><span class="sxs-lookup"><span data-stu-id="e1e00-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="e1e00-120">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="e1e00-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="e1e00-122">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="e1e00-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Контекстное меню, открытое на таблице Movie](sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](sql/_static/dv.png)

<span data-ttu-id="e1e00-125">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="e1e00-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e1e00-126">По умолчанию EF создает свойство с именем `ID` для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="e1e00-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="e1e00-127">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="e1e00-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="e1e00-129">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="e1e00-129">Seed the database</span></span>

<span data-ttu-id="e1e00-130">Создайте класс `SeedData` в папке *Models*.</span><span class="sxs-lookup"><span data-stu-id="e1e00-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="e1e00-131">Замените сгенерированный код следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e1e00-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="e1e00-132">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="e1e00-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="e1e00-133">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="e1e00-133">Add the seed initializer</span></span>

<span data-ttu-id="e1e00-134">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="e1e00-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="e1e00-135">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e1e00-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="e1e00-136">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="e1e00-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="e1e00-137">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="e1e00-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="e1e00-138">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1e00-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="e1e00-139">Рабочее приложение не вызывает `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="e1e00-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="e1e00-140">Он добавляется в предыдущем коде, чтобы предотвратить следующее исключение, если `Update-Database` не был запущен.</span><span class="sxs-lookup"><span data-stu-id="e1e00-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="e1e00-141">SqlException: не удается открыть базу данных RazorPagesMovieContext-21, запрошенную при входе.</span><span class="sxs-lookup"><span data-stu-id="e1e00-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="e1e00-142">Сбой при входе.</span><span class="sxs-lookup"><span data-stu-id="e1e00-142">The login failed.</span></span>
<span data-ttu-id="e1e00-143">Сбой при входе в систему пользователя user name.</span><span class="sxs-lookup"><span data-stu-id="e1e00-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="e1e00-144">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="e1e00-144">Test the app</span></span>

* <span data-ttu-id="e1e00-145">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="e1e00-145">Delete all the records in the DB.</span></span> <span data-ttu-id="e1e00-146">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="e1e00-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="e1e00-147">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="e1e00-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e1e00-148">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e1e00-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e1e00-149">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="e1e00-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e1e00-150">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="e1e00-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

    * <span data-ttu-id="e1e00-153">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="e1e00-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="e1e00-154">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="e1e00-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="e1e00-155">В приложении отображаются заполненные данные.</span><span class="sxs-lookup"><span data-stu-id="e1e00-155">The app shows the seeded data:</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

<span data-ttu-id="e1e00-157">В следующем учебнике будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="e1e00-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e1e00-158">[Предыдущая тема. Шаблонные страницы Razor](xref:tutorials/razor-pages/page)
> [Следующая тема. Обновление страниц](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="e1e00-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
