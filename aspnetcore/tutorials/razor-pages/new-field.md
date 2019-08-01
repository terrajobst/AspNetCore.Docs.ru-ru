---
title: Добавление нового поля на страницу Razor в ASP.NET Core
author: rick-anderson
description: Демонстрирует, как добавить новое поле на страницу Razor с помощью Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 4af41422288847c5e77057215390b901d1074c88
ms.sourcegitcommit: 7001657c00358b082734ba4273693b9b3ed35d2a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68670102"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="48886-103">Добавление нового поля на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48886-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="48886-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="48886-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="48886-105">В этом разделе [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations используется для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="48886-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="48886-106">Добавление нового поля в модель.</span><span class="sxs-lookup"><span data-stu-id="48886-106">Add a new field to the model.</span></span>
* <span data-ttu-id="48886-107">Перенос изменений в схеме нового поля в базу данных.</span><span class="sxs-lookup"><span data-stu-id="48886-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="48886-108">Если вы используете EF Code First для автоматического создания базы данных, Code First:</span><span class="sxs-lookup"><span data-stu-id="48886-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="48886-109">добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="48886-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="48886-110">если классы модели не синхронизированы с базой данных, EF выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="48886-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="48886-111">Автоматическая проверка синхронизации схемы и модели упрощает поиск нарушений согласованности базы данных и кода.</span><span class="sxs-lookup"><span data-stu-id="48886-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="48886-112">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="48886-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="48886-113">Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="48886-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="48886-114">Построение приложения.</span><span class="sxs-lookup"><span data-stu-id="48886-114">Build the app.</span></span>

<span data-ttu-id="48886-115">Измените файл *Pages/Movies/Index.cshtml* и добавьте в него поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="48886-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexRating.cshtml?highlight=40-42,62-64)]

<span data-ttu-id="48886-116">Обновите следующие страницы:</span><span class="sxs-lookup"><span data-stu-id="48886-116">Update the following pages:</span></span>

* <span data-ttu-id="48886-117">Добавьте поле `Rating` на страницы "Delete" (Удаление) и "Details" (Сведения).</span><span class="sxs-lookup"><span data-stu-id="48886-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="48886-118">Обновите файл [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml), добавив в него поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48886-118">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="48886-119">Добавьте поле `Rating` на страницу "Edit" (Редактирование).</span><span class="sxs-lookup"><span data-stu-id="48886-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="48886-120">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="48886-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="48886-121">Если запустить приложение сейчас, возникнет исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="48886-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="48886-122">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в базе данных.</span><span class="sxs-lookup"><span data-stu-id="48886-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="48886-123">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="48886-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="48886-124">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="48886-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="48886-125">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="48886-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="48886-126">Этот подход удобен на ранних стадиях цикла разработки. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="48886-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="48886-127">Недостатком такого подхода является потеря существующих данных в базе.</span><span class="sxs-lookup"><span data-stu-id="48886-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="48886-128">В рабочей базе данных применять этот подход не следует!</span><span class="sxs-lookup"><span data-stu-id="48886-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="48886-129">При разработке приложения часто выполняется удаление базы данных при изменении схемы, для чего используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="48886-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="48886-130">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="48886-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="48886-131">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="48886-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="48886-132">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="48886-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="48886-133">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="48886-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="48886-134">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="48886-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="48886-135">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="48886-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="48886-136">Ниже показан пример изменения, которое необходимо выполнить для каждого блока `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="48886-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="48886-137">См. [готовый файл SeedData.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="48886-137">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="48886-138">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="48886-138">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48886-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48886-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="48886-140">Добавление миграции для поля Rating</span><span class="sxs-lookup"><span data-stu-id="48886-140">Add a migration for the rating field</span></span>

<span data-ttu-id="48886-141">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="48886-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="48886-142">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="48886-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="48886-143">Команда `Add-Migration` задает следующие инструкции для платформы:</span><span class="sxs-lookup"><span data-stu-id="48886-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="48886-144">Сравните модель `Movie` со схемой базы данных `Movie`.</span><span class="sxs-lookup"><span data-stu-id="48886-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="48886-145">Создайте код для переноса схемы базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="48886-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="48886-146">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="48886-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="48886-147">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="48886-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="48886-148">Команда `Update-Database` указывает платформе, что нужно применить изменения схемы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="48886-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="48886-149">Если удалить все записи из базы данных, при инициализации она будет заполнена начальными значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48886-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="48886-150">Это можно сделать с помощью ссылок удаления в браузере или из [обозревателя объектов SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="48886-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="48886-151">Другой вариант — удалить базу данных и использовать миграции для повторного создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="48886-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="48886-152">Удаление базы данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="48886-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="48886-153">Выберите базу данных в SSOX.</span><span class="sxs-lookup"><span data-stu-id="48886-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="48886-154">Щелкните базу данных правой кнопкой мыши и выберите *Удалить*.</span><span class="sxs-lookup"><span data-stu-id="48886-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="48886-155">Выберите **Закрыть существующие соединения**.</span><span class="sxs-lookup"><span data-stu-id="48886-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="48886-156">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="48886-156">Select **OK**.</span></span>
* <span data-ttu-id="48886-157">Обновите базу данных в [PMC](xref:tutorials/razor-pages/new-field#pmc).</span><span class="sxs-lookup"><span data-stu-id="48886-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="48886-158">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="48886-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="48886-159">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="48886-159">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="48886-160">Удалите папку миграции.</span><span class="sxs-lookup"><span data-stu-id="48886-160">Delete the migration folder.</span></span>  <span data-ttu-id="48886-161">Используйте следующие команды, чтобы заново создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="48886-161">Use the following commands to recreate the database.</span></span>

```console
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

<span data-ttu-id="48886-162">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48886-162">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="48886-163">Если база данных не заполнена начальными значениями, задайте точку останова в методе `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="48886-163">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48886-164">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="48886-164">Additional resources</span></span>

* [<span data-ttu-id="48886-165">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="48886-165">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="48886-166">[Предыдущая статья. Добавление поиска](xref:tutorials/razor-pages/search)
> [Следующая статья. Добавление проверки](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="48886-166">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="48886-167">В этом разделе [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations используется для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="48886-167">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="48886-168">Добавление нового поля в модель.</span><span class="sxs-lookup"><span data-stu-id="48886-168">Add a new field to the model.</span></span>
* <span data-ttu-id="48886-169">Перенос изменений в схеме нового поля в базу данных.</span><span class="sxs-lookup"><span data-stu-id="48886-169">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="48886-170">Если вы используете EF Code First для автоматического создания базы данных, Code First:</span><span class="sxs-lookup"><span data-stu-id="48886-170">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="48886-171">добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="48886-171">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="48886-172">если классы модели не синхронизированы с базой данных, EF выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="48886-172">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="48886-173">Автоматическая проверка синхронизации схемы и модели упрощает поиск нарушений согласованности базы данных и кода.</span><span class="sxs-lookup"><span data-stu-id="48886-173">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="48886-174">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="48886-174">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="48886-175">Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="48886-175">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="48886-176">Построение приложения.</span><span class="sxs-lookup"><span data-stu-id="48886-176">Build the app.</span></span>

<span data-ttu-id="48886-177">Измените файл *Pages/Movies/Index.cshtml* и добавьте в него поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="48886-177">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="48886-178">Обновите следующие страницы:</span><span class="sxs-lookup"><span data-stu-id="48886-178">Update the following pages:</span></span>

* <span data-ttu-id="48886-179">Добавьте поле `Rating` на страницы "Delete" (Удаление) и "Details" (Сведения).</span><span class="sxs-lookup"><span data-stu-id="48886-179">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="48886-180">Обновите файл [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml), добавив в него поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48886-180">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="48886-181">Добавьте поле `Rating` на страницу "Edit" (Редактирование).</span><span class="sxs-lookup"><span data-stu-id="48886-181">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="48886-182">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="48886-182">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="48886-183">Если запустить приложение сейчас, возникнет исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="48886-183">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="48886-184">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в базе данных.</span><span class="sxs-lookup"><span data-stu-id="48886-184">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="48886-185">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="48886-185">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="48886-186">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="48886-186">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="48886-187">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="48886-187">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="48886-188">Этот подход удобен на ранних стадиях цикла разработки. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="48886-188">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="48886-189">Недостатком такого подхода является потеря существующих данных в базе.</span><span class="sxs-lookup"><span data-stu-id="48886-189">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="48886-190">В рабочей базе данных применять этот подход не следует!</span><span class="sxs-lookup"><span data-stu-id="48886-190">Don't use this approach on a production database!</span></span> <span data-ttu-id="48886-191">При разработке приложения часто выполняется удаление базы данных при изменении схемы, для чего используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="48886-191">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="48886-192">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="48886-192">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="48886-193">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="48886-193">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="48886-194">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="48886-194">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="48886-195">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="48886-195">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="48886-196">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="48886-196">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="48886-197">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="48886-197">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="48886-198">Ниже показан пример изменения, которое необходимо выполнить для каждого блока `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="48886-198">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="48886-199">См. [готовый файл SeedData.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="48886-199">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="48886-200">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="48886-200">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="48886-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="48886-201">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="48886-202">Добавление миграции для поля Rating</span><span class="sxs-lookup"><span data-stu-id="48886-202">Add a migration for the rating field</span></span>

<span data-ttu-id="48886-203">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="48886-203">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="48886-204">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="48886-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="48886-205">Команда `Add-Migration` задает следующие инструкции для платформы:</span><span class="sxs-lookup"><span data-stu-id="48886-205">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="48886-206">Сравните модель `Movie` со схемой базы данных `Movie`.</span><span class="sxs-lookup"><span data-stu-id="48886-206">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="48886-207">Создайте код для переноса схемы базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="48886-207">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="48886-208">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="48886-208">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="48886-209">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="48886-209">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="48886-210">Команда `Update-Database` указывает платформе, что нужно применить изменения схемы к базе данных.</span><span class="sxs-lookup"><span data-stu-id="48886-210">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="48886-211">Если удалить все записи из базы данных, при инициализации она будет заполнена начальными значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48886-211">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="48886-212">Это можно сделать с помощью ссылок удаления в браузере или из [обозревателя объектов SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="48886-212">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="48886-213">Другой вариант — удалить базу данных и использовать миграции для повторного создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="48886-213">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="48886-214">Удаление базы данных в SSOX:</span><span class="sxs-lookup"><span data-stu-id="48886-214">To delete the database in SSOX:</span></span>

* <span data-ttu-id="48886-215">Выберите базу данных в SSOX.</span><span class="sxs-lookup"><span data-stu-id="48886-215">Select the database in SSOX.</span></span>
* <span data-ttu-id="48886-216">Щелкните базу данных правой кнопкой мыши и выберите *Удалить*.</span><span class="sxs-lookup"><span data-stu-id="48886-216">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="48886-217">Выберите **Закрыть существующие соединения**.</span><span class="sxs-lookup"><span data-stu-id="48886-217">Check **Close existing connections**.</span></span>
* <span data-ttu-id="48886-218">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="48886-218">Select **OK**.</span></span>
* <span data-ttu-id="48886-219">Обновите базу данных в [PMC](xref:tutorials/razor-pages/new-field#pmc).</span><span class="sxs-lookup"><span data-stu-id="48886-219">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="48886-220">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="48886-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="48886-221">Удаление и повторное создание базы данных</span><span class="sxs-lookup"><span data-stu-id="48886-221">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="48886-222">Удалите базу данных и используйте миграции для повторного создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="48886-222">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="48886-223">Чтобы удалить базу данных, удалите файл базы данных (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="48886-223">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="48886-224">Затем выполните команду `ef database update`:</span><span class="sxs-lookup"><span data-stu-id="48886-224">Then run the `ef database update` command:</span></span>

```console
dotnet ef database update
```

---

<span data-ttu-id="48886-225">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="48886-225">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="48886-226">Если база данных не заполнена начальными значениями, задайте точку останова в методе `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="48886-226">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48886-227">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="48886-227">Additional resources</span></span>

* [<span data-ttu-id="48886-228">Версия руководства на YouTube</span><span class="sxs-lookup"><span data-stu-id="48886-228">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="48886-229">[Предыдущая статья. Добавление поиска](xref:tutorials/razor-pages/search)
> [Следующая статья. Добавление проверки](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="48886-229">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end
