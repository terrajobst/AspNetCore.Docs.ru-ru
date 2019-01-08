---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Добавление нового поля | Документация Майкрософт
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 950ae17ebd6b0f15520c2a4e9372703f5374dfbe
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098692"
---
<a name="adding-a-new-field"></a><span data-ttu-id="59e32-102">Добавление нового поля</span><span class="sxs-lookup"><span data-stu-id="59e32-102">Adding a New Field</span></span>
====================
<span data-ttu-id="59e32-103">по [Рик Андерсон]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="59e32-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="59e32-104">В этом разделе используется Entity Framework Code First Migrations перенести некоторые изменения в классы моделей для применения изменений к базе данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-104">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="59e32-105">По умолчанию при использовании Entity Framework Code First для автоматического создания базы данных, как это делалось ранее в этом учебнике, Code First добавляет таблицу в базу данных, которая позволяет отслеживать синхронизацию с классами модели, в которой она была создана из схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-105">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="59e32-106">Если синхронизация нарушена, Entity Framework завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="59e32-106">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="59e32-107">Это упрощает для выявления проблем во время разработки, в противном случае только выявленных (по непонятные ошибки) во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="59e32-107">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="59e32-108">Настройка Code First Migrations для изменения модели</span><span class="sxs-lookup"><span data-stu-id="59e32-108">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="59e32-109">Перейдите в обозревателе решений.</span><span class="sxs-lookup"><span data-stu-id="59e32-109">Navigate to Solution Explorer.</span></span> <span data-ttu-id="59e32-110">Щелкните правой кнопкой мыши *Movies.mdf* файл и выберите **удалить** удаление базы данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="59e32-110">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span> <span data-ttu-id="59e32-111">Если вы не видите *Movies.mdf* файл, щелкните **Показать все файлы** значок, показано ниже в красный контур.</span><span class="sxs-lookup"><span data-stu-id="59e32-111">If you don't see the *Movies.mdf* file, click on the **Show All Files** icon shown below in the red outline.</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="59e32-112">Постройте приложение, чтобы убедиться в отсутствии ошибок.</span><span class="sxs-lookup"><span data-stu-id="59e32-112">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="59e32-113">Из **средства** меню, щелкните **диспетчер пакетов NuGet** и затем **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="59e32-113">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Добавьте пакет Man](adding-a-new-field/_static/image2.png)

<span data-ttu-id="59e32-115">В **консоль диспетчера пакетов** окно при `PM>` окно командной строки введите</span><span class="sxs-lookup"><span data-stu-id="59e32-115">In the **Package Manager Console** window at the `PM>` prompt enter</span></span>

<span data-ttu-id="59e32-116">Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext</span><span class="sxs-lookup"><span data-stu-id="59e32-116">Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext</span></span>

![](adding-a-new-field/_static/image3.png)

<span data-ttu-id="59e32-117">**Enable-Migrations** (как показано выше) команда создает *Configuration.cs* файл в новом *миграций* папки.</span><span class="sxs-lookup"><span data-stu-id="59e32-117">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field/_static/image4.png)

<span data-ttu-id="59e32-118">Visual Studio открывает *Configuration.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="59e32-118">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="59e32-119">Замените `Seed` метод в *Configuration.cs* файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="59e32-119">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

<span data-ttu-id="59e32-120">Наведите указатель мыши на волнистую красную линию под `Movie` и нажмите кнопку `Show Potential Fixes` и нажмите кнопку **с помощью** **MvcMovie.Models;**</span><span class="sxs-lookup"><span data-stu-id="59e32-120">Hover over the red squiggly line under `Movie` and click `Show Potential Fixes` and then click **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field/_static/image5.png)

<span data-ttu-id="59e32-121">Это добавляет следующий оператор using:</span><span class="sxs-lookup"><span data-stu-id="59e32-121">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE]
> 
> <span data-ttu-id="59e32-122">Code First Migrations вызовы `Seed` метод после каждой миграции (то есть вызов **обновления базы данных** в консоли диспетчера пакетов), и этот метод обновляет строки, которые уже были вставлены или вставляет их, если они еще не существует.</span><span class="sxs-lookup"><span data-stu-id="59e32-122">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>
> 
> <span data-ttu-id="59e32-123">[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) операцией «вставки-обновления» выполняет метод в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="59e32-123">The [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method in the following code performs an "upsert" operation:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> <span data-ttu-id="59e32-124">Так как [начальное значение](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) метод работает с каждой миграции, нельзя просто вставить данные, так как строки, вы пытаетесь добавить уже будут существует после первой миграции, который создает базу данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-124">Because the [Seed](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) method runs with every migration, you can't just insert data, because the rows you are trying to add will already be there after the first migration that creates the database.</span></span> <span data-ttu-id="59e32-125">"[Upsert](http://en.wikipedia.org/wiki/Upsert)" операция позволяет избежать ошибок, произойдет при попытке вставить строку, которая уже существует, но этот параметр переопределяет любые изменения данных, внесенные во время тестирования приложения.</span><span class="sxs-lookup"><span data-stu-id="59e32-125">The "[upsert](http://en.wikipedia.org/wiki/Upsert)" operation prevents errors that would happen if you try to insert a row that already exists, but it overrides any changes to data that you may have made while testing the application.</span></span> <span data-ttu-id="59e32-126">Тестовыми данными в некоторых таблицах вы не хотите, чтобы происходить: в некоторых случаях при изменении данных во время тестирования необходимым вам изменениям после обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-126">With test data in some tables you might not want that to happen: in some cases when you change data while testing you want your changes to remain after database updates.</span></span> <span data-ttu-id="59e32-127">В этом случае необходимо выполнить операцию вставки условного: вставить строку, только в том случае, если он еще не существует.</span><span class="sxs-lookup"><span data-stu-id="59e32-127">In that case you want to do a conditional insert operation: insert a row only if it doesn't already exist.</span></span>   
> 
> <span data-ttu-id="59e32-128">Первый параметр, передаваемый [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метод задает свойство, используемое для проверки, если строка уже существует.</span><span class="sxs-lookup"><span data-stu-id="59e32-128">The first parameter passed to the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method specifies the property to use to check if a row already exists.</span></span> <span data-ttu-id="59e32-129">Для тестовых данных фильмов, вы предоставляете `Title` свойство может использоваться для этой цели, так как каждая книга из списка является уникальным:</span><span class="sxs-lookup"><span data-stu-id="59e32-129">For the test movie data that you are providing, the `Title` property can be used for this purpose since each title in the list is unique:</span></span>
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> <span data-ttu-id="59e32-130">Предполагается, что заголовки являются уникальными.</span><span class="sxs-lookup"><span data-stu-id="59e32-130">This code assumes that titles are unique.</span></span> <span data-ttu-id="59e32-131">Если вручную добавить дублирующийся заголовок, вы получите следующее исключение при очередном выполнении миграции.</span><span class="sxs-lookup"><span data-stu-id="59e32-131">If you manually add a duplicate title, you'll get the following exception the next time you perform a migration.</span></span>   
> 
>  <span data-ttu-id="59e32-132">*Последовательность содержит более одного элемента*</span><span class="sxs-lookup"><span data-stu-id="59e32-132">*Sequence contains more than one element*</span></span>  
> 
> <span data-ttu-id="59e32-133">Дополнительные сведения о [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) метод, см. в разделе [Будьте внимательны с помощью метода AddOrUpdate 4.3 EF](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)...</span><span class="sxs-lookup"><span data-stu-id="59e32-133">For more information about the [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) method, see [Take care with EF 4.3 AddOrUpdate Method](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)..</span></span>


<span data-ttu-id="59e32-134">**Нажмите клавиши CTRL + SHIFT + B для сборки проекта.** (Ниже завершится ошибкой, если сборка не на этом этапе.)</span><span class="sxs-lookup"><span data-stu-id="59e32-134">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if you don't build at this point.)</span></span>

<span data-ttu-id="59e32-135">Следующим шагом является создание `DbMigration` класс для первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="59e32-135">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="59e32-136">Этот вид миграции создает новую базу данных, поэтому вы удалили *movie.mdf* файл на предыдущем шаге.</span><span class="sxs-lookup"><span data-stu-id="59e32-136">This migration creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="59e32-137">В **консоль диспетчера пакетов** окно, введите команду `add-migration Initial` для создания первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="59e32-137">In the **Package Manager Console** window, enter the command `add-migration Initial` to create the initial migration.</span></span> <span data-ttu-id="59e32-138">Имя «Начальный» является произвольным и используется для именования создан файл для миграции.</span><span class="sxs-lookup"><span data-stu-id="59e32-138">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field/_static/image6.png)

<span data-ttu-id="59e32-139">Code First Migrations создает еще один файл класса в *миграций* папку (с именем *{метки даты}\_Initial.cs* ), и этот класс содержит код, создающий схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-139">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="59e32-140">Имя файла миграции предварительно исправлена с меткой времени для облегчения упорядочения.</span><span class="sxs-lookup"><span data-stu-id="59e32-140">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="59e32-141">Изучите *{метки даты}\_Initial.cs* файл, он содержит инструкции по созданию `Movies` таблицы для базы данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="59e32-141">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the `Movies` table for the Movie DB.</span></span> <span data-ttu-id="59e32-142">При обновлении базы данных в инструкциях ниже, это *{метки даты}\_Initial.cs* файл будет выполняться и создать схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-142">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="59e32-143">Затем **начальное значение** метод выполняется для заполнения БАЗЫ данных тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="59e32-143">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="59e32-144">В **консоль диспетчера пакетов**, введите команду `update-database` для создания базы данных и запуска `Seed` метод.</span><span class="sxs-lookup"><span data-stu-id="59e32-144">In the **Package Manager Console**, enter the command `update-database` to create the database and run the `Seed` method.</span></span>

![](adding-a-new-field/_static/image7.png)

<span data-ttu-id="59e32-145">Если отобразится сообщение об ошибке, указывающее таблицу, уже существует и не может быть создан, то, скорее всего вы запустили приложения после удаления базы данных и до выполнения `update-database`.</span><span class="sxs-lookup"><span data-stu-id="59e32-145">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="59e32-146">В этом случае удалите *Movies.mdf* файл еще раз и повторите попытку `update-database` команды.</span><span class="sxs-lookup"><span data-stu-id="59e32-146">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="59e32-147">Если вы по-прежнему возникает ошибка, удалите папку migrations и содержимое, а затем запустите с инструкциями в верхней части этой страницы (это delete *Movies.mdf* файл, а затем перейти к Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="59e32-147">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span> <span data-ttu-id="59e32-148">Если вы по-прежнему возникает ошибка, откройте обозреватель объектов SQL Server и удалите базу данных из списка.</span><span class="sxs-lookup"><span data-stu-id="59e32-148">If you still get an error, open SQL Server Object Explorer and remove the database from the list.</span></span>

<span data-ttu-id="59e32-149">Запустите приложение и перейдите к */Movies* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="59e32-149">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="59e32-150">Отображается начальные данные.</span><span class="sxs-lookup"><span data-stu-id="59e32-150">The seed data is displayed.</span></span>

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="59e32-151">Добавление свойства Rating в модель Movie</span><span class="sxs-lookup"><span data-stu-id="59e32-151">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="59e32-152">Начните с добавления нового `Rating` к существующему полю `Movie` класса.</span><span class="sxs-lookup"><span data-stu-id="59e32-152">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="59e32-153">Откройте *Models\Movie.cs* файл и добавьте `Rating` свойство следующего вида:</span><span class="sxs-lookup"><span data-stu-id="59e32-153">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

<span data-ttu-id="59e32-154">Полный `Movie` класса сейчас выглядит аналогично следующему коду:</span><span class="sxs-lookup"><span data-stu-id="59e32-154">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

<span data-ttu-id="59e32-155">Постройте приложение (Ctrl + Shift + B).</span><span class="sxs-lookup"><span data-stu-id="59e32-155">Build the application (Ctrl+Shift+B).</span></span>

<span data-ttu-id="59e32-156">Так как вы добавили новое поле `Movie` класса, также необходимо обновить привязки *в список разрешенных* , это новое свойство будут включены.</span><span class="sxs-lookup"><span data-stu-id="59e32-156">Because you've added a new field to the `Movie` class, you also need to update the binding *white list* so this new property will be included.</span></span> <span data-ttu-id="59e32-157">Обновление `bind` для атрибута `Create` и `Edit` методы действий для включения `Rating` свойство:</span><span class="sxs-lookup"><span data-stu-id="59e32-157">Update the `bind` attribute for `Create` and `Edit` action methods to include the `Rating` property:</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

<span data-ttu-id="59e32-158">Также необходимо обновить шаблоны представлений, чтобы реализовать отображение, создание и редактирование нового свойства `Rating` в представлении браузера.</span><span class="sxs-lookup"><span data-stu-id="59e32-158">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="59e32-159">Откройте *\Views\Movies\Index.cshtml* файл и добавьте `<th>Rating</th>` сразу после заголовка столбца **цена** столбца.</span><span class="sxs-lookup"><span data-stu-id="59e32-159">Open the *\Views\Movies\Index.cshtml* file and add a `<th>Rating</th>` column heading just after the **Price** column.</span></span> <span data-ttu-id="59e32-160">Затем добавьте `<td>` столбца в конце шаблона для отображения `@item.Rating` значение.</span><span class="sxs-lookup"><span data-stu-id="59e32-160">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="59e32-161">Ниже приведен какие обновленный *Index.cshtml* Просмотр шаблона выглядит как:</span><span class="sxs-lookup"><span data-stu-id="59e32-161">Below is what the updated *Index.cshtml* view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

<span data-ttu-id="59e32-162">Затем откройте *\Views\Movies\Create.cshtml* файл и добавьте `Rating` поле, используя следующую разметку highlighed.</span><span class="sxs-lookup"><span data-stu-id="59e32-162">Next, open the *\Views\Movies\Create.cshtml* file and add the `Rating` field with the following highlighed markup.</span></span> <span data-ttu-id="59e32-163">Это отрисовывает текстовое поле, можно указать оценку, если создается новый фильм.</span><span class="sxs-lookup"><span data-stu-id="59e32-163">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

<span data-ttu-id="59e32-164">Теперь вы обновили код приложения, поддерживающие новое `Rating` свойство.</span><span class="sxs-lookup"><span data-stu-id="59e32-164">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="59e32-165">Запустите приложение и перейдите к */Movies* URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="59e32-165">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="59e32-166">При этом, однако вы увидите одно из следующих ошибок:</span><span class="sxs-lookup"><span data-stu-id="59e32-166">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field/_static/image9.png)  
  
<span data-ttu-id="59e32-167">Модель, поддерживающая контекст «MovieDBContext» изменилось с момента создания базы данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-167">The model backing the 'MovieDBContext' context has changed since the database was created.</span></span> <span data-ttu-id="59e32-168">Рассмотрите возможность использования Code First Migrations для обновления базы данных (https://go.microsoft.com/fwlink/?LinkId=238269).</span><span class="sxs-lookup"><span data-stu-id="59e32-168">Consider using Code First Migrations to update the database (https://go.microsoft.com/fwlink/?LinkId=238269).</span></span>

![](adding-a-new-field/_static/image10.png)

<span data-ttu-id="59e32-169">Вы видите эту ошибку, так как обновленный `Movie` класс модели в приложение теперь отличается от схемы `Movie` таблицы в существующей базе данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-169">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="59e32-170">(В таблице базы данных отсутствует столбец `Rating`.)</span><span class="sxs-lookup"><span data-stu-id="59e32-170">(There's no `Rating` column in the database table.)</span></span>


<span data-ttu-id="59e32-171">Устранить эту ошибку можно несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="59e32-171">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="59e32-172">Можно с помощью Entity Framework автоматически удалить и повторно создать базу данных на основе новой схемы класса модели.</span><span class="sxs-lookup"><span data-stu-id="59e32-172">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="59e32-173">Этот подход удобен на ранних стадиях цикла разработки, когда все действия осуществляются с тестовой базой данных. В этом случае развитие модели и схемы базы данных осуществляется одновременно.</span><span class="sxs-lookup"><span data-stu-id="59e32-173">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="59e32-174">Недостатком, однако является потеря существующих данных в базе данных, поэтому вы *не* хотите использовать этот подход на производственной базы данных!</span><span class="sxs-lookup"><span data-stu-id="59e32-174">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="59e32-175">При разработке приложения часто используется инициализатор для автоматического заполнения базы тестовыми данными.</span><span class="sxs-lookup"><span data-stu-id="59e32-175">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="59e32-176">Дополнительные сведения об инициализаторах базы данных Entity Framework, см. в разделе [учебник по ASP.NET MVC и Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="59e32-176">For more information on Entity Framework database initializers, see [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="59e32-177">Можно явно изменить схему существующей базы данных в соответствии с новыми классами модели.</span><span class="sxs-lookup"><span data-stu-id="59e32-177">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="59e32-178">Преимущество такого подхода состоит в том, что сохраняются все данные.</span><span class="sxs-lookup"><span data-stu-id="59e32-178">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="59e32-179">Это изменение можно выполнить как вручную, так и с помощью соответствующего скрипта базы данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-179">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="59e32-180">Можно обновить схему базы данных с помощью Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="59e32-180">Use Code First Migrations to update the database schema.</span></span>


<span data-ttu-id="59e32-181">В этом руководстве используется Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="59e32-181">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="59e32-182">Обновите метод заполнения, таким образом, чтобы он предоставлял значение нового столбца.</span><span class="sxs-lookup"><span data-stu-id="59e32-182">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="59e32-183">Откройте файл Migrations\Configuration.cs и добавьте поле оценки для каждого объекта фильма.</span><span class="sxs-lookup"><span data-stu-id="59e32-183">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

<span data-ttu-id="59e32-184">Выполните сборку решения, а затем откройте **консоль диспетчера пакетов** окна и введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="59e32-184">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration Rating`

<span data-ttu-id="59e32-185">`add-migration` Команда указывает платформе миграции для проверки текущей модели фильма с текущей схемой базы данных фильмов и создать необходимый код для переноса базы данных в новую модель.</span><span class="sxs-lookup"><span data-stu-id="59e32-185">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="59e32-186">Имя *Оценка* является произвольным и используется для имени файла переноса.</span><span class="sxs-lookup"><span data-stu-id="59e32-186">The name *Rating* is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="59e32-187">Рекомендуется использовать понятное имя для этапов миграции.</span><span class="sxs-lookup"><span data-stu-id="59e32-187">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="59e32-188">По завершении этой команды Visual Studio открывает файл класса, который определяет новый `DbMigration` производного класса и в `Up` метода вы увидите код, создающий новый столбец.</span><span class="sxs-lookup"><span data-stu-id="59e32-188">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

<span data-ttu-id="59e32-189">Выполните сборку решения, а затем введите `update-database` в команду **консоль диспетчера пакетов** окна.</span><span class="sxs-lookup"><span data-stu-id="59e32-189">Build the solution, and then enter the `update-database` command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="59e32-190">На следующем рисунке показаны выходные данные в **консоль диспетчера пакетов** окна (Метка даты атрибут As *Оценка* будет отличаться.)</span><span class="sxs-lookup"><span data-stu-id="59e32-190">The following image shows the output in the **Package Manager Console** window (The date stamp prepending *Rating* will be different.)</span></span>

![](adding-a-new-field/_static/image11.png)

<span data-ttu-id="59e32-191">Повторно запустите приложение и перейдите к /Movies URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="59e32-191">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="59e32-192">Вы увидите новое поле оценки.</span><span class="sxs-lookup"><span data-stu-id="59e32-192">You can see the new Rating field.</span></span>

![](adding-a-new-field/_static/image12.png)

<span data-ttu-id="59e32-193">Нажмите кнопку **Create New** ссылку, чтобы добавить новый фильм.</span><span class="sxs-lookup"><span data-stu-id="59e32-193">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="59e32-194">Обратите внимание на то, что вы можете добавить оценку.</span><span class="sxs-lookup"><span data-stu-id="59e32-194">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field/_static/image13.png)

<span data-ttu-id="59e32-196">Нажмите кнопку **Создать**.</span><span class="sxs-lookup"><span data-stu-id="59e32-196">Click **Create**.</span></span> <span data-ttu-id="59e32-197">Этот новый фильм, включая оценку, отображается в окне списка фильмов:</span><span class="sxs-lookup"><span data-stu-id="59e32-197">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

<span data-ttu-id="59e32-199">Теперь, когда проект использует миграций, не нужно будет удалить базу данных, при добавлении нового поля или в противном случае обновления схемы.</span><span class="sxs-lookup"><span data-stu-id="59e32-199">Now that the project is using migrations, you won't need to drop the database when you add a new field or otherwise update the schema.</span></span> <span data-ttu-id="59e32-200">В следующем разделе мы внести дополнительные изменения схемы и использовать миграции для обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="59e32-200">In the next section, we'll make more schema changes and use migrations to update the database.</span></span>

<span data-ttu-id="59e32-201">Также следует добавить `Rating` поле редактирования, подробности и удалить шаблоны представлений.</span><span class="sxs-lookup"><span data-stu-id="59e32-201">You should also add the `Rating` field to the Edit, Details, and Delete view templates.</span></span>

<span data-ttu-id="59e32-202">Можно ввести команду «update-database» в **консоль диспетчера пакетов** окно еще раз и без миграции кода будет выполнен, поскольку схема соответствует модели.</span><span class="sxs-lookup"><span data-stu-id="59e32-202">You could enter the "update-database" command in the **Package Manager Console** window again and no migration code would run, because the schema matches the model.</span></span> <span data-ttu-id="59e32-203">Тем не менее, будет выполняться под управлением «update-database» `Seed` метод, и при изменении любого начального значения данных, изменения будут потеряны, поскольку `Seed` метод вставляет данные.</span><span class="sxs-lookup"><span data-stu-id="59e32-203">However, running "update-database" will run the `Seed` method again, and if you changed any of the Seed data, the changes will be lost because the `Seed` method upserts data.</span></span> <span data-ttu-id="59e32-204">Дополнительные сведения о `Seed` метод в том Дайкстра популярных [учебник по ASP.NET MVC и Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="59e32-204">You can read more about the `Seed` method in Tom Dykstra's popular [ASP.NET MVC/Entity Framework tutorial](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="59e32-205">В этом разделе вы узнали, как можно изменять объекты модели и синхронизации базы данных с изменениями.</span><span class="sxs-lookup"><span data-stu-id="59e32-205">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="59e32-206">Вы также узнали способ заполнения вновь созданную базу данных с демонстрационными данными, можно опробовать сценарии.</span><span class="sxs-lookup"><span data-stu-id="59e32-206">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="59e32-207">Это было просто краткие сведения о Code First, см. в разделе [Создание модели данных Entity Framework для приложения ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) более полное руководство по этой теме.</span><span class="sxs-lookup"><span data-stu-id="59e32-207">This was just a quick introduction to Code First, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) for a more complete tutorial on the subject.</span></span> <span data-ttu-id="59e32-208">Теперь давайте взглянем на как можно добавить более широкие логику проверки в классы модели и включить некоторые бизнес-правила, которые будут применяться.</span><span class="sxs-lookup"><span data-stu-id="59e32-208">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="59e32-209">[Назад](adding-search.md)
> [Вперед](adding-validation.md)</span><span class="sxs-lookup"><span data-stu-id="59e32-209">[Previous](adding-search.md)
[Next](adding-validation.md)</span></span>
