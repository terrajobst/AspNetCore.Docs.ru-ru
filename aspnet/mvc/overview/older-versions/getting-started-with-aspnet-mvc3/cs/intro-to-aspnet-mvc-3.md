---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Введение в ASP.NET MVC 3 (C#) | Документация Майкрософт
author: Rick-Anderson
description: Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, который является...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: fe98ad8a9f7891b3bf17d2da840d0c333d96a210
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836608"
---
<a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="83dfd-103">Введение в ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="83dfd-103">Intro to ASP.NET MVC 3 (C#)</span></span>
====================
<span data-ttu-id="83dfd-104">по [Рик Андерсон](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="83dfd-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="83dfd-105">Обновленную версию этого учебника доступен [здесь](../../../getting-started/introduction/getting-started.md) , использующий ASP.NET MVC 5 и Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="83dfd-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="83dfd-106">Она более безопасные, гораздо проще следовать и показаны дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="83dfd-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="83dfd-107">Этом учебнике описываются основы создания MVC веб-приложения ASP.NET с помощью Microsoft Visual Web Developer 2010 Express пакетом обновления 1, которой является бесплатной версии Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="83dfd-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="83dfd-108">Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже.</span><span class="sxs-lookup"><span data-stu-id="83dfd-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="83dfd-109">Все из них можно установить, щелкнув следующую ссылку: [установщика веб-платформы](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="83dfd-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="83dfd-110">Кроме того можно установить по отдельности необходимых компонентов, с помощью следующих ссылок:</span><span class="sxs-lookup"><span data-stu-id="83dfd-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="83dfd-111">Необходимые компоненты для Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="83dfd-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="83dfd-112">Обновление средств ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="83dfd-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="83dfd-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(среды выполнения и средства поддержки)</span><span class="sxs-lookup"><span data-stu-id="83dfd-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="83dfd-114">Если вы используете Visual Studio 2010 вместо Visual Web Developer 2010, необходимо установить необходимые компоненты, щелкнув следующую ссылку: [необходимых компонентов Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="83dfd-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="83dfd-115">Проект Visual Web Developer с исходным кодом C# — прилагаются в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="83dfd-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="83dfd-116">[Загрузить версию C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="83dfd-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="83dfd-117">Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/intro-to-aspnet-mvc-3.md) работы с этим руководством.</span><span class="sxs-lookup"><span data-stu-id="83dfd-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="83dfd-118">Что вы создадите</span><span class="sxs-lookup"><span data-stu-id="83dfd-118">What You'll Build</span></span>

<span data-ttu-id="83dfd-119">Вы реализуете простое приложение списка фильмов, который поддерживает создание, изменение и список фильмов из базы данных.</span><span class="sxs-lookup"><span data-stu-id="83dfd-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="83dfd-120">Ниже приведены два снимка экрана приложения, который вам предстоит создать.</span><span class="sxs-lookup"><span data-stu-id="83dfd-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="83dfd-121">Он включает страницы, отображающей список фильмов из базы данных:</span><span class="sxs-lookup"><span data-stu-id="83dfd-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="83dfd-123">Приложение также позволяет добавить, изменить и удалить фильмов, а также сведения о см. в разделе сведений об отдельных проблемах.</span><span class="sxs-lookup"><span data-stu-id="83dfd-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="83dfd-124">Все сценарии ввода данных включают проверку, чтобы гарантировать правильность данных, хранящихся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="83dfd-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="83dfd-125">Навыки, которые вы узнаете</span><span class="sxs-lookup"><span data-stu-id="83dfd-125">Skills You'll Learn</span></span>

<span data-ttu-id="83dfd-126">Вот, вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="83dfd-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="83dfd-127">Как создать новый проект ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="83dfd-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="83dfd-128">Как создать ASP.NET MVC контроллеры и представления.</span><span class="sxs-lookup"><span data-stu-id="83dfd-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="83dfd-129">Как создать новую базу данных с помощью Entity Framework Code First парадигма.</span><span class="sxs-lookup"><span data-stu-id="83dfd-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="83dfd-130">Способы получения и отображения данных.</span><span class="sxs-lookup"><span data-stu-id="83dfd-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="83dfd-131">Об изменении данных и проверки данных.</span><span class="sxs-lookup"><span data-stu-id="83dfd-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="83dfd-132">Начало работы</span><span class="sxs-lookup"><span data-stu-id="83dfd-132">Getting Started</span></span>

<span data-ttu-id="83dfd-133">Загрузите с Visual Web Developer 2010 Express («Visual Web Developer» для краткости), а затем выберите **новый проект** из **запустить** страницы.</span><span class="sxs-lookup"><span data-stu-id="83dfd-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="83dfd-134">Visual Web Developer — это интегрированная среда разработки, или интегрированной среды разработки.</span><span class="sxs-lookup"><span data-stu-id="83dfd-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="83dfd-135">Так же, как использовать Microsoft Word для записи документов, вы используете интегрированную среду разработки для создания приложений.</span><span class="sxs-lookup"><span data-stu-id="83dfd-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="83dfd-136">В Visual Web Developer имеется панель инструментов в верхней, показывающий различные параметры, доступные для вас.</span><span class="sxs-lookup"><span data-stu-id="83dfd-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="83dfd-137">Имеется также меню, которое предоставляет еще один способ выполнения задач в интегрированной среде разработки.</span><span class="sxs-lookup"><span data-stu-id="83dfd-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="83dfd-138">(Например, вместо выбора **новый проект** из **запустить** страницы, можно использовать меню и выберите **файл** &gt; **новый проект**.)</span><span class="sxs-lookup"><span data-stu-id="83dfd-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="83dfd-139">Создание первого приложения</span><span class="sxs-lookup"><span data-stu-id="83dfd-139">Creating Your First Application</span></span>

<span data-ttu-id="83dfd-140">Можно создавать приложения с помощью Visual Basic или Visual C# в качестве языка программирования.</span><span class="sxs-lookup"><span data-stu-id="83dfd-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="83dfd-141">Выберите Visual C# в левой части, а затем выберите **веб-приложение ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="83dfd-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="83dfd-142">Назовите проект «MvcMovie» и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="83dfd-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="83dfd-143">(Если вы предпочитаете Visual Basic, переключитесь в [версии Visual Basic](../vb/intro-to-aspnet-mvc-3.md) данного руководства.)</span><span class="sxs-lookup"><span data-stu-id="83dfd-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="83dfd-144">В **Создание проекта ASP.NET MVC 3** выберите **веб-приложение**.</span><span class="sxs-lookup"><span data-stu-id="83dfd-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="83dfd-145">Проверьте **HTML5 использования разметки** и оставить **Razor** как обработчик представлений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="83dfd-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="83dfd-146">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="83dfd-146">Click **OK**.</span></span> <span data-ttu-id="83dfd-147">Visual Web Developer используется шаблон по умолчанию для проекта ASP.NET MVC, который вы только что создали, поэтому у вас есть рабочее приложение прямо сейчас не выполняя никаких действий!</span><span class="sxs-lookup"><span data-stu-id="83dfd-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="83dfd-148">Это связано с простых «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="83dfd-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="83dfd-149">проекта, а вот отлично подходит для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="83dfd-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="83dfd-150">В меню **Отладка** выберите пункт **Начать отладку**.</span><span class="sxs-lookup"><span data-stu-id="83dfd-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="83dfd-151">Обратите внимание, что сочетание клавиш, чтобы начать отладку F5.</span><span class="sxs-lookup"><span data-stu-id="83dfd-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="83dfd-152">F5 приводит к Visual Web Developer для запуска веб-сервера разработки и выполнения веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="83dfd-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="83dfd-153">Затем Visual Web Developer запускает браузер и откроется домашняя страница приложения.</span><span class="sxs-lookup"><span data-stu-id="83dfd-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="83dfd-154">Обратите внимание, что говорит в адресной строке браузера `localhost` и не что-либо типа `example.com`.</span><span class="sxs-lookup"><span data-stu-id="83dfd-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="83dfd-155">Это потому, что `localhost` всегда указывает на локальном компьютере, который выполняется, в этом случае приложение, вы только что выполнили.</span><span class="sxs-lookup"><span data-stu-id="83dfd-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="83dfd-156">При запуске веб-проекта Visual Web Developer для веб-сервера используется случайный порт.</span><span class="sxs-lookup"><span data-stu-id="83dfd-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="83dfd-157">В приведенном ниже рисунке случайный номер порта является 43246.</span><span class="sxs-lookup"><span data-stu-id="83dfd-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="83dfd-158">При запуске приложения, вы, скорее всего, увидите другой номер порта.</span><span class="sxs-lookup"><span data-stu-id="83dfd-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="83dfd-159">Готовые этот шаблон по умолчанию позволяет две страницы, чтобы перейти на страницу основное имя для входа.</span><span class="sxs-lookup"><span data-stu-id="83dfd-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="83dfd-160">Следующий шаг — изменить работу этого приложения и немного поговорим об ASP.NET MVC в процессе.</span><span class="sxs-lookup"><span data-stu-id="83dfd-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="83dfd-161">Закройте обозреватель и изменим код.</span><span class="sxs-lookup"><span data-stu-id="83dfd-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="83dfd-162">Вперед</span><span class="sxs-lookup"><span data-stu-id="83dfd-162">Next</span></span>](adding-a-controller.md)
