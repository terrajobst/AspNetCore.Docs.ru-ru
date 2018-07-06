---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Доступ к данным модели из контроллера | Документация Майкрософт
author: shanselman
description: Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC. Создание простого веб-приложения, которое считывает и записывает в базу данных.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 556eed77cbd9e81c0a2a1334bb0a8ee56abafd34
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815011"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="c14ad-104">Доступ к данным модели из контроллера</span><span class="sxs-lookup"><span data-stu-id="c14ad-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="c14ad-105">по [(Scott hanselman)](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c14ad-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c14ad-106">Это руководство для начинающих, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c14ad-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c14ad-107">Вы создадите простое веб-приложение, которое считывает и записывает в базу данных.</span><span class="sxs-lookup"><span data-stu-id="c14ad-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c14ad-108">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="c14ad-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="c14ad-109">В этом разделе мы собираемся создавать новый класс MoviesController и написать код, получающий данные фильма и отображает его обратно в браузере с помощью шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="c14ad-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="c14ad-110">Щелкните правой кнопкой папку Controllers и сделать новый MoviesController.</span><span class="sxs-lookup"><span data-stu-id="c14ad-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="c14ad-111">[![Добавление контроллера](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c14ad-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="c14ad-112">Это создаст новый файл «MoviesController.cs» под наших \Controllers папку в наш проект.</span><span class="sxs-lookup"><span data-stu-id="c14ad-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="c14ad-113">Давайте обновим шаблонов для MovieController для извлечения из базы данных вновь заполненный список фильмов.</span><span class="sxs-lookup"><span data-stu-id="c14ad-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="c14ad-114">Мы занимаемся запрос LINQ, так что мы только получения фильмов, вышедших после лето 1984 г.</span><span class="sxs-lookup"><span data-stu-id="c14ad-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="c14ad-115">Нам потребуется шаблону представления для отображения этот список фильмов обратно, поэтому щелкните правой кнопкой мыши в методе и выберите Add View, для его создания.</span><span class="sxs-lookup"><span data-stu-id="c14ad-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="c14ad-116">В диалоговом окне Добавление представления мы будем указывать, что мы передаем список&lt;Movies.Models.Movie&gt; к шаблону представления.</span><span class="sxs-lookup"><span data-stu-id="c14ad-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="c14ad-117">В отличие от ранее измеренным временем, мы использовали диалоговое окно Добавить представление и создать шаблон «Empty» это время, мы будем указать, что мы хотим Visual Studio автоматически «сформировать шаблон» шаблона представления для нас с содержимое по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="c14ad-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="c14ad-118">Мы сделаем это, выбрав элемент «Список» в «просмотр содержимого в раскрывающемся меню.</span><span class="sxs-lookup"><span data-stu-id="c14ad-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="c14ad-119">Помните, что когда был создан новый класс, вам потребуется скомпилировать приложение для его появления в диалоговом окне Добавить представление.</span><span class="sxs-lookup"><span data-stu-id="c14ad-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Добавление представления](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="c14ad-121">Щелкните "Добавить", и система автоматически создаст код для представления для нас, отображает наш список фильмов.</span><span class="sxs-lookup"><span data-stu-id="c14ad-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="c14ad-122">Это хорошая возможность изменить &lt;h2&gt; заголовок примерно в «Мои Movie List», как это делалось ранее с помощью представления Hello World.</span><span class="sxs-lookup"><span data-stu-id="c14ad-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="c14ad-123">[![Фильмы - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c14ad-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="c14ad-124">Запустите приложение и посетите /Movies в адресной строке.</span><span class="sxs-lookup"><span data-stu-id="c14ad-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="c14ad-125">Теперь мы извлечение данных из базы данных, с помощью простого запроса внутри контроллера и возвращаются данные для представления, который знает о фильмах.</span><span class="sxs-lookup"><span data-stu-id="c14ad-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="c14ad-126">Представление затем вращается через список фильмов и создает таблицу данных для нас.</span><span class="sxs-lookup"><span data-stu-id="c14ad-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="c14ad-127">[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="c14ad-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="c14ad-128">Мы не реализация функциональных возможностей редактирования "," Сведения "и" Delete с помощью этого приложения - поэтому нам не нужен ссылки по умолчанию, который каркаса шаблон создан для нас.</span><span class="sxs-lookup"><span data-stu-id="c14ad-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="c14ad-129">Откройте файл /Movies/Index.aspx и удалить их.</span><span class="sxs-lookup"><span data-stu-id="c14ad-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="c14ad-130">Ниже приведен исходный код для наших обновленный шаблон представления должно выглядеть сразу после этих изменений:</span><span class="sxs-lookup"><span data-stu-id="c14ad-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="c14ad-131">Идет создание ссылки, которые нам не требуется, поэтому мы удалим их в этом примере.</span><span class="sxs-lookup"><span data-stu-id="c14ad-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="c14ad-132">Мы будем сообщать наши Создание новой связи, так как именно Далее!</span><span class="sxs-lookup"><span data-stu-id="c14ad-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="c14ad-133">Вот, как выглядит наше приложение с выбранным столбцом удалены.</span><span class="sxs-lookup"><span data-stu-id="c14ad-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="c14ad-134">[![Список фильмов - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="c14ad-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="c14ad-135">Теперь у нас есть простой список наших данных фильмов.</span><span class="sxs-lookup"><span data-stu-id="c14ad-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="c14ad-136">Тем не менее если мы щелкните ссылку «Создать», мы получаем сообщение об ошибке, так как он не подключен!</span><span class="sxs-lookup"><span data-stu-id="c14ad-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="c14ad-137">Давайте реализовать метод действия Create что позволяет пользователю для ввода новых фильмов в нашей базе данных.</span><span class="sxs-lookup"><span data-stu-id="c14ad-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c14ad-138">[Назад](getting-started-with-mvc-part4.md)
> [Вперед](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="c14ad-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
