---
title: "Добавление нового поля"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 16efbacf-fe7b-4b41-84b0-06a1574b95c2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 52fe9ee7bf006553c7dcb17c00ff659d3797b204
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="adding-a-new-field"></a><span data-ttu-id="5e672-103">Добавление нового поля</span><span class="sxs-lookup"><span data-stu-id="5e672-103">Adding a New Field</span></span>

<span data-ttu-id="5e672-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5e672-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5e672-105">В этом разделе вы будете использовать [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First Migrations для добавления нового поля к модели и переноса этого изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="5e672-105">In this section you'll use [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="5e672-106">Если вы используете EF Code First для автоматического создания базы данных, Code First добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="5e672-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="5e672-107">Если синхронизация нарушена, EF вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="5e672-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="5e672-108">Это позволяет упростить поиск проблем с согласованностью между базой данных и кодом.</span><span class="sxs-lookup"><span data-stu-id="5e672-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="5e672-109">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="5e672-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="5e672-110">Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="5e672-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

<span data-ttu-id="5e672-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="5e672-111">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

<span data-ttu-id="5e672-112">Постройте приложение (CTRL+SHIFT+B).</span><span class="sxs-lookup"><span data-stu-id="5e672-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="5e672-113">Поскольку в класс `Movie` было добавлено новое поле, необходимо также обновить белый список привязки, включив в него новое свойство.</span><span class="sxs-lookup"><span data-stu-id="5e672-113">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="5e672-114">В файле *MoviesController.cs* обновите атрибут `[Bind]` для методов действия `Create` и `Edit`, включив свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="5e672-114">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="5e672-115">Также необходимо обновить шаблоны представлений, чтобы реализовать отображение, создание и редактирование нового свойства `Rating` в представлении браузера.</span><span class="sxs-lookup"><span data-stu-id="5e672-115">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="5e672-116">Измените файл */Views/Movies/Index.cshtml* и добавьте поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="5e672-116">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

<span data-ttu-id="5e672-117">[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]</span><span class="sxs-lookup"><span data-stu-id="5e672-117">[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]</span></span>

<span data-ttu-id="5e672-118">Обновите файл */Views/Movies/Create.cshtml*, указав поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="5e672-118">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="5e672-119">Вы можете скопировать и вставить предыдущую "группу форм" и дождаться автоматического обновления полей с помощью IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="5e672-119">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="5e672-120">IntelliSense работает со [вспомогательными функциями тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="5e672-120">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="5e672-121">Примечание. В RTM-версии Visual Studio 2017 необходимо установить [службы языка Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) для Razor IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="5e672-121">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="5e672-122">Эта неполадка будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="5e672-122">This will be fixed in the next release.</span></span>

![Разработчик ввел букву R в качестве значения атрибута asp-for во втором элементе label представления.](new-field/_static/cr.png)

<span data-ttu-id="5e672-126">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="5e672-126">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="5e672-127">Если запустить приложение сейчас, появится следующее исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="5e672-127">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="5e672-128">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в существующей базе данных.</span><span class="sxs-lookup"><span data-stu-id="5e672-128">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="5e672-129">(В таблице базы данных отсутствует столбец Rating.)</span><span class="sxs-lookup"><span data-stu-id="5e672-129">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="5e672-130">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="5e672-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="5e672-131">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="5e672-131">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="5e672-132">Этот подход удобен на ранних стадиях цикла разработки, когда все действия осуществляются с тестовой базой данных. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="5e672-132">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="5e672-133">Недостатком такого подхода является потеря существующих данных в базе, в связи с чем использовать его в рабочей базе данных невозможно.</span><span class="sxs-lookup"><span data-stu-id="5e672-133">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="5e672-134">При разработке приложения часто используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="5e672-134">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="5e672-135">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="5e672-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="5e672-136">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="5e672-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="5e672-137">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="5e672-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="5e672-138">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="5e672-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="5e672-139">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="5e672-139">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="5e672-140">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="5e672-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="5e672-141">Ниже показан пример изменения, которое необходимо выполнить для каждого `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="5e672-141">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

<span data-ttu-id="5e672-142">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="5e672-142">[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span></span>

<span data-ttu-id="5e672-143">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="5e672-143">Build the solution.</span></span>

<span data-ttu-id="5e672-144">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="5e672-144">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Меню PMC](adding-model/_static/pmc.png)

<span data-ttu-id="5e672-146">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="5e672-146">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Rating
Update-Database
```

<span data-ttu-id="5e672-147">Команда `Add-Migration` указывает платформе миграции на необходимость проверить текущую модель `Movie` с текущей схемой базы данных `Movie` и создать нужный код для переноса базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="5e672-147">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="5e672-148">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="5e672-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="5e672-149">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="5e672-149">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="5e672-150">Если удалить все записи из базы данных, при инициализации она будет заполнена значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="5e672-150">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="5e672-151">Это можно сделать с помощью ссылок удаления в браузере или из SSOX.</span><span class="sxs-lookup"><span data-stu-id="5e672-151">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="5e672-152">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="5e672-152">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="5e672-153">Также следует добавить поле `Rating` в шаблоны представлений `Edit`, `Details` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="5e672-153">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5e672-154">[Назад](search.md)
[Вперед](validation.md)</span><span class="sxs-lookup"><span data-stu-id="5e672-154">[Previous](search.md)
[Next](validation.md)</span></span>  
