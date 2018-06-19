---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Часть 1: Общие сведения и файл -> Новый проект | Документы Microsoft'
author: jongalloway
description: Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store. Часть 1 рассматриваются общие сведения и файл -> Новый проект.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868975"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="fb0b9-104">Часть 1: Общие сведения и файл -> Новый проект</span><span class="sxs-lookup"><span data-stu-id="fb0b9-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="fb0b9-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="fb0b9-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="fb0b9-106">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="fb0b9-107">MVC Music Store показана реализация хранилища упрощенных образец продает велосипеды через Интернет альбомы, которое реализует Администрирование сайта основные, вход пользователей и функциональные возможности корзины покупок.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="fb0b9-108">Этот учебник ряд подробно описываются шаги, необходимые для построения образца приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="fb0b9-109">Часть 1 рассматриваются общие сведения и файл -&gt;новый проект.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="fb0b9-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="fb0b9-110">Overview</span></span>

<span data-ttu-id="fb0b9-111">MVC Music Store является учебное приложение, которое вводятся и описываются пошаговые способы использования ASP.NET MVC и Visual Web Developer для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="fb0b9-112">Мы будем медленному запуску, поэтому новичок уровня веб-разработки допустимо.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="fb0b9-113">Мы будем строить приложения — это простой музыка хранилище.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="fb0b9-114">Существуют три основные части в приложение: покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="fb0b9-115">Посетители могут перейдите альбомов по жанру.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="fb0b9-116">Их можно просматривать одного альбома и добавить его в корзину.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="fb0b9-117">Их можно просмотреть их покупок, удаляя все элементы, которые больше не нужны.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="fb0b9-118">Продолжить извлечение будет предлагать им вход или Зарегистрируйтесь для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="fb0b9-119">После создания учетной записи, они могут выполнить порядок, заполнив доставки и сведения о платеже.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="fb0b9-120">Для простоты мы запускаем прекрасных повышения роли: все, что предоставляется бесплатно, если они введите код рекламной акции «FREE»!</span><span class="sxs-lookup"><span data-stu-id="fb0b9-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="fb0b9-121">После упорядочения, они увидят экран простой подтверждения:</span><span class="sxs-lookup"><span data-stu-id="fb0b9-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="fb0b9-122">Помимо страниц faceing клиента мы также построения администратора раздел, описывающий список альбомы, из которых администраторы могут создавать, редактировать и удалить альбомы:</span><span class="sxs-lookup"><span data-stu-id="fb0b9-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="fb0b9-123">1. Файл —&gt; нового проекта</span><span class="sxs-lookup"><span data-stu-id="fb0b9-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="fb0b9-124">Установка программного обеспечения</span><span class="sxs-lookup"><span data-stu-id="fb0b9-124">Installing the software</span></span>

<span data-ttu-id="fb0b9-125">Этот учебник начнется путем создания нового проекта ASP.NET MVC 3 с помощью свободного Visual Web Developer 2010 Express (который предоставляется бесплатно), а затем постепенно добавим компонентов, чтобы создать работающее приложение.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="fb0b9-126">Попутно мы обсудим доступ к базе данных, сценарии учета формы, проверка данных, использование главных страниц для согласованного макета страницы, с помощью AJAX для обновления страницы и проверки, имя входа пользователя и многое другое.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="fb0b9-127">Можно продолжить работу, шаг за шагом, или можно загрузить завершенное приложение из [MVC Music Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="fb0b9-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="fb0b9-128">С пакетом обновления 1 для Visual Studio 2010 или Visual Web Developer 2010 Express с пакетом обновления 1 (это бесплатная версия Visual Studio 2010) можно использовать для построения приложения.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="fb0b9-129">Мы будем работать с SQL Server Compact (также бесплатно) для размещения базы данных.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="fb0b9-130">Прежде чем начать, убедитесь, что вы установили необходимые компоненты, перечисленные ниже.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="fb0b9-131">[Необходимых компонентов visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="fb0b9-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="fb0b9-132">[Обновление средств ASP.NET MVC 3]</span><span class="sxs-lookup"><span data-stu-id="fb0b9-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="fb0b9-133">[SQL Server Compact 4.0] - включая поддержку среды выполнения и средств</span><span class="sxs-lookup"><span data-stu-id="fb0b9-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="fb0b9-134">Создание нового проекта ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="fb0b9-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="fb0b9-135">Мы начнем, выбрав «Новый проект» из меню «файл» в Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="fb0b9-136">Откроется диалоговое окно нового проекта.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="fb0b9-137">Мы выберем Visual C# -&gt; веб-шаблонов группе в левой части экрана, а затем выберите шаблон «ASP.NET MVC 3 веб-приложения» в центральном столбце.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="fb0b9-138">Имя проекта MvcMusicStore и нажмите кнопку OK.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="fb0b9-139">Это позволит отобразить получателей диалоговое окно, которое позволяет получить некоторые MVC определенные параметры для нашей проекта.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="fb0b9-140">Выберите следующие параметры.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-140">Select the following:</span></span>

<span data-ttu-id="fb0b9-141">Шаблон проекта: щелкните пустое</span><span class="sxs-lookup"><span data-stu-id="fb0b9-141">Project Template - select Empty</span></span>

<span data-ttu-id="fb0b9-142">Просмотр обработчика - выберите Razor</span><span class="sxs-lookup"><span data-stu-id="fb0b9-142">View Engine - select Razor</span></span>

<span data-ttu-id="fb0b9-143">Использовать семантическую разметку HTML5 — установлен</span><span class="sxs-lookup"><span data-stu-id="fb0b9-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="fb0b9-144">Убедитесь, что параметры, как показано ниже, а затем нажмите кнопку OK.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="fb0b9-145">Это создаст нашего проекта.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-145">This will create our project.</span></span> <span data-ttu-id="fb0b9-146">Давайте рассмотрим папки, которые были добавлены для нашего приложения в обозревателе решений на правой стороне.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="fb0b9-147">Не полностью пустой шаблон пустой MVC 3 — он добавляет структура базовой папки:</span><span class="sxs-lookup"><span data-stu-id="fb0b9-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="fb0b9-148">ASP.NET MVC использует некоторые основные стандарты именования в именах папок:</span><span class="sxs-lookup"><span data-stu-id="fb0b9-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="fb0b9-149">**Folder**</span><span class="sxs-lookup"><span data-stu-id="fb0b9-149">**Folder**</span></span> | <span data-ttu-id="fb0b9-150">**Назначение**</span><span class="sxs-lookup"><span data-stu-id="fb0b9-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="fb0b9-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="fb0b9-151">**/Controllers**</span></span> | <span data-ttu-id="fb0b9-152">Контроллеры реагировать на ввод из браузера, решить, что с ним делать и возвращает его пользователю.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="fb0b9-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="fb0b9-153">**/Views**</span></span> | <span data-ttu-id="fb0b9-154">Представления содержат наши шаблоны пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="fb0b9-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="fb0b9-155">**И моделей**</span><span class="sxs-lookup"><span data-stu-id="fb0b9-155">**/Models**</span></span> | <span data-ttu-id="fb0b9-156">Модели хранения и управления данными</span><span class="sxs-lookup"><span data-stu-id="fb0b9-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="fb0b9-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="fb0b9-157">**/Content**</span></span> | <span data-ttu-id="fb0b9-158">Эта папка содержит нашей изображений, CSS и статическое содержимое</span><span class="sxs-lookup"><span data-stu-id="fb0b9-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="fb0b9-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="fb0b9-159">**/Scripts**</span></span> | <span data-ttu-id="fb0b9-160">Эта папка содержит файлы нашей JavaScript</span><span class="sxs-lookup"><span data-stu-id="fb0b9-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="fb0b9-161">Поскольку платформа ASP.NET MVC по умолчанию используется подход «соглашение по конфигурации» и делает некоторые предположения по умолчанию, исходя из соглашений об именовании папки этих папок включаются даже в пустой MVC-приложениях ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="fb0b9-162">Для экземпляра контроллеров поиск представлений в папке «представления» по умолчанию не требуется явно указать это в коде.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="fb0b9-163">Использовать именно по умолчанию соглашения уменьшает объем кода, который требуется написать, и можно также упростить его другими разработчиками понять проекта.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="fb0b9-164">Мы объясним этим соглашениям дополнительные, когда мы создаем нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="fb0b9-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fb0b9-165">Вперед</span><span class="sxs-lookup"><span data-stu-id="fb0b9-165">Next</span></span>](mvc-music-store-part-2.md)
