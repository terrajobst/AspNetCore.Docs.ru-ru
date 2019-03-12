---
title: Работа с базой данных и ASP.NET Core
author: rick-anderson
description: В этой статье описывается работа с базой данных и ASP.NET Core.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 9f01f5c9fcf9c423ccc2af8ae6ca55c2f427c9c1
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346032"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="e07ef-103">Работа с базой данных и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e07ef-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="e07ef-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="e07ef-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="e07ef-105">Объект `RazorPagesMovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="e07ef-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e07ef-106">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e07ef-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e07ef-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e07ef-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e07ef-108">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e07ef-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e07ef-109">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e07ef-109">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="e07ef-110">Дополнительные сведения о методах, которые используются в `ConfigureServices`, см.:</span><span class="sxs-lookup"><span data-stu-id="e07ef-110">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="e07ef-111">[Общий регламент по защите данных (GDPR), принятый в ЕС, в ASP.NET Core](xref:security/gdpr) для `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="e07ef-111">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="e07ef-112">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="e07ef-112">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="e07ef-113">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e07ef-113">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e07ef-114">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e07ef-114">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e07ef-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e07ef-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e07ef-116">Значение имени для базы данных (`Database={Database name}`) будет отличаться для созданного кода.</span><span class="sxs-lookup"><span data-stu-id="e07ef-116">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="e07ef-117">Значение имени является произвольным.</span><span class="sxs-lookup"><span data-stu-id="e07ef-117">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e07ef-118">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e07ef-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e07ef-119">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e07ef-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="e07ef-120">Если приложение развертывается на тестовом или рабочем сервере, можно задать строку подключения к реальному серверу базы данных с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="e07ef-120">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="e07ef-121">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e07ef-121">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e07ef-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e07ef-122">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e07ef-123">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="e07ef-123">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e07ef-124">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="e07ef-124">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="e07ef-125">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="e07ef-125">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="e07ef-126">По умолчанию база данных LocalDB создает файлы `*.mdf` в каталоге `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="e07ef-126">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="e07ef-127">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="e07ef-127">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="e07ef-129">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="e07ef-129">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Контекстное меню, открытое на таблице Movie](sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](sql/_static/dv.png)

<span data-ttu-id="e07ef-132">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="e07ef-132">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e07ef-133">По умолчанию EF создает свойство с именем `ID` для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="e07ef-133">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="e07ef-134">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="e07ef-134">Right click on the `Movie` table and select **View Data**:</span></span>

  <span data-ttu-id="e07ef-135">![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="e07ef-135">![Movie table open showing table data](sql/_static/vd22.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e07ef-136">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e07ef-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e07ef-137">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e07ef-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="e07ef-138">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="e07ef-138">Seed the database</span></span>

<span data-ttu-id="e07ef-139">Создайте класс `SeedData` в папке *Models* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="e07ef-139">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="e07ef-140">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="e07ef-140">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="e07ef-141">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="e07ef-141">Add the seed initializer</span></span>

<span data-ttu-id="e07ef-142">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="e07ef-142">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="e07ef-143">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="e07ef-143">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="e07ef-144">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="e07ef-144">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="e07ef-145">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="e07ef-145">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="e07ef-146">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="e07ef-146">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="e07ef-147">Рабочее приложение не вызывает `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="e07ef-147">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="e07ef-148">Он добавляется в предыдущем коде, чтобы предотвратить следующее исключение, если `Update-Database` не был запущен.</span><span class="sxs-lookup"><span data-stu-id="e07ef-148">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="e07ef-149">SqlException: Не удается открыть базу данных "RazorPagesMovieContext-21", запрашиваемую именем входа.</span><span class="sxs-lookup"><span data-stu-id="e07ef-149">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="e07ef-150">Сбой при входе.</span><span class="sxs-lookup"><span data-stu-id="e07ef-150">The login failed.</span></span>
<span data-ttu-id="e07ef-151">Сбой при входе в систему пользователя user name.</span><span class="sxs-lookup"><span data-stu-id="e07ef-151">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="e07ef-152">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="e07ef-152">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e07ef-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e07ef-153">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e07ef-154">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="e07ef-154">Delete all the records in the DB.</span></span> <span data-ttu-id="e07ef-155">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="e07ef-155">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="e07ef-156">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="e07ef-156">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e07ef-157">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e07ef-157">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e07ef-158">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="e07ef-158">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e07ef-159">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="e07ef-159">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

    * <span data-ttu-id="e07ef-162">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="e07ef-162">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="e07ef-163">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="e07ef-163">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e07ef-164">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e07ef-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e07ef-165">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="e07ef-165">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="e07ef-166">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="e07ef-166">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="e07ef-167">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="e07ef-167">The app shows the seeded data.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e07ef-168">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="e07ef-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e07ef-169">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="e07ef-169">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="e07ef-170">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="e07ef-170">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="e07ef-171">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="e07ef-171">The app shows the seeded data.</span></span>

---  
<!-- End of VS tabs -->


   
<span data-ttu-id="e07ef-172">В приложении отображаются заполненные данные.</span><span class="sxs-lookup"><span data-stu-id="e07ef-172">The app shows the seeded data:</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

<span data-ttu-id="e07ef-174">В следующем учебнике будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="e07ef-174">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e07ef-175">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e07ef-175">Additional resources</span></span>

* [<span data-ttu-id="e07ef-176">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="e07ef-176">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="e07ef-177">[Предыдущая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)
> [Следующая статья. Изменение созданных страниц](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="e07ef-177">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
