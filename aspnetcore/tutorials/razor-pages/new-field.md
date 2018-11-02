---
title: Добавление нового поля на страницу Razor в ASP.NET Core
author: rick-anderson
description: Демонстрирует, как добавить новое поле на страницу Razor с помощью Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: f8be269887903797803257d8a21e002519102047
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089517"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="51869-103">Добавление нового поля на страницу Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51869-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="51869-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="51869-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="51869-105">В этом разделе вы будете использовать [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations для добавления нового поля к модели и переноса этого изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="51869-105">In this section you use [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="51869-106">Если вы используете EF Code First для автоматического создания базы данных, Code First:</span><span class="sxs-lookup"><span data-stu-id="51869-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="51869-107">добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="51869-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="51869-108">если классы модели не синхронизированы с базой данных, EF выдает исключение.</span><span class="sxs-lookup"><span data-stu-id="51869-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="51869-109">Автоматическая проверка синхронизации схемы и модели упрощает поиск нарушений согласованности базы данных и кода.</span><span class="sxs-lookup"><span data-stu-id="51869-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="51869-110">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="51869-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="51869-111">Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="51869-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="51869-112">Постройте приложение (CTRL+SHIFT+B).</span><span class="sxs-lookup"><span data-stu-id="51869-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="51869-113">Измените файл *Pages/Movies/Index.cshtml* и добавьте в него поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="51869-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="51869-114">Добавьте поле `Rating` на страницы "Delete" (Удаление) и "Details" (Сведения).</span><span class="sxs-lookup"><span data-stu-id="51869-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="51869-115">Обновите файл *Create.cshtml*, добавив в него поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="51869-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="51869-116">Вы можете скопировать и вставить предыдущий элемент `<div>` и дождаться автоматического обновления полей с помощью IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="51869-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="51869-117">IntelliSense работает со [вспомогательными функциями тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="51869-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Разработчик ввел букву R в качестве значения атрибута asp-for во втором элементе label представления.](new-field/_static/cr.png)

<span data-ttu-id="51869-121">В следующем коде показан файл *Create.cshtml* с полем `Rating`:</span><span class="sxs-lookup"><span data-stu-id="51869-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="51869-122">Добавьте поле `Rating` на страницу "Edit" (Редактирование).</span><span class="sxs-lookup"><span data-stu-id="51869-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="51869-123">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="51869-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="51869-124">Если запустить приложение сейчас, возникнет исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="51869-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="51869-125">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в базе данных.</span><span class="sxs-lookup"><span data-stu-id="51869-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="51869-126">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="51869-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="51869-127">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="51869-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="51869-128">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="51869-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="51869-129">Этот подход удобен на ранних стадиях цикла разработки. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="51869-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="51869-130">Недостатком такого подхода является потеря существующих данных в базе.</span><span class="sxs-lookup"><span data-stu-id="51869-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="51869-131">В рабочей базе данных применять этот подход невозможно.</span><span class="sxs-lookup"><span data-stu-id="51869-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="51869-132">При разработке приложения часто выполняется удаление базы данных при изменении схемы, для чего используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="51869-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="51869-133">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="51869-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="51869-134">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="51869-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="51869-135">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="51869-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="51869-136">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="51869-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="51869-137">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="51869-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="51869-138">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="51869-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="51869-139">Ниже показан пример изменения, которое необходимо выполнить для каждого блока `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="51869-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="51869-140">См. [готовый файл SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="51869-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="51869-141">См. [готовый файл SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="51869-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>

::: moniker-end

<span data-ttu-id="51869-142">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="51869-142">Build the solution.</span></span>

<a name="pmc"></a>

<span data-ttu-id="51869-143">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="51869-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="51869-144">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="51869-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="51869-145">Команда `Add-Migration` задает следующие инструкции для платформы:</span><span class="sxs-lookup"><span data-stu-id="51869-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="51869-146">Сравните модель `Movie` со схемой базы данных `Movie`.</span><span class="sxs-lookup"><span data-stu-id="51869-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="51869-147">Создайте код для переноса схемы базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="51869-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="51869-148">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="51869-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="51869-149">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="51869-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a>

<span data-ttu-id="51869-150">Если удалить все записи из базы данных, при инициализации она будет заполнена начальными значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="51869-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="51869-151">Это можно сделать с помощью ссылок удаления в браузере или из [обозревателя объектов SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="51869-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="51869-152">Удаление базы данных из SSOX:</span><span class="sxs-lookup"><span data-stu-id="51869-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="51869-153">Выберите базу данных в SSOX.</span><span class="sxs-lookup"><span data-stu-id="51869-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="51869-154">Щелкните базу данных правой кнопкой мыши и выберите *Удалить*.</span><span class="sxs-lookup"><span data-stu-id="51869-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="51869-155">Выберите **Закрыть существующие соединения**.</span><span class="sxs-lookup"><span data-stu-id="51869-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="51869-156">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="51869-156">Select **OK**.</span></span>
* <span data-ttu-id="51869-157">Обновите базу данных в [PMC](xref:tutorials/razor-pages/new-field#pmc).</span><span class="sxs-lookup"><span data-stu-id="51869-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="51869-158">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="51869-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="51869-159">Если база данных не заполнена начальными значениями, остановите IIS Express и затем запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="51869-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="51869-160">[Предыдущая тема — "Добавление поиска"](xref:tutorials/razor-pages/search)
> [Следующая тема — "Добавление проверки"](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="51869-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
