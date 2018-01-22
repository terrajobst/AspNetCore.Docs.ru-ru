---
title: "Добавление нового поля на страницу Razor"
author: rick-anderson
description: "Демонстрирует, как добавить новое поле на страницу Razor с помощью Entity Framework Core"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: cd804f127a32f0c6f9488b6bf7bf88be062335d0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="855d8-103">Добавление нового поля на страницу Razor</span><span class="sxs-lookup"><span data-stu-id="855d8-103">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="855d8-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="855d8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="855d8-105">В этом разделе вы будете использовать [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations для добавления нового поля к модели и переноса этого изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="855d8-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="855d8-106">Если вы используете EF Code First для автоматического создания базы данных, Code First добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="855d8-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="855d8-107">Если синхронизация нарушена, EF вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="855d8-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="855d8-108">Это позволяет упростить поиск проблем с согласованностью между базой данных и кодом.</span><span class="sxs-lookup"><span data-stu-id="855d8-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="855d8-109">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="855d8-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="855d8-110">Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="855d8-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="855d8-111">Постройте приложение (CTRL+SHIFT+B).</span><span class="sxs-lookup"><span data-stu-id="855d8-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="855d8-112">Измените файл *Pages/Movies/Index.cshtml* и добавьте в него поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="855d8-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="855d8-113">Добавьте поле `Rating` на страницы "Delete" (Удаление) и "Details" (Сведения).</span><span class="sxs-lookup"><span data-stu-id="855d8-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="855d8-114">Обновите файл *Create.cshtml*, добавив в него поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="855d8-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="855d8-115">Вы можете скопировать и вставить предыдущий элемент `<div>` и дождаться автоматического обновления полей с помощью IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="855d8-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="855d8-116">IntelliSense работает со [вспомогательными функциями тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="855d8-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Разработчик ввел букву R в качестве значения атрибута asp-for во втором элементе label представления.](new-field/_static/cr.png)

<span data-ttu-id="855d8-120">В следующем коде показан файл *Create.cshtml* с полем `Rating`:</span><span class="sxs-lookup"><span data-stu-id="855d8-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="855d8-121">Добавьте поле `Rating` на страницу "Edit" (Редактирование).</span><span class="sxs-lookup"><span data-stu-id="855d8-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="855d8-122">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="855d8-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="855d8-123">Если запустить приложение сейчас, возникнет исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="855d8-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="855d8-124">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в базе данных.</span><span class="sxs-lookup"><span data-stu-id="855d8-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="855d8-125">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="855d8-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="855d8-126">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="855d8-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="855d8-127">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="855d8-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="855d8-128">Этот подход удобен на ранних стадиях цикла разработки. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="855d8-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="855d8-129">Недостатком такого подхода является потеря существующих данных в базе.</span><span class="sxs-lookup"><span data-stu-id="855d8-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="855d8-130">В рабочей базе данных применять этот подход невозможно.</span><span class="sxs-lookup"><span data-stu-id="855d8-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="855d8-131">При разработке приложения часто выполняется удаление базы данных при изменении схемы, для чего используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="855d8-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="855d8-132">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="855d8-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="855d8-133">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="855d8-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="855d8-134">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="855d8-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="855d8-135">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="855d8-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="855d8-136">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="855d8-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="855d8-137">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="855d8-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="855d8-138">Ниже показан пример изменения, которое необходимо выполнить для каждого блока `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="855d8-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="855d8-139">См. [готовый файл SeedData.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="855d8-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="855d8-140">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="855d8-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="855d8-141">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="855d8-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="855d8-142">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="855d8-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="855d8-143">Команда `Add-Migration` задает следующие инструкции для платформы:</span><span class="sxs-lookup"><span data-stu-id="855d8-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="855d8-144">Сравните модель `Movie` со схемой базы данных `Movie`.</span><span class="sxs-lookup"><span data-stu-id="855d8-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="855d8-145">Создайте код для переноса схемы базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="855d8-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="855d8-146">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="855d8-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="855d8-147">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="855d8-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="855d8-148">Если удалить все записи из базы данных, при инициализации она будет заполнена значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="855d8-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="855d8-149">Это можно сделать с помощью ссылок удаления в браузере или из [обозревателя объектов SQL Server](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="855d8-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="855d8-150">Удаление базы данных из SSOX:</span><span class="sxs-lookup"><span data-stu-id="855d8-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="855d8-151">Выберите базу данных в SSOX.</span><span class="sxs-lookup"><span data-stu-id="855d8-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="855d8-152">Щелкните базу данных правой кнопкой мыши и выберите *Удалить*.</span><span class="sxs-lookup"><span data-stu-id="855d8-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="855d8-153">Выберите **Закрыть существующие соединения**.</span><span class="sxs-lookup"><span data-stu-id="855d8-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="855d8-154">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="855d8-154">Select **OK**.</span></span>
* <span data-ttu-id="855d8-155">Обновите базу данных в [PMC](xref:tutorials/razor-pages/new-field#pmc).</span><span class="sxs-lookup"><span data-stu-id="855d8-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="855d8-156">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="855d8-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="855d8-157">Если база данных не заполнена начальными значениями, остановите IIS Express и затем запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="855d8-157">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="855d8-158">[Предыдущая тема — "Добавление поиска"](xref:tutorials/razor-pages/search)
[Следующая тема — "Добавление проверки"](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="855d8-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
