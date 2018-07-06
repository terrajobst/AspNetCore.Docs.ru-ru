---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Введение в учебнике по NerdDinner | Документация Майкрософт
author: shanselman
description: — Это наилучший способ подробнее узнать это новая платформа для создания чего-то с ним. В этом учебнике описывается создание небольшой, но законченного приложения с помощью ASP.NE...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 9188df1eca7f488502640bc17d5034f93f4ac82c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805114"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="b4a7c-104">Введение в учебнике по NerdDinner</span><span class="sxs-lookup"><span data-stu-id="b4a7c-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="b4a7c-105">по [(Scott hanselman)](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="b4a7c-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="b4a7c-106">Загрузить PDF-файл</span><span class="sxs-lookup"><span data-stu-id="b4a7c-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="b4a7c-107">— Это наилучший способ подробнее узнать это новая платформа для создания чего-то с ним.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="b4a7c-108">Этом руководстве описано, как создать небольшой, но завершить приложение с помощью ASP.NET MVC 1 и рассматриваются некоторые основные принципы его.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="b4a7c-109">Если вы используете ASP.NET MVC 3, рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="b4a7c-110">Учебнике по NerdDinner</span><span class="sxs-lookup"><span data-stu-id="b4a7c-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="b4a7c-111">— Это наилучший способ подробнее узнать это новая платформа для создания чего-то с ним.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="b4a7c-112">Этом руководстве описано, как создать небольшой, но завершить приложение с помощью ASP.NET MVC и рассматриваются некоторые основные принципы его.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="b4a7c-113">Приложение, которое мы собираемся строить называется «NerdDinner».</span><span class="sxs-lookup"><span data-stu-id="b4a7c-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="b4a7c-114">NerdDinner предоставляет простой способ для пользователей, для поиска и упорядочения ужинов через Интернет:</span><span class="sxs-lookup"><span data-stu-id="b4a7c-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="b4a7c-115">Зарегистрированные пользователи могут создавать, изменять и удалять ужинов NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="b4a7c-116">Он содержит согласованный набор проверки и бизнес-правила для приложения:</span><span class="sxs-lookup"><span data-stu-id="b4a7c-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="b4a7c-117">Посетители можно использовать карты на основе AJAX, поиск предстоящих ужинов, удерживаемая рядом с ними:</span><span class="sxs-lookup"><span data-stu-id="b4a7c-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="b4a7c-118">Компании dinner ведет на страницу подробностей, где они могут узнать больше об этом:</span><span class="sxs-lookup"><span data-stu-id="b4a7c-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="b4a7c-119">Если они интересуют посещение обед, они могут войти в систему или зарегистрируйтесь на сайте:</span><span class="sxs-lookup"><span data-stu-id="b4a7c-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="b4a7c-120">Они затем можно щелкнуть ссылку RSVP на основе AJAX, чтобы принять участие в конференции:</span><span class="sxs-lookup"><span data-stu-id="b4a7c-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="b4a7c-121">Реализация NerdDinner</span><span class="sxs-lookup"><span data-stu-id="b4a7c-121">Implementing NerdDinner</span></span>

<span data-ttu-id="b4a7c-122">Мы хотим начать наше приложение NerdDinner с помощью файла -&gt;команду Новый проект в Visual Studio, чтобы создать новый проект ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="b4a7c-123">Мы будем затем постепенно добавлять функциональность и возможности.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="b4a7c-124">Попутно мы обсудим:</span><span class="sxs-lookup"><span data-stu-id="b4a7c-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="b4a7c-125">Как создать новый проект ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b4a7c-125">How to create a new ASP.NET MVC Project</span></span>](# "создайте новый проект ASP.NET MVC")
2. [<span data-ttu-id="b4a7c-126">Как создать базу данных</span><span class="sxs-lookup"><span data-stu-id="b4a7c-126">How to create a database</span></span>](# "Создание базы данных")
3. [<span data-ttu-id="b4a7c-127">Как создать модель с проверками бизнес-правил</span><span class="sxs-lookup"><span data-stu-id="b4a7c-127">How to build a model with business rule validations</span></span>](# "Создание модели с проверками бизнес-правил")
4. [<span data-ttu-id="b4a7c-128">Практическое использование контроллеров и представлений для реализации списка и сведений пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="b4a7c-128">How to use controllers and views to implement a listing/details UI</span></span>](# "использование контроллеров и представлений для реализации пользовательского интерфейса списка и сведений")
5. <span data-ttu-id="b4a7c-129">[Как обеспечить CRUD (Создание, чтение, обновление и удаление) данных образуют запись поддержки](# "предоставляют CRUD (Create, Read, Update, Delete) запись данных формы поддерживает")</span><span class="sxs-lookup"><span data-stu-id="b4a7c-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="b4a7c-130">Использование ViewData и реализация классов ViewModel</span><span class="sxs-lookup"><span data-stu-id="b4a7c-130">How to use ViewData and implement ViewModel classes</span></span>](# "использование ViewData и реализация классов ViewModel")
7. [<span data-ttu-id="b4a7c-131">Как повторно использовать пользовательский Интерфейс, с помощью главных страниц и частичных представлений</span><span class="sxs-lookup"><span data-stu-id="b4a7c-131">How to re-use UI using master pages and partials</span></span>](# "повторного использования пользовательского интерфейса с помощью главных страниц и частичных представлений")
8. [<span data-ttu-id="b4a7c-132">Как реализовать разбиение по страницам данных эффективно</span><span class="sxs-lookup"><span data-stu-id="b4a7c-132">How to implement efficient data paging</span></span>](# "реализовать эффективный данных разбиение по страницам")
9. [<span data-ttu-id="b4a7c-133">Защита приложений с помощью проверки подлинности и авторизации</span><span class="sxs-lookup"><span data-stu-id="b4a7c-133">How to secure applications using authentication and authorization</span></span>](# "безопасных приложений с использованием проверки подлинности и авторизации")
10. [<span data-ttu-id="b4a7c-134">Использование AJAX для доставки динамических обновлений</span><span class="sxs-lookup"><span data-stu-id="b4a7c-134">How to use AJAX to deliver dynamic updates</span></span>](# "использовать AJAX для доставки динамических обновлений")
11. [<span data-ttu-id="b4a7c-135">Использование AJAX для реализации сценариев сопоставления</span><span class="sxs-lookup"><span data-stu-id="b4a7c-135">How to use AJAX to implement mapping scenarios</span></span>](# "использовать AJAX для реализации сценариев сопоставления")
12. [<span data-ttu-id="b4a7c-136">Как включить автоматическое тестирование модулей</span><span class="sxs-lookup"><span data-stu-id="b4a7c-136">How to enable automated unit testing</span></span>](# "Включение автоматического модульного тестирования")

<span data-ttu-id="b4a7c-137">Можно создать собственную копию NerdDinner с нуля, выполнив каждый шаг мы пошагового руководства в этой главе.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="b4a7c-138">Кроме того, можно скачать готовую версию исходного кода, здесь: [NerdDinner на сайте GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="b4a7c-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="b4a7c-139">Можно также при необходимости также [Загрузите бесплатную версию PDF данного учебника](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Если вы хотите прочитать руководство в автономном режиме.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="b4a7c-140">Visual Studio 2008 или бесплатный Visual Web Developer 2008 Express можно использовать для построения приложения.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="b4a7c-141">Для базы данных можно использовать SQL Server или бесплатная SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="b4a7c-142">Вы можете установить ASP.NET MVC, Visual Web Developer 2008 Express и SQL Server Express (бесплатно) с помощью версии 2 [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="b4a7c-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="b4a7c-143">Теперь приступим к работе...</span><span class="sxs-lookup"><span data-stu-id="b4a7c-143">Now let's get started....</span></span>

<span data-ttu-id="b4a7c-144">Теперь, когда мы рассмотрели, что такое NerdDinner, давайте наших рукава и написать код.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="b4a7c-145">Начнем с помощью файла -&gt;новый проект в Visual Studio, чтобы создать приложение NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="b4a7c-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b4a7c-146">Вперед</span><span class="sxs-lookup"><span data-stu-id="b4a7c-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
