---
title: Работа с базой данных и ASP.NET Core
author: rick-anderson
description: В этой статье описывается работа с базой данных и ASP.NET Core.
ms.author: riande
ms.date: 7/22/2019
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 197697f28e9faa45c1ac2b7f993bde15994957e5
ms.sourcegitcommit: 051f068c78931432e030b60094c38376d64d013e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68440374"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="6a38a-103">Работа с базой данных и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6a38a-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="6a38a-104">Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Джо Одетт](https://twitter.com/joeaudette) (Joe Audette)</span><span class="sxs-lookup"><span data-stu-id="6a38a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="6a38a-105">Объект `RazorPagesMovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="6a38a-106">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a38a-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a38a-108">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-108">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="6a38a-109">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="6a38a-110">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6a38a-110">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a38a-112">Значение имени для базы данных (`Database={Database name}`) будет отличаться для созданного кода.</span><span class="sxs-lookup"><span data-stu-id="6a38a-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="6a38a-113">Значение имени является произвольным.</span><span class="sxs-lookup"><span data-stu-id="6a38a-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie30/appsettings.json)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a38a-114">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-114">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="6a38a-115">Если приложение развертывается на тестовом или рабочем сервере, можно задать строку подключения к реальному серверу базы данных с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="6a38a-115">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="6a38a-116">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6a38a-116">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-117">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="6a38a-118">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="6a38a-118">SQL Server Express LocalDB</span></span>

<span data-ttu-id="6a38a-119">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="6a38a-119">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="6a38a-120">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="6a38a-120">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="6a38a-121">По умолчанию база данных LocalDB создает файлы `*.mdf` в каталоге `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-121">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="6a38a-122">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="6a38a-122">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="6a38a-124">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="6a38a-124">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Контекстные меню, открытые на таблице Movie](sql/_static/design.png)

  ![Таблицы Movie, открытые в конструкторе](sql/_static/dv.png)

<span data-ttu-id="6a38a-127">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-127">Note the key icon next to `ID`.</span></span> <span data-ttu-id="6a38a-128">По умолчанию EF создает свойство с именем `ID` для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="6a38a-128">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="6a38a-129">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="6a38a-129">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a38a-131">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-131">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="6a38a-132">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="6a38a-132">Seed the database</span></span>

<span data-ttu-id="6a38a-133">Создайте класс `SeedData` в папке *Models* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6a38a-133">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="6a38a-134">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="6a38a-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="6a38a-135">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="6a38a-135">Add the seed initializer</span></span>

<span data-ttu-id="6a38a-136">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="6a38a-136">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="6a38a-137">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6a38a-137">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="6a38a-138">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="6a38a-138">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="6a38a-139">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="6a38a-139">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="6a38a-140">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="6a38a-140">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Program.cs)]

<span data-ttu-id="6a38a-141">Рабочее приложение не вызывает `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-141">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="6a38a-142">Он добавляется в предыдущем коде, чтобы предотвратить следующее исключение, если `Update-Database` не был запущен.</span><span class="sxs-lookup"><span data-stu-id="6a38a-142">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="6a38a-143">SqlException: Не удается открыть базу данных "RazorPagesMovieContext-21", запрашиваемую именем входа.</span><span class="sxs-lookup"><span data-stu-id="6a38a-143">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="6a38a-144">Сбой при входе.</span><span class="sxs-lookup"><span data-stu-id="6a38a-144">The login failed.</span></span>
<span data-ttu-id="6a38a-145">Сбой при входе в систему пользователя user name.</span><span class="sxs-lookup"><span data-stu-id="6a38a-145">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="6a38a-146">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="6a38a-146">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-147">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a38a-148">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-148">Delete all the records in the DB.</span></span> <span data-ttu-id="6a38a-149">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="6a38a-149">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="6a38a-150">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="6a38a-150">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="6a38a-151">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6a38a-151">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="6a38a-152">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="6a38a-152">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="6a38a-153">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выход** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="6a38a-153">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

    * <span data-ttu-id="6a38a-156">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="6a38a-156">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="6a38a-157">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="6a38a-157">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a38a-158">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="6a38a-159">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="6a38a-159">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="6a38a-160">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-160">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="6a38a-161">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="6a38a-161">The app shows the seeded data.</span></span>

---

<span data-ttu-id="6a38a-162">В следующем руководстве будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-162">The next tutorial will improve the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a38a-163">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6a38a-163">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6a38a-164">[Предыдущая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)
> [Следующая статья. Изменение созданных страниц](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="6a38a-164">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="6a38a-165">Объект `RazorPagesMovieContext` обрабатывает задачу подключения к базе данных и сопоставления объектов `Movie` с записями базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-165">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="6a38a-166">Контекст базы данных регистрируется с помощью контейнера [внедрения зависимостей](xref:fundamentals/dependency-injection) в методе `ConfigureServices` в файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="6a38a-166">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-167">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6a38a-168">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-168">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---

<span data-ttu-id="6a38a-169">Дополнительные сведения о методах, которые используются в `ConfigureServices`, см.:</span><span class="sxs-lookup"><span data-stu-id="6a38a-169">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="6a38a-170">[Общий регламент по защите данных (GDPR), принятый в ЕС, в ASP.NET Core](xref:security/gdpr) для `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-170">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="6a38a-171">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="6a38a-171">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="6a38a-172">Система [конфигурации](xref:fundamentals/configuration/index) ASP.NET Core считывает `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-172">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="6a38a-173">Для разработки на локальном уровне она получает строку подключения из файла *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6a38a-173">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6a38a-175">Значение имени для базы данных (`Database={Database name}`) будет отличаться для созданного кода.</span><span class="sxs-lookup"><span data-stu-id="6a38a-175">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="6a38a-176">Значение имени является произвольным.</span><span class="sxs-lookup"><span data-stu-id="6a38a-176">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a38a-177">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6a38a-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a38a-178">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---

<span data-ttu-id="6a38a-179">Если приложение развертывается на тестовом или рабочем сервере, можно задать строку подключения к реальному серверу базы данных с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="6a38a-179">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="6a38a-180">Дополнительные сведения см. в статье [Конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6a38a-180">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-181">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-181">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="6a38a-182">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="6a38a-182">SQL Server Express LocalDB</span></span>

<span data-ttu-id="6a38a-183">LocalDB — это упрощенная версия ядра СУБД SQL Server Express, предназначенная для разработки программ.</span><span class="sxs-lookup"><span data-stu-id="6a38a-183">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="6a38a-184">LocalDB запускается по запросу в пользовательском режиме, поэтому настройки не слишком сложны.</span><span class="sxs-lookup"><span data-stu-id="6a38a-184">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="6a38a-185">По умолчанию база данных LocalDB создает файлы `*.mdf` в каталоге `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-185">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="6a38a-186">В меню **Вид** откройте **обозреватель объектов SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="6a38a-186">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Меню "Вид"](sql/_static/ssox.png)

* <span data-ttu-id="6a38a-188">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Конструктор представлений**.</span><span class="sxs-lookup"><span data-stu-id="6a38a-188">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Контекстное меню, открытое на таблице Movie](sql/_static/design.png)

  ![Таблица Movie, открытая в конструкторе](sql/_static/dv.png)

<span data-ttu-id="6a38a-191">Обратите внимание на значок с изображением ключа рядом с `ID`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-191">Note the key icon next to `ID`.</span></span> <span data-ttu-id="6a38a-192">По умолчанию EF создает свойство с именем `ID` для первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="6a38a-192">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="6a38a-193">Щелкните правой кнопкой мыши таблицу `Movie` и выберите пункт **Просмотреть данные**.</span><span class="sxs-lookup"><span data-stu-id="6a38a-193">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Открытая таблица Movie с отображением данных](sql/_static/vd22.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a38a-195">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6a38a-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a38a-196">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---

## <a name="seed-the-database"></a><span data-ttu-id="6a38a-197">Заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="6a38a-197">Seed the database</span></span>

<span data-ttu-id="6a38a-198">Создайте класс `SeedData` в папке *Models* со следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="6a38a-198">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="6a38a-199">Если в базе данных есть фильмы, возвращается инициализатор заполнения и фильмы не добавляются.</span><span class="sxs-lookup"><span data-stu-id="6a38a-199">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="6a38a-200">Добавление инициализатора заполнения</span><span class="sxs-lookup"><span data-stu-id="6a38a-200">Add the seed initializer</span></span>

<span data-ttu-id="6a38a-201">В файле *Program.cs* измените метод `Main`, чтобы реализовать следующее:</span><span class="sxs-lookup"><span data-stu-id="6a38a-201">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="6a38a-202">Получение экземпляра контекста базы данных из контейнера внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="6a38a-202">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="6a38a-203">Вызов метода инициализации с передачей ему контекста.</span><span class="sxs-lookup"><span data-stu-id="6a38a-203">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="6a38a-204">Высвобождение контекста после завершения работы метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="6a38a-204">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="6a38a-205">В следующем примере кода показан обновленный файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="6a38a-205">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="6a38a-206">Рабочее приложение не вызывает `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="6a38a-206">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="6a38a-207">Он добавляется в предыдущем коде, чтобы предотвратить следующее исключение, если `Update-Database` не был запущен.</span><span class="sxs-lookup"><span data-stu-id="6a38a-207">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="6a38a-208">SqlException: Не удается открыть базу данных "RazorPagesMovieContext-21", запрашиваемую именем входа.</span><span class="sxs-lookup"><span data-stu-id="6a38a-208">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="6a38a-209">Сбой при входе.</span><span class="sxs-lookup"><span data-stu-id="6a38a-209">The login failed.</span></span>
<span data-ttu-id="6a38a-210">Сбой при входе в систему пользователя user name.</span><span class="sxs-lookup"><span data-stu-id="6a38a-210">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="6a38a-211">Тестирование приложения</span><span class="sxs-lookup"><span data-stu-id="6a38a-211">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6a38a-212">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6a38a-212">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6a38a-213">Удалите все записи из базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-213">Delete all the records in the DB.</span></span> <span data-ttu-id="6a38a-214">Это можно сделать с помощью ссылок удаления в браузере или из [SSOX](xref:tutorials/razor-pages/new-field#ssox).</span><span class="sxs-lookup"><span data-stu-id="6a38a-214">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="6a38a-215">Необходимо выполнить инициализацию (вызывать методы в классе `Startup`), чтобы запустить метод заполнения.</span><span class="sxs-lookup"><span data-stu-id="6a38a-215">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="6a38a-216">Для этого следует остановить и перезапустить IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6a38a-216">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="6a38a-217">Воспользуйтесь одним из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="6a38a-217">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="6a38a-218">Щелкните правой кнопкой мыши значок IIS Express в области уведомлений и выберите **Выйти** или **Остановить сайт**.</span><span class="sxs-lookup"><span data-stu-id="6a38a-218">Right-click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Значок IIS Express в области уведомлений](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Контекстное меню](sql/_static/stopIIS.png)

    * <span data-ttu-id="6a38a-221">Если среда Visual Studio была запущена в режиме без отладки, нажмите клавишу F5 для запуска в режиме отладки.</span><span class="sxs-lookup"><span data-stu-id="6a38a-221">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="6a38a-222">Если среда Visual Studio была запущена в режиме отладки, остановите отладчик и нажмите клавишу F5.</span><span class="sxs-lookup"><span data-stu-id="6a38a-222">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6a38a-223">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="6a38a-223">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6a38a-224">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="6a38a-224">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="6a38a-225">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-225">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="6a38a-226">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="6a38a-226">The app shows the seeded data.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6a38a-227">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="6a38a-227">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6a38a-228">Удалите все записи в базе данных для запуска метода заполнения.</span><span class="sxs-lookup"><span data-stu-id="6a38a-228">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="6a38a-229">Остановите и запустите приложение, чтобы начать заполнение базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-229">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="6a38a-230">В приложении будут отображены данные.</span><span class="sxs-lookup"><span data-stu-id="6a38a-230">The app shows the seeded data.</span></span>

---

<span data-ttu-id="6a38a-231">В приложении отображаются заполненные данные.</span><span class="sxs-lookup"><span data-stu-id="6a38a-231">The app shows the seeded data:</span></span>

![Приложение Movie с данными по фильмам, открытое в Chrome](sql/_static/m55.png)

<span data-ttu-id="6a38a-233">В следующем учебнике будет улучшено представление данных.</span><span class="sxs-lookup"><span data-stu-id="6a38a-233">The next tutorial will clean up the presentation of the data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6a38a-234">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6a38a-234">Additional resources</span></span>

* [<span data-ttu-id="6a38a-235">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="6a38a-235">YouTube version of this tutorial</span></span>](https://youtu.be/A_5ff11sDHY)

> [!div class="step-by-step"]
> <span data-ttu-id="6a38a-236">[Предыдущая статья. Сформированные страницы Razor Pages](xref:tutorials/razor-pages/page)
> [Следующая статья. Изменение созданных страниц](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="6a38a-236">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>

::: moniker-end