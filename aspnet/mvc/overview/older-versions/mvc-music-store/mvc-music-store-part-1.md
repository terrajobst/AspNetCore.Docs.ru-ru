---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Часть 1: Обзор и файл -> Создать проект | Документация Майкрософт'
author: jongalloway
description: В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store. Часть 1 рассматриваются общие сведения и файл -> Создать проект.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: d84a525221e40b79853be55069367134d17fb5ec
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829170"
---
<a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="6a19c-104">Часть 1: Обзор и файл -> Создать проект</span><span class="sxs-lookup"><span data-stu-id="6a19c-104">Part 1: Overview and File->New Project</span></span>
====================
<span data-ttu-id="6a19c-105">по [Джон Гэллоуэй](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6a19c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6a19c-106">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые использование ASP.NET MVC и Visual Studio для разработки веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="6a19c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6a19c-107">Store музыки MVC — это реализация хранилища упрощенный пример, который продает музыкальные альбомы через Интернет и реализует базовый сайт администрирования, вход пользователя и корзины для покупок функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="6a19c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6a19c-108">В этой серии руководств описаны все шаги, необходимые для построения примера приложения ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="6a19c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6a19c-109">Часть 1 рассматриваются общие сведения и файл -&gt;новый проект.</span><span class="sxs-lookup"><span data-stu-id="6a19c-109">Part 1 covers Overview and File-&gt;New Project.</span></span>


## <a name="overview"></a><span data-ttu-id="6a19c-110">Обзор</span><span class="sxs-lookup"><span data-stu-id="6a19c-110">Overview</span></span>

<span data-ttu-id="6a19c-111">Store музыки MVC является учебного приложения, которое вводятся и описываются пошаговые способы использования ASP.NET MVC и Visual Web Developer для веб-разработки.</span><span class="sxs-lookup"><span data-stu-id="6a19c-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="6a19c-112">Мы будет запускаться медленно, поэтому уровня веб-разработка для начинающих допустимо.</span><span class="sxs-lookup"><span data-stu-id="6a19c-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="6a19c-113">Приложение, которое мы будем строить является простой музыкального магазина.</span><span class="sxs-lookup"><span data-stu-id="6a19c-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="6a19c-114">Существуют три основные части к приложению: покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="6a19c-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="6a19c-115">Посетители можно ввести альбомов по жанру.</span><span class="sxs-lookup"><span data-stu-id="6a19c-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="6a19c-116">Их можно просматривать одного альбома и добавить его в корзину.</span><span class="sxs-lookup"><span data-stu-id="6a19c-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="6a19c-117">Их можно просмотреть их покупок, удаляя все элементы, которые больше не нужны.</span><span class="sxs-lookup"><span data-stu-id="6a19c-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="6a19c-118">Продолжить извлечение получит запрос на вход в систему или зарегистрироваться для учетной записи пользователя.</span><span class="sxs-lookup"><span data-stu-id="6a19c-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="6a19c-119">После создания учетной записи, они могут выполнить порядок, заполнив сведения оплаты и доставки.</span><span class="sxs-lookup"><span data-stu-id="6a19c-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="6a19c-120">Для простоты мы запускаем удивительные повышения роли: все, что предоставляется бесплатно, если они введите код акции «Бесплатный»!</span><span class="sxs-lookup"><span data-stu-id="6a19c-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="6a19c-121">После упорядочения, они увидят экран подтверждения простой:</span><span class="sxs-lookup"><span data-stu-id="6a19c-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="6a19c-122">Помимо страниц faceing клиента мы также создавать раздел администратора, в которой отображается список альбомов, из которых администраторы могут создавать, Правка и удалить альбомов:</span><span class="sxs-lookup"><span data-stu-id="6a19c-122">In addition to customer-faceing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="6a19c-123">1. Файл —&gt; нового проекта</span><span class="sxs-lookup"><span data-stu-id="6a19c-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="6a19c-124">Установка программного обеспечения</span><span class="sxs-lookup"><span data-stu-id="6a19c-124">Installing the software</span></span>

<span data-ttu-id="6a19c-125">Этот учебник начнется, создав новый проект ASP.NET MVC 3, используя бесплатный Visual Web Developer 2010 Express (которое является бесплатным), а затем мы постепенно предстоит добавить компоненты, чтобы создать работающее приложение.</span><span class="sxs-lookup"><span data-stu-id="6a19c-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="6a19c-126">Попутно мы обсудим доступа к базе данных, сценарии учета формы, проверка данных, использование главных страниц для согласованный макет страницы, с помощью AJAX для обновления страницы и проверки входа пользователя и многое другое.</span><span class="sxs-lookup"><span data-stu-id="6a19c-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="6a19c-127">Пошаговая процедура выполнения этой процедуры, или можно загрузить готовое приложение из [MVC-музыка-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="6a19c-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="6a19c-128">С пакетом обновления 1 для Visual Studio 2010 или Visual Web Developer 2010 Express с пакетом обновления 1 (бесплатная версия Visual Studio 2010) можно использовать для построения приложения.</span><span class="sxs-lookup"><span data-stu-id="6a19c-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="6a19c-129">Мы будем использовать SQL Server Compact (также бесплатно) для размещения базы данных.</span><span class="sxs-lookup"><span data-stu-id="6a19c-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="6a19c-130">Перед началом работы убедитесь, что вы установили необходимые компоненты, перечисленные ниже.</span><span class="sxs-lookup"><span data-stu-id="6a19c-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>


- <span data-ttu-id="6a19c-131">[Предварительные требования для visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="6a19c-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="6a19c-132">[Обновление средств ASP.NET MVC 3]</span><span class="sxs-lookup"><span data-stu-id="6a19c-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="6a19c-133">[SQL Server Compact 4.0] - включая среду выполнения и средства поддержки</span><span class="sxs-lookup"><span data-stu-id="6a19c-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>


### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="6a19c-134">Создание нового проекта ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="6a19c-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="6a19c-135">Мы начнем, выбрав «New Project» в меню "файл" в Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="6a19c-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="6a19c-136">Откроется диалоговое окно нового проекта.</span><span class="sxs-lookup"><span data-stu-id="6a19c-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="6a19c-137">Мы выберем Visual C# -&gt; веб-шаблонов группе в левой части, а затем выберите шаблон «ASP.NET MVC 3 веб-приложение» в центральном столбце.</span><span class="sxs-lookup"><span data-stu-id="6a19c-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="6a19c-138">Имя проекта MvcMusicStore и нажмите кнопку OK.</span><span class="sxs-lookup"><span data-stu-id="6a19c-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="6a19c-139">При этом отобразится вторичной диалогового окна, что позволяет нам сделать некоторых индивидуальных параметров MVC для нашего проекта.</span><span class="sxs-lookup"><span data-stu-id="6a19c-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="6a19c-140">Выберите следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="6a19c-140">Select the following:</span></span>

<span data-ttu-id="6a19c-141">Шаблон проекта — Выбор пуст</span><span class="sxs-lookup"><span data-stu-id="6a19c-141">Project Template - select Empty</span></span>

<span data-ttu-id="6a19c-142">Просмотр ядра - выберите Razor</span><span class="sxs-lookup"><span data-stu-id="6a19c-142">View Engine - select Razor</span></span>

<span data-ttu-id="6a19c-143">Использовать семантическую разметку HTML5 - проверяется</span><span class="sxs-lookup"><span data-stu-id="6a19c-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="6a19c-144">Убедитесь, что параметры, как показано ниже, а затем нажмите кнопку OK.</span><span class="sxs-lookup"><span data-stu-id="6a19c-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="6a19c-145">Наш проект будет создан.</span><span class="sxs-lookup"><span data-stu-id="6a19c-145">This will create our project.</span></span> <span data-ttu-id="6a19c-146">Давайте рассмотрим папки, которые были добавлены к нашему приложению в обозревателе решений правой стороны.</span><span class="sxs-lookup"><span data-stu-id="6a19c-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="6a19c-147">Пустой шаблон MVC 3 не полностью пустой — она добавляет структуру к базовой папки:</span><span class="sxs-lookup"><span data-stu-id="6a19c-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="6a19c-148">ASP.NET MVC использует некоторые основные стандарты именования в именах папок:</span><span class="sxs-lookup"><span data-stu-id="6a19c-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="6a19c-149">**Папка**</span><span class="sxs-lookup"><span data-stu-id="6a19c-149">**Folder**</span></span> | <span data-ttu-id="6a19c-150">**Назначение**</span><span class="sxs-lookup"><span data-stu-id="6a19c-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="6a19c-151">**/ Контроллеров**</span><span class="sxs-lookup"><span data-stu-id="6a19c-151">**/Controllers**</span></span> | <span data-ttu-id="6a19c-152">Контроллеры реагировать на ввод из браузера, решить, что делать дальше и возврат ответа для пользователя.</span><span class="sxs-lookup"><span data-stu-id="6a19c-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="6a19c-153">**И представлений**</span><span class="sxs-lookup"><span data-stu-id="6a19c-153">**/Views**</span></span> | <span data-ttu-id="6a19c-154">Представления хранения наши шаблоны пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="6a19c-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="6a19c-155">**/ Моделей**</span><span class="sxs-lookup"><span data-stu-id="6a19c-155">**/Models**</span></span> | <span data-ttu-id="6a19c-156">Модели хранения и обработки данных</span><span class="sxs-lookup"><span data-stu-id="6a19c-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="6a19c-157">**/ Content**</span><span class="sxs-lookup"><span data-stu-id="6a19c-157">**/Content**</span></span> | <span data-ttu-id="6a19c-158">Эта папка содержит наши образы, CSS и статическое содержимое</span><span class="sxs-lookup"><span data-stu-id="6a19c-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="6a19c-159">**/ Scripts**</span><span class="sxs-lookup"><span data-stu-id="6a19c-159">**/Scripts**</span></span> | <span data-ttu-id="6a19c-160">Эта папка содержит наш файлы JavaScript</span><span class="sxs-lookup"><span data-stu-id="6a19c-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="6a19c-161">Эти папки включаются даже в приложении ASP.NET MVC, пустой, так как платформа ASP.NET MVC по умолчанию использует подход «соглашение отностительно конфигурации» и ряд предположений по умолчанию, в зависимости от соглашения об именовании папки.</span><span class="sxs-lookup"><span data-stu-id="6a19c-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="6a19c-162">Например контроллеров представлений в папке Views по умолчанию ищут вам не нужно явно указать это в коде.</span><span class="sxs-lookup"><span data-stu-id="6a19c-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="6a19c-163">Придерживаясь соглашения по умолчанию уменьшает объем кода, вам нужно написать, и можно также упростить его для других разработчиков понять проекта.</span><span class="sxs-lookup"><span data-stu-id="6a19c-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="6a19c-164">Мы объясним эти соглашения, дополнительные, когда мы создаем нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="6a19c-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6a19c-165">Вперед</span><span class="sxs-lookup"><span data-stu-id="6a19c-165">Next</span></span>](mvc-music-store-part-2.md)
