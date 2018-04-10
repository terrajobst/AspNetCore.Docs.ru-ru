---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Добавление столбца в модель | Документы Microsoft
author: shanselman
description: Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/10/2018
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="53156-104">Добавление столбца модели</span><span class="sxs-lookup"><span data-stu-id="53156-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="53156-105">по [Скотт Хансельман](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="53156-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="53156-106">Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="53156-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="53156-107">Вы создадите простое веб-приложение выполняет чтение и запись из базы данных.</span><span class="sxs-lookup"><span data-stu-id="53156-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="53156-108">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="53156-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="53156-109">В этом разделе мы собираемся показывает, как можно внести изменения в схему базе данных и обработки изменений в приложении.</span><span class="sxs-lookup"><span data-stu-id="53156-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="53156-110">Давайте добавим столбца «Оценка» таблицы фильма.</span><span class="sxs-lookup"><span data-stu-id="53156-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="53156-111">Вернитесь в интегрированную среду разработки и выберите в обозревателе базы данных.</span><span class="sxs-lookup"><span data-stu-id="53156-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="53156-112">Щелкните правой кнопкой мыши таблицу фильма и выберите Открыть определение таблицы.</span><span class="sxs-lookup"><span data-stu-id="53156-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="53156-113">Добавьте столбец «Оценка», как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="53156-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="53156-114">Поскольку мы не имеют любые оценки, столбец можно разрешить значения NULL.</span><span class="sxs-lookup"><span data-stu-id="53156-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="53156-115">Нажмите кнопку Сохранить.</span><span class="sxs-lookup"><span data-stu-id="53156-115">Click Save.</span></span>

<span data-ttu-id="53156-116">[![Изменение таблицы фильмов](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="53156-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="53156-117">Затем вернуться в обозревателе решений и откройте файл Movies.edmx (который находится в папке \Models).</span><span class="sxs-lookup"><span data-stu-id="53156-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="53156-118">Щелкните правой кнопкой мыши в области конструктора (белая область) и выберите модель обновление из базы данных.</span><span class="sxs-lookup"><span data-stu-id="53156-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="53156-119">[![Фильмы - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="53156-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="53156-120">Это приведет к запуску «Мастера обновления».</span><span class="sxs-lookup"><span data-stu-id="53156-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="53156-121">Перейдите на вкладку обновления внутри нее и нажмите "Готово".</span><span class="sxs-lookup"><span data-stu-id="53156-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="53156-122">Класс модели нашей фильма обновляются с помощью нового столбца.</span><span class="sxs-lookup"><span data-stu-id="53156-122">Our Movie model class will then be updated with the new column.</span></span>

![Мастер обновления (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="53156-124">После нажатия кнопки Готово, вы увидите, что в нашей модели сущности фильма добавлен новый столбец оценки.</span><span class="sxs-lookup"><span data-stu-id="53156-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="53156-125">[![Сущности фильма](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="53156-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="53156-126">Мы добавили столбец в модели базы данных, но представления не знаете о нем.</span><span class="sxs-lookup"><span data-stu-id="53156-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="53156-127">Обновление представлений с изменениями в модели</span><span class="sxs-lookup"><span data-stu-id="53156-127">Update Views with Model Changes</span></span>

<span data-ttu-id="53156-128">Корпорация Майкрософт может обновлять наши шаблоны представления для отражения нового столбца оценки несколькими способами.</span><span class="sxs-lookup"><span data-stu-id="53156-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="53156-129">Поскольку мы создали эти представления путем создания их через диалоговое окно добавления представления, мы их удалить и повторно создать их повторно.</span><span class="sxs-lookup"><span data-stu-id="53156-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="53156-130">Однако обычно людей будет уже были внесены изменения в свои шаблоны представления из первоначального формирования формирования шаблонов и требуется добавить или удалить поля вручную, так же, как это делалось с поля «идентификатор» для создания.</span><span class="sxs-lookup"><span data-stu-id="53156-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="53156-131">Откройте шаблон \Views\Movies\Index.aspx и добавьте &lt;й&gt;Оценка&lt;/th&gt; на заголовок таблицы фильма.</span><span class="sxs-lookup"><span data-stu-id="53156-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="53156-132">Я добавил мое после жанра.</span><span class="sxs-lookup"><span data-stu-id="53156-132">I added mine after Genre.</span></span> <span data-ttu-id="53156-133">В той же позиции столбца, но ниже, добавьте строку для вывода нашей новой оценки.</span><span class="sxs-lookup"><span data-stu-id="53156-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="53156-134">Наш окончательного Index.aspx шаблона будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="53156-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="53156-135">Давайте затем откройте шаблон \Views\Movies\Create.aspx и добавить метку и текстовое поле для наших новое свойство оценки:</span><span class="sxs-lookup"><span data-stu-id="53156-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="53156-136">Нашей окончательного шаблон Create.aspx будет выглядеть следующим образом и изменим наш обозреватель заголовок и вторичный &lt;h2&gt; заголовка примерно в «Создать фильма «пока мы здесь!</span><span class="sxs-lookup"><span data-stu-id="53156-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="53156-137">Запуск приложения, и теперь у вас есть новое поле в базе данных, который добавляется на страницу создания.</span><span class="sxs-lookup"><span data-stu-id="53156-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="53156-138">Добавление нового фильма - это время с оценку - и нажмите кнопку Создать.</span><span class="sxs-lookup"><span data-stu-id="53156-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="53156-139">[![Создание фильма - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="53156-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="53156-140">Нажмите кнопку "Создать", вы являетесь отправлена страницы индекса там, где вы новые фильма указана с нового столбца оценки в базе данных</span><span class="sxs-lookup"><span data-stu-id="53156-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="53156-141">[![Список фильмов - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="53156-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="53156-142">Это основам получен работы, делая контроллеров, связывая их с представлениями и передачи данных жестко.</span><span class="sxs-lookup"><span data-stu-id="53156-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="53156-143">Затем мы создан и предназначенных базы данных и поместить некоторые данные в.</span><span class="sxs-lookup"><span data-stu-id="53156-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="53156-144">Мы извлекает данные из базы данных и он отображается в HTML-таблицу.</span><span class="sxs-lookup"><span data-stu-id="53156-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="53156-145">Затем мы добавили создать форму, которая позволяют добавлять данные в базу данных сами из веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="53156-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="53156-146">Мы добавлены проверки, а затем сделать проверки используется JavaScript на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="53156-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="53156-147">Наконец мы изменил базу данных, чтобы включить новый столбец данных, а затем обновлены наши две страницы для создания и отображения новых данных.</span><span class="sxs-lookup"><span data-stu-id="53156-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="53156-148">Читателю теперь можно переходить к обучении промежуточного уровня «[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)», а также многие видеоролики и ресурсы на [ https://asp.net/mvc ](https://asp.net/mvc) Чтобы получить дополнительные сведения о ASP.NET MVC!</span><span class="sxs-lookup"><span data-stu-id="53156-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="53156-149">Желаем удачи!</span><span class="sxs-lookup"><span data-stu-id="53156-149">Enjoy!</span></span>

- <span data-ttu-id="53156-150">Скотт Хансельман - [ http://hanselman.com ](http://hanselman.com) и [ @shanselman ](http://twitter.com/shanselman) в Twitter.</span><span class="sxs-lookup"><span data-stu-id="53156-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="53156-151">Назад</span><span class="sxs-lookup"><span data-stu-id="53156-151">Previous</span></span>](getting-started-with-mvc-part7.md)
