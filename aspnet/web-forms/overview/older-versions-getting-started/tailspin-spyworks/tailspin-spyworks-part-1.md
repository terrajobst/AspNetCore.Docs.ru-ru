---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Часть 1: Файл -> Создать проект | Документация Майкрософт'
author: JoeStagner
description: В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks. Часть 1 рассматриваются общие сведения и файл/создать проект.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 1c8ba561745c9c02b5513e326b4739ec97338393
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825691"
---
<a name="part-1-file--new-project"></a><span data-ttu-id="37e16-104">Часть 1: Файл -> Создать проект</span><span class="sxs-lookup"><span data-stu-id="37e16-104">Part 1: File-> New Project</span></span>
====================
<span data-ttu-id="37e16-105">по [(Joe Stagner)](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="37e16-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="37e16-106">Tailspin Spyworks демонстрирует, как чрезвычайно прост процесс создания мощных и масштабируемых приложений для платформы .NET.</span><span class="sxs-lookup"><span data-stu-id="37e16-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="37e16-107">Он демонстрирует способы использования новые возможности в ASP.NET 4 для создание Интернет-магазина, включая покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="37e16-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="37e16-108">В этой серии руководств описаны все действия, предпринимаемые для создайте пример приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="37e16-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="37e16-109">Часть 1 рассматриваются общие сведения и файл/создать проект.</span><span class="sxs-lookup"><span data-stu-id="37e16-109">Part 1 covers Overview and File/New Project.</span></span>


## <a id="_Toc260221666"></a>  <span data-ttu-id="37e16-110">Общие сведения о</span><span class="sxs-lookup"><span data-stu-id="37e16-110">Overview</span></span>

<span data-ttu-id="37e16-111">Это руководство представляет собой введение в веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="37e16-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="37e16-112">Мы будет запускаться медленно, поэтому уровня веб-разработка для начинающих допустимо.</span><span class="sxs-lookup"><span data-stu-id="37e16-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="37e16-113">Приложение, которое мы будем строить является простого Интернет-магазина.</span><span class="sxs-lookup"><span data-stu-id="37e16-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)


<span data-ttu-id="37e16-114">Посетители могут просматривать продукты по категориям:</span><span class="sxs-lookup"><span data-stu-id="37e16-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="37e16-115">Их можно просматривать один продукт и добавить его в корзину.</span><span class="sxs-lookup"><span data-stu-id="37e16-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="37e16-116">Их можно просмотреть их покупок, удаляя все элементы, которые больше не нужны.</span><span class="sxs-lookup"><span data-stu-id="37e16-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="37e16-117">Продолжить извлечение получит запрос на</span><span class="sxs-lookup"><span data-stu-id="37e16-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="37e16-118">После упорядочения, они увидят экран подтверждения простой:</span><span class="sxs-lookup"><span data-stu-id="37e16-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)


<span data-ttu-id="37e16-119">Начнем с создания проекта веб-форм ASP.NET в Visual Studio 2010, и постепенно добавлять функции, чтобы создать работающее приложение.</span><span class="sxs-lookup"><span data-stu-id="37e16-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="37e16-120">Попутно мы обсудим доступа к базе данных, представления списка и сетки, обновление страниц данных, проверка данных с помощью главных страниц для согласованный макет страницы, AJAX, проверки, членство пользователей и многое другое.</span><span class="sxs-lookup"><span data-stu-id="37e16-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="37e16-121">Пошаговая процедура выполнения этой процедуры, или можно загрузить готовое приложение из [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="37e16-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="37e16-122">Можно использовать Visual Studio 2010 или бесплатный Visual Web Developer 2010 из [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="37e16-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="37e16-123">Для построения приложения, можно использовать SQL Server или бесплатная SQL Server Express для размещения базы данных.</span><span class="sxs-lookup"><span data-stu-id="37e16-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a>  <span data-ttu-id="37e16-124">Файл / Создать проект</span><span class="sxs-lookup"><span data-stu-id="37e16-124">File / New Project</span></span>

<span data-ttu-id="37e16-125">Мы начнем, выбрав в меню "файл" в Visual Studio новый проект.</span><span class="sxs-lookup"><span data-stu-id="37e16-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="37e16-126">Откроется диалоговое окно нового проекта.</span><span class="sxs-lookup"><span data-stu-id="37e16-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="37e16-127">Мы выберем Visual C# / группе в левой части веб-шаблонов и затем выберите шаблон «Веб-приложение ASP.NET» в центральном столбце.</span><span class="sxs-lookup"><span data-stu-id="37e16-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="37e16-128">Имя проекта TailspinSpyworks и нажмите кнопку OK.</span><span class="sxs-lookup"><span data-stu-id="37e16-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="37e16-129">Наш проект будет создан.</span><span class="sxs-lookup"><span data-stu-id="37e16-129">This will create our project.</span></span> <span data-ttu-id="37e16-130">Давайте рассмотрим папки, которые включены в наше приложение в обозревателе решений правой стороны.</span><span class="sxs-lookup"><span data-stu-id="37e16-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="37e16-131">Пустое решение не полностью пустой — она добавляет структуру к базовой папки:</span><span class="sxs-lookup"><span data-stu-id="37e16-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="37e16-132">Обратите внимание, соглашения, реализовано с помощью шаблона проекта по умолчанию ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="37e16-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="37e16-133">Папка «Учетная запись» реализует основной пользовательский интерфейс для ASP. Подсистема членства NET.</span><span class="sxs-lookup"><span data-stu-id="37e16-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="37e16-134">В папку «Скрипты» используется в качестве репозитория для файлов JavaScript на стороне клиента и JS-файлы jQuery core становятся доступными по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="37e16-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="37e16-135">Папка «Стили» используется для упорядочивания визуальные элементы (таблицы стилей CSS), веб-сайта</span><span class="sxs-lookup"><span data-stu-id="37e16-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="37e16-136">После нажатия клавиши F5 для запуска приложения и отображать страницу default.aspx, вы увидите следующее.</span><span class="sxs-lookup"><span data-stu-id="37e16-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="37e16-137">Наш первый расширения приложение будет заменить файл Style.css в шаблоне веб-форм по умолчанию классы CSS и связанные файлы изображений, которые будут отображать visual asthetics, для которого требуется для нашего приложения Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="37e16-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="37e16-138">После этого страницу default.aspx отображает следующим образом.</span><span class="sxs-lookup"><span data-stu-id="37e16-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="37e16-139">Обратите внимание, что ссылки изображений в верхней правой части страницы и элементы, которые были добавлены на главную страницу.</span><span class="sxs-lookup"><span data-stu-id="37e16-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="37e16-140">Только эти ссылки «Вход» и «Учетная запись» указывают на страницы, существующих (созданных с помощью шаблона по умолчанию) и остальная часть страницы, которые мы будем реализовывать, когда мы создаем нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="37e16-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="37e16-141">Мы также хотим переместить главной страницы в каталоге стили.</span><span class="sxs-lookup"><span data-stu-id="37e16-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="37e16-142">Хотя это только предпочтения он может облегчить немного Если мы решили сделать наше приложение «skinable» в будущем.</span><span class="sxs-lookup"><span data-stu-id="37e16-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="37e16-143">После этого нам нужно будет изменить главной страницы ссылки во всех файлах .aspx создается по умолчанию страницы веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="37e16-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="37e16-144">Вперед</span><span class="sxs-lookup"><span data-stu-id="37e16-144">Next</span></span>](tailspin-spyworks-part-2.md)
