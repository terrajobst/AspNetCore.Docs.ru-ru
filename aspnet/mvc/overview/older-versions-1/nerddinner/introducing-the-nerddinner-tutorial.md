---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Введение в учебник обновление NerdDinner | Документы Microsoft
author: shanselman
description: Лучший способ изучения новой платформы является построения каких-то с ним. В этом учебнике представлены пошаговые инструкции для создания приложения с небольшой, но завершенный, с помощью ASP.NE...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868559"
---
<a name="introducing-the-nerddinner-tutorial"></a><span data-ttu-id="782ca-104">Введение в учебник обновление NerdDinner</span><span class="sxs-lookup"><span data-stu-id="782ca-104">Introducing the NerdDinner Tutorial</span></span>
====================
<span data-ttu-id="782ca-105">по [Скотт Хансельман](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="782ca-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

[<span data-ttu-id="782ca-106">Скачать PDF</span><span class="sxs-lookup"><span data-stu-id="782ca-106">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="782ca-107">Лучший способ изучения новой платформы является построения каких-то с ним.</span><span class="sxs-lookup"><span data-stu-id="782ca-107">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="782ca-108">Этот учебник рассматриваются способы построения небольшой, но завершается приложения с помощью ASP.NET MVC 1 и рассматриваются некоторые основные понятия, стоящий за ней.</span><span class="sxs-lookup"><span data-stu-id="782ca-108">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC 1, and introduces some of the core concepts behind it.</span></span>
> 
> <span data-ttu-id="782ca-109">При использовании ASP.NET MVC 3, которому рекомендуется следовать [Приступая к работе с MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) или [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) учебники.</span><span class="sxs-lookup"><span data-stu-id="782ca-109">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-tutorial"></a><span data-ttu-id="782ca-110">Обновление NerdDinner учебника</span><span class="sxs-lookup"><span data-stu-id="782ca-110">NerdDinner Tutorial</span></span>

<span data-ttu-id="782ca-111">Лучший способ изучения новой платформы является построения каких-то с ним.</span><span class="sxs-lookup"><span data-stu-id="782ca-111">The best way to learn a new framework is to build something with it.</span></span> <span data-ttu-id="782ca-112">Рассматриваются способы построения небольшой, но завершается приложения ASP.NET MVC с помощью этого учебника и рассматриваются некоторые основные понятия, стоящий за ней.</span><span class="sxs-lookup"><span data-stu-id="782ca-112">This tutorial walks through how to build a small, but complete, application using ASP.NET MVC, and introduces some of the core concepts behind it.</span></span>

<span data-ttu-id="782ca-113">Приложение, которое мы собираемся строить называется «Обновление NerdDinner».</span><span class="sxs-lookup"><span data-stu-id="782ca-113">The application we are going to build is called "NerdDinner".</span></span> <span data-ttu-id="782ca-114">Обновление NerdDinner позволяет легко найти и организовать ужинов сети для пользователей:</span><span class="sxs-lookup"><span data-stu-id="782ca-114">NerdDinner provides an easy way for people to find and organize dinners online:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image1.png)

<span data-ttu-id="782ca-115">Обновление NerdDinner зарегистрированные пользователи могут создавать, изменять и удалять ужинов.</span><span class="sxs-lookup"><span data-stu-id="782ca-115">NerdDinner enables registered users to create, edit and delete dinners.</span></span> <span data-ttu-id="782ca-116">Тем самым согласованного набора проверки и бизнес-правила во всем приложении:</span><span class="sxs-lookup"><span data-stu-id="782ca-116">It enforces a consistent set of validation and business rules across the application:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image2.png)

<span data-ttu-id="782ca-117">Посетители могут использовать карты на основе AJAX для поиска предстоящих ужинов удержания рядом с ними:</span><span class="sxs-lookup"><span data-stu-id="782ca-117">Visitors can use an AJAX-based map to search for upcoming dinners being held near them:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image3.png)

<span data-ttu-id="782ca-118">Обед щелкнув ведет на страницу сведений где можно узнать дополнительные сведения об этом:</span><span class="sxs-lookup"><span data-stu-id="782ca-118">Clicking a dinner will take them to a details page where they can learn more about it:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image4.png)

<span data-ttu-id="782ca-119">Если они посещение dinner они смогут войти в систему или зарегистрировать на сайте:</span><span class="sxs-lookup"><span data-stu-id="782ca-119">If they are interested in attending the dinner they can login or register on the site:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image5.png)

<span data-ttu-id="782ca-120">Затем необходимо щелкнуть ссылку на основе AJAX RSVP должны посетить события:</span><span class="sxs-lookup"><span data-stu-id="782ca-120">They can then click an AJAX-based RSVP link to attend the event:</span></span>

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a><span data-ttu-id="782ca-121">Реализация обновление NerdDinner</span><span class="sxs-lookup"><span data-stu-id="782ca-121">Implementing NerdDinner</span></span>

<span data-ttu-id="782ca-122">Мы хотим начать обновление NerdDinner приложения с помощью файла -&gt;команды новый проект в Visual Studio для создания нового проекта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="782ca-122">We are going to begin our NerdDinner application by using the File-&gt;New Project command within Visual Studio to create a brand new ASP.NET MVC project.</span></span> <span data-ttu-id="782ca-123">Затем последовательно добавим функциональность и возможности.</span><span class="sxs-lookup"><span data-stu-id="782ca-123">We will then incrementally add functionality and features.</span></span> <span data-ttu-id="782ca-124">Попутно мы расскажем:</span><span class="sxs-lookup"><span data-stu-id="782ca-124">Along the way we'll cover:</span></span>

1. [<span data-ttu-id="782ca-125">Создание нового проекта ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="782ca-125">How to create a new ASP.NET MVC Project</span></span>](# "создайте новый проект ASP.NET MVC")
2. [<span data-ttu-id="782ca-126">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="782ca-126">How to create a database</span></span>](# "Создание базы данных")
3. [<span data-ttu-id="782ca-127">Как создать модель с бизнес-правило проверок</span><span class="sxs-lookup"><span data-stu-id="782ca-127">How to build a model with business rule validations</span></span>](# "построить модель с бизнес-правило проверок")
4. [<span data-ttu-id="782ca-128">Как использовать контроллеры и представления для реализации и сведения о вхождении пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="782ca-128">How to use controllers and views to implement a listing/details UI</span></span>](# "использование контроллеров и представлений для реализации пользовательского интерфейса сведения о вхождении")
5. <span data-ttu-id="782ca-129">[Как обеспечить CRUD (Создание, чтение, обновление и удаление) данных образуют запись поддержки](# "предоставляют CRUD (Создание, чтение, обновление, удаление) запись данных формы поддерживает")</span><span class="sxs-lookup"><span data-stu-id="782ca-129">[How to provide CRUD (create, read, update, delete) data form entry support](# "Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support")</span></span>
6. [<span data-ttu-id="782ca-130">Способы использования ViewData и реализовать такие классы ViewModel</span><span class="sxs-lookup"><span data-stu-id="782ca-130">How to use ViewData and implement ViewModel classes</span></span>](# "использовать ViewData и реализовать классы ViewModel")
7. [<span data-ttu-id="782ca-131">Как повторно использовать пользовательский Интерфейс, с помощью главных страниц и частичными репликами</span><span class="sxs-lookup"><span data-stu-id="782ca-131">How to re-use UI using master pages and partials</span></span>](# "повторного использования пользовательского интерфейса с помощью главных страниц и частичными репликами")
8. [<span data-ttu-id="782ca-132">Как реализовать разбиение по страницам данных эффективный</span><span class="sxs-lookup"><span data-stu-id="782ca-132">How to implement efficient data paging</span></span>](# "реализации эффективного данных разбиения на страницы")
9. [<span data-ttu-id="782ca-133">Защита приложений с использованием проверки подлинности и авторизации</span><span class="sxs-lookup"><span data-stu-id="782ca-133">How to secure applications using authentication and authorization</span></span>](# "безопасных приложений с использованием проверки подлинности и авторизации")
10. [<span data-ttu-id="782ca-134">Использование AJAX для динамического обновления</span><span class="sxs-lookup"><span data-stu-id="782ca-134">How to use AJAX to deliver dynamic updates</span></span>](# "использовать AJAX для динамического обновления")
11. [<span data-ttu-id="782ca-135">Использование AJAX для реализации сценариев сопоставления</span><span class="sxs-lookup"><span data-stu-id="782ca-135">How to use AJAX to implement mapping scenarios</span></span>](# "использовать AJAX для реализации сценариев сопоставления")
12. [<span data-ttu-id="782ca-136">Как включить автоматическое тестирование модулей</span><span class="sxs-lookup"><span data-stu-id="782ca-136">How to enable automated unit testing</span></span>](# "включить автоматические модульное тестирование")

<span data-ttu-id="782ca-137">Можно создать собственную копию обновление NerdDinner с нуля, выполнив каждый шаг мы Пошаговое руководство в этой главе.</span><span class="sxs-lookup"><span data-stu-id="782ca-137">You can build your own copy of NerdDinner from scratch by completing each step we walkthrough in this chapter.</span></span> <span data-ttu-id="782ca-138">Кроме того, можно загрузить полную версию исходного кода здесь: [обновление NerdDinner на GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span><span class="sxs-lookup"><span data-stu-id="782ca-138">Alternatively, you can download a completed version of the source code here: [NerdDinner on GitHub](https://github.com/AspNetMVPSamples/NerdDinner).</span></span> <span data-ttu-id="782ca-139">Можно также Дополнительно [загрузить бесплатную версию PDF этого учебника](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) Если вы хотите прочитать учебник в автономном режиме.</span><span class="sxs-lookup"><span data-stu-id="782ca-139">You can also optionally also [download a free PDF version of this tutorial](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) if you want to read the tutorial offline.</span></span>

<span data-ttu-id="782ca-140">Visual Studio 2008 или произвольным Visual Web Developer 2008 Express можно использовать для построения приложения.</span><span class="sxs-lookup"><span data-stu-id="782ca-140">You can use either Visual Studio 2008 or the free Visual Web Developer 2008 Express to build the application.</span></span> <span data-ttu-id="782ca-141">Для базы данных можно использовать SQL Server или произвольным SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="782ca-141">You can use either SQL Server or the free SQL Server Express for the database.</span></span>

<span data-ttu-id="782ca-142">Можно установить ASP.NET MVC, Visual Web Developer 2008 Express и SQL Server Express (бесплатно) с помощью V2 из [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span><span class="sxs-lookup"><span data-stu-id="782ca-142">You can install ASP.NET MVC, Visual Web Developer 2008 Express, and SQL Server Express (all free) using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)</span></span>

### <a name="now-lets-get-started"></a><span data-ttu-id="782ca-143">Теперь давайте начнем...</span><span class="sxs-lookup"><span data-stu-id="782ca-143">Now let's get started....</span></span>

<span data-ttu-id="782ca-144">Теперь, когда мы рассмотрели, что такое обновление NerdDinner, давайте нашей рукава и написать код.</span><span class="sxs-lookup"><span data-stu-id="782ca-144">Now that we've covered what NerdDinner is, let's roll up our sleeves and write some code.</span></span>

<span data-ttu-id="782ca-145">Начнем с помощью файла -&gt;новый проект в Visual Studio для создания приложения обновление NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="782ca-145">We'll begin by using File-&gt;New Project within Visual Studio to create the NerdDinner application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="782ca-146">Вперед</span><span class="sxs-lookup"><span data-stu-id="782ca-146">Next</span></span>](create-a-new-aspnet-mvc-project.md)
