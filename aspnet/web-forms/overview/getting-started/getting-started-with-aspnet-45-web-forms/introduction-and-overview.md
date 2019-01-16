---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Приступая к работе с 4,7 веб-форм ASP.NET и Visual Studio 2017 | Документация Майкрософт
author: Erikre
description: Этой пошаговые серии руководств будет основы создания приложений веб-форм ASP.NET с помощью ASP.NET 4.7 и Microsoft Visual Studio
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341557"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a><span data-ttu-id="2f1bd-103">Приступая к работе с ASP.NET 4.5 и Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2f1bd-103">Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2017</span></span>
====================

<span data-ttu-id="2f1bd-104">[Скачайте пример проекта Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) или [скачайте электронную книгу (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span><span class="sxs-lookup"><span data-stu-id="2f1bd-104">[Download Wingtip Toys Sample Project (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) or [Download E-book (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)</span></span>

<span data-ttu-id="2f1bd-105">В этой серии руководств показано, как создать приложение веб-форм ASP.NET с ASP.NET 4.5 и Microsoft Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-105">This tutorial series shows you how to build an ASP.NET Web Forms application with ASP.NET 4.5 and Microsoft Visual Studio 2017.</span></span> 

## <a name="introduction"></a><span data-ttu-id="2f1bd-106">Вступление</span><span class="sxs-lookup"><span data-stu-id="2f1bd-106">Introduction</span></span>

<span data-ttu-id="2f1bd-107">В этой серии руководств, чтобы руководство по созданию приложения веб-форм ASP.NET с помощью Visual Studio 2017 и ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-107">This tutorial series guides you through creating an ASP.NET Web Forms application using Visual Studio 2017 and ASP.NET 4.5.</span></span> <span data-ttu-id="2f1bd-108">Вы создадите приложение с именем **Wingtip Toys** — упрощенный веб-сайт Интернет-магазин на продаже товаров через Интернет.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-108">You'll create an application named **Wingtip Toys** - a simplified storefront web site selling items online.</span></span> <span data-ttu-id="2f1bd-109">Во время ряда выделяются новые функции ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-109">During the series, new ASP.NET 4.5 features are highlighted.</span></span>

### <a name="target-audience"></a><span data-ttu-id="2f1bd-110">Целевая аудитория</span><span class="sxs-lookup"><span data-stu-id="2f1bd-110">Target audience</span></span>

<span data-ttu-id="2f1bd-111">Разработчики знакомы с веб-форм ASP.NET, целевая аудитория для этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-111">Developers new to ASP.NET Web Forms are the target audience for this tutorial series.</span></span>

<span data-ttu-id="2f1bd-112">Вам необходимо иметь некоторые знания в следующих областях:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-112">You should have some knowledge in the following areas:</span></span>

- <span data-ttu-id="2f1bd-113">Объектно ориентированное программирование (ООП) и языков</span><span class="sxs-lookup"><span data-stu-id="2f1bd-113">Object-oriented programming (OOP) and languages</span></span>
- <span data-ttu-id="2f1bd-114">Веб-разработки (HTML, CSS, JavaScript)</span><span class="sxs-lookup"><span data-stu-id="2f1bd-114">Web development (HTML, CSS, JavaScript)</span></span>
- <span data-ttu-id="2f1bd-115">реляционные базы данных</span><span class="sxs-lookup"><span data-stu-id="2f1bd-115">Relational databases</span></span>
- <span data-ttu-id="2f1bd-116">N-уровневой архитектуре</span><span class="sxs-lookup"><span data-stu-id="2f1bd-116">N-tier architecture</span></span>

<span data-ttu-id="2f1bd-117">Для просмотра этих областей, рассмотрите возможность изучения следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-117">To review these areas, consider studying the following content:</span></span>

- [<span data-ttu-id="2f1bd-118">Начало работы с Visual C#</span><span class="sxs-lookup"><span data-stu-id="2f1bd-118">Getting Started with Visual C#</span></span>](https://msdn.microsoft.com/library/a72418yk.aspx)
- <span data-ttu-id="2f1bd-119">[Разработка веб-](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span><span class="sxs-lookup"><span data-stu-id="2f1bd-119">[Web Development](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)</span></span>
- [<span data-ttu-id="2f1bd-120">Реляционная база данных</span><span class="sxs-lookup"><span data-stu-id="2f1bd-120">Relational database</span></span>](http://en.wikipedia.org/wiki/Relational_database)
- [<span data-ttu-id="2f1bd-121">Многоуровневая архитектура</span><span class="sxs-lookup"><span data-stu-id="2f1bd-121">Multitier architecture</span></span>](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a><span data-ttu-id="2f1bd-122">Компоненты приложений</span><span class="sxs-lookup"><span data-stu-id="2f1bd-122">Application features</span></span>

<span data-ttu-id="2f1bd-123">Функции веб-форма ASP.NET, представленные в этой серии:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-123">The ASP.NET Web Form features presented in this series include:</span></span>

- <span data-ttu-id="2f1bd-124">Проект веб-приложения (не проект веб-сайта)</span><span class="sxs-lookup"><span data-stu-id="2f1bd-124">The Web Application Project (not Web Site Project)</span></span>
- <span data-ttu-id="2f1bd-125">Веб-формы</span><span class="sxs-lookup"><span data-stu-id="2f1bd-125">Web Forms</span></span>
- <span data-ttu-id="2f1bd-126">Главные страницы, конфигурации</span><span class="sxs-lookup"><span data-stu-id="2f1bd-126">Master Pages, Configuration</span></span>
- <span data-ttu-id="2f1bd-127">Начальная загрузка</span><span class="sxs-lookup"><span data-stu-id="2f1bd-127">Bootstrap</span></span>
- <span data-ttu-id="2f1bd-128">Платформа Entity Framework Code First, LocalDB</span><span class="sxs-lookup"><span data-stu-id="2f1bd-128">Entity Framework Code First, LocalDB</span></span>
- <span data-ttu-id="2f1bd-129">Проверка запросов</span><span class="sxs-lookup"><span data-stu-id="2f1bd-129">Request Validation</span></span>
- <span data-ttu-id="2f1bd-130">Элементы управления данные со строгой типизацией</span><span class="sxs-lookup"><span data-stu-id="2f1bd-130">Strongly-typed Data Controls</span></span>
- <span data-ttu-id="2f1bd-131">Привязка модели</span><span class="sxs-lookup"><span data-stu-id="2f1bd-131">Model Binding</span></span>
- <span data-ttu-id="2f1bd-132">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="2f1bd-132">Data Annotations</span></span>
- <span data-ttu-id="2f1bd-133">Поставщики значений</span><span class="sxs-lookup"><span data-stu-id="2f1bd-133">Value Providers</span></span>
- <span data-ttu-id="2f1bd-134">SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="2f1bd-134">SSL and OAuth</span></span>
- <span data-ttu-id="2f1bd-135">ASP.NET Identity, конфигурации и авторизации</span><span class="sxs-lookup"><span data-stu-id="2f1bd-135">ASP.NET Identity, Configuration, and Authorization</span></span>
- <span data-ttu-id="2f1bd-136">Малозаметная проверка</span><span class="sxs-lookup"><span data-stu-id="2f1bd-136">Unobtrusive Validation</span></span>
- <span data-ttu-id="2f1bd-137">Маршрутизация</span><span class="sxs-lookup"><span data-stu-id="2f1bd-137">Routing</span></span>
- <span data-ttu-id="2f1bd-138">Обработка ошибок ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2f1bd-138">ASP.NET Error Handling</span></span>

### <a name="application-scenarios-and-tasks"></a><span data-ttu-id="2f1bd-139">Задачи и сценариях приложений</span><span class="sxs-lookup"><span data-stu-id="2f1bd-139">Application scenarios and tasks</span></span>

<span data-ttu-id="2f1bd-140">Следующие задачи серии руководств.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-140">Tutorial series tasks include:</span></span>

- <span data-ttu-id="2f1bd-141">Создание, проверка и выполнение проекта</span><span class="sxs-lookup"><span data-stu-id="2f1bd-141">Creating, reviewing, and running a new project</span></span>
- <span data-ttu-id="2f1bd-142">Создание структуры базы данных</span><span class="sxs-lookup"><span data-stu-id="2f1bd-142">Creating a database structure</span></span>
- <span data-ttu-id="2f1bd-143">Инициализация и заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="2f1bd-143">Initializing and seeding a database</span></span>
- <span data-ttu-id="2f1bd-144">Настройка пользовательского интерфейса с помощью стилей, графики и главной страницы</span><span class="sxs-lookup"><span data-stu-id="2f1bd-144">Customizing the UI with styles, graphics, and a master page</span></span>
- <span data-ttu-id="2f1bd-145">Добавление страниц и элементов навигации</span><span class="sxs-lookup"><span data-stu-id="2f1bd-145">Adding pages and navigation</span></span>
- <span data-ttu-id="2f1bd-146">Отображение меню сведений и данные о продуктах</span><span class="sxs-lookup"><span data-stu-id="2f1bd-146">Displaying menu details and product data</span></span>
- <span data-ttu-id="2f1bd-147">Создание корзины для покупок</span><span class="sxs-lookup"><span data-stu-id="2f1bd-147">Creating a shopping cart</span></span>
- <span data-ttu-id="2f1bd-148">Поддержка добавления SSL и OAuth</span><span class="sxs-lookup"><span data-stu-id="2f1bd-148">Adding SSL and OAuth support</span></span>
- <span data-ttu-id="2f1bd-149">Добавление метода оплаты</span><span class="sxs-lookup"><span data-stu-id="2f1bd-149">Adding a payment method</span></span>
- <span data-ttu-id="2f1bd-150">Включая роль администратора и пользователя в приложение</span><span class="sxs-lookup"><span data-stu-id="2f1bd-150">Including an administrator role and a user to the application</span></span>
- <span data-ttu-id="2f1bd-151">Ограничение доступа к определенным страницам и папки</span><span class="sxs-lookup"><span data-stu-id="2f1bd-151">Restricting access to specific pages and folder</span></span>
- <span data-ttu-id="2f1bd-152">Отправка файла веб-приложения</span><span class="sxs-lookup"><span data-stu-id="2f1bd-152">Uploading a file to the web application</span></span>
- <span data-ttu-id="2f1bd-153">Реализация проверки входных данных</span><span class="sxs-lookup"><span data-stu-id="2f1bd-153">Implementing input validation</span></span>
- <span data-ttu-id="2f1bd-154">Регистрация маршрутов для веб-приложения</span><span class="sxs-lookup"><span data-stu-id="2f1bd-154">Registering routes for the web application</span></span>
- <span data-ttu-id="2f1bd-155">Реализация обработки ошибок и ведения журнала ошибок</span><span class="sxs-lookup"><span data-stu-id="2f1bd-155">Implementing error handling and error logging</span></span>

## <a name="overview"></a><span data-ttu-id="2f1bd-156">Обзор</span><span class="sxs-lookup"><span data-stu-id="2f1bd-156">Overview</span></span>

<span data-ttu-id="2f1bd-157">В этой серии руководств, предназначен для кого-то знакомы основные понятия программирования, но создавать новые для веб-форм ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-157">This tutorial series is intended for someone familiar with programming concepts, but new to ASP.NET Web Forms.</span></span> <span data-ttu-id="2f1bd-158">Если вы уже знакомы с веб-форм ASP.NET, в этой серии по-прежнему помогут вам узнать о новых возможностях ASP.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-158">If you're already familiar with ASP.NET Web Forms, this series can still help you learn about new ASP.NET 4.5 features.</span></span> <span data-ttu-id="2f1bd-159">Читатели знакомы с программирования основные понятия и веб-форм ASP.NET, см. Дополнительные руководства веб-форм, в [Приступая к работе](../../../index.md) разделе на веб-сайте ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-159">For readers unfamiliar with programming concepts and ASP.NET Web Forms, see the additional Web Forms tutorials provided in the [Getting Started](../../../index.md) section on the ASP.NET Web site.</span></span>

<span data-ttu-id="2f1bd-160">ASP.NET 4.5, приведенные в этой серии руководств включает следующие компоненты:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-160">The ASP.NET 4.5 provided in this tutorial series includes the following features:</span></span>

- <span data-ttu-id="2f1bd-161">Простой пользовательский Интерфейс для создания проектов, которая предлагает [поддержку многих платформ ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (веб-форм, MVC и веб-API).</span><span class="sxs-lookup"><span data-stu-id="2f1bd-161">A simple UI for creating projects that offers [support for many ASP.NET frameworks](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (Web Forms, MVC, and Web API).</span></span>
- <span data-ttu-id="2f1bd-162">[Начальная загрузка](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), макет, темы и framework гибкий Дизайн.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-162">[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), a layout, theming, and responsive design framework.</span></span>
- <span data-ttu-id="2f1bd-163">[ASP.NET Identity](../../../../identity/index.md), новая система членства ASP.NET, который работает так же, во всех платформах ASP.NET и работает с веб-размещения программного обеспечения, отличные от IIS.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-163">[ASP.NET Identity](../../../../identity/index.md), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
- [<span data-ttu-id="2f1bd-164">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2f1bd-164">Entity Framework 6</span></span>](https://msdn.microsoft.com/data/ef.aspx)

  <span data-ttu-id="2f1bd-165">Обновление для платформы Entity Framework, что позволяет:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-165">An update to the Entity Framework enabling you to:</span></span>
  - <span data-ttu-id="2f1bd-166">Извлечения и манипулирования данными в виде строго типизированных объектов</span><span class="sxs-lookup"><span data-stu-id="2f1bd-166">Retrieve and manipulate data as strongly-typed objects</span></span>
  - <span data-ttu-id="2f1bd-167">Асинхронно получить доступ к данным</span><span class="sxs-lookup"><span data-stu-id="2f1bd-167">Access data asynchronously</span></span>
  - <span data-ttu-id="2f1bd-168">Обрабатывать временные сбои соединений</span><span class="sxs-lookup"><span data-stu-id="2f1bd-168">Handle transient connection faults</span></span>
  - <span data-ttu-id="2f1bd-169">Журнал инструкций SQL</span><span class="sxs-lookup"><span data-stu-id="2f1bd-169">Log SQL statements</span></span>

<span data-ttu-id="2f1bd-170">Полный список компонентов ASP.NET 4.5, см. в разделе [ASP.NET and Web Tools для заметки о выпуске Visual Studio 2013](../../../../visual-studio/overview/2013/release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="2f1bd-170">For a complete ASP.NET 4.5 feature list, see [ASP.NET and Web Tools for Visual Studio 2013 Release Notes](../../../../visual-studio/overview/2013/release-notes.md).</span></span>

### <a name="the-wingtip-toys-sample-application"></a><span data-ttu-id="2f1bd-171">Пример приложения Wingtip Toys</span><span class="sxs-lookup"><span data-stu-id="2f1bd-171">The Wingtip Toys sample application</span></span>

<span data-ttu-id="2f1bd-172">Следующие снимки экрана представляют среду из приложения веб-форм ASP.NET, созданный в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-172">The following screenshots are from the ASP.NET Web Forms application that you create in this tutorial series.</span></span> <span data-ttu-id="2f1bd-173">При запуске приложения в Visual Studio появится на домашнюю страницу.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-173">When you run the application in Visual Studio, the following web Home page appears.</span></span>

![Компания Wingtip Toys - страница по умолчанию](introduction-and-overview/_static/image1.png)

<span data-ttu-id="2f1bd-175">Можно зарегистрировать в качестве нового пользователя или войдите в систему как существующего пользователя.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-175">You can register as a new user, or sign in as an existing user.</span></span> <span data-ttu-id="2f1bd-176">Верхней панели навигации содержатся ссылки на категории продуктов и их продукты из базы данных.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-176">The top navigation has links to product categories and their products from the database.</span></span>

<span data-ttu-id="2f1bd-177">При выборе **продуктов**, отображаются все доступные продукты.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-177">If you select **Products**, all available products are displayed.</span></span> 

![Компания Wingtip Toys - продукты](introduction-and-overview/_static/image2.png)

<span data-ttu-id="2f1bd-179">Если выбран определенный продукт, отображаются сведения о продукте.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-179">If you select a specific product, product details are displayed.</span></span>


![Компания Wingtip Toys — сведения о продукте](introduction-and-overview/_static/image3.png)

<span data-ttu-id="2f1bd-181">Как пользователь можно зарегистрировать и войдите, используя функциональные возможности по умолчанию шаблон веб-форм.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-181">As a user, you can register and sign in with Web Forms template default functionality.</span></span> <span data-ttu-id="2f1bd-182">Этот учебник также объясняется, как выполнить вход с использованием существующей учетной записи Gmail.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-182">This tutorial also explains how to sign in using an existing Gmail account.</span></span> <span data-ttu-id="2f1bd-183">Кроме того вы можете войти в от имени администратора, добавлять и удалять продукты из базы данных.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-183">Additionally, you can sign in as the administrator to add and remove products from the database.</span></span>

![Компания Wingtip Toys - вход](introduction-and-overview/_static/image4.png)

<span data-ttu-id="2f1bd-185">После входа качестве пользователя, можно добавить продукты в корзину для покупок и оформление заказа с использованием системы PayPal.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-185">Once you've signed in as a user, you can add products to the shopping cart and checkout with PayPal.</span></span> <span data-ttu-id="2f1bd-186">Пример приложения предназначен для работы в «песочнице» для разработчиков в PayPal.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-186">The sample application is designed to work in PayPal's developer sandbox.</span></span> <span data-ttu-id="2f1bd-187">Транзакция фактические суммы расходов не выполняется.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-187">No actual money transaction takes place.</span></span>

![Компания Wingtip Toys - Корзина для покупок](introduction-and-overview/_static/image5.png)

<span data-ttu-id="2f1bd-189">PayPal подтверждает данные учетной записи, заказа и оплаты.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-189">PayPal confirms your account, order, and payment information.</span></span>

![Компания Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

<span data-ttu-id="2f1bd-191">После возврата из PayPal, можно просмотреть и оформить заказ.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-191">After returning from PayPal, you can review and complete your order.</span></span>

![Компания Wingtip Toys — Просмотр заказа](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a><span data-ttu-id="2f1bd-193">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="2f1bd-193">Prerequisites</span></span>

<span data-ttu-id="2f1bd-194">Перед началом работы убедитесь, что на компьютере установлено следующее программное обеспечение:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-194">Before you start, make sure the following software is installed on your computer:</span></span>

- <span data-ttu-id="2f1bd-195">[Microsoft Visual Studio 2017 или Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="2f1bd-195">[Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).</span></span>

<span data-ttu-id="2f1bd-196">Платформа .NET Framework устанавливается автоматически.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-196">The .NET Framework is installed automatically.</span></span>

<span data-ttu-id="2f1bd-197">В этой серии руководств используется Microsoft Visual Studio Community 2017.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-197">This tutorial series uses Microsoft Visual Studio Community 2017.</span></span> <span data-ttu-id="2f1bd-198">Можно использовать либо, или Microsoft Visual Studio 2017 для завершения этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-198">You can use either that or Microsoft Visual Studio 2017 to complete this tutorial series.</span></span>

<span data-ttu-id="2f1bd-199">Обратите внимание на следующие особенности Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-199">Note the following about Visual Studio:</span></span>

* <span data-ttu-id="2f1bd-200">Microsoft Visual Studio 2017 и Microsoft Visual Studio Community 2017, называются *Visual Studio* данной серии учебных курсов.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-200">Microsoft Visual Studio 2017 and Microsoft Visual Studio Community 2017 are referred to as *Visual Studio* throughout this tutorial series.</span></span>

* <span data-ttu-id="2f1bd-201">Visual Studio 2017 устанавливается рядом со все более ранние версии уже установлена.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-201">Visual Studio 2017 is installed next to any older versions already installed.</span></span> <span data-ttu-id="2f1bd-202">Узлы, созданные в более ранних версиях можно открывать в Visual Studio 2017 и в предыдущих версиях по-прежнему.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-202">Sites created in earlier versions can be opened in Visual Studio 2017 and continue to open in previous versions.</span></span>

* <span data-ttu-id="2f1bd-203">При первом запуске Visual Studio, предполагается, вы выбрали *веб-разработки* параметры.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-203">The first time you started Visual Studio, it is assumed you selected the *Web Development* settings.</span></span> <span data-ttu-id="2f1bd-204">Дополнительные сведения см. в разделе [Как Выберите параметры среды веб-разработки](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="2f1bd-204">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

<span data-ttu-id="2f1bd-205">После установки необходимых компонентов, вы будете готовы приступить к созданию веб-проекта, представленные в этой серии руководств.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-205">After installing the prerequisites, you're ready to begin creating the Web project presented in this tutorial series.</span></span>

## <a name="download-the-sample-application"></a><span data-ttu-id="2f1bd-206">Загрузить пример приложения</span><span class="sxs-lookup"><span data-stu-id="2f1bd-206">Download the sample application</span></span>

 <span data-ttu-id="2f1bd-207">Можно загрузить applicatiion готовый пример, в любое время на сайте образцов MSDN:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-207">You can download the completed sample applicatiion at anytime from the MSDN Samples site:</span></span>

<span data-ttu-id="2f1bd-208">[Приступая к работе с ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span><span class="sxs-lookup"><span data-stu-id="2f1bd-208">[Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)</span></span> 

 <span data-ttu-id="2f1bd-209">Данный файл содержит следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-209">This download has the following items:</span></span>

- <span data-ttu-id="2f1bd-210">В примере приложения в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-210">The sample application in the *WingtipToys* folder.</span></span>
- <span data-ttu-id="2f1bd-211">Ресурсы, используемые для создания примера приложения в *WingtipToys-активы* папку в *WingtipToys* папки.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-211">The resources used to create the sample application in the *WingtipToys-Assets* folder in the *WingtipToys* folder.</span></span>

<span data-ttu-id="2f1bd-212">Загрузки *ZIP-файл* файл.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-212">The download is a *.zip* file.</span></span> <span data-ttu-id="2f1bd-213">Чтобы просмотреть завершенный проект, который создает этой серии руководств, найдите и выберите *C#* папку в ZIP-файл.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-213">To see the completed project that this tutorial series creates, find and select the *C#* folder in the .zip file.</span></span> <span data-ttu-id="2f1bd-214">Сохранить C# папки в папку, для работы с проектами Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-214">Save the C# folder to the folder you use to work with Visual Studio projects.</span></span> <span data-ttu-id="2f1bd-215">По умолчанию имеет папку "Проекты" Visual Studio 2017:</span><span class="sxs-lookup"><span data-stu-id="2f1bd-215">By default, the Visual Studio 2017 projects folder is:</span></span>

<span data-ttu-id="2f1bd-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span><span class="sxs-lookup"><span data-stu-id="2f1bd-216"><strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong></span></span>

<span data-ttu-id="2f1bd-217">Переименуйте ***C#*** папку ***WingtipToys***.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-217">Rename the ***C#*** folder to ***WingtipToys***.</span></span>

> [!NOTE]
> <span data-ttu-id="2f1bd-218">Если у вас уже есть папка с именем *WingtipToys* в папке проектов временно переименуйте этот существующую папку перед переименованием *C#* папку *WingtipToys*.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-218">If you already have a folder named *WingtipToys* in your Projects folder, temporarily rename that existing folder before renaming the *C#* folder to *WingtipToys*.</span></span>

<span data-ttu-id="2f1bd-219">Для запуска готового проекта, откройте *WingtipToys* папку и дважды щелкните *WingtipToys.sln* файла.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-219">To run the completed project, open the *WingtipToys* folder and double-click the *WingtipToys.sln* file.</span></span> <span data-ttu-id="2f1bd-220">Visual Studio 2017 открывает проект.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-220">Visual Studio 2017 opens the project.</span></span> <span data-ttu-id="2f1bd-221">После этого щелкните правой кнопкой мыши *Default.aspx* файл **обозревателе решений** и выберите **просмотреть в браузере**.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-221">Next, right-click the *Default.aspx* file in **Solution Explorer** and select **View In Browser**.</span></span>

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a><span data-ttu-id="2f1bd-222">Выполните тест веб-форм ASP.NET, чтобы просмотреть содержимое</span><span class="sxs-lookup"><span data-stu-id="2f1bd-222">Take a ASP.NET Web Forms quiz to review content</span></span>

<span data-ttu-id="2f1bd-223">После выполнения этой серии руководств, выполните тест, чтобы проверить свои знания и подчеркивает основные понятия.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-223">After completing the tutorial series, take a quiz to test your knowledge and reinforce key concepts.</span></span> <span data-ttu-id="2f1bd-224">Каждый вопрос содержит объяснение и ссылки на дополнительные руководства.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-224">Each question provides an explanation and links to additional guidance.</span></span>

 * [<span data-ttu-id="2f1bd-225">Веб-форм ASP.NET головоломки</span><span class="sxs-lookup"><span data-stu-id="2f1bd-225">ASP.NET Web Forms Quiz</span></span>](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a><span data-ttu-id="2f1bd-226">Поддержка руководства и комментарии</span><span class="sxs-lookup"><span data-stu-id="2f1bd-226">Tutorial support and comments</span></span>

<span data-ttu-id="2f1bd-227">Вопросы и комментарии, используйте раздел A и Q на [начало работы с веб-форм ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) образца страницы.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-227">For questions and comments, use the Q and A section included on the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span>

<span data-ttu-id="2f1bd-228">Комментарии в этой серии руководств приветствуются.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-228">Comments on this tutorial series are welcome.</span></span> <span data-ttu-id="2f1bd-229">При обновлении этой серии руководств, необходимо учитывать исправлений или предложений для улучшения выполняется все усилия.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-229">When this tutorial series is updated, every effort is made to consider corrections or suggestions for improvements.</span></span>

<span data-ttu-id="2f1bd-230">Если возникает ошибка, соответствующее сообщение об ошибке может привести к путанице, безо всякого объяснения хороший по ее устранению.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-230">If an error occurs, the corresponding error messages could be confusing, with no good explanation on how to fix it.</span></span> <span data-ttu-id="2f1bd-231">Получить справку, вы можете проверить [форумы ASP.NET](https://forums.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="2f1bd-231">For help, you can check the [ASP.NET forums](https://forums.asp.net/).</span></span> <span data-ttu-id="2f1bd-232">Хорошим источником является раздел A и Q в [начало работы с веб-форм ASP.NET 4.5 и Visual Studio 2013 — Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) образца страницы.</span><span class="sxs-lookup"><span data-stu-id="2f1bd-232">Another good source is the Q and A section in the [Getting Started with ASP.NET 4.5 Web Forms and Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) sample page.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="2f1bd-233">Вперед</span><span class="sxs-lookup"><span data-stu-id="2f1bd-233">Next</span></span>](create-the-project.md)
