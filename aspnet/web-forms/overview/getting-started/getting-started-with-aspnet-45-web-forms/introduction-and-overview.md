---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 | Документация Майкрософт
author: Erikre
description: Этой пошаговые серии руководств будет основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio выраз...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 55e7a5e8c0c7df9597f8597cba49d33da23e9159
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365410"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a><span data-ttu-id="78156-103">Приступая к работе с ASP.NET 4.5 и Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="78156-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013</span></span>
====================
<span data-ttu-id="78156-104">по [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="78156-104">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="78156-105">[Скачайте пример проекта Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) или [скачайте электронную книгу (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="78156-105">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

> <span data-ttu-id="78156-106">Этой пошаговые серии руководств будет основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.5 и Microsoft Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="78156-106">This step-by-step tutorial series will teach you the basics of building an ASP.NET Web Forms application using ASP.NET 4.5 and Microsoft Visual Studio Express 2013 for Web.</span></span> [<span data-ttu-id="78156-107">Веб-форм ASP.NET головоломки</span><span class="sxs-lookup"><span data-stu-id="78156-107">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> <span data-ttu-id="78156-108">Проверьте свои знания и подчеркивает основные понятия, используя ASP.NET Web Forms тест.</span><span class="sxs-lookup"><span data-stu-id="78156-108">Test your knowledge and reinforce key concepts by taking the ASP.NET Web Forms Quiz.</span></span> <span data-ttu-id="78156-109">Этот тест был специально разработан из содержимого, содержащихся в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="78156-109">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="78156-110">Каждый вопрос, на решение головоломки содержится пояснение, а также ссылки на дополнительные руководства.</span><span class="sxs-lookup"><span data-stu-id="78156-110">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>


## <a name="introduction"></a><span data-ttu-id="78156-111">Вступление</span><span class="sxs-lookup"><span data-stu-id="78156-111">Introduction</span></span>

<span data-ttu-id="78156-112">Этой серии руководств поможет выполнить шаги, необходимые для создания приложения веб-форм ASP.NET с помощью Visual Studio Express 2013 для Интернета и ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="78156-112">This series of tutorials guides you through the steps required to create an ASP.NET Web Forms application using Visual Studio Express 2013 for Web and ASP.NET 4.5.</span></span>

<span data-ttu-id="78156-113">Приложение, вы создадите называется **Wingtip Toys**.</span><span class="sxs-lookup"><span data-stu-id="78156-113">The application you'll create is named **Wingtip Toys**.</span></span> <span data-ttu-id="78156-114">Это упрощенный пример того, веб-сайт магазина, который продает товары через Интернет.</span><span class="sxs-lookup"><span data-stu-id="78156-114">It's a simplified example of a store front web site that sells items online.</span></span> <span data-ttu-id="78156-115">В этой серии руководств рассказывает о новых возможностях, доступных в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="78156-115">This tutorial series highlights new features available in ASP.NET 4.5.</span></span>

<span data-ttu-id="78156-116">Комментарии приветствуются, и мы сделаем все возможное для обновления этой серии руководств, в зависимости от ваших предложений.</span><span class="sxs-lookup"><span data-stu-id="78156-116">Comments are welcome, and we'll make every effort to update this tutorial series based on your suggestions.</span></span>

### <a name="download-completed-project"></a><span data-ttu-id="78156-117">Процент загрузки проекта</span><span class="sxs-lookup"><span data-stu-id="78156-117">Download completed project</span></span>

<span data-ttu-id="78156-118">Можно загрузить проект C#, содержащий полное руководство.</span><span class="sxs-lookup"><span data-stu-id="78156-118">You can download a C# project that contains the completed tutorial.</span></span>

- <span data-ttu-id="78156-119">[Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="78156-119">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span>

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a><span data-ttu-id="78156-120">Проверьте содержимое, используя связанный тест веб-форм ASP.NET</span><span class="sxs-lookup"><span data-stu-id="78156-120">Review the content by taking the related ASP.NET Web Forms quiz</span></span>

<span data-ttu-id="78156-121">После завершения этого учебника, проверьте свои знания и усилить основные понятия, выполнив [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span><span class="sxs-lookup"><span data-stu-id="78156-121">After you complete this tutorial, test your knowledge and reinforce key concepts by taking the [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001).</span></span> <span data-ttu-id="78156-122">Этот тест был специально разработан из содержимого, содержащихся в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="78156-122">This quiz was specifically designed from content contained in this tutorial series.</span></span> <span data-ttu-id="78156-123">Каждый вопрос, на решение головоломки содержится пояснение, а также ссылки на дополнительные руководства.</span><span class="sxs-lookup"><span data-stu-id="78156-123">Each question in the quiz provides an explanation along with links to additional guidance.</span></span>

- [<span data-ttu-id="78156-124">Веб-форм ASP.NET головоломки</span><span class="sxs-lookup"><span data-stu-id="78156-124">ASP.NET Web Forms Quiz</span></span>](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a><span data-ttu-id="78156-125">Аудитория</span><span class="sxs-lookup"><span data-stu-id="78156-125">Audience</span></span>

<span data-ttu-id="78156-126">Предполагаемая аудитория этой серии руководств — опытных разработчиков, незнакомых с веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="78156-126">The intended audience of this tutorial series is experienced developers who are new to ASP.NET Web Forms.</span></span> <span data-ttu-id="78156-127">Разработчиков, заинтересованных в этой серии руководств должна иметь следующие навыки:</span><span class="sxs-lookup"><span data-stu-id="78156-127">A developer interested in this tutorial series should have the following skills:</span></span>

- <span data-ttu-id="78156-128">Объект, которым известна ориентированный язык программирования (OOP)</span><span class="sxs-lookup"><span data-stu-id="78156-128">Familiar with an object oriented programming (OOP) language</span></span>
- <span data-ttu-id="78156-129">Основные принципы разработки Web (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="78156-129">Familiar with Web development concepts (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="78156-130">Знакомы с понятиями реляционной базы данных</span><span class="sxs-lookup"><span data-stu-id="78156-130">Familiar with relational database concepts</span></span>
- <span data-ttu-id="78156-131">Знакомы с основными понятиями n уровневой архитектуре</span><span class="sxs-lookup"><span data-stu-id="78156-131">Familiar with n-tier architecture concepts</span></span>

<span data-ttu-id="78156-132">Если вы заинтересованы в просмотре областях, перечисленных выше, просмотрите следующее содержимое:</span><span class="sxs-lookup"><span data-stu-id="78156-132">If you are interested in reviewing the areas listed above, consider reviewing the following content:</span></span>

- [<span data-ttu-id="78156-133">Начало работы с Visual C#</span><span class="sxs-lookup"><span data-stu-id="78156-133">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="78156-134">[Разработка веб-](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="78156-134">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="78156-135">Реляционная база данных</span><span class="sxs-lookup"><span data-stu-id="78156-135">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="78156-136">Многоуровневая архитектура</span><span class="sxs-lookup"><span data-stu-id="78156-136">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="78156-137">Компоненты приложений</span><span class="sxs-lookup"><span data-stu-id="78156-137">Application Features</span></span>

<span data-ttu-id="78156-138">Функции веб-форма ASP.NET, представленные в этой серии:</span><span class="sxs-lookup"><span data-stu-id="78156-138">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="78156-139">Проект веб-приложения (не проект веб-сайта)</span><span class="sxs-lookup"><span data-stu-id="78156-139">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="78156-140">Веб-формы</span><span class="sxs-lookup"><span data-stu-id="78156-140">Web Forms</span></span>
- <span data-ttu-id="78156-141">Главные страницы, конфигурации</span><span class="sxs-lookup"><span data-stu-id="78156-141">Master Pages, Configuration</span></span>
- <span data-ttu-id="78156-142">Начальная загрузка</span><span class="sxs-lookup"><span data-stu-id="78156-142">Bootstrap</span></span>
- <span data-ttu-id="78156-143">Платформа Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="78156-143">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="78156-144">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="78156-144">Request Validation</span></span>
- <span data-ttu-id="78156-145">Строго типизированные элементы управления данными, привязка данных заметки, моделей и значение поставщиков</span><span class="sxs-lookup"><span data-stu-id="78156-145">Strongly Typed Data Controls, Model Binding, Data Annotations, and Value Providers</span></span>
- <span data-ttu-id="78156-146">SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="78156-146">SSL and OAuth</span></span>
- <span data-ttu-id="78156-147">ASP.NET Identity, конфигурации и авторизации</span><span class="sxs-lookup"><span data-stu-id="78156-147">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="78156-148">Малозаметная проверка</span><span class="sxs-lookup"><span data-stu-id="78156-148">Unobtrusive Validation</span></span>
- <span data-ttu-id="78156-149">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="78156-149">Routing</span></span>
- <span data-ttu-id="78156-150">Обработка ошибок ASP.NET</span><span class="sxs-lookup"><span data-stu-id="78156-150">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="78156-151">Задачи и сценариях приложений</span><span class="sxs-lookup"><span data-stu-id="78156-151">Application Scenarios and Tasks</span></span>

<span data-ttu-id="78156-152">Задачи, описанные в этой серии включают:</span><span class="sxs-lookup"><span data-stu-id="78156-152">Tasks demonstrated in this series include:</span></span>

- <span data-ttu-id="78156-153">Создание, просмотр и выполнение нового проекта</span><span class="sxs-lookup"><span data-stu-id="78156-153">Creating, reviewing and running the new project</span></span>
- <span data-ttu-id="78156-154">Создание структуры базы данных</span><span class="sxs-lookup"><span data-stu-id="78156-154">Creating the database structure</span></span>
- <span data-ttu-id="78156-155">Инициализация и заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="78156-155">Initializing and seeding the database</span></span>
- <span data-ttu-id="78156-156">Настройка пользовательского интерфейса, используя стили, графики и главной страницы</span><span class="sxs-lookup"><span data-stu-id="78156-156">Customizing the UI using styles, graphics and a master page</span></span>
- <span data-ttu-id="78156-157">Добавление страниц и элементов навигации</span><span class="sxs-lookup"><span data-stu-id="78156-157">Adding pages and navigation</span></span>
- <span data-ttu-id="78156-158">Отображение меню сведений и данные о продуктах</span><span class="sxs-lookup"><span data-stu-id="78156-158">Displaying menu details and product data</span></span>
- <span data-ttu-id="78156-159">Создание корзины для покупок</span><span class="sxs-lookup"><span data-stu-id="78156-159">Creating a shopping cart</span></span>
- <span data-ttu-id="78156-160">Поддержка добавления SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="78156-160">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="78156-161">Добавление метода оплаты</span><span class="sxs-lookup"><span data-stu-id="78156-161">Adding a payment method</span></span>
- <span data-ttu-id="78156-162">Включая роль администратора и пользователя в приложение</span><span class="sxs-lookup"><span data-stu-id="78156-162">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="78156-163">Ограничение доступа к определенным страницам и папки</span><span class="sxs-lookup"><span data-stu-id="78156-163">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="78156-164">Отправка файла веб-приложения</span><span class="sxs-lookup"><span data-stu-id="78156-164">Uploading a file to the web application</span></span>
- <span data-ttu-id="78156-165">Реализация проверки входных данных</span><span class="sxs-lookup"><span data-stu-id="78156-165">Implementing input validation</span></span>
- <span data-ttu-id="78156-166">Регистрация маршрутов для веб-приложения</span><span class="sxs-lookup"><span data-stu-id="78156-166">Registering routes for the web application</span></span>
- <span data-ttu-id="78156-167">Реализация обработки ошибок и ведения журнала ошибок</span><span class="sxs-lookup"><span data-stu-id="78156-167">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="78156-168">Обзор</span><span class="sxs-lookup"><span data-stu-id="78156-168">Overview</span></span>

<span data-ttu-id="78156-169">Если вы не знакомы с веб-форм ASP.NET но Знакомство с принципами программирования, у вас есть правой руководства.</span><span class="sxs-lookup"><span data-stu-id="78156-169">If you are new to ASP.NET Web Forms but have familiarity with programming concepts, you have the right tutorial.</span></span> <span data-ttu-id="78156-170">Если вы уже знакомы с веб-форм ASP.NET, можно воспользоваться преимуществами этой серии руководств новые функции, доступные в ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="78156-170">If you are already familiar with ASP.NET Web Forms, you can benefit from this tutorial series by the new features available in ASP.NET 4.5.</span></span> <span data-ttu-id="78156-171">Если вы не знакомы с программирования основные понятия и веб-форм ASP.NET, см. в разделе Дополнительные руководства, в веб-форм [Приступая к работе](../../../index.md) разделе на веб-сайте ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="78156-171">If you are unfamiliar with programming concepts and ASP.NET Web Forms, see the additional tutorials provided in the Web Forms [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="78156-172">Конкретный **последнюю** ASP.NET 4.5 функций, предоставляемых в этой веб-форм серии руководств включают следующее:</span><span class="sxs-lookup"><span data-stu-id="78156-172">The specific **latest** ASP.NET 4.5 features provided in this Web Forms tutorial series include the following:</span></span>

- <span data-ttu-id="78156-173">Простой пользовательский Интерфейс для создания проектов предложения [поддержка нескольких платформ ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (веб-форм, MVC и веб-API).</span><span class="sxs-lookup"><span data-stu-id="78156-173">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="78156-174">[Начальная загрузка](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), это платформа макет и темы, которая предоставляет быстрые гибкие возможности проектирования и.</span><span class="sxs-lookup"><span data-stu-id="78156-174">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout and theming framework that provides responsive design and theming capabilities.</span></span>
- <span data-ttu-id="78156-175">[ASP.NET Identity](../../../../identity/index.md), новая система членства ASP.NET, который работает так же, во всех платформах ASP.NET и работает с веб-размещения программного обеспечения, отличные от IIS.</span><span class="sxs-lookup"><span data-stu-id="78156-175">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- <span data-ttu-id="78156-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), обновление для платформы Entity Framework, которая позволяет получить и манипулировать данными в строго типизированные объекты, доступ к данным асинхронно, обрабатывать временные сбои соединений, а также регистрировались инструкции SQL.</span><span class="sxs-lookup"><span data-stu-id="78156-176">[Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), an update to the Entity Framework which allows you retrieve and manipulate data as strongly typed objects, access data asynchronously, handle transient connection faults, and log SQL statements.</span></span>

<span data-ttu-id="78156-177">Полный список функций ASP.NET 4.5, см. в разделе [ASP.NET and Web Tools для заметки о выпуске Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="78156-177">For a complete list of ASP.NET 4.5 features, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="78156-178">Пример приложения Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="78156-178">The Wingtip Toys Sample Application</span></span>

<span data-ttu-id="78156-179">На снимках экрана ниже предоставляют краткий обзор приложения ASP.NET Web forms, который вы создадите в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="78156-179">The following screen shots provide a quick view of the ASP.NET Web forms application that you will create in this tutorial series.</span></span> <span data-ttu-id="78156-180">При запуске приложения из Visual Studio Express 2013 для Web, вы увидите на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="78156-180">When you run the application from Visual Studio Express 2013 for Web, you will see the following web Home page.</span></span>

![Компания Wingtip Toys - страница по умолчанию](introduction-and-overview/_static/image1.png)

<span data-ttu-id="78156-182">Можно зарегистрировать в качестве нового пользователя или войдите в качестве существующего пользователя.</span><span class="sxs-lookup"><span data-stu-id="78156-182">You can register as a new user, or log in as an existing user.</span></span> <span data-ttu-id="78156-183">Получая доступные продукты из базы данных предоставляется навигации в верхней для каждой категории продукта.</span><span class="sxs-lookup"><span data-stu-id="78156-183">Navigation is provided at the top for each product category by retrieving the available products from the database.</span></span>

<span data-ttu-id="78156-184">Выбрав ссылку продуктов, можно просмотреть список всех доступных продуктов.</span><span class="sxs-lookup"><span data-stu-id="78156-184">By selecting the Products link, you will be able to see a list of all available products.</span></span>

![Компания Wingtip Toys - продукты](introduction-and-overview/_static/image2.png)

<span data-ttu-id="78156-186">Также можно просмотреть сведения отдельного продукта, выбрав любую из перечисленных продуктов.</span><span class="sxs-lookup"><span data-stu-id="78156-186">You can also see individual product details by selecting any of the listed products.</span></span>

![Компания Wingtip Toys — сведения о продукте](introduction-and-overview/_static/image3.png)

<span data-ttu-id="78156-188">От имени пользователя можно зарегистрировать и выполнить вход с помощью функции шаблона веб-форм по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="78156-188">As a user, you can register and log in using the default functionality of the Web Forms template.</span></span> <span data-ttu-id="78156-189">Этот учебник также объясняется, как выполнить вход с использованием существующей учетной записи Gmail.</span><span class="sxs-lookup"><span data-stu-id="78156-189">This tutorial also explains how to login using an existing Gmail account.</span></span> <span data-ttu-id="78156-190">Кроме того можно выполнить вход от имени администратора, добавлять и удалять продукты из базы данных.</span><span class="sxs-lookup"><span data-stu-id="78156-190">Additionally, you can login as the administrator to add and remove products from the database.</span></span>

![Компания Wingtip Toys — журнал](introduction-and-overview/_static/image4.png)

<span data-ttu-id="78156-192">После входа качестве пользователя, можно добавить продукты в корзину для покупок и оформление заказа с использованием системы PayPal.</span><span class="sxs-lookup"><span data-stu-id="78156-192">Once you have logged in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="78156-193">Обратите внимание на то, что в этом образце приложения предназначен для работы с песочницей разработчика в PayPal.</span><span class="sxs-lookup"><span data-stu-id="78156-193">Note that this sample application is designed to function with PayPal's developer sandbox.</span></span> <span data-ttu-id="78156-194">Конференция пройдет транзакция не фактические суммы расходов.</span><span class="sxs-lookup"><span data-stu-id="78156-194">No actual money transaction will take place.</span></span>

![Компания Wingtip Toys - Корзина для покупок](introduction-and-overview/_static/image5.png)

<span data-ttu-id="78156-196">PayPal подтвердит вашей учетной записи, порядок и сведений об оплате.</span><span class="sxs-lookup"><span data-stu-id="78156-196">PayPal will confirm your account, order, and payment information.</span></span>

![Компания Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="78156-198">После возврата из PayPal, можно просмотреть и оформить заказ.</span><span class="sxs-lookup"><span data-stu-id="78156-198">After returning from PayPal, you can review and complete your order.</span></span>

![Компания Wingtip Toys — Просмотр заказа](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="78156-200">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="78156-200">Prerequisites</span></span>

<span data-ttu-id="78156-201">Перед началом работы убедитесь, что у вас есть следующее программное обеспечение на компьютере:</span><span class="sxs-lookup"><span data-stu-id="78156-201">Before you start, make sure that you have the following software installed on your computer:</span></span>

- <span data-ttu-id="78156-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) или [Microsoft Visual Studio Express 2013 для Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="78156-202">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="78156-203">Платформа .NET Framework устанавливается автоматически.</span><span class="sxs-lookup"><span data-stu-id="78156-203">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="78156-204">В этой серии руководств используется Microsoft Visual Studio Express 2013 для Web.</span><span class="sxs-lookup"><span data-stu-id="78156-204">This tutorial series uses Microsoft Visual Studio Express 2013 for Web.</span></span> <span data-ttu-id="78156-205">Microsoft Visual Studio Express 2013 для Web или Microsoft Visual Studio 2013 можно использовать для выполнения этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="78156-205">You can use either Microsoft Visual Studio Express 2013 for Web or Microsoft Visual Studio 2013 to complete this tutorial series.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="78156-206">Microsoft Visual Studio 2013 и Microsoft Visual Studio Express 2013 для Web будет часто обращаться как к Visual Studio данной серии учебных курсов.</span><span class="sxs-lookup"><span data-stu-id="78156-206">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>


<span data-ttu-id="78156-207">Если у вас уже есть версию Visual Studio, процесс установки установит Visual Studio 2013 или Microsoft Visual Studio Express 2013 для Web рядом с существующей версии.</span><span class="sxs-lookup"><span data-stu-id="78156-207">If you already have a Visual Studio version installed, the installation process will install Visual Studio 2013 or Microsoft Visual Studio Express 2013 for Web next to the existing version.</span></span> <span data-ttu-id="78156-208">Узлы, созданные в более ранних версиях можно открывать в Visual Studio 2013 и в предыдущих версиях по-прежнему.</span><span class="sxs-lookup"><span data-stu-id="78156-208">Sites that you created in earlier versions can be opened in Visual Studio 2013 and continue to open in previous versions.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="78156-209">В этом пошаговом руководстве предполагается, что выбрана *веб-разработки* набор параметров при первом запуске Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78156-209">This walkthrough assumes that you selected the *Web Development* collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="78156-210">Дополнительные сведения см. в разделе [как: выберите параметры среды веб-разработки](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="78156-210">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>


## <a name="download-the-sample-application"></a><span data-ttu-id="78156-211">Загрузить пример приложения</span><span class="sxs-lookup"><span data-stu-id="78156-211">Download the Sample Application</span></span>

<span data-ttu-id="78156-212">После установки необходимых компонентов, вы готовы приступить к созданию нового веб-проекта, представленные в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="78156-212">After installing the prerequisites, you are ready to begin creating the new Web project that is presented in this tutorial series.</span></span> <span data-ttu-id="78156-213">Если вы хотите **при необходимости** запустить пример приложения, который создает этой серии руководств, скачав его с сайта примеров MSDN.</span><span class="sxs-lookup"><span data-stu-id="78156-213">If you would like to **optionally** run the sample application that this tutorial series creates, you can download it from the MSDN Samples site.</span></span> <span data-ttu-id="78156-214">Данный файл содержит следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="78156-214">This download contains the following:</span></span>

- <span data-ttu-id="78156-215">В примере приложения в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="78156-215">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="78156-216">Ресурсы, используемые для создания примера приложения в *WingtipToys-активы* папку в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="78156-216">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

#### <a name="download-the-file-from-msdn-samples-site"></a><span data-ttu-id="78156-217">Скачайте файл с сайта с примерами MSDN:</span><span class="sxs-lookup"><span data-stu-id="78156-217">Download the file from MSDN Samples site:</span></span>

<span data-ttu-id="78156-218">[Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="78156-218">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

<span data-ttu-id="78156-219">Загрузки <em>ZIP-файл</em> файл.</span><span class="sxs-lookup"><span data-stu-id="78156-219">The download is a <em>.zip</em> file.</span></span> <span data-ttu-id="78156-220">Чтобы просмотреть завершенный проект, который создает этой серии руководств, найдите и выберите <em>C#</em>папку в <em>ZIP-файл</em> файл.</span><span class="sxs-lookup"><span data-stu-id="78156-220">To see the completed project that this tutorial series creates, find and select the <em>C#</em>folder in the <em>.zip</em> file.</span></span> <span data-ttu-id="78156-221">Сохранить <em>C#</em> folderto папку, для работы с проектами Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="78156-221">Save the <em>C#</em> folderto the folder you use to work with Visual Studio 2013 projects.</span></span> <span data-ttu-id="78156-222">По умолчанию папку "Проекты" Visual Studio 2013 выглядит так:</span><span class="sxs-lookup"><span data-stu-id="78156-222">By default, the Visual Studio 2013 projects folder is the following:</span></span>

<span data-ttu-id="78156-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span><span class="sxs-lookup"><span data-stu-id="78156-223"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong></span></span>

<span data-ttu-id="78156-224">Переименуйте ***C#*** папку ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="78156-224">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="78156-225">Если у вас уже есть папка с именем *WingtipToys* в папке проектов временно переименуйте этот существующую папку перед переименованием *C#* папку *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="78156-225">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>


<span data-ttu-id="78156-226">Для запуска готового проекта, откройте *WingtipToys* папку и дважды щелкните *WingtipToys.sln* файла.</span><span class="sxs-lookup"><span data-stu-id="78156-226">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="78156-227">Проект будет открыть в Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="78156-227">Visual Studio 2013 will open the project.</span></span> <span data-ttu-id="78156-228">После этого щелкните правой кнопкой мыши *Default.aspx* в окне обозревателя решений и щелкните просмотреть в браузере из контекстного меню.</span><span class="sxs-lookup"><span data-stu-id="78156-228">Next, right-click the *Default.aspx* file in the Solution Explorer window and click View In Browser from the right-click menu.</span></span>

### <a name="tutorial-support-and-comments"></a><span data-ttu-id="78156-229">Поддержка руководства и комментарии</span><span class="sxs-lookup"><span data-stu-id="78156-229">Tutorial Support and Comments</span></span>

<span data-ttu-id="78156-230">Используйте раздел A и Q, состав [начало работы с веб-форм ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) пример (C#) все свои вопросы или комментарии.</span><span class="sxs-lookup"><span data-stu-id="78156-230">Use the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample for any questions or comments.</span></span>

<span data-ttu-id="78156-231">Комментарии в этой серии руководств, принимаются, и при обновлении этой серии руководств с весовым коэффициентом учетной записи исправлений или предложений по улучшениям, которые приведены в руководстве комментарии будут внесены все усилия.</span><span class="sxs-lookup"><span data-stu-id="78156-231">Comments on this tutorial series are welcome, and when this tutorial series is updated every effort will be made to take into account corrections or suggestions for improvements that are provided in the tutorial comments.</span></span>

<span data-ttu-id="78156-232">При возникновении ошибки во время разработки или веб-сайт работает неправильно, сообщения об ошибках может привести к сложных выявить источник проблемы или не может отобразить способы ее устранения.</span><span class="sxs-lookup"><span data-stu-id="78156-232">When an error happens during development, or if the Web site does not run correctly, the error messages may give complex clues to the source of the problem or might not explain how to fix it.</span></span> <span data-ttu-id="78156-233">Чтобы помочь вам с некоторых распространенных проблем, можно также использовать [форумы ASP.NET](https://forums.asp.net/) или разделе Q и объект, в состав [начало работы с веб-форм ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) образца.</span><span class="sxs-lookup"><span data-stu-id="78156-233">To help you with some common problem scenarios, you can also use the [ASP.NET forums](https://forums.asp.net/) or the Q AND A section included with the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample.</span></span> <span data-ttu-id="78156-234">Если вы получаете сообщение об ошибке, или что-то не работает, как просмотреть в руководствах, убедитесь, что выше расположениях.</span><span class="sxs-lookup"><span data-stu-id="78156-234">If you get an error message or something doesn't work as you go through the tutorials, be sure to check the above locations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="78156-235">Вперед</span><span class="sxs-lookup"><span data-stu-id="78156-235">Next</span></span>](create-the-project.md)
