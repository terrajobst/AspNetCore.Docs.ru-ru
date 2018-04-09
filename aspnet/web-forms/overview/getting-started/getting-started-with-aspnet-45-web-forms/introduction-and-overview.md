---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Приступая к работе с 4,5 веб-форм ASP.NET и Visual Studio 2013 | Документы Microsoft
author: Erikre
description: Этот пошаговые руководства ряд описываются основы построения приложения веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio выражать...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 572b263a5f968b473457771a1dd4075910218c01
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="507c5-103">Приступая к работе с 4,5 веб-форм ASP.NET и Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="507c5-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="507c5-104">по [Эрик Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="507c5-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="507c5-105">[Загрузите образец проекта Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) или [загрузить электронную (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="507c5-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="507c5-106">Этот пошаговые руководства ряд описываются основы построения с помощью ASP.NET 4.5 и Microsoft Visual Studio Express 2013 для веб-приложения веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="507c5-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="507c5-107">Веб-форм ASP.NET головоломки</span><span class="sxs-lookup"><span data-stu-id="507c5-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="507c5-108">Проверьте свои знания и дополнять основные понятия, используя ASP.NET Web Forms тест.</span><span class="sxs-lookup"><span data-stu-id="507c5-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="507c5-109">Этот тест был специально из содержимого, содержащихся в данной серии учебных курсов.</span><span class="sxs-lookup"><span data-stu-id="507c5-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="507c5-110">Каждый вопрос в головоломку объясняется, а также ссылки на дополнительные руководства.</span><span class="sxs-lookup"><span data-stu-id="507c5-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="507c5-111">Вступление</span><span class="sxs-lookup"><span data-stu-id="507c5-111">Introduction</span></span>

<span data-ttu-id="507c5-112">Ряд учебники поможет выполнить шаги, необходимые для создания приложения веб-форм ASP.NET с помощью Visual Studio Express 2013 для Web и ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="507c5-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="507c5-113">Вы создадите приложение называется **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="507c5-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="507c5-114">Это упрощенный пример веб-сайт магазина, продающей элементов в Интернете.</span><span class="sxs-lookup"><span data-stu-id="507c5-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="507c5-115">Этот учебник ряд выделяет новых возможностей, доступных в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="507c5-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="507c5-116">Приглашаем вас комментарии и сделаем все усилия, чтобы обновить этот учебник ряд основании ваши предложения.</span><span class="sxs-lookup"><span data-stu-id="507c5-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="507c5-117">Процент загрузки проекта</span><span class="sxs-lookup"><span data-stu-id="507c5-117">Download completed project</span></span>

<span data-ttu-id="507c5-118">Вы можете загрузить проект C#, содержащий завершенной работы с учебником.</span><span class="sxs-lookup"><span data-stu-id="507c5-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="507c5-119">[Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="507c5-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="507c5-120">Просмотрите содержимое, используя связанный тест веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="507c5-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="507c5-121">После завершения этого учебника, проверьте свои знания и дополнять основные понятия, выполнив [ASP.NET Web Forms головоломка](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="507c5-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="507c5-122">Этот тест был специально из содержимого, содержащихся в данной серии учебных курсов.</span><span class="sxs-lookup"><span data-stu-id="507c5-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="507c5-123">Каждый вопрос в головоломку объясняется, а также ссылки на дополнительные руководства.</span><span class="sxs-lookup"><span data-stu-id="507c5-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="507c5-124">Веб-форм ASP.NET головоломки</span><span class="sxs-lookup"><span data-stu-id="507c5-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="507c5-125">Аудитория</span><span class="sxs-lookup"><span data-stu-id="507c5-125">Audience</span></span>

<span data-ttu-id="507c5-126">Целевая аудитория этой серии учебника является опытных разработчиков, незнакомых с веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="507c5-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="507c5-127">Разработчиков, заинтересованных в данной серии учебных курсов должны иметь следующие навыки:</span><span class="sxs-lookup"><span data-stu-id="507c5-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="507c5-128">Объект, которым известна ориентированный язык программирования (OOP)</span><span class="sxs-lookup"><span data-stu-id="507c5-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="507c5-129">Получив концепции веб-разработки (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="507c5-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="507c5-130">Знакомы с понятиями реляционной базы данных</span><span class="sxs-lookup"><span data-stu-id="507c5-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="507c5-131">Знакомы с основными понятиями n уровневая архитектура</span><span class="sxs-lookup"><span data-stu-id="507c5-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="507c5-132">Если вы заинтересованы в просмотре областях, перечисленных выше, просмотрите следующие разделы:</span><span class="sxs-lookup"><span data-stu-id="507c5-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="507c5-133">Приступая к работе с Visual C#</span><span class="sxs-lookup"><span data-stu-id="507c5-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="507c5-134">[Веб-разработки](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="507c5-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="507c5-135">Реляционная база данных</span><span class="sxs-lookup"><span data-stu-id="507c5-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="507c5-136">Многоуровневая архитектура</span><span class="sxs-lookup"><span data-stu-id="507c5-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="507c5-137">Компоненты приложений</span><span class="sxs-lookup"><span data-stu-id="507c5-137">Application Features</span></span>

<span data-ttu-id="507c5-138">Возможности веб-форма ASP.NET, представленных в этой серии:</span><span class="sxs-lookup"><span data-stu-id="507c5-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="507c5-139">Проект веб-приложения (не проект веб-сайта)</span><span class="sxs-lookup"><span data-stu-id="507c5-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="507c5-140">Веб-формы</span><span class="sxs-lookup"><span data-stu-id="507c5-140">Web Forms</span></span>
- <span data-ttu-id="507c5-141">Главные страницы, конфигурации</span><span class="sxs-lookup"><span data-stu-id="507c5-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="507c5-142">начальной загрузки</span><span class="sxs-lookup"><span data-stu-id="507c5-142">Bootstrap</span></span>
- <span data-ttu-id="507c5-143">Платформа Entity Framework Code во-первых, LocalDB</span><span class="sxs-lookup"><span data-stu-id="507c5-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="507c5-144">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="507c5-144">Request Validation</span></span>
- <span data-ttu-id="507c5-145">Строго типизированные элементы управления данными, привязки данных заметок, модели и значение поставщиков</span><span class="sxs-lookup"><span data-stu-id="507c5-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="507c5-146">SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="507c5-146">SSL and OAuth</span></span>
- <span data-ttu-id="507c5-147">ASP.NET Identity, настройки и авторизации</span><span class="sxs-lookup"><span data-stu-id="507c5-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="507c5-148">Ненавязчивой проверки</span><span class="sxs-lookup"><span data-stu-id="507c5-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="507c5-149">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="507c5-149">Routing</span></span>
- <span data-ttu-id="507c5-150">Обработка ошибок ASP.NET</span><span class="sxs-lookup"><span data-stu-id="507c5-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="507c5-151">Приложения сценариев и задач</span><span class="sxs-lookup"><span data-stu-id="507c5-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="507c5-152">Следующие задачи, показанные в этой серии.</span><span class="sxs-lookup"><span data-stu-id="507c5-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="507c5-153">Создание, просмотр и запуск нового проекта</span><span class="sxs-lookup"><span data-stu-id="507c5-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="507c5-154">Создание структуры базы данных</span><span class="sxs-lookup"><span data-stu-id="507c5-154">Creating the database structure</span></span>
- <span data-ttu-id="507c5-155">Инициализация и заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="507c5-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="507c5-156">Настройка пользовательского интерфейса, используя стили, графики и главную страницу</span><span class="sxs-lookup"><span data-stu-id="507c5-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="507c5-157">Добавление страниц и навигации</span><span class="sxs-lookup"><span data-stu-id="507c5-157">Adding pages and navigation</span></span>
- <span data-ttu-id="507c5-158">Отображение меню сведения и данные о продуктах</span><span class="sxs-lookup"><span data-stu-id="507c5-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="507c5-159">Создание корзины</span><span class="sxs-lookup"><span data-stu-id="507c5-159">Creating a shopping cart</span></span>
- <span data-ttu-id="507c5-160">Поддержка добавления SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="507c5-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="507c5-161">Добавление метода оплаты</span><span class="sxs-lookup"><span data-stu-id="507c5-161">Adding a payment method</span></span>
- <span data-ttu-id="507c5-162">Включая роль администратора и пользователя для приложения</span><span class="sxs-lookup"><span data-stu-id="507c5-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="507c5-163">Ограничение доступа к определенным страницам и папки</span><span class="sxs-lookup"><span data-stu-id="507c5-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="507c5-164">Отправка файла на веб-приложения</span><span class="sxs-lookup"><span data-stu-id="507c5-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="507c5-165">Реализация проверки входных данных</span><span class="sxs-lookup"><span data-stu-id="507c5-165">Implementing input validation</span></span>
- <span data-ttu-id="507c5-166">Регистрация маршрутов для веб-приложения</span><span class="sxs-lookup"><span data-stu-id="507c5-166">Registering routes for the web application</span></span>
- <span data-ttu-id="507c5-167">Реализация обработки ошибок и ведения журнала ошибок</span><span class="sxs-lookup"><span data-stu-id="507c5-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="507c5-168">Обзор</span><span class="sxs-lookup"><span data-stu-id="507c5-168">Overview</span></span>

<span data-ttu-id="507c5-169">Если вы не знакомы с веб-форм ASP.NET но имеете представление о понятиях программирования, у вас есть правой учебника.</span><span class="sxs-lookup"><span data-stu-id="507c5-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="507c5-170">Если вы уже знакомы с веб-форм ASP.NET, вы может выиграть от этого учебника ряда, о новых возможностях ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="507c5-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="507c5-171">Если вы не знакомы с программирования основные понятия и веб-форм ASP.NET, см. другие учебники, в веб-форм [Приступая к работе](../../../index.md) раздел на веб-сайте ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="507c5-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="507c5-172">Конкретный **последние** функций ASP.NET 4.5 в этом веб-форм учебника рядов, относятся следующие:</span><span class="sxs-lookup"><span data-stu-id="507c5-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="507c5-173">Простой пользовательский Интерфейс для создания проектов, предложение [поддержка нескольких платформ ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (веб-форм, MVC и веб-API).</span><span class="sxs-lookup"><span data-stu-id="507c5-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="507c5-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), макет и темы платформа, которая предоставляет гибкий возможности конструктора и темы.</span><span class="sxs-lookup"><span data-stu-id="507c5-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="507c5-175">[ASP.NET Identity](../../../../identity/index.md), новой системы членства ASP.NET, которая работает одинаково во всех платформ ASP.NET и работает с веб-размещения программного обеспечения, отличные от IIS.</span><span class="sxs-lookup"><span data-stu-id="507c5-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="507c5-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), обновление на платформу Entity Framework, которая позволяет получить и управления данными, как строго типизированные объекты, доступ к данным асинхронно, обрабатывать временные обрывы соединения и входа инструкции SQL.</span><span class="sxs-lookup"><span data-stu-id="507c5-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="507c5-177">Полный список функций ASP.NET 4.5 см. в разделе [ASP.NET и веб-инструменты Visual Studio 2013 заметки о выпуске](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="507c5-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="507c5-178">Пример приложения Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="507c5-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="507c5-179">На приведенных ниже снимках экрана обеспечивают быстрый просмотр приложения ASP.NET Web forms, будет создан в данной серии учебных курсов.</span><span class="sxs-lookup"><span data-stu-id="507c5-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="507c5-180">При запуске приложения из Visual Studio Express 2013 для Web появится следующей веб-странице Home.</span><span class="sxs-lookup"><span data-stu-id="507c5-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Wingtip Toys - страница по умолчанию](introduction-and-overview/_static/image1.png)

<span data-ttu-id="507c5-182">Регистрация нового пользователя или войдите в систему как существующего пользователя.</span><span class="sxs-lookup"><span data-stu-id="507c5-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="507c5-183">Путем получения доступных продуктов из базы данных предоставляется навигации в верхней для каждой категории продукта.</span><span class="sxs-lookup"><span data-stu-id="507c5-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="507c5-184">Выбрав ссылку продуктов, можно просмотреть список всех доступных продуктов.</span><span class="sxs-lookup"><span data-stu-id="507c5-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Wingtip Toys - продуктов](introduction-and-overview/_static/image2.png)

<span data-ttu-id="507c5-186">Также вы увидите сведения об отдельных продуктов, выбрав любой из перечисленных продуктов.</span><span class="sxs-lookup"><span data-stu-id="507c5-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Wingtip Toys - сведения о продукте](introduction-and-overview/_static/image3.png)

<span data-ttu-id="507c5-188">Как пользователь можно регистрировать и войдите, используя функциональные возможности по умолчанию шаблона веб-форм.</span><span class="sxs-lookup"><span data-stu-id="507c5-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="507c5-189">Этот учебник также объясняется, как выполнить вход с использованием существующую учетную запись Gmail.</span><span class="sxs-lookup"><span data-stu-id="507c5-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="507c5-190">Кроме того можно выполнить вход от имени администратора, для добавления и удаления продуктов из базы данных.</span><span class="sxs-lookup"><span data-stu-id="507c5-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Wingtip Toys - вход](introduction-and-overview/_static/image4.png)

<span data-ttu-id="507c5-192">После входа качестве пользователя, можно добавить продукты в корзину и извлечения с PayPal.</span><span class="sxs-lookup"><span data-stu-id="507c5-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="507c5-193">Обратите внимание, что в этом образце приложения предназначен для работы с песочницей разработчика в PayPal.</span><span class="sxs-lookup"><span data-stu-id="507c5-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="507c5-194">Транзакция отсутствует фактический money будет распространяться.</span><span class="sxs-lookup"><span data-stu-id="507c5-194">No actual money transaction will take place.</span></span>

![Wingtip Toys - Корзина для покупок](introduction-and-overview/_static/image5.png)

<span data-ttu-id="507c5-196">PayPal подтвердит вашей учетной записи, порядок и сведения об оплате.</span><span class="sxs-lookup"><span data-stu-id="507c5-196">PayPal will confirm your account, order, and payment information.</span></span>

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="507c5-198">После возвращения из PayPal, можно просмотреть и завершения оформления заказа.</span><span class="sxs-lookup"><span data-stu-id="507c5-198">After returning from PayPal, you can review and complete your order.</span></span>

![Wingtip Toys - проверки заказа](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="507c5-200">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="507c5-200">Prerequisites</span></span>

<span data-ttu-id="507c5-201">Прежде чем начать, убедитесь, что имеется следующее программное обеспечение на компьютере:</span><span class="sxs-lookup"><span data-stu-id="507c5-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="507c5-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) или [Microsoft Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="507c5-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="507c5-203">Платформа .NET Framework устанавливается автоматически.</span><span class="sxs-lookup"><span data-stu-id="507c5-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="507c5-204">Этот учебник ряд использует Microsoft Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="507c5-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="507c5-205">Для завершения этого учебника ряда можно использовать либо Microsoft Visual Studio Express 2013 для Web или Microsoft Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="507c5-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="507c5-206">Microsoft Visual Studio 2013 и Microsoft Visual Studio Express 2013 для Web будет часто называться Visual Studio на протяжении этого учебника ряда.</span><span class="sxs-lookup"><span data-stu-id="507c5-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="507c5-207">Если уже установлена версия Visual Studio, в процессе установки выполнит установку Visual Studio 2013 или Microsoft Visual Studio Express 2013 для Web рядом с существующие версии.</span><span class="sxs-lookup"><span data-stu-id="507c5-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="507c5-208">Узлы, созданные в более ранних версиях может быть открыто в Visual Studio 2013 и открыть в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="507c5-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="507c5-209">Это пошаговое руководство предполагает, что выбрана *веб-разработки* набор параметров при первом запуске Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="507c5-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="507c5-210">Дополнительные сведения см. в разделе [как: выберите параметры среды веб-разработки](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="507c5-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="507c5-211">Загрузить пример приложения</span><span class="sxs-lookup"><span data-stu-id="507c5-211">Download the Sample Application</span></span>

<span data-ttu-id="507c5-212">После установки необходимых компонентов, вы готовы приступить к созданию нового веб-проекта, которая содержится в этой серии учебника.</span><span class="sxs-lookup"><span data-stu-id="507c5-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="507c5-213">Если вы хотите **при необходимости** запуска примера приложения, который создает этот учебник ряд, его можно загрузить с сайта образцов MSDN.</span><span class="sxs-lookup"><span data-stu-id="507c5-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="507c5-214">Данный файл содержит следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="507c5-214">This download contains the following:</span></span>

- <span data-ttu-id="507c5-215">Образец приложения в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="507c5-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="507c5-216">Ресурсы, используемые для создания примера приложения в *WingtipToys активы* папки в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="507c5-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="507c5-217">Чтобы Загрузите файл с сайта MSDN образцов.</span><span class="sxs-lookup"><span data-stu-id="507c5-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="507c5-218">[Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="507c5-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="507c5-219">Загрузка <em>.zip</em> файла.</span><span class="sxs-lookup"><span data-stu-id="507c5-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="507c5-220">Чтобы просмотреть завершенный проект, создающем этого учебника ряда, найдите и выберите <em>C#</em>папки в <em>.zip</em> файла.</span><span class="sxs-lookup"><span data-stu-id="507c5-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="507c5-221">Сохранить <em>C#</em> folderto папки, предназначенный для работы с проектами Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="507c5-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="507c5-222">По умолчанию папку Проекты Visual Studio 2013 выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="507c5-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="507c5-223"><strong>C:\Users\</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="507c5-223"><strong>C:\Users\</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="507c5-224">Переименуйте ***C#*** папки ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="507c5-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="507c5-225">Если уже имеется папка с именем *WingtipToys* в папке проектов временно переименуйте этот существующую папку перед переименованием *C#* папки *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="507c5-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="507c5-226">Чтобы запустить завершенный проект, откройте *WingtipToys* папку и дважды щелкните *WingtipToys.sln* файла.</span><span class="sxs-lookup"><span data-stu-id="507c5-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="507c5-227">Visual Studio 2013 Откроется проект.</span><span class="sxs-lookup"><span data-stu-id="507c5-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="507c5-228">После этого щелкните правой кнопкой мыши *Default.aspx* файла в окне обозревателя решений и щелкните просмотреть в браузере из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="507c5-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="507c5-229">Поддержка учебника и комментарии</span><span class="sxs-lookup"><span data-stu-id="507c5-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="507c5-230">Используйте раздел A и вопросы, входящий в состав [Приступая к работе с веб-форм ASP.NET 4.5 и Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) образец (C#) для вас есть вопросы или комментарии.</span><span class="sxs-lookup"><span data-stu-id="507c5-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="507c5-231">Приглашаем вас комментарии для этого учебника рядов и при обновлении этого учебника ряда все усилия будут внесены учитывает учетной записи исправлений или подсказок для улучшения, которые предоставляются в комментариях учебника.</span><span class="sxs-lookup"><span data-stu-id="507c5-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="507c5-232">При возникновении ошибки во время разработки или веб-сайт работает неправильно, сообщения об ошибках может привести сложные выявить источник проблемы или может приводятся сведения по ее устранению.</span><span class="sxs-lookup"><span data-stu-id="507c5-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="507c5-233">Чтобы упростить некоторых распространенных проблем, можно также использовать [форумы ASP.NET](https://forums.asp.net/) либо в разделе вопросов и объект, входящий в состав [Приступая к работе с веб-форм ASP.NET 4.5 и Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) образца.</span><span class="sxs-lookup"><span data-stu-id="507c5-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="507c5-234">Если вы получаете сообщение об ошибке или что-то не работает, как пройти учебники, убедитесь, что выше расположениях.</span><span class="sxs-lookup"><span data-stu-id="507c5-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="507c5-235">Вперед</span><span class="sxs-lookup"><span data-stu-id="507c5-235">Next</span></span>](create-the-project.md)
