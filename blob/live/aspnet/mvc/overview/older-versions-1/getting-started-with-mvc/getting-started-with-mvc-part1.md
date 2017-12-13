---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: "Введение в ASP.NET MVC | Документы Microsoft"
author: shanselman
description: "Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC. Вы создадите простое веб-приложение выполняет чтение и запись из базы данных."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: b884c3c535929038f047e2c4ceedf9e7ca543292
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="d7c4c-104">Введение в ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d7c4c-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="d7c4c-105">по [Скотт Хансельман](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="d7c4c-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="d7c4c-106">Обновленная версия, если этот учебник доступен [здесь](../../getting-started/introduction/getting-started.md) с помощью [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span><span class="sxs-lookup"><span data-stu-id="d7c4c-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).</span></span> <span data-ttu-id="d7c4c-107">Новое учебное использует ASP.NET MVC 5 обеспечивает множество преимуществ по сравнению этого учебника.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
> 
> 
> <span data-ttu-id="d7c4c-108">Это руководство для новичков, в котором представлены основные сведения по ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="d7c4c-109">Вы создадите простое веб-приложение выполняет чтение и запись из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="d7c4c-110">Посетите [центр обучения ASP.NET MVC](../../../index.md) для поиска других ASP.NET MVC, учебники и примеры.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="d7c4c-111">Создадим нашей первой веб-приложение ASP.NET MVC с помощью [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="d7c4c-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="d7c4c-112">Мы выполним немного приложения список фильмов, сообщите нам создать и список фильмов.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="d7c4c-113">Что мы создадим</span><span class="sxs-lookup"><span data-stu-id="d7c4c-113">What You'll Build</span></span>

<span data-ttu-id="d7c4c-114">Вот приложение, которое мы создадим два снимка экрана.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="d7c4c-115">Имеется простой таблицы фильмов с различными столбцами.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="d7c4c-116">[![Список фильмов - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d7c4c-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="d7c4c-117">И необходимо создать форму, можно добавить в список фильмов.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="d7c4c-118">[![Создание фильма - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d7c4c-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="d7c4c-119">Навыки, которые вы узнаете</span><span class="sxs-lookup"><span data-stu-id="d7c4c-119">Skills You'll Learn</span></span>

<span data-ttu-id="d7c4c-120">Этот учебник поможет узнать основы создания веб-приложения ASP.NET MVC с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="d7c4c-121">Вы узнаете следующее.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-121">You'll learn:</span></span>

- <span data-ttu-id="d7c4c-122">Создание нового проекта ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="d7c4c-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="d7c4c-123">Как создать новую базу данных с SQL Server</span><span class="sxs-lookup"><span data-stu-id="d7c4c-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="d7c4c-124">Создание ASP.NET MVC контроллеры и представления</span><span class="sxs-lookup"><span data-stu-id="d7c4c-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="d7c4c-125">Способы получения и отображения данных</span><span class="sxs-lookup"><span data-stu-id="d7c4c-125">How to retrieve and display data</span></span>
- <span data-ttu-id="d7c4c-126">Включение проверки данных и изменение данных</span><span class="sxs-lookup"><span data-stu-id="d7c4c-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="d7c4c-127">Как обновить схему базы данных</span><span class="sxs-lookup"><span data-stu-id="d7c4c-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="d7c4c-128">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="d7c4c-128">Get Started</span></span>

<span data-ttu-id="d7c4c-129">Запустить Visual Web Developer 2010 Express (назовем ее «VWD» теперь) и выберите новый проект с экрана "Пуск".</span><span class="sxs-lookup"><span data-stu-id="d7c4c-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="d7c4c-130">Visual Web Developer — это интегрированная среда разработки, или интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="d7c4c-131">Так же, как используется Microsoft Word для записи документов, воспользуемся интегрированной среды разработки для создания приложений.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="d7c4c-132">Панель инструментов в верхней, показывающий различные параметры, доступные для вас, а также меню, вы также использовали выберите файл | Новый проект.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="d7c4c-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d7c4c-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="d7c4c-134">Создание первого приложения</span><span class="sxs-lookup"><span data-stu-id="d7c4c-134">Creating Your First Application</span></span>

<span data-ttu-id="d7c4c-135">Можно создавать приложения, Visual Basic или Visual C#.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="d7c4c-136">Теперь выберите Visual C# в левой части экрана, затем выберите «Веб-приложение ASP.NET MVC 2».</span><span class="sxs-lookup"><span data-stu-id="d7c4c-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="d7c4c-137">Присвойте проекту имя «Фильмов» и нажмите кнопку ОК.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="d7c4c-138">[![Создание проекта](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d7c4c-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="d7c4c-139">С правой стороны находится в обозревателе решений все файлы и папки в приложении.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="d7c4c-140">Большие окна в середине — где изменения кода и большую часть времени.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="d7c4c-141">Visual Studio используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому в данный момент есть рабочее приложение, не выполняя никаких действий!</span><span class="sxs-lookup"><span data-stu-id="d7c4c-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="d7c4c-142">Это простое «Hello World!</span><span class="sxs-lookup"><span data-stu-id="d7c4c-142">This is a simple "Hello World!</span></span> <span data-ttu-id="d7c4c-143">проект и является хорошей отправной точкой для нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="d7c4c-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="d7c4c-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="d7c4c-145">Нажмите кнопку «Воспроизведение» на панели инструментов.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-145">Select the "play" button to the toolbar.</span></span>

![Начало отладки](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="d7c4c-147">Это зеленая стрелка вправо, который будет выполняться компиляция программы и запустить приложение в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="d7c4c-148">*Примечание: Можно вместо этого нажмите клавишу F5 на клавиатуре или выберите отладки -&gt;запуск отладки из меню «Отладка».*</span><span class="sxs-lookup"><span data-stu-id="d7c4c-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="d7c4c-149">Это приведет к Visual Web Developer для запуска веб сервера разработки и выполнения наших веб-приложения (не существует конфигурации или вручную шаги, необходимые для этого).</span><span class="sxs-lookup"><span data-stu-id="d7c4c-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="d7c4c-150">Затем будет запустите обозреватель и настроить его для просмотра домашней страницы приложения.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="d7c4c-151">Обратите внимание, ниже говорится в адресной строке браузера, «localhost», а не примерно example.com. Это, так как localhost всегда указывает на локальном компьютере -, в этом случае работающего приложения, который мы только что создали.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-151">Notice below that the address bar of the browser says "localhost", and not something like example.com. That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="d7c4c-152">[![Домашняя страница](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="d7c4c-152">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="d7c4c-153">Без дополнительной настройки этот шаблон по умолчанию предоставляет основные входа и вы две страницы для перехода.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-153">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="d7c4c-154">Давайте изменить работу этого приложения и немного узнать о ASP.NET MVC в процессе.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-154">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="d7c4c-155">Закройте браузер, а также позволяет изменить часть кода.</span><span class="sxs-lookup"><span data-stu-id="d7c4c-155">Close your browser and lets change some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d7c4c-156">Вперед</span><span class="sxs-lookup"><span data-stu-id="d7c4c-156">Next</span></span>](getting-started-with-mvc-part2.md)
