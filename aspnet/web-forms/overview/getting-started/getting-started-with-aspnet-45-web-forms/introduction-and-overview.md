---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 | Документация Майкрософт
author: Erikre
description: Этой пошаговые серии руководств будет основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio выраз...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: b9ed6ce4ac13f047f53c56e183433cbd038ea15c
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450688"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="98f02-103">Приступая к работе с ASP.NET 4.5 и Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="98f02-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="98f02-104">по [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="98f02-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="98f02-105">[Скачайте пример проекта Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) или [скачайте электронную книгу (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="98f02-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="98f02-106">Этой пошаговые серии руководств будет основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="98f02-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> 

## <a name="introduction"></a><span data-ttu-id="98f02-107">Вступление</span><span class="sxs-lookup"><span data-stu-id="98f02-107">Introduction</span></span>

<span data-ttu-id="98f02-108">Этой серии руководств поможет выполнить шаги, необходимые для создания приложения веб-форм ASP.NET с помощью Visual Studio Express 2013 для Интернета и ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="98f02-108">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="98f02-109">Приложение, вы создадите называется **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="98f02-109">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="98f02-110">Это упрощенный пример того, веб-сайт магазина, который продает товары через Интернет.</span><span class="sxs-lookup"><span data-stu-id="98f02-110">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="98f02-111">В этой серии руководств рассказывает о новых возможностях, доступных в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="98f02-111">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="98f02-112">Комментарии приветствуются, и мы сделаем все возможное для обновления этой серии руководств, в зависимости от ваших предложений.</span><span class="sxs-lookup"><span data-stu-id="98f02-112">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="98f02-113">Процент загрузки проекта</span><span class="sxs-lookup"><span data-stu-id="98f02-113">Download completed project</span></span>

<span data-ttu-id="98f02-114">Можно загрузить проект C#, содержащий полное руководство.</span><span class="sxs-lookup"><span data-stu-id="98f02-114">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="98f02-115">[Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="98f02-115">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="98f02-116">Проверьте содержимое, используя связанный тест веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="98f02-116">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="98f02-117">После завершения этого учебника, проверьте свои знания и усилить основные понятия, выполнив [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="98f02-117">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="98f02-118">Этот тест был специально разработан из содержимого, содержащихся в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="98f02-118">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="98f02-119">Каждый вопрос, на решение головоломки содержится пояснение, а также ссылки на дополнительные руководства.</span><span class="sxs-lookup"><span data-stu-id="98f02-119">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="98f02-120">Веб-форм ASP.NET головоломки</span><span class="sxs-lookup"><span data-stu-id="98f02-120">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="98f02-121">Аудитория</span><span class="sxs-lookup"><span data-stu-id="98f02-121">Audience</span></span>

<span data-ttu-id="98f02-122">Предполагаемая аудитория этой серии руководств — опытных разработчиков, незнакомых с веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="98f02-122">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="98f02-123">Разработчиков, заинтересованных в этой серии руководств должна иметь следующие навыки:</span><span class="sxs-lookup"><span data-stu-id="98f02-123">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="98f02-124">Объект, которым известна ориентированный язык программирования (OOP)</span><span class="sxs-lookup"><span data-stu-id="98f02-124">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="98f02-125">Основные принципы разработки Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="98f02-125">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="98f02-126">Знакомы с понятиями реляционной базы данных</span><span class="sxs-lookup"><span data-stu-id="98f02-126">Familiar with relational database concepts</span></span>
- <span data-ttu-id="98f02-127">Знакомы с основными понятиями n уровневой архитектуре</span><span class="sxs-lookup"><span data-stu-id="98f02-127">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="98f02-128">Если вы заинтересованы в просмотре областях, перечисленных выше, просмотрите следующее содержимое:</span><span class="sxs-lookup"><span data-stu-id="98f02-128">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="98f02-129">Начало работы с Visual C#</span><span class="sxs-lookup"><span data-stu-id="98f02-129">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="98f02-130">[Разработка веб-](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="98f02-130">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="98f02-131">Реляционная база данных</span><span class="sxs-lookup"><span data-stu-id="98f02-131">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="98f02-132">Многоуровневая архитектура</span><span class="sxs-lookup"><span data-stu-id="98f02-132">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="98f02-133">Компоненты приложений</span><span class="sxs-lookup"><span data-stu-id="98f02-133">Application Features</span></span>

<span data-ttu-id="98f02-134">Функции веб-форма ASP.NET, представленные в этой серии:</span><span class="sxs-lookup"><span data-stu-id="98f02-134">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="98f02-135">Проект веб-приложения (не проект веб-сайта)</span><span class="sxs-lookup"><span data-stu-id="98f02-135">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="98f02-136">Веб-формы</span><span class="sxs-lookup"><span data-stu-id="98f02-136">Web Forms</span></span>
- <span data-ttu-id="98f02-137">Главные страницы, конфигурации</span><span class="sxs-lookup"><span data-stu-id="98f02-137">Master Pages, Configuration</span></span>
- <span data-ttu-id="98f02-138">Начальная загрузка</span><span class="sxs-lookup"><span data-stu-id="98f02-138">Bootstrap</span></span>
- <span data-ttu-id="98f02-139">Платформа Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="98f02-139">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="98f02-140">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="98f02-140">Request Validation</span></span>
- <span data-ttu-id="98f02-141">Строго типизированные элементы управления данными, привязка данных заметки, моделей и значение поставщиков</span><span class="sxs-lookup"><span data-stu-id="98f02-141">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="98f02-142">SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="98f02-142">SSL and OAuth</span></span>
- <span data-ttu-id="98f02-143">ASP.NET Identity, конфигурации и авторизации</span><span class="sxs-lookup"><span data-stu-id="98f02-143">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="98f02-144">Малозаметная проверка</span><span class="sxs-lookup"><span data-stu-id="98f02-144">Unobtrusive Validation</span></span>
- <span data-ttu-id="98f02-145">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="98f02-145">Routing</span></span>
- <span data-ttu-id="98f02-146">Обработка ошибок ASP.NET</span><span class="sxs-lookup"><span data-stu-id="98f02-146">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="98f02-147">Задачи и сценариях приложений</span><span class="sxs-lookup"><span data-stu-id="98f02-147">Application Scenarios and Tasks</span></span>

<span data-ttu-id="98f02-148">Задачи, описанные в этой серии включают:</span><span class="sxs-lookup"><span data-stu-id="98f02-148">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="98f02-149">Создание, просмотр и выполнение нового проекта</span><span class="sxs-lookup"><span data-stu-id="98f02-149">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="98f02-150">Создание структуры базы данных</span><span class="sxs-lookup"><span data-stu-id="98f02-150">Creating the database structure</span></span>
- <span data-ttu-id="98f02-151">Инициализация и заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="98f02-151">Initializing and seeding the database</span></span>
- <span data-ttu-id="98f02-152">Настройка пользовательского интерфейса, используя стили, графики и главной страницы</span><span class="sxs-lookup"><span data-stu-id="98f02-152">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="98f02-153">Добавление страниц и элементов навигации</span><span class="sxs-lookup"><span data-stu-id="98f02-153">Adding pages and navigation</span></span>
- <span data-ttu-id="98f02-154">Отображение меню сведений и данные о продуктах</span><span class="sxs-lookup"><span data-stu-id="98f02-154">Displaying menu details and product data</span></span>
- <span data-ttu-id="98f02-155">Создание корзины для покупок</span><span class="sxs-lookup"><span data-stu-id="98f02-155">Creating a shopping cart</span></span>
- <span data-ttu-id="98f02-156">Поддержка добавления SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="98f02-156">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="98f02-157">Добавление метода оплаты</span><span class="sxs-lookup"><span data-stu-id="98f02-157">Adding a payment method</span></span>
- <span data-ttu-id="98f02-158">Включая роль администратора и пользователя в приложение</span><span class="sxs-lookup"><span data-stu-id="98f02-158">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="98f02-159">Ограничение доступа к определенным страницам и папки</span><span class="sxs-lookup"><span data-stu-id="98f02-159">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="98f02-160">Отправка файла веб-приложения</span><span class="sxs-lookup"><span data-stu-id="98f02-160">Uploading a file to the web application</span></span>
- <span data-ttu-id="98f02-161">Реализация проверки входных данных</span><span class="sxs-lookup"><span data-stu-id="98f02-161">Implementing input validation</span></span>
- <span data-ttu-id="98f02-162">Регистрация маршрутов для веб-приложения</span><span class="sxs-lookup"><span data-stu-id="98f02-162">Registering routes for the web application</span></span>
- <span data-ttu-id="98f02-163">Реализация обработки ошибок и ведения журнала ошибок</span><span class="sxs-lookup"><span data-stu-id="98f02-163">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="98f02-164">Обзор</span><span class="sxs-lookup"><span data-stu-id="98f02-164">Overview</span></span>

<span data-ttu-id="98f02-165">Если вы не знакомы с веб-форм ASP.NET но Знакомство с принципами программирования, у вас есть правой руководства.</span><span class="sxs-lookup"><span data-stu-id="98f02-165">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="98f02-166">Если вы уже знакомы с веб-форм ASP.NET, можно воспользоваться преимуществами этой серии руководств новые функции, доступные в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="98f02-166">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="98f02-167">Если вы не знакомы с программирования основные понятия и веб-форм ASP.NET, см. в разделе Дополнительные руководства, в веб-форм [Приступая к работе](../../../index.md) разделе на веб-сайте ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="98f02-167">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="98f02-168">Конкретный **последнюю** ASP.NET 4.5 функций, предоставляемых в этой веб-форм серии руководств включают следующее:</span><span class="sxs-lookup"><span data-stu-id="98f02-168">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="98f02-169">Простой пользовательский Интерфейс для создания проектов предложения [поддержка нескольких платформ ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (веб-форм, MVC и веб-API).</span><span class="sxs-lookup"><span data-stu-id="98f02-169">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="98f02-170">[Начальная загрузка](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), это платформа макет и темы, которая предоставляет быстрые гибкие возможности проектирования и.</span><span class="sxs-lookup"><span data-stu-id="98f02-170">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="98f02-171">[ASP.NET Identity](../../../../identity/index.md), новая система членства ASP.NET, который работает так же, во всех платформах ASP.NET и работает с веб-размещения программного обеспечения, отличные от IIS.</span><span class="sxs-lookup"><span data-stu-id="98f02-171">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="98f02-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), обновление для платформы Entity Framework, которая позволяет получить и манипулировать данными в строго типизированные объекты, доступ к данным асинхронно, обрабатывать временные сбои соединений, а также регистрировались инструкции SQL.</span><span class="sxs-lookup"><span data-stu-id="98f02-172">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="98f02-173">Полный список функций ASP.NET 4.5, см. в разделе [ASP.NET and Web Tools для заметки о выпуске Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="98f02-173">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="98f02-174">Пример приложения Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="98f02-174">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="98f02-175">На снимках экрана ниже предоставляют краткий обзор приложения ASP.NET Web forms, который вы создадите в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="98f02-175">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="98f02-176">При запуске приложения из Visual Studio Express 2013 для Web, вы увидите на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="98f02-176">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Компания Wingtip Toys - страница по умолчанию](introduction-and-overview/_static/image1.png)

<span data-ttu-id="98f02-178">Можно зарегистрировать в качестве нового пользователя или войдите в качестве существующего пользователя.</span><span class="sxs-lookup"><span data-stu-id="98f02-178">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="98f02-179">Получая доступные продукты из базы данных предоставляется навигации в верхней для каждой категории продукта.</span><span class="sxs-lookup"><span data-stu-id="98f02-179">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="98f02-180">Выбрав ссылку продуктов, можно просмотреть список всех доступных продуктов.</span><span class="sxs-lookup"><span data-stu-id="98f02-180">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Компания Wingtip Toys - продукты](introduction-and-overview/_static/image2.png)

<span data-ttu-id="98f02-182">Также можно просмотреть сведения отдельного продукта, выбрав любую из перечисленных продуктов.</span><span class="sxs-lookup"><span data-stu-id="98f02-182">You can also see individual product details by selecting any of the listed products.</span></span>

![Компания Wingtip Toys — сведения о продукте](introduction-and-overview/_static/image3.png)

<span data-ttu-id="98f02-184">От имени пользователя можно зарегистрировать и выполнить вход с помощью функции шаблона веб-форм по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="98f02-184">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="98f02-185">Этот учебник также объясняется, как выполнить вход с использованием существующей учетной записи Gmail.</span><span class="sxs-lookup"><span data-stu-id="98f02-185">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="98f02-186">Кроме того можно выполнить вход от имени администратора, добавлять и удалять продукты из базы данных.</span><span class="sxs-lookup"><span data-stu-id="98f02-186">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Компания Wingtip Toys — журнал](introduction-and-overview/_static/image4.png)

<span data-ttu-id="98f02-188">После входа качестве пользователя, можно добавить продукты в корзину для покупок и оформление заказа с использованием системы PayPal.</span><span class="sxs-lookup"><span data-stu-id="98f02-188">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="98f02-189">Обратите внимание на то, что в этом образце приложения предназначен для работы с песочницей разработчика в PayPal.</span><span class="sxs-lookup"><span data-stu-id="98f02-189">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="98f02-190">Конференция пройдет транзакция не фактические суммы расходов.</span><span class="sxs-lookup"><span data-stu-id="98f02-190">No actual money transaction will take place.</span></span>

![Компания Wingtip Toys - Корзина для покупок](introduction-and-overview/_static/image5.png)

<span data-ttu-id="98f02-192">PayPal подтвердит вашей учетной записи, порядок и сведений об оплате.</span><span class="sxs-lookup"><span data-stu-id="98f02-192">PayPal will confirm your account, order, and payment information.</span></span>

![Компания Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="98f02-194">После возврата из PayPal, можно просмотреть и оформить заказ.</span><span class="sxs-lookup"><span data-stu-id="98f02-194">After returning from PayPal, you can review and complete your order.</span></span>

![Компания Wingtip Toys — Просмотр заказа](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="98f02-196">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="98f02-196">Prerequisites</span></span>

<span data-ttu-id="98f02-197">Перед началом работы убедитесь, что у вас есть следующее программное обеспечение на компьютере:</span><span class="sxs-lookup"><span data-stu-id="98f02-197">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="98f02-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) или [Microsoft Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="98f02-198">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="98f02-199">Платформа .NET Framework устанавливается автоматически.</span><span class="sxs-lookup"><span data-stu-id="98f02-199">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="98f02-200">В этой серии руководств используется Microsoft Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="98f02-200">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="98f02-201">Microsoft Visual Studio Express 2013 для Web или Microsoft Visual Studio 2013 можно использовать для выполнения этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="98f02-201">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="98f02-202">Microsoft Visual Studio 2013 и Microsoft Visual Studio Express 2013 для Web будет часто обращаться как к Visual Studio данной серии учебных курсов.</span><span class="sxs-lookup"><span data-stu-id="98f02-202">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="98f02-203">Если у вас уже есть версию Visual Studio, процесс установки установит Visual Studio 2013 или Microsoft Visual Studio Express 2013 для Web рядом с существующей версии.</span><span class="sxs-lookup"><span data-stu-id="98f02-203">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="98f02-204">Узлы, созданные в более ранних версиях можно открывать в Visual Studio 2013 и в предыдущих версиях по-прежнему.</span><span class="sxs-lookup"><span data-stu-id="98f02-204">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="98f02-205">В этом пошаговом руководстве предполагается, что выбрана *веб-разработки* набор параметров при первом запуске Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="98f02-205">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="98f02-206">Дополнительные сведения см. в разделе [как: выберите параметры среды веб-разработки](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="98f02-206">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="98f02-207">Загрузить пример приложения</span><span class="sxs-lookup"><span data-stu-id="98f02-207">Download the Sample Application</span></span>

<span data-ttu-id="98f02-208">После установки необходимых компонентов, вы готовы приступить к созданию нового веб-проекта, представленные в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="98f02-208">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="98f02-209">Если вы хотите **при необходимости** запустить пример приложения, который создает этой серии руководств, скачав его с сайта примеров MSDN.</span><span class="sxs-lookup"><span data-stu-id="98f02-209">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="98f02-210">Данный файл содержит следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="98f02-210">This download contains the following:</span></span>

- <span data-ttu-id="98f02-211">В примере приложения в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="98f02-211">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="98f02-212">Ресурсы, используемые для создания примера приложения в *WingtipToys-активы* папку в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="98f02-212">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="98f02-213">Скачайте файл с сайта с примерами MSDN:</span><span class="sxs-lookup"><span data-stu-id="98f02-213">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="98f02-214">[Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="98f02-214">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="98f02-215">Загрузки <em>ZIP-файл</em> файл.</span><span class="sxs-lookup"><span data-stu-id="98f02-215">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="98f02-216">Чтобы просмотреть завершенный проект, который создает этой серии руководств, найдите и выберите <em>C#</em>папку в <em>ZIP-файл</em> файл.</span><span class="sxs-lookup"><span data-stu-id="98f02-216">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="98f02-217">Сохранить <em>C#</em> folderto папку, для работы с проектами Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="98f02-217">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="98f02-218">По умолчанию папку "Проекты" Visual Studio 2013 выглядит так:</span><span class="sxs-lookup"><span data-stu-id="98f02-218">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="98f02-219"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="98f02-219"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="98f02-220">Переименуйте ***C#*** папку ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="98f02-220">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="98f02-221">Если у вас уже есть папка с именем *WingtipToys* в папке проектов временно переименуйте этот существующую папку перед переименованием *C#* папку *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="98f02-221">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="98f02-222">Для запуска готового проекта, откройте *WingtipToys* папку и дважды щелкните *WingtipToys.sln* файла.</span><span class="sxs-lookup"><span data-stu-id="98f02-222">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="98f02-223">Проект будет открыть в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="98f02-223">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="98f02-224">После этого щелкните правой кнопкой мыши *Default.aspx* в окне обозревателя решений и щелкните просмотреть в браузере из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="98f02-224">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="98f02-225">Поддержка руководства и комментарии</span><span class="sxs-lookup"><span data-stu-id="98f02-225">Tutorial Support and Comments</span></span>

<span data-ttu-id="98f02-226">Используйте раздел A и Q, состав [начало работы с веб-форм ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) пример (C#) все свои вопросы или комментарии.</span><span class="sxs-lookup"><span data-stu-id="98f02-226">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="98f02-227">Комментарии в этой серии руководств, принимаются, и при обновлении этой серии руководств с весовым коэффициентом учетной записи исправлений или предложений по улучшениям, которые приведены в руководстве комментарии будут внесены все усилия.</span><span class="sxs-lookup"><span data-stu-id="98f02-227">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="98f02-228">При возникновении ошибки во время разработки или веб-сайт работает неправильно, сообщения об ошибках может привести к сложных выявить источник проблемы или не может отобразить способы ее устранения.</span><span class="sxs-lookup"><span data-stu-id="98f02-228">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="98f02-229">Чтобы помочь вам с некоторых распространенных проблем, можно также использовать [форумы ASP.NET](https://forums.asp.net/) или разделе Q и объект, в состав [начало работы с веб-форм ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) образца.</span><span class="sxs-lookup"><span data-stu-id="98f02-229">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="98f02-230">Если вы получаете сообщение об ошибке, или что-то не работает, как просмотреть в руководствах, убедитесь, что выше расположениях.</span><span class="sxs-lookup"><span data-stu-id="98f02-230">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="98f02-231">Вперед</span><span class="sxs-lookup"><span data-stu-id="98f02-231">Next</span></span>](create-the-project.md)
