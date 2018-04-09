---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Часть 1: Файл -> Новый проект | Документы Microsoft'
author: JoeStagner
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks. Часть 1 рассматриваются общие сведения и файл/создать проект.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: a1b9681516e626b6a0eec420b168a74e05d88afb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="part-1-file--new-project"></a><span data-ttu-id="13af8-104">Часть 1: Файл -> Новый проект</span><span class="sxs-lookup"><span data-stu-id="13af8-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="13af8-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="13af8-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="13af8-106">Tailspin Spyworks показано, как чрезвычайно простой можно создавать мощные и масштабируемые приложения для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="13af8-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="13af8-107">Он показывает как использовать новые возможности в ASP.NET 4 для создания Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="13af8-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="13af8-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="13af8-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="13af8-109">Часть 1 рассматриваются общие сведения и файл/создать проект.</span><span class="sxs-lookup"><span data-stu-id="13af8-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="13af8-110">Общие сведения</span><span class="sxs-lookup"><span data-stu-id="13af8-110">Overview</span></span>

<span data-ttu-id="13af8-111">Этот учебник содержит основные сведения о веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13af8-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="13af8-112">Мы будем медленному запуску, поэтому новичок уровня веб-разработки допустимо.</span><span class="sxs-lookup"><span data-stu-id="13af8-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="13af8-113">Мы будем строить приложения — это простой интерактивной хранилище.</span><span class="sxs-lookup"><span data-stu-id="13af8-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="13af8-114">Посетители могут просматривать продукты по категориям:</span><span class="sxs-lookup"><span data-stu-id="13af8-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="13af8-115">Их можно просматривать один продукт и добавить его в корзину.</span><span class="sxs-lookup"><span data-stu-id="13af8-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="13af8-116">Их можно просмотреть их покупок, удаляя все элементы, которые больше не нужны.</span><span class="sxs-lookup"><span data-stu-id="13af8-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="13af8-117">Продолжить извлечение будет запрашивать</span><span class="sxs-lookup"><span data-stu-id="13af8-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="13af8-118">После упорядочения, они увидят экран простой подтверждения:</span><span class="sxs-lookup"><span data-stu-id="13af8-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="13af8-119">Начнем с создания нового проекта веб-форм ASP.NET в Visual Studio 2010 и постепенно мы добавим компонентов, чтобы создать работающее приложение.</span><span class="sxs-lookup"><span data-stu-id="13af8-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="13af8-120">Попутно мы обсудим доступ к базе данных, представления списка и сетки, обновление страниц данных, проверка данных с помощью главных страниц для согласованного макета страницы, AJAX, проверки, членство и многое другое.</span><span class="sxs-lookup"><span data-stu-id="13af8-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="13af8-121">Шаг за шагом продолжить работу, или можно загрузить из завершенного приложения [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="13af8-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="13af8-122">Можно использовать Visual Studio 2010 или произвольным Visual Web Developer 2010 из [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="13af8-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="13af8-123">Чтобы построить приложение, можно использовать SQL Server или произвольным SQL Server Express для размещения базы данных.</span><span class="sxs-lookup"><span data-stu-id="13af8-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="13af8-124">Файл / Создать проект</span><span class="sxs-lookup"><span data-stu-id="13af8-124">File / New Project</span></span>

<span data-ttu-id="13af8-125">Мы начнем, выбрав в меню "файл" в Visual Studio новый проект.</span><span class="sxs-lookup"><span data-stu-id="13af8-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="13af8-126">Откроется диалоговое окно нового проекта.</span><span class="sxs-lookup"><span data-stu-id="13af8-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="13af8-127">Мы выберем Visual C# / веб-шаблонов группе в левой части экрана и выберите шаблон «Веб-приложение ASP.NET» в центральном столбце.</span><span class="sxs-lookup"><span data-stu-id="13af8-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="13af8-128">Имя проекта TailspinSpyworks и нажмите кнопку OK.</span><span class="sxs-lookup"><span data-stu-id="13af8-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="13af8-129">Это создаст нашего проекта.</span><span class="sxs-lookup"><span data-stu-id="13af8-129">This will create our project.</span></span> <span data-ttu-id="13af8-130">Давайте рассмотрим папки, которые включаются в приложении в обозреватель решений на правой стороне.</span><span class="sxs-lookup"><span data-stu-id="13af8-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="13af8-131">Пустое решение не полностью пустой — он добавляет структура базовой папки:</span><span class="sxs-lookup"><span data-stu-id="13af8-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="13af8-132">Обратите внимание, соглашения, реализовано с помощью шаблона проекта ASP.NET 4 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13af8-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="13af8-133">Папка «Учетная запись» реализует базовый пользовательский интерфейс для ASP. Подсистема членства в NET.</span><span class="sxs-lookup"><span data-stu-id="13af8-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="13af8-134">В папку «Скрипты» используется в качестве репозитория для файлов JavaScript на клиентской стороне и core jQuery JS-файлы, становятся доступными по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13af8-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="13af8-135">Папка «Стили» используется для упорядочивания визуальные элементы веб-сайта (таблицы стилей CSS)</span><span class="sxs-lookup"><span data-stu-id="13af8-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="13af8-136">После нажатия клавиши F5 для запуска приложения и визуализации страницы default.aspx, мы увидим следующее.</span><span class="sxs-lookup"><span data-stu-id="13af8-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="13af8-137">Заменить файл Style.css от шаблона по умолчанию веб-форм с классами CSS и связанное изображение файлы, которые будут отображать visual asthetics, для которого требуется для нашего приложения Tailspin Spyworks будет наш первый расширения приложения.</span><span class="sxs-lookup"><span data-stu-id="13af8-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="13af8-138">После этого нашу страницу default.aspx отрисовывает следующим образом.</span><span class="sxs-lookup"><span data-stu-id="13af8-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="13af8-139">Обратите внимание, ссылки изображения в верхней правой части страницы и элементы меню, которые были добавлены на главную страницу.</span><span class="sxs-lookup"><span data-stu-id="13af8-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="13af8-140">Только «Вход» и «Учетная запись» ссылки указывают на страницы, которые существуют (созданной с помощью шаблона по умолчанию) и остальная часть страницы, которые мы будем реализовывать при создании приложения.</span><span class="sxs-lookup"><span data-stu-id="13af8-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="13af8-141">Мы также будем переместить каталог стили главной страницы.</span><span class="sxs-lookup"><span data-stu-id="13af8-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="13af8-142">Хотя это только предпочтений он может облегчить немного Если мы решили сделать наше приложение «skinable» в будущем.</span><span class="sxs-lookup"><span data-stu-id="13af8-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="13af8-143">После этого необходимо изменить на главную страницу ссылки в ASPX-файлах, созданных по умолчанию страницы веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="13af8-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="13af8-144">Вперед</span><span class="sxs-lookup"><span data-stu-id="13af8-144">Next</span></span>](tailspin-spyworks-part-2.md)
