---
title: Добавление нового поля на страницу Razor в ASP.NET Core
author: rick-anderson
description: Демонстрирует, как добавить новое поле на страницу Razor с помощью Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 98de39c63c992dce7d60563df316d848339b811a
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410379"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="cdd5c-103">Добавление нового поля на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cdd5c-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="cdd5c-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="cdd5c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="cdd5c-105">В этом разделе [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations используется для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="cdd5c-106">Добавление нового поля в модель.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-106">Add a new field to the model.</span></span>
* <span data-ttu-id="cdd5c-107">Перенос изменений в схеме нового поля в базу данных.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="cdd5c-108">Если вы используете EF Code First для автоматического создания базы данных, Code First:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="cdd5c-109">добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="cdd5c-110">если классы модели не синхронизированы с базой данных, EF выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="cdd5c-111">Автоматическая проверка синхронизации схемы и модели упрощает поиск нарушений согласованности базы данных и кода.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="cdd5c-112">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="cdd5c-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="cdd5c-113">Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="cdd5c-114">Построение приложения.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-114">Build the app.</span></span>

<span data-ttu-id="cdd5c-115">Измените файл *Pages/Movies/Index.cshtml* и добавьте в него поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="cdd5c-116">Обновите следующие страницы:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-116">Update the following pages:</span></span>

* <span data-ttu-id="cdd5c-117">Добавьте поле `Rating` на страницы "Delete" (Удаление) и "Details" (Сведения).</span><span class="sxs-lookup"><span data-stu-id="cdd5c-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="cdd5c-118">Обновите файл [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml), добавив в него поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="cdd5c-119">Добавьте поле `Rating` на страницу "Edit" (Редактирование).</span><span class="sxs-lookup"><span data-stu-id="cdd5c-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="cdd5c-120">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="cdd5c-121">Если запустить приложение сейчас, возникнет исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="cdd5c-122">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="cdd5c-123">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="cdd5c-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="cdd5c-124">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="cdd5c-125">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="cdd5c-126">Этот подход удобен на ранних стадиях цикла разработки. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="cdd5c-127">Недостатком такого подхода является потеря существующих данных в базе.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="cdd5c-128">В рабочей базе данных применять этот подход не следует!</span><span class="sxs-lookup"><span data-stu-id="cdd5c-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="cdd5c-129">При разработке приложения часто выполняется удаление базы данных при изменении схемы, для чего используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="cdd5c-130">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="cdd5c-131">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="cdd5c-132">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="cdd5c-133">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="cdd5c-134">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="cdd5c-135">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="cdd5c-136">Ниже показан пример изменения, которое необходимо выполнить для каждого блока `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="cdd5c-137">См. [готовый файл SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="cdd5c-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="cdd5c-138">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cdd5c-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cdd5c-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="cdd5c-140">Добавление миграции для поля Rating</span><span class="sxs-lookup"><span data-stu-id="cdd5c-140">Add a migration for the rating field</span></span>

<span data-ttu-id="cdd5c-141">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="cdd5c-142">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="cdd5c-143">Команда `Add-Migration` задает следующие инструкции для платформы:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="cdd5c-144">Сравните модель `Movie` со схемой базы данных `Movie`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="cdd5c-145">Создайте код для переноса схемы базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="cdd5c-146">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="cdd5c-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="cdd5c-147">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="cdd5c-148">Команда `Update-Database` указывает платформе, что нужно применить изменения схемы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="cdd5c-149">Если удалить все записи из базы данных, при инициализации она будет заполнена начальными значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="cdd5c-150">Это можно сделать с помощью ссылок удаления в браузере или из [обозревателя объектов SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="cdd5c-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="cdd5c-151">Другой вариант — удалить базу данных и использовать миграции для повторного создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="cdd5c-152">Удаление базы данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="cdd5c-153">Выберите базу данных в SSOX.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="cdd5c-154">Щелкните базу данных правой кнопкой мыши и выберите *Удалить*.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="cdd5c-155">Выберите **Закрыть существующие соединения**.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="cdd5c-156">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-156">Select **OK**.</span></span>
* <span data-ttu-id="cdd5c-157">Обновите базу данных в [PMC](xref:tutorials/razor-pages/new-field#pmc).</span><span class="sxs-lookup"><span data-stu-id="cdd5c-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cdd5c-158">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="cdd5c-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

<span data-ttu-id="cdd5c-159">Выполните следующие команды интерфейса командной строки .NET Core:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-159">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<span data-ttu-id="cdd5c-160">Команда `ef migrations add` задает следующие инструкции для платформы:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-160">The `ef migrations add` command tells the framework to:</span></span>

* <span data-ttu-id="cdd5c-161">Сравните модель `Movie` со схемой базы данных `Movie`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-161">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="cdd5c-162">Создайте код для переноса схемы базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-162">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="cdd5c-163">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="cdd5c-163">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="cdd5c-164">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-164">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="cdd5c-165">Команда `ef database update` указывает платформе, что нужно применить изменения схемы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-165">The `ef database update` command tells the framework to apply the schema changes to the database.</span></span>

<span data-ttu-id="cdd5c-166">Если удалить все записи из базы данных, при инициализации она будет заполнена начальными значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-166">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="cdd5c-167">Это можно сделать с помощью ссылок удаления в браузере или с помощью инструмента SQLite.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-167">You can do this with the delete links in the browser or by using a SQLite tool.</span></span>

<span data-ttu-id="cdd5c-168">Другой вариант — удалить базу данных и использовать миграции для повторного создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-168">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="cdd5c-169">Чтобы удалить базу данных, удалите файл базы данных (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="cdd5c-169">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="cdd5c-170">Затем выполните команду `ef database update`:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-170">Then run the `ef database update` command:</span></span> 

```console
dotnet ef database update
```

> [!NOTE]
> <span data-ttu-id="cdd5c-171">Многие операции изменения схемы не поддерживаются поставщиком EF Core SQLite.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-171">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="cdd5c-172">Например, добавление столбца поддерживается, но удаление столбца не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-172">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="cdd5c-173">При добавлении миграции для удаления столбца команда `ef migrations add` выполняется успешно, а команда `ef database update` — нет.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-173">If you add a migration to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="cdd5c-174">Можно обойти некоторые ограничения вручную, написав код миграции для перестроения таблицы.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-174">You can work around some of the limitations by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="cdd5c-175">Перестройка таблицы включает в себя переименование существующей таблицы, создание новой таблицы, копирование данных в новую таблицу и удаление старой таблицы.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-175">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="cdd5c-176">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="cdd5c-176">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="cdd5c-177">Ограничения поставщика базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="cdd5c-177">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="cdd5c-178">Настройка кода миграции</span><span class="sxs-lookup"><span data-stu-id="cdd5c-178">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="cdd5c-179">Присвоение начальных значений данных</span><span class="sxs-lookup"><span data-stu-id="cdd5c-179">Data seeding</span></span>](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

<span data-ttu-id="cdd5c-180">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-180">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="cdd5c-181">Если база данных не заполнена начальными значениями, задайте точку останова в методе `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="cdd5c-181">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cdd5c-182">[Предыдущая статья. Добавление поиска](xref:tutorials/razor-pages/search)
> [Следующая статья. Добавление проверки](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="cdd5c-182">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
