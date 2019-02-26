---
title: Добавление нового поля в приложение MVC ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать Entity Framework Code First Migrations для добавления нового поля к модели и переноса этого изменения в базу данных.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 7993b36bf9115225e082d2929bb253aba5b18310
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207373"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="95cf7-103">Добавление нового поля в приложение MVC ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="95cf7-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="95cf7-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="95cf7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="95cf7-105">В этом разделе [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations используется для выполнения следующих задач:</span><span class="sxs-lookup"><span data-stu-id="95cf7-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="95cf7-106">Добавление нового поля в модель.</span><span class="sxs-lookup"><span data-stu-id="95cf7-106">Add a new field to the model.</span></span>
* <span data-ttu-id="95cf7-107">Перенос нового поля в базу данных.</span><span class="sxs-lookup"><span data-stu-id="95cf7-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="95cf7-108">Если вы используете EF Code First для автоматического создания базы данных, Code First:</span><span class="sxs-lookup"><span data-stu-id="95cf7-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="95cf7-109">Добавляет таблицу в базу данных для отслеживания схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="95cf7-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="95cf7-110">Проверяет, синхронизирована ли база данных с классами модели, из которых она была создана.</span><span class="sxs-lookup"><span data-stu-id="95cf7-110">Verify the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="95cf7-111">Если синхронизация нарушена, EF вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="95cf7-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="95cf7-112">Это позволяет упростить поиск проблем с согласованностью между базой данных и кодом.</span><span class="sxs-lookup"><span data-stu-id="95cf7-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="95cf7-113">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="95cf7-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="95cf7-114">Добавьте свойство `Rating` в *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="95cf7-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="95cf7-115">Постройте приложение (CTRL+SHIFT+B).</span><span class="sxs-lookup"><span data-stu-id="95cf7-115">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="95cf7-116">Поскольку в класс `Movie` было добавлено новое поле, необходимо обновить утвержденный список привязки, включив в него новое свойство.</span><span class="sxs-lookup"><span data-stu-id="95cf7-116">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="95cf7-117">В файле *MoviesController.cs* обновите атрибут `[Bind]` для методов действия `Create` и `Edit`, включив свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="95cf7-117">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="95cf7-118">Обновите шаблоны представлений, чтобы реализовать отображение, создание и редактирование нового свойства `Rating` в представлении браузера.</span><span class="sxs-lookup"><span data-stu-id="95cf7-118">Update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="95cf7-119">Измените файл */Views/Movies/Index.cshtml* и добавьте поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="95cf7-119">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

<span data-ttu-id="95cf7-120">Обновите файл */Views/Movies/Create.cshtml*, указав поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="95cf7-120">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<!-- VS -------------------------->
# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="95cf7-121">Visual Studio / Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="95cf7-121">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="95cf7-122">Вы можете скопировать и вставить предыдущую "группу форм" и дождаться автоматического обновления полей с помощью IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="95cf7-122">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="95cf7-123">IntelliSense работает со [вспомогательными функциями тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="95cf7-123">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Разработчик ввел букву R в качестве значения атрибута asp-for во втором элементе label представления.](new-field/_static/cr.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="95cf7-127">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="95cf7-127">Visual Studio Code</span></span>](#tab/visual-studio-code)
<!-- This tab intentionally left blank. -->
---  
<!-- End of VS tabs -->

<span data-ttu-id="95cf7-128">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="95cf7-128">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="95cf7-129">Ниже показан пример изменения, которое необходимо выполнить для каждого `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="95cf7-129">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="95cf7-130">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="95cf7-130">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="95cf7-131">Если он запущена, возникает следующее исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="95cf7-131">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="95cf7-132">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в существующей базе данных.</span><span class="sxs-lookup"><span data-stu-id="95cf7-132">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="95cf7-133">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="95cf7-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="95cf7-134">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="95cf7-134">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="95cf7-135">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="95cf7-135">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="95cf7-136">Этот подход удобен на ранних стадиях цикла разработки, когда все действия осуществляются с тестовой базой данных. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="95cf7-136">This approach is very convenient early in the development cycle when you're doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="95cf7-137">Недостатком такого подхода является потеря существующих данных в базе, в связи с чем использовать его в рабочей базе данных невозможно.</span><span class="sxs-lookup"><span data-stu-id="95cf7-137">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="95cf7-138">При разработке приложения часто используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="95cf7-138">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="95cf7-139">Это хороший подход на ранних этапах разработки и при использовании SQLite.</span><span class="sxs-lookup"><span data-stu-id="95cf7-139">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="95cf7-140">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="95cf7-140">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="95cf7-141">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="95cf7-141">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="95cf7-142">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="95cf7-142">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="95cf7-143">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="95cf7-143">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="95cf7-144">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="95cf7-144">For this tutorial, Code First Migrations is used.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="95cf7-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95cf7-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="95cf7-146">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="95cf7-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Меню PMC](adding-model/_static/pmc.png)

<span data-ttu-id="95cf7-148">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="95cf7-148">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="95cf7-149">Команда `Add-Migration` указывает платформе миграции на необходимость проверить текущую модель `Movie` с текущей схемой базы данных `Movie` и создать нужный код для переноса базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="95cf7-149">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="95cf7-150">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="95cf7-150">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="95cf7-151">Выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="95cf7-151">Run the following command:</span></span>

```cli
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="95cf7-152">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="95cf7-152">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="95cf7-153">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="95cf7-153">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="95cf7-154">Если удалить все записи из базы данных, при инициализации она будет заполнена значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="95cf7-154">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

<span data-ttu-id="95cf7-155">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="95cf7-155">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="95cf7-156">Следует добавить поле `Rating` в шаблоны представлений `Edit`, `Details` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="95cf7-156">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="95cf7-157">[Назад](search.md)
> [Вперед](validation.md)</span><span class="sxs-lookup"><span data-stu-id="95cf7-157">[Previous](search.md)
[Next](validation.md)</span></span>  
