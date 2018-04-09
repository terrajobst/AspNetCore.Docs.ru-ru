---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Доступ к вашей модели данных от контроллера | Документы Microsoft
author: shanselman
description: Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, чтение и запись из базы данных.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 2ba1b73f40a920e27e4a03d9f703e62054d3f25c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="80905-104">Доступ к вашей модели данных из контроллера</span><span class="sxs-lookup"><span data-stu-id="80905-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="80905-105">по [Скотт Хансельман](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="80905-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="80905-106">Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="80905-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="80905-107">Вы создадите простое веб-приложение выполняет чтение и запись из базы данных.</span><span class="sxs-lookup"><span data-stu-id="80905-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="80905-108">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="80905-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="80905-109">В этом разделе мы собираемся создавать новый класс MoviesController и написать код, получающий наши данные фильма и отображает его обратно в браузер, с использованием шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="80905-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="80905-110">Щелкните правой кнопкой папку Controllers и сделать новый MoviesController.</span><span class="sxs-lookup"><span data-stu-id="80905-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="80905-111">[![Добавление контроллера](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="80905-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="80905-112">Это создаст новый файл «MoviesController.cs» под нашей \Controllers папку в данном проекте.</span><span class="sxs-lookup"><span data-stu-id="80905-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="80905-113">Давайте обновить MovieController, чтобы получить список фильмов из наших заполненными базы данных.</span><span class="sxs-lookup"><span data-stu-id="80905-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="80905-114">Мы занимаемся запрос LINQ, чтобы только извлечение фильмы, выпущенным после лето 1984.</span><span class="sxs-lookup"><span data-stu-id="80905-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="80905-115">Он понадобится Просмотр шаблона отрисовки этот список фильмов обратно, поэтому щелкните правой кнопкой мыши в методе и выберите Добавить представление для его создания.</span><span class="sxs-lookup"><span data-stu-id="80905-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="80905-116">В диалоговом окне Добавление представления мы будем указывать мы передаем список&lt;Movies.Models.Movie&gt; для наших шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="80905-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="80905-117">В отличие от ранее измеренным временем мы используется диалоговое окно Добавить представление и создать шаблон «Empty» это время мы будем указать, что мы хотим автоматически» на основе скаффолдинга» Просмотр шаблона нам с содержимое по умолчанию в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="80905-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="80905-118">Это нужно сделать, выбрав элемент «Список» в «просмотр содержимого в раскрывающемся меню.</span><span class="sxs-lookup"><span data-stu-id="80905-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="80905-119">Помните, что когда был создан новый класс, необходимо скомпилировать приложение для его отображения в диалоговом окне Добавить представление.</span><span class="sxs-lookup"><span data-stu-id="80905-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Добавить представление](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="80905-121">Нажмите кнопку Добавить, и система автоматически создаст код для представления для нас, отображает наш список фильмов.</span><span class="sxs-lookup"><span data-stu-id="80905-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="80905-122">Это подходящий момент, чтобы изменить &lt;h2&gt; заголовок примерно в «Мой список фильмов», как мы делали ранее с помощью представления Hello World.</span><span class="sxs-lookup"><span data-stu-id="80905-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="80905-123">[![Фильмы - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="80905-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="80905-124">Запустите приложение и посетите /Movies в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="80905-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="80905-125">Теперь мы загрузили данные из базы данных, с помощью простого запроса в контроллере и возвращаются данные для представления, который знает о фильмах.</span><span class="sxs-lookup"><span data-stu-id="80905-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="80905-126">Это представление, то вращается через список фильмов и создает таблицу данных для нас.</span><span class="sxs-lookup"><span data-stu-id="80905-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="80905-127">[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="80905-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="80905-128">Мы не будет реализован функциональные возможности редактирования, подробные сведения и удалить с помощью этого приложения -, нам не нужен ссылки по умолчанию, шаблон формирования создан для нас.</span><span class="sxs-lookup"><span data-stu-id="80905-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="80905-129">Откройте файл /Movies/Index.aspx и удалите их.</span><span class="sxs-lookup"><span data-stu-id="80905-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="80905-130">Ниже приведен исходный код для наших обновленный шаблон представления должны выглядеть после внесении этих изменений:</span><span class="sxs-lookup"><span data-stu-id="80905-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="80905-131">Он является создание ссылок, которые нам не требуется, поэтому мы удалим их в этом примере.</span><span class="sxs-lookup"><span data-stu-id="80905-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="80905-132">Мы сохранит нашей Создание новой связи, как это Далее!</span><span class="sxs-lookup"><span data-stu-id="80905-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="80905-133">Вот как выглядит нашего приложения с выбранным столбцом удалены.</span><span class="sxs-lookup"><span data-stu-id="80905-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="80905-134">[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="80905-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="80905-135">Теперь у нас есть простого списка наши данные фильма.</span><span class="sxs-lookup"><span data-stu-id="80905-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="80905-136">Тем не менее если щелкнуть ссылку «Создать», мы получите ошибку, не подключен!</span><span class="sxs-lookup"><span data-stu-id="80905-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="80905-137">Давайте реализовать метод действия Create и дать пользователю возможность ввести новые фильмы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="80905-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="80905-138">[Назад](getting-started-with-mvc-part4.md)
> [Вперед](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="80905-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
