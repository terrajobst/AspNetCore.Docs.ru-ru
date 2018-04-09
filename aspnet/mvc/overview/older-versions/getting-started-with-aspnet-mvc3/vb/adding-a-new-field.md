---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Добавление нового поля в таблице базы данных (Visual Basic) и модели фильма | Документы Microsoft
author: Rick-Anderson
description: Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, являющийся...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a><span data-ttu-id="36dde-103">Добавление нового поля в таблице базы данных (Visual Basic) и модели фильма</span><span class="sxs-lookup"><span data-stu-id="36dde-103">Adding a New Field to the Movie Model and Database Table (VB)</span></span>
====================
<span data-ttu-id="36dde-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="36dde-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="36dde-105">Этот учебник поможет узнать основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой — это бесплатная версия Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36dde-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="36dde-106">Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже.</span><span class="sxs-lookup"><span data-stu-id="36dde-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="36dde-107">Все из них можно установить, щелкнув по следующей ссылке: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="36dde-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="36dde-108">Кроме того можно установить отдельно предварительные требования, используя следующие ссылки:</span><span class="sxs-lookup"><span data-stu-id="36dde-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="36dde-109">Необходимые компоненты Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="36dde-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="36dde-110">Обновление средств ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="36dde-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="36dde-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среда выполнения + средства поддержки)</span><span class="sxs-lookup"><span data-stu-id="36dde-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="36dde-112">Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув по следующей ссылке: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="36dde-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="36dde-113">Проект Visual Web Developer с VB.NET исходный код доступен по следующему адресу.</span><span class="sxs-lookup"><span data-stu-id="36dde-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="36dde-114">[Загрузить версию VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="36dde-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="36dde-115">Если вы предпочитаете C#, переключитесь в [C# версии](../cs/adding-a-new-field.md) этого учебника.</span><span class="sxs-lookup"><span data-stu-id="36dde-115">If you prefer C#, switch to the [C# version](../cs/adding-a-new-field.md) of this tutorial.</span></span>


<span data-ttu-id="36dde-116">В этом разделе будет внести некоторые изменения в классы модели и узнайте, как можно обновить схему базы данных в соответствии с изменениями в модели.</span><span class="sxs-lookup"><span data-stu-id="36dde-116">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="36dde-117">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="36dde-117">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="36dde-118">Начните с добавления нового `Rating` свойства для существующего `Movie` класса.</span><span class="sxs-lookup"><span data-stu-id="36dde-118">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="36dde-119">Откройте *Movie.cs* и добавьте `Rating` свойства следующим образом:</span><span class="sxs-lookup"><span data-stu-id="36dde-119">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

<span data-ttu-id="36dde-120">Полный `Movie` класса сейчас выглядит как следующий код:</span><span class="sxs-lookup"><span data-stu-id="36dde-120">The complete `Movie` class now looks like the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

<span data-ttu-id="36dde-121">Перекомпилировать приложение с помощью **отладки** &gt; **построения фильма** команду меню.</span><span class="sxs-lookup"><span data-stu-id="36dde-121">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="36dde-122">Теперь, когда вы обновили `Model` класса, необходимо также обновить *\Views\Movies\Index.vbhtml* и *\Views\Movies\Create.vbhtml* просмотра шаблонов для поддержки новых `Rating`свойство.</span><span class="sxs-lookup"><span data-stu-id="36dde-122">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.vbhtml* and *\Views\Movies\Create.vbhtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="36dde-123">Откройте<em>\Views\Movies\Index.vbhtml</em> и добавьте `<th>Rating</th>` сразу после заголовка столбца <strong>цены</strong> столбца.</span><span class="sxs-lookup"><span data-stu-id="36dde-123">Open the<em>\Views\Movies\Index.vbhtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="36dde-124">Затем добавьте `<td>` столбец ближе к концу шаблона для отображения `@item.Rating` значение.</span><span class="sxs-lookup"><span data-stu-id="36dde-124">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="36dde-125">Ниже приведен какие обновленный <em>Index.vbhtml</em> Просмотр шаблона выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="36dde-125">Below is what the updated <em>Index.vbhtml</em> view template looks like:</span></span>

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

<span data-ttu-id="36dde-126">Затем откройте *\Views\Movies\Create.vbhtml* и добавьте следующую разметку в конец формы.</span><span class="sxs-lookup"><span data-stu-id="36dde-126">Next, open the *\Views\Movies\Create.vbhtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="36dde-127">Это отрисовывает текстовое поле, рейтинг можно указать при создании новых фильмов.</span><span class="sxs-lookup"><span data-stu-id="36dde-127">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="36dde-128">Управление модели и различий схемы базы данных</span><span class="sxs-lookup"><span data-stu-id="36dde-128">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="36dde-129">Теперь вы обновили код приложения, поддерживающие новое `Rating` свойство.</span><span class="sxs-lookup"><span data-stu-id="36dde-129">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="36dde-130">Теперь запустите приложение и перейдите к */Movies* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="36dde-130">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="36dde-131">При этом, вы увидите следующую ошибку:</span><span class="sxs-lookup"><span data-stu-id="36dde-131">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="36dde-132">Вы видите эту ошибку, так как обновленный `Movie` класс модели в приложении теперь отличается от схемы `Movie` существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="36dde-132">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="36dde-133">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="36dde-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="36dde-134">По умолчанию при использовании Entity Framework Code First автоматически создать базу данных, как это было сделано ранее в этом учебнике Code First добавляет таблицу в базу данных для отслеживания, является ли синхронизация с помощью классов модели, он был создан из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="36dde-134">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="36dde-135">Если они не являются синхронизированными, Entity Framework выдает ошибку.</span><span class="sxs-lookup"><span data-stu-id="36dde-135">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="36dde-136">Это упрощает для выявления проблем во время разработки, которые может в противном случае только найти (непонятные ошибки) во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="36dde-136">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="36dde-137">Средство проверки синхронизации, в каком случае сообщение об ошибке для отображения, который вы видели.</span><span class="sxs-lookup"><span data-stu-id="36dde-137">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="36dde-138">Существует два подхода к устранению ошибки.</span><span class="sxs-lookup"><span data-stu-id="36dde-138">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="36dde-139">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="36dde-139">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="36dde-140">Этот подход очень удобна при выполнении active разработки на тестовую базу данных, так как позволяет быстро меняются схему модели и базы данных вместе.</span><span class="sxs-lookup"><span data-stu-id="36dde-140">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="36dde-141">Недостатком такого подхода является потеря существующие данные в базе данных, поэтому вы *не* Чтобы использовать этот подход для производственной базы данных!</span><span class="sxs-lookup"><span data-stu-id="36dde-141">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="36dde-142">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="36dde-142">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="36dde-143">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="36dde-143">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="36dde-144">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="36dde-144">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="36dde-145">В этом учебнике мы будем использовать первый подход — у вас Entity Framework Code First автоматически воссоздать базу данных, каждый раз, когда изменения модели.</span><span class="sxs-lookup"><span data-stu-id="36dde-145">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="36dde-146">Автоматическое повторное создание базы данных на изменения в модели</span><span class="sxs-lookup"><span data-stu-id="36dde-146">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="36dde-147">Давайте обновить приложение, чтобы Code First автоматически удаляет и повторно создает базу данных в любое время изменить модель для приложения.</span><span class="sxs-lookup"><span data-stu-id="36dde-147">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="36dde-148">**Предупреждение** следует включить автоматическое удаление и повторное создание базы данных только в том случае, если вы используете разработки или тестирования базы данных, этот подход и *никогда не* рабочей базы данных, которая содержит реальные данные.</span><span class="sxs-lookup"><span data-stu-id="36dde-148">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="36dde-149">С его помощью на рабочем сервере могут привести к потере данных.</span><span class="sxs-lookup"><span data-stu-id="36dde-149">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="36dde-150">В **обозревателе решений**, щелкните правой кнопкой мыши *моделей* выберите **добавить**и выберите **класса**.</span><span class="sxs-lookup"><span data-stu-id="36dde-150">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="36dde-151">Имя класса &quot;MovieInitializer&quot;.</span><span class="sxs-lookup"><span data-stu-id="36dde-151">Name the class &quot;MovieInitializer&quot;.</span></span> <span data-ttu-id="36dde-152">Обновление `MovieInitializer` класс, содержащий следующий код:</span><span class="sxs-lookup"><span data-stu-id="36dde-152">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

<span data-ttu-id="36dde-153">`MovieInitializer` Класс указывает, удален базы данных, используемых в модели и они будут автоматически создан повторно, если когда-либо изменить классы модели.</span><span class="sxs-lookup"><span data-stu-id="36dde-153">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="36dde-154">Код включает `Seed` метод, чтобы указать время, некоторые данные по умолчанию, чтобы автоматически добавить в базу данных любой создании (или повторном создании).</span><span class="sxs-lookup"><span data-stu-id="36dde-154">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="36dde-155">Это обеспечивает удобный способ заполнения базы данных с образцами данных, без необходимости вручную заполнять каждый раз при внесении изменения модели.</span><span class="sxs-lookup"><span data-stu-id="36dde-155">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="36dde-156">Теперь, когда вы определили `MovieInitializer` класс, имеет смысл подключить его, чтобы каждый раз при запуске приложения, он проверяет, различаются ли модель классов из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="36dde-156">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="36dde-157">Если это так, можно запустить инициализатор для повторного создания базы данных совпадает с моделью, а затем заполнить базу данных с образцом данных.</span><span class="sxs-lookup"><span data-stu-id="36dde-157">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="36dde-158">Откройте *Global.asax* файл, находящийся в корне `MvcMovies` проекта:</span><span class="sxs-lookup"><span data-stu-id="36dde-158">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

<span data-ttu-id="36dde-159">*Global.asax* файл содержит класс, определяющий завершенное приложение для проекта и содержит `Application_Start` обработчик событий, который выполняется при первом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="36dde-159">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="36dde-160">Найти `Application_Start` метода и добавьте вызов `Database.SetInitializer` в начале метода, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="36dde-160">Find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

<span data-ttu-id="36dde-161">`Database.SetInitializer` Только что добавленного оператора указывает, что базы данных используется `MovieDBContext` экземпляр должен автоматически удалена и создана заново, если схемы и базы данных не совпадают.</span><span class="sxs-lookup"><span data-stu-id="36dde-161">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="36dde-162">И как видно, он также заполнить базу данных с образцом данных, указанной в `MovieInitializer` класса.</span><span class="sxs-lookup"><span data-stu-id="36dde-162">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="36dde-163">Закрыть *Global.asax* файла.</span><span class="sxs-lookup"><span data-stu-id="36dde-163">Close the *Global.asax* file.</span></span>

<span data-ttu-id="36dde-164">Повторно запустите приложение и перейдите к */Movies* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="36dde-164">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="36dde-165">При запуске приложения, он обнаруживает, что структура модели больше не соответствует схеме базы данных.</span><span class="sxs-lookup"><span data-stu-id="36dde-165">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="36dde-166">Автоматически повторно создает базу данных для соответствия новой структуры модели и вносит в базу данных Образец фильмов:</span><span class="sxs-lookup"><span data-stu-id="36dde-166">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

<span data-ttu-id="36dde-168">Нажмите кнопку **создать новый** ссылку, чтобы добавить новый фильма.</span><span class="sxs-lookup"><span data-stu-id="36dde-168">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="36dde-169">Обратите внимание, что можно добавить оценку.</span><span class="sxs-lookup"><span data-stu-id="36dde-169">Note that you can add a rating.</span></span>

<span data-ttu-id="36dde-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="36dde-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span></span>

<span data-ttu-id="36dde-171">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="36dde-171">Click **Create**.</span></span> <span data-ttu-id="36dde-172">Новый фильм, включая оценку, теперь отображается список фильмов:</span><span class="sxs-lookup"><span data-stu-id="36dde-172">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

<span data-ttu-id="36dde-174">В этом разделе вы узнали, как можно изменять объекты модели и синхронизировать базу данных с изменениями.</span><span class="sxs-lookup"><span data-stu-id="36dde-174">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="36dde-175">Вы также узнали способ заполнения вновь созданной базы данных с образцами данных, можно опробовать сценариев.</span><span class="sxs-lookup"><span data-stu-id="36dde-175">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="36dde-176">Теперь давайте взглянем на как можно добавить расширенные логику проверки в классы модели и включить некоторые бизнес-правила для применения.</span><span class="sxs-lookup"><span data-stu-id="36dde-176">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36dde-177">[Назад](examining-the-edit-methods-and-edit-view.md)
> [Вперед](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="36dde-177">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
