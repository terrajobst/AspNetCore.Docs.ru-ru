---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Добавление нового поля в модель Movie и таблицу | Документация Майкрософт
author: Rick-Anderson
description: 'Примечание: Обновленную версию этого учебника доступен здесь, использующий ASP.NET MVC 5 и Visual Studio 2013. Это более безопасное и гораздо проще выполнить и демонстрационных версий...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 25a29e783f02e66e1713d8120eb526e1a02961a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366480"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="04094-104">Добавление нового поля в модель Movie и таблицу</span><span class="sxs-lookup"><span data-stu-id="04094-104">Adding a New Field to the Movie Model and Table</span></span>
====================
<span data-ttu-id="04094-105">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="04094-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="04094-106">Обновленную версию этого учебника доступен [здесь](../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="04094-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="04094-107">Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="04094-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="04094-108">В этом разделе используется Entity Framework Code First Migrations перенести некоторые изменения в классы моделей для применения изменений к базе данных.</span><span class="sxs-lookup"><span data-stu-id="04094-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="04094-109">По умолчанию при использовании Entity Framework Code First для автоматического создания базы данных, как это делалось ранее в этом учебнике, Code First добавляет таблицу в базу данных, которая позволяет отслеживать синхронизацию с классами модели, в которой она была создана из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="04094-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="04094-110">Если синхронизация нарушена, Entity Framework завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="04094-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="04094-111">Это упрощает для выявления проблем во время разработки, в противном случае только выявленных (по непонятные ошибки) во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="04094-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="04094-112">Настройка Code First Migrations для изменения модели</span><span class="sxs-lookup"><span data-stu-id="04094-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="04094-113">Если вы используете Visual Studio 2012, дважды щелкните *Movies.mdf* файл из обозревателя решений, чтобы открыть средство баз данных.</span><span class="sxs-lookup"><span data-stu-id="04094-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="04094-114">Visual Studio Express для Web будет Показать обозреватель баз данных, Visual Studio 2012 будет отображаться в обозревателе серверов.</span><span class="sxs-lookup"><span data-stu-id="04094-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="04094-115">Если вы используете Visual Studio 2010, используйте обозреватель объектов SQL Server.</span><span class="sxs-lookup"><span data-stu-id="04094-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="04094-116">В средстве базы данных (обозреватель баз данных, обозреватель серверов или обозревателе объектов SQL Server), щелкните правой кнопкой мыши `MovieDBContext` и выберите **удалить** удалить базу данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="04094-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="04094-117">Перейдите обратно в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="04094-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="04094-118">Щелкните правой кнопкой мыши *Movies.mdf* файл и выберите **удалить** удаление базы данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="04094-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="04094-119">Постройте приложение, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="04094-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="04094-120">Из **средства** меню, щелкните **диспетчер пакетов библиотеки** и затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="04094-120">From the **Tools** menu, click **Library Package Manager** and then **Package Manager Console**.</span></span>

![Добавьте пакет Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="04094-122">В **консоль диспетчера пакетов** окно при `PM>` командной строке введите «Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext».</span><span class="sxs-lookup"><span data-stu-id="04094-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="04094-123">**Enable-Migrations** (как показано выше) команда создает *Configuration.cs* файл в новом *миграций* папки.</span><span class="sxs-lookup"><span data-stu-id="04094-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="04094-124">Visual Studio открывает *Configuration.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="04094-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="04094-125">Замените `Seed` метод в *Configuration.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="04094-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="04094-126">Щелкните правой кнопкой мыши на волнистую красную линию под `Movie` и выберите **устранить** затем **с помощью** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="04094-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="04094-127">Это добавляет следующий оператор using:</span><span class="sxs-lookup"><span data-stu-id="04094-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="04094-128">Code First Migrations вызовы `Seed` метод после каждой миграции (то есть вызов **обновления базы данных** в консоли диспетчера пакетов), и этот метод обновляет строки, которые уже были вставлены или вставляет их, если они еще не существует.</span><span class="sxs-lookup"><span data-stu-id="04094-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>


<span data-ttu-id="04094-129">**Нажмите клавиши CTRL + SHIFT + B для сборки проекта.** (Ниже завершится ошибкой, если ваш не на этом этапе построения.)</span><span class="sxs-lookup"><span data-stu-id="04094-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="04094-130">Следующим шагом является создание `DbMigration` класс для первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="04094-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="04094-131">Этот перенос создает новую базу данных, поэтому вы удалили *movie.mdf* файл на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="04094-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="04094-132">В **консоль диспетчера пакетов** окно, введите команду «add-migration Initial» для создания первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="04094-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="04094-133">Имя «Начальный» является произвольным и используется для именования создан файл для миграции.</span><span class="sxs-lookup"><span data-stu-id="04094-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="04094-134">Code First Migrations создает еще один файл класса в *миграций* папку (с именем *{метки даты}\_Initial.cs* ), и этот класс содержит код, создающий схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="04094-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="04094-135">Имя файла миграции предварительно исправлена с меткой времени для облегчения упорядочения.</span><span class="sxs-lookup"><span data-stu-id="04094-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="04094-136">Изучите *{метки даты}\_Initial.cs* файл, он содержит инструкции для создания таблицы фильмы для базой данных Movie.</span><span class="sxs-lookup"><span data-stu-id="04094-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="04094-137">При обновлении базы данных в инструкциях ниже, это *{метки даты}\_Initial.cs* файл будет выполняться и создать схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="04094-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="04094-138">Затем **начальное значение** метод выполняется для заполнения БАЗЫ данных тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="04094-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="04094-139">В **консоль диспетчера пакетов**, введите команду «update-database» для создания базы данных и запуска **начальное значение** метод.</span><span class="sxs-lookup"><span data-stu-id="04094-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="04094-140">Если отобразится сообщение об ошибке, указывающее таблицу, уже существует и не может быть создан, то, скорее всего вы запустили приложения после удаления базы данных и до выполнения `update-database`.</span><span class="sxs-lookup"><span data-stu-id="04094-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="04094-141">В этом случае удалите *Movies.mdf* файл еще раз и повторите попытку `update-database` команды.</span><span class="sxs-lookup"><span data-stu-id="04094-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="04094-142">Если вы по-прежнему возникает ошибка, удалите папку migrations и содержимое, а затем запустите с инструкциями в верхней части этой страницы (это delete *Movies.mdf* файл, а затем перейти к Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="04094-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="04094-143">Запустите приложение и перейдите к */Movies* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="04094-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="04094-144">Отображается начальные данные.</span><span class="sxs-lookup"><span data-stu-id="04094-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="04094-145">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="04094-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="04094-146">Начните с добавления нового `Rating` к существующему полю `Movie` класса.</span><span class="sxs-lookup"><span data-stu-id="04094-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="04094-147">Откройте *Models\Movie.cs* файл и добавьте `Rating` свойство следующего вида:</span><span class="sxs-lookup"><span data-stu-id="04094-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="04094-148">Полный `Movie` класса сейчас выглядит аналогично следующему коду:</span><span class="sxs-lookup"><span data-stu-id="04094-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="04094-149">Постройте приложение в среде **построения** &gt; **построения фильма** меню команду или нажав сочетание клавиш CTRL-SHIFT-B.</span><span class="sxs-lookup"><span data-stu-id="04094-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="04094-150">Теперь, когда вы обновили `Model` класса, необходимо также обновить *\Views\Movies\Index.cshtml* и *\Views\Movies\Create.cshtml* просмотра шаблонов для отображения новых `Rating`свойства в представлении браузера.</span><span class="sxs-lookup"><span data-stu-id="04094-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="04094-151">Откройте<em>\Views\Movies\Index.cshtml</em> файл и добавьте `<th>Rating</th>` сразу после заголовка столбца <strong>цена</strong> столбца.</span><span class="sxs-lookup"><span data-stu-id="04094-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="04094-152">Затем добавьте `<td>` столбца в конце шаблона для отображения `@item.Rating` значение.</span><span class="sxs-lookup"><span data-stu-id="04094-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="04094-153">Ниже приведен какие обновленный <em>Index.cshtml</em> Просмотр шаблона выглядит как:</span><span class="sxs-lookup"><span data-stu-id="04094-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="04094-154">Затем откройте *\Views\Movies\Create.cshtml* файл и добавьте следующую разметку в конце формы.</span><span class="sxs-lookup"><span data-stu-id="04094-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="04094-155">Это отрисовывает текстовое поле, можно указать оценку, если создается новый фильм.</span><span class="sxs-lookup"><span data-stu-id="04094-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="04094-156">Теперь вы обновили код приложения, поддерживающие новое `Rating` свойство.</span><span class="sxs-lookup"><span data-stu-id="04094-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="04094-157">Теперь запустите приложение и перейдите к */Movies* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="04094-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="04094-158">При этом, однако вы увидите одно из следующих ошибок:</span><span class="sxs-lookup"><span data-stu-id="04094-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="04094-159">Вы видите эту ошибку, так как обновленный `Movie` класс модели в приложение теперь отличается от схемы `Movie` таблицы в существующей базе данных.</span><span class="sxs-lookup"><span data-stu-id="04094-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="04094-160">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="04094-160">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="04094-161">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="04094-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="04094-162">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="04094-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="04094-163">Такой подход очень удобен при выполнении активное развертывание в тестовой базы данных; он позволяет быстро развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="04094-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="04094-164">Недостатком, однако является потеря существующих данных в базе данных, поэтому вы *не* хотите использовать этот подход на производственной базы данных!</span><span class="sxs-lookup"><span data-stu-id="04094-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="04094-165">Часто используется инициализатор для автоматического заполнения базы тестовыми данными является эффективный способ разработки приложения.</span><span class="sxs-lookup"><span data-stu-id="04094-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="04094-166">Дополнительные сведения об инициализаторах базы данных Entity Framework, см. в разделе том Дайкстра [учебник по ASP.NET MVC и Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="04094-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="04094-167">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="04094-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="04094-168">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="04094-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="04094-169">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="04094-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="04094-170">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="04094-170">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="04094-171">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="04094-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="04094-172">Обновите метод заполнения, таким образом, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="04094-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="04094-173">Откройте файл Migrations\Configuration.cs и добавьте поле оценки для каждого объекта фильма.</span><span class="sxs-lookup"><span data-stu-id="04094-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="04094-174">Выполните сборку решения, а затем откройте **консоль диспетчера пакетов** окна и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="04094-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="04094-175">`add-migration` Команда указывает платформе миграции для проверки текущей модели фильма с текущей схемой базы данных фильмов и создать необходимый код для переноса базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="04094-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="04094-176">AddRatingMig является произвольным и используется для имени файла переноса.</span><span class="sxs-lookup"><span data-stu-id="04094-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="04094-177">Рекомендуется использовать понятное имя для этапов миграции.</span><span class="sxs-lookup"><span data-stu-id="04094-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="04094-178">По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMIgration` производного класса и в `Up` метода вы увидите код, создающий новый столбец.</span><span class="sxs-lookup"><span data-stu-id="04094-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="04094-179">Выполните сборку решения, а затем введите команду «update-database» в **консоль диспетчера пакетов** окна.</span><span class="sxs-lookup"><span data-stu-id="04094-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="04094-180">На следующем рисунке показаны выходные данные в **консоль диспетчера пакетов** окна (Метка даты, добавляя AddRatingMig будет отличаться.)</span><span class="sxs-lookup"><span data-stu-id="04094-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="04094-181">Повторно запустите приложение и перейдите к /Movies URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="04094-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="04094-182">Вы увидите новое поле оценки.</span><span class="sxs-lookup"><span data-stu-id="04094-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="04094-183">Нажмите кнопку **Create New** ссылку, чтобы добавить новый фильм.</span><span class="sxs-lookup"><span data-stu-id="04094-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="04094-184">Обратите внимание на то, что вы можете добавить оценку.</span><span class="sxs-lookup"><span data-stu-id="04094-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="04094-186">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="04094-186">Click **Create**.</span></span> <span data-ttu-id="04094-187">Этот новый фильм, включая оценку, отображается в окне списка фильмов:</span><span class="sxs-lookup"><span data-stu-id="04094-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="04094-189">Также следует добавить `Rating` поле для изменения "," Сведения "и" SearchIndex шаблонов представлений.</span><span class="sxs-lookup"><span data-stu-id="04094-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="04094-190">Можно ввести команду «update-database» в **консоль диспетчера пакетов** окно еще раз и изменения не будут внесены, так как схема соответствует модели.</span><span class="sxs-lookup"><span data-stu-id="04094-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="04094-191">В этом разделе вы узнали, как можно изменять объекты модели и синхронизации базы данных с изменениями.</span><span class="sxs-lookup"><span data-stu-id="04094-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="04094-192">Вы также узнали способ заполнения вновь созданную базу данных с демонстрационными данными, можно опробовать сценарии.</span><span class="sxs-lookup"><span data-stu-id="04094-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="04094-193">Теперь давайте взглянем на как можно добавить более широкие логику проверки в классы модели и включить некоторые бизнес-правила, которые будут применяться.</span><span class="sxs-lookup"><span data-stu-id="04094-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="04094-194">[Назад](examining-the-edit-methods-and-edit-view.md)
> [Вперед](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="04094-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
