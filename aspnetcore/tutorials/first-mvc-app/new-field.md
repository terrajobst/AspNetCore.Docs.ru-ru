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
ms.openlocfilehash: 9c872a48aba4974ddac2e49ca40c944da356f0e0
ms.sourcegitcommit: 79bbe7481c3d1297a0db8e41dd2b635b0f778264
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-new-field"></a><span data-ttu-id="98953-103">Добавление нового поля</span><span class="sxs-lookup"><span data-stu-id="98953-103">Adding a New Field</span></span>

<span data-ttu-id="98953-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="98953-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="98953-105">В этом разделе вы будете использовать [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations для добавления нового поля к модели и переноса этого изменения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="98953-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="98953-106">Если вы используете EF Code First для автоматического создания базы данных, Code First добавляет в нее таблицу, которая позволяет отслеживать синхронизацию схемы базы данных с классами модели, на основе которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="98953-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="98953-107">Если синхронизация нарушена, EF вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="98953-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="98953-108">Это позволяет упростить поиск проблем с согласованностью между базой данных и кодом.</span><span class="sxs-lookup"><span data-stu-id="98953-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="98953-109">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="98953-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="98953-110">Откройте файл *Models/Movie.cs* и добавьте свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="98953-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="98953-111">Постройте приложение (CTRL+SHIFT+B).</span><span class="sxs-lookup"><span data-stu-id="98953-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="98953-112">Поскольку в класс `Movie` было добавлено новое поле, необходимо также обновить белый список привязки, включив в него новое свойство.</span><span class="sxs-lookup"><span data-stu-id="98953-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="98953-113">В файле *MoviesController.cs* обновите атрибут `[Bind]` для методов действия `Create` и `Edit`, включив свойство `Rating`:</span><span class="sxs-lookup"><span data-stu-id="98953-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="98953-114">Также необходимо обновить шаблоны представлений, чтобы реализовать отображение, создание и редактирование нового свойства `Rating` в представлении браузера.</span><span class="sxs-lookup"><span data-stu-id="98953-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="98953-115">Измените файл */Views/Movies/Index.cshtml* и добавьте поле `Rating`:</span><span class="sxs-lookup"><span data-stu-id="98953-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="98953-116">Обновите файл */Views/Movies/Create.cshtml*, указав поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="98953-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="98953-117">Вы можете скопировать и вставить предыдущую "группу форм" и дождаться автоматического обновления полей с помощью IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="98953-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="98953-118">IntelliSense работает со [вспомогательными функциями тегов](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="98953-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="98953-119">Примечание. В RTM-версии Visual Studio 2017 необходимо установить [службы языка Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) для Razor IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="98953-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="98953-120">Эта неполадка будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="98953-120">This will be fixed in the next release.</span></span>

![Разработчик ввел букву R в качестве значения атрибута asp-for во втором элементе label представления.](new-field/_static/cr.png)

<span data-ttu-id="98953-124">Для работы приложения необходимо обновить базу данных, включив в нее новое поле.</span><span class="sxs-lookup"><span data-stu-id="98953-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="98953-125">Если запустить приложение сейчас, появится следующее исключение `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="98953-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="98953-126">Эта ошибка связана с тем, что обновленный класс модели Movie отличается от схемы таблицы Movie в существующей базе данных.</span><span class="sxs-lookup"><span data-stu-id="98953-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="98953-127">(В таблице базы данных отсутствует столбец Rating.)</span><span class="sxs-lookup"><span data-stu-id="98953-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="98953-128">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="98953-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="98953-129">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="98953-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="98953-130">Этот подход удобен на ранних стадиях цикла разработки, когда все действия осуществляются с тестовой базой данных. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="98953-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="98953-131">Недостатком такого подхода является потеря существующих данных в базе, в связи с чем использовать его в рабочей базе данных невозможно.</span><span class="sxs-lookup"><span data-stu-id="98953-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="98953-132">При разработке приложения часто используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="98953-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="98953-133">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="98953-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="98953-134">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="98953-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="98953-135">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="98953-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="98953-136">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="98953-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="98953-137">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="98953-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="98953-138">Обновите класс `SeedData` так, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="98953-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="98953-139">Ниже показан пример изменения, которое необходимо выполнить для каждого `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="98953-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="98953-140">Постройте решение.</span><span class="sxs-lookup"><span data-stu-id="98953-140">Build the solution.</span></span>

<span data-ttu-id="98953-141">В меню **Сервис** последовательно выберите пункты **Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="98953-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Меню PMC](adding-model/_static/pmc.png)

<span data-ttu-id="98953-143">В PMC введите следующие команды:</span><span class="sxs-lookup"><span data-stu-id="98953-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="98953-144">Команда `Add-Migration` указывает платформе миграции на необходимость проверить текущую модель `Movie` с текущей схемой базы данных `Movie` и создать нужный код для переноса базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="98953-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="98953-145">В качестве имени файла переноса используется произвольное имя "Rating".</span><span class="sxs-lookup"><span data-stu-id="98953-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="98953-146">Рекомендуется присваивать этому файлу понятное имя.</span><span class="sxs-lookup"><span data-stu-id="98953-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="98953-147">Если удалить все записи из базы данных, при инициализации она будет заполнена значениями и в нее будет включено поле `Rating`.</span><span class="sxs-lookup"><span data-stu-id="98953-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="98953-148">Это можно сделать с помощью ссылок удаления в браузере или из SSOX.</span><span class="sxs-lookup"><span data-stu-id="98953-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="98953-149">Запустите приложение и проверьте возможность создания, редактирования и отображения фильмов с использованием поля `Rating`.</span><span class="sxs-lookup"><span data-stu-id="98953-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="98953-150">Также следует добавить поле `Rating` в шаблоны представлений `Edit`, `Details` и `Delete`.</span><span class="sxs-lookup"><span data-stu-id="98953-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="98953-151">[Назад](search.md)
[Вперед](validation.md)</span><span class="sxs-lookup"><span data-stu-id="98953-151">[Previous](search.md)
[Next](validation.md)</span></span>  
