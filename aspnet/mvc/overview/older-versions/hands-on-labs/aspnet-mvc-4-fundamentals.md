---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: "Принципы работы ASP.NET MVC 4 | Документы Microsoft"
author: rick-anderson
description: "Это Практическое лабораторное занятие основан на MVC (Model View Controller) Music Store, учебное приложение, которое вводятся и описываются пошаговые способы использования ASP.NET MV..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 468f6d5dabb645b1c005680dc5a1ffc4debd63b6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="45271-103">Принципы работы ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="45271-103">ASP.NET MVC 4 Fundamentals</span></span>
====================
<span data-ttu-id="45271-104">по [Web лагеря команды](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="45271-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="45271-105">Это Практическое лабораторное занятие основано на MVC (Model View Controller) Music Store, учебное приложение, которое вводятся и описываются пошаговые способы использования Visual Studio и ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="45271-105">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="45271-106">В лаборатории. Здесь простоты еще питания друг с другом с помощью этих технологий.</span><span class="sxs-lookup"><span data-stu-id="45271-106">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="45271-107">Начнем с простого приложения и будет постройте его, пока не будут полностью функционален ASP.NET MVC 4 веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="45271-107">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>
> 
> <span data-ttu-id="45271-108">Эта лаборатория работает с ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="45271-108">This Lab works with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="45271-109">Если вы хотите изучить версию ASP.NET MVC 3 учебного приложения, его можно найти в [MVC Music Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="45271-109">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="45271-110">Это Практическое лабораторное занятие предполагает, что разработчик имеет опыт в технологии веб-разработки, такие как HTML и JavaScript.</span><span class="sxs-lookup"><span data-stu-id="45271-110">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>
> 
> 
> <span data-ttu-id="45271-111">Все образцы кода и фрагменты кода включаются в Web лагеря комплект учебных материалов, доступных в [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="45271-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="45271-112">Приложение Music Store</span><span class="sxs-lookup"><span data-stu-id="45271-112">The Music Store application</span></span>

<span data-ttu-id="45271-113">Веб-приложение Music Store, который должен быть построен на протяжении этого занятия состоит из трех главных частей: покупок, извлечения и администрирования.</span><span class="sxs-lookup"><span data-stu-id="45271-113">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="45271-114">Посетители смогут Обзор альбомов по жанру, добавить в корзину альбомы, просмотрите свой выбор и наконец продолжить извлечение, чтобы войти в систему и завершить создание заказа.</span><span class="sxs-lookup"><span data-stu-id="45271-114">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="45271-115">Кроме того хранилище Администраторы получат доступ для управления доступными альбомы, а также их основные свойства.</span><span class="sxs-lookup"><span data-stu-id="45271-115">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="45271-116">![Экраны Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "экраны Music Store")</span><span class="sxs-lookup"><span data-stu-id="45271-116">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="45271-117">*Экраны Music Store*</span><span class="sxs-lookup"><span data-stu-id="45271-117">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="45271-118">Essentials ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="45271-118">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="45271-119">Приложение Music Store будет построено с помощью **модель View Controller (MVC)**, архитектуре, которая разделяет приложение на три основных компонента:</span><span class="sxs-lookup"><span data-stu-id="45271-119">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="45271-120">**Модели**: объекты моделей являются частями приложения, реализующими доменную логику.</span><span class="sxs-lookup"><span data-stu-id="45271-120">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="45271-121">Многие модели объектов также получить и сохраняют состояние модели в базе данных.</span><span class="sxs-lookup"><span data-stu-id="45271-121">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="45271-122">**Представления:** представления служат для отображения приложения пользовательского интерфейса (UI).</span><span class="sxs-lookup"><span data-stu-id="45271-122">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="45271-123">Как правило этот пользовательский Интерфейс создается на основе данных модели.</span><span class="sxs-lookup"><span data-stu-id="45271-123">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="45271-124">Примером такого представления изменения альбомов, содержит текстовые поля и раскрывающегося списка, исходя из текущего состояния объекта альбома.</span><span class="sxs-lookup"><span data-stu-id="45271-124">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="45271-125">**Контроллеры:** контроллеров служат для управления взаимодействием с пользователем, обработки модели, а также Выбор представления для отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="45271-125">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="45271-126">В приложении MVC представления только отображают сведения; контроллер обрабатывает и реагирует на ввод данных пользователем и взаимодействия.</span><span class="sxs-lookup"><span data-stu-id="45271-126">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="45271-127">Шаблон MVC позволяет создавать приложения, различные аспекты приложения (логика ввода, бизнес-логика и логика пользовательского интерфейса), обеспечивая при этом слабую связь между этими элементами.</span><span class="sxs-lookup"><span data-stu-id="45271-127">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="45271-128">Это разделение позволяет работать со сложными структурами при построении приложения, как он позволяет сосредоточиться на один аспект реализации одновременно.</span><span class="sxs-lookup"><span data-stu-id="45271-128">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="45271-129">Кроме того шаблон MVC упрощает для тестирования приложений, также рекомендовать использование управляемой тестами разработки (TDD) для создания приложений.</span><span class="sxs-lookup"><span data-stu-id="45271-129">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="45271-130">**ASP.NET MVC** framework представляет собой альтернативу шаблон веб-форм ASP.NET для создания приложений веб-узла ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="45271-130">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="45271-131">**ASP.NET MVC** framework является легковесной платформой, (как в случае с веб формы приложения) интегрирована с существующими функциями ASP.NET, такие как главные страницы и на основе членства Проверка подлинности, чтобы получить все возможности платформы core .NET framework.</span><span class="sxs-lookup"><span data-stu-id="45271-131">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="45271-132">Это полезно, если вы уже знакомы с веб-форм ASP.NET, так как все библиотеки, которые уже используются в ASP.NET MVC 4, также доступны.</span><span class="sxs-lookup"><span data-stu-id="45271-132">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="45271-133">Кроме того слабую связь между основными компонентами приложения MVC также облегчает параллельную разработку.</span><span class="sxs-lookup"><span data-stu-id="45271-133">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="45271-134">Для экземпляра один разработчик может создавать представление, второй разработчик может создавать логику контроллера и третий разработчику сосредоточиться на бизнес-логики в модель.</span><span class="sxs-lookup"><span data-stu-id="45271-134">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="45271-135">Цели</span><span class="sxs-lookup"><span data-stu-id="45271-135">Objectives</span></span>

<span data-ttu-id="45271-136">В этой лаборатории практических вы узнаете, как:</span><span class="sxs-lookup"><span data-stu-id="45271-136">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="45271-137">Создание приложения ASP.NET MVC с нуля, в зависимости от приложения Music Store учебника</span><span class="sxs-lookup"><span data-stu-id="45271-137">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="45271-138">Добавить контроллеры для обработки URL-адреса на домашнюю страницу сайта, а также для просмотра ее основные функциональные возможности</span><span class="sxs-lookup"><span data-stu-id="45271-138">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="45271-139">Добавить представление для настройки содержимого, отображаемого вместе с его стиля</span><span class="sxs-lookup"><span data-stu-id="45271-139">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="45271-140">Добавление модели классов для хранения и управления логику данных и домен</span><span class="sxs-lookup"><span data-stu-id="45271-140">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="45271-141">Использовать шаблон модель представлений для передачи данных из действия контроллера шаблонов представлений</span><span class="sxs-lookup"><span data-stu-id="45271-141">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="45271-142">Просмотр нового шаблона ASP.NET MVC 4 для веб-приложений</span><span class="sxs-lookup"><span data-stu-id="45271-142">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="45271-143">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="45271-143">Prerequisites</span></span>

<span data-ttu-id="45271-144">Необходимо иметь следующие элементы для этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="45271-145">[Visual Studio 2012 Express для Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (чтение [приложение A](#AppendixA) инструкции по его установке)</span><span class="sxs-lookup"><span data-stu-id="45271-145">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="45271-146">Установка</span><span class="sxs-lookup"><span data-stu-id="45271-146">Setup</span></span>

<span data-ttu-id="45271-147">**Установка фрагменты кода**</span><span class="sxs-lookup"><span data-stu-id="45271-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="45271-148">Для удобства большая часть кода, который вы планируете управлять вдоль этой лаборатории доступна как фрагменты кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="45271-149">Чтобы установить фрагменты кода запуска **.\Source\Setup\CodeSnippets.vsi** файла.</span><span class="sxs-lookup"><span data-stu-id="45271-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="45271-150">Если вы не знакомы с фрагментов кода Visual Studio и хотите узнать о способах их использования, можно ссылаться в приложение из этого документа &quot; [фрагменты кода с помощью приложение C:](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="45271-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="45271-151">Упражнения</span><span class="sxs-lookup"><span data-stu-id="45271-151">Exercises</span></span>

<span data-ttu-id="45271-152">Это Практическое лабораторное занятие состоит в выполнении следующих упражнений.</span><span class="sxs-lookup"><span data-stu-id="45271-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="45271-153">Упражнение 1: Создание проекта веб-приложения MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="45271-153">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="45271-154">Упражнение 2: Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="45271-154">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="45271-155">Упражнение 3: Передача параметров контроллера</span><span class="sxs-lookup"><span data-stu-id="45271-155">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="45271-156">Упражнение 4: Создание представления</span><span class="sxs-lookup"><span data-stu-id="45271-156">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="45271-157">Упражнение 5: Создание модели представления</span><span class="sxs-lookup"><span data-stu-id="45271-157">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="45271-158">Упражнение 6: Использование параметров в представлении</span><span class="sxs-lookup"><span data-stu-id="45271-158">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="45271-159">Упражнение 7. Знакомство с функциями новый шаблон ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="45271-159">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="45271-160">Каждого упражнения сопровождается **окончания** папку, содержащую полученное в результате решение, должен быть получен после завершения выполнения упражнений.</span><span class="sxs-lookup"><span data-stu-id="45271-160">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="45271-161">Это решение можно использовать как руководство, если вам нужна дополнительная помощь при выполнении упражнений.</span><span class="sxs-lookup"><span data-stu-id="45271-161">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="45271-162">Предполагаемое время для выполнения этого занятия: **60 минут**.</span><span class="sxs-lookup"><span data-stu-id="45271-162">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="45271-163">Упражнение 1: Создание проекта веб-приложения MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="45271-163">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="45271-164">В этом упражнении вы узнаете, как создать приложение ASP.NET MVC в Visual Studio 2012 Express для Web, а также их организация в главную папку.</span><span class="sxs-lookup"><span data-stu-id="45271-164">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="45271-165">Кроме того вы узнаете, как добавить новый контроллер и сделать его отображения в простую строку на домашней странице приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-165">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="45271-166">Задача 1. Создание проекта веб-приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="45271-166">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="45271-167">В этой задаче вы создадите пустой проект приложения ASP.NET MVC с помощью шаблона MVC Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-167">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="45271-168">Запуск **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="45271-168">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="45271-169">В меню **Файл** выберите пункт **Создать проект**.</span><span class="sxs-lookup"><span data-stu-id="45271-169">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="45271-170">В **новый проект** диалогового окна выберите **веб-приложение ASP.NET MVC 4** тип, расположенный в узле проекта **Visual C#** **Web** шаблона список.</span><span class="sxs-lookup"><span data-stu-id="45271-170">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="45271-171">Изменение **имя** для *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="45271-171">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="45271-172">Задание расположения в новое решение **начать** папки в этом упражнении исходной папки, например **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR ХОЛЬ путь]**.</span><span class="sxs-lookup"><span data-stu-id="45271-172">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="45271-173">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="45271-173">Click **OK**.</span></span>

    <span data-ttu-id="45271-174">![Создание нового проекта-диалоговое окно](aspnet-mvc-4-fundamentals/_static/image2.png "Создание нового проекта-диалоговое окно")</span><span class="sxs-lookup"><span data-stu-id="45271-174">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="45271-175">*Создание нового проекта-диалоговое окно*</span><span class="sxs-lookup"><span data-stu-id="45271-175">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="45271-176">В **нового проекта ASP.NET MVC 4** диалогового окна выберите **основные** шаблона и убедитесь, что **обработчик представлений** выбранный **Razor**.</span><span class="sxs-lookup"><span data-stu-id="45271-176">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="45271-177">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="45271-177">Click **OK**.</span></span>

    <span data-ttu-id="45271-178">![Диалоговое окно "новый проект ASP.NET MVC 4"](aspnet-mvc-4-fundamentals/_static/image3.png "новое диалоговое окно проекта ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="45271-178">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="45271-179">*Диалоговое окно "новый проект ASP.NET MVC 4"*</span><span class="sxs-lookup"><span data-stu-id="45271-179">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="45271-180">Задача 2 - изучение структуры решения</span><span class="sxs-lookup"><span data-stu-id="45271-180">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="45271-181">Платформы ASP.NET MVC входит шаблон проекта Visual Studio, которая помогает создавать веб-приложения, поддерживающие шаблон MVC.</span><span class="sxs-lookup"><span data-stu-id="45271-181">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="45271-182">Этот шаблон создает новое приложение веб-ASP.NET MVC с необходимые папки, шаблоны элементов и записи файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="45271-182">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="45271-183">В этой задаче вы проверяете структуры решения понимание элементов, которые участвуют и их связи.</span><span class="sxs-lookup"><span data-stu-id="45271-183">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="45271-184">Поскольку платформа ASP.NET MVC по умолчанию использует следующие папки включены в ASP.NET MVC application &quot;соглашение по конфигурации&quot; подход и делает некоторые предположения по умолчанию основании наименования папок соглашения.</span><span class="sxs-lookup"><span data-stu-id="45271-184">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="45271-185">После создания проекта Просмотрите структуру папок, созданной в обозревателе решений на правой стороне.</span><span class="sxs-lookup"><span data-stu-id="45271-185">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="45271-186">![Структура папки MVC ASP.NET в обозревателе решений](aspnet-mvc-4-fundamentals/_static/image4.png "структура папки MVC ASP.NET в обозревателе решений")</span><span class="sxs-lookup"><span data-stu-id="45271-186">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="45271-187">*Структура папки MVC ASP.NET в обозревателе решений*</span><span class="sxs-lookup"><span data-stu-id="45271-187">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

    1. <span data-ttu-id="45271-188">**Контроллеры**.</span><span class="sxs-lookup"><span data-stu-id="45271-188">**Controllers**.</span></span> <span data-ttu-id="45271-189">Эта папка будет содержать классы контроллера.</span><span class="sxs-lookup"><span data-stu-id="45271-189">This folder will contain the controller classes.</span></span> <span data-ttu-id="45271-190">В приложении на основе MVC контроллеры отвечают за обработку взаимодействия с конечным пользователем, работы с моделью и в конечном счете, Выбор представления для отображения пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="45271-190">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

        > [!NOTE]
        > <span data-ttu-id="45271-191">MVC требует имена контроллеров с присвоением &quot;контроллера&quot;-например, HomeController, LoginController или ProductController.</span><span class="sxs-lookup"><span data-stu-id="45271-191">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
    2. <span data-ttu-id="45271-192">**Модели**.</span><span class="sxs-lookup"><span data-stu-id="45271-192">**Models**.</span></span> <span data-ttu-id="45271-193">Эта папка предоставляется для классов, которые представляют модель приложения для веб-приложения MVC.</span><span class="sxs-lookup"><span data-stu-id="45271-193">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="45271-194">Это обычно содержит код, который определяет объекты и логику взаимодействия с хранилищем данных.</span><span class="sxs-lookup"><span data-stu-id="45271-194">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="45271-195">Как правило сами объекты модели будет в отдельных библиотеках классов.</span><span class="sxs-lookup"><span data-stu-id="45271-195">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="45271-196">Тем не менее при создании нового приложения может включить классы и переместить их в отдельные библиотеки на более позднем этапе цикла разработки.</span><span class="sxs-lookup"><span data-stu-id="45271-196">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
    3. <span data-ttu-id="45271-197">**Представления**.</span><span class="sxs-lookup"><span data-stu-id="45271-197">**Views**.</span></span> <span data-ttu-id="45271-198">Эта папка — рекомендуемое расположение для представлений, компоненты, отвечает за отображение пользовательского интерфейса приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-198">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="45271-199">Представления использования файлов .aspx, .ascx, cshtml и .master, помимо других файлов, которые относятся к визуализации представления.</span><span class="sxs-lookup"><span data-stu-id="45271-199">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="45271-200">Папка Views содержит папки для каждого контроллера; папка называется с префиксом имени контроллера.</span><span class="sxs-lookup"><span data-stu-id="45271-200">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="45271-201">Например, если существует контроллер с именем **HomeController**, папку Views будет содержать папка с именем Home.</span><span class="sxs-lookup"><span data-stu-id="45271-201">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="45271-202">По умолчанию при загрузке представления платформой ASP.NET MVC выполняется поиск ASPX-файл с именем запрошенное представление в папке Views\controllerName (**представления [Имя_контроллера] [действие] .aspx**) или (**представления [Имя_контроллера] [Действие] .cshtml**) для представлений Razor.</span><span class="sxs-lookup"><span data-stu-id="45271-202">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-203">В дополнение к перечисленным выше папкам веб-приложение MVC использует **Global.asax** файл для задания глобальной маршрутизации URL-адрес по умолчанию и использует **Web.config** файл, чтобы настроить приложение.</span><span class="sxs-lookup"><span data-stu-id="45271-203">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="45271-204">Задача 3. Добавление HomeController</span><span class="sxs-lookup"><span data-stu-id="45271-204">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="45271-205">В приложениях ASP.NET, не использующих платформу MVC взаимодействие с пользователем организуется вокруг страницы и создания и обработки событий из этих страниц.</span><span class="sxs-lookup"><span data-stu-id="45271-205">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="45271-206">Напротив взаимодействие пользователей с приложениями ASP.NET MVC основаны на контроллерах и методах их действия.</span><span class="sxs-lookup"><span data-stu-id="45271-206">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="45271-207">С другой стороны платформа ASP.NET MVC сопоставляет URL-адреса с классами, называются контроллеров.</span><span class="sxs-lookup"><span data-stu-id="45271-207">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="45271-208">Контроллеры обрабатывают входящие запросы, вводимые пользователями данные и взаимодействия, реализуют необходимую логику приложений и определения ответа для отправки обратно клиенту (отображения HTML-кода, загрузите файл, перенаправление на другой URL-адрес, и т. д.).</span><span class="sxs-lookup"><span data-stu-id="45271-208">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="45271-209">В случае отображения HTML, класс контроллера обычно вызывает отдельный компонент представления для создания разметки HTML для запроса.</span><span class="sxs-lookup"><span data-stu-id="45271-209">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="45271-210">В приложении MVC представления только отображают сведения; контроллер обрабатывает и реагирует на ввод данных пользователем и взаимодействия.</span><span class="sxs-lookup"><span data-stu-id="45271-210">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="45271-211">В этой задаче вы добавите класс контроллера, который будет обрабатывать URL-адреса на домашнюю страницу сайта Music Store.</span><span class="sxs-lookup"><span data-stu-id="45271-211">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="45271-212">Щелкните правой кнопкой мыши **контроллеров** папки в обозревателе решений выберите **добавить** и затем **контроллера** команды:</span><span class="sxs-lookup"><span data-stu-id="45271-212">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="45271-213">![Добавление команды контроллера](aspnet-mvc-4-fundamentals/_static/image5.png "добавьте команду контроллера")</span><span class="sxs-lookup"><span data-stu-id="45271-213">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="45271-214">*Добавьте команду контроллера*</span><span class="sxs-lookup"><span data-stu-id="45271-214">*Add Controller Command*</span></span>
2. <span data-ttu-id="45271-215">**Добавить контроллер** откроется диалоговое окно.</span><span class="sxs-lookup"><span data-stu-id="45271-215">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="45271-216">Назовите контроллер *HomeController* и нажмите клавишу **добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-216">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="45271-217">![Диалоговое окно добавления контроллера](aspnet-mvc-4-fundamentals/_static/image6.png "диалоговое окно добавления контроллера")</span><span class="sxs-lookup"><span data-stu-id="45271-217">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="45271-218">*Диалоговое окно добавления контроллера*</span><span class="sxs-lookup"><span data-stu-id="45271-218">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="45271-219">Файл **HomeController.cs** создается в **контроллеров** папки.</span><span class="sxs-lookup"><span data-stu-id="45271-219">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="45271-220">Чтобы получить **HomeController** возвращают строку не на его индекс действия, замените **индекс** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="45271-220">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="45271-221">(Фрагмент - кода *основы ASP.NET MVC 4 - индекс HomeController сервера Ex1*)</span><span class="sxs-lookup"><span data-stu-id="45271-221">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="45271-222">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="45271-222">Task 4 - Running the Application</span></span>

<span data-ttu-id="45271-223">В этой задаче будет опробовать приложение в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="45271-223">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="45271-224">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-224">Press **F5** to run the Application.</span></span> <span data-ttu-id="45271-225">Компиляции проекта и запускает локальный веб-сервер IIS.</span><span class="sxs-lookup"><span data-stu-id="45271-225">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="45271-226">Локальный веб-сервер IIS автоматически открывается веб-браузер, указывающий на URL-адрес веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="45271-226">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="45271-227">![Приложение, работающее в браузере](aspnet-mvc-4-fundamentals/_static/image7.png "приложение, работающее в браузере")</span><span class="sxs-lookup"><span data-stu-id="45271-227">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="45271-228">*Приложение, работающее в браузере*</span><span class="sxs-lookup"><span data-stu-id="45271-228">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-229">Локальный сервер IIS будет выполняться веб-сайт на случайный бесплатный номер порта.</span><span class="sxs-lookup"><span data-stu-id="45271-229">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="45271-230">На приведенном выше рисунке, что сайт работает в `http://localhost:50103/`, поэтому он использует порт 50103.</span><span class="sxs-lookup"><span data-stu-id="45271-230">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="45271-231">Этот номер порта может меняться.</span><span class="sxs-lookup"><span data-stu-id="45271-231">Your port number may vary.</span></span>
2. <span data-ttu-id="45271-232">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="45271-232">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="45271-233">Упражнение 2: Создание контроллера</span><span class="sxs-lookup"><span data-stu-id="45271-233">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="45271-234">В этом упражнении вы узнаете, как обновить контроллер для реализации простой функциональности приложение Music Store.</span><span class="sxs-lookup"><span data-stu-id="45271-234">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="45271-235">Этот контроллер будет определяют методы действий для обработки каждого из следующих определенные запросы:</span><span class="sxs-lookup"><span data-stu-id="45271-235">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="45271-236">Страницы список жанров музыки в Music Store</span><span class="sxs-lookup"><span data-stu-id="45271-236">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="45271-237">Страница обзора, в котором перечислены все альбомы для определенного жанра</span><span class="sxs-lookup"><span data-stu-id="45271-237">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="45271-238">Веб-страницу и отображает сведения о конкретных музыкальный альбом</span><span class="sxs-lookup"><span data-stu-id="45271-238">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="45271-239">Для этого упражнения области эти действия просто возвращают строку к этому моменту.</span><span class="sxs-lookup"><span data-stu-id="45271-239">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="45271-240">Задача 1. Добавление нового класса StoreController</span><span class="sxs-lookup"><span data-stu-id="45271-240">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="45271-241">В этой задаче будет добавлен новый контроллер.</span><span class="sxs-lookup"><span data-stu-id="45271-241">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="45271-242">Если это еще не открыто, запустите **VS Express для Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="45271-242">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="45271-243">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="45271-243">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="45271-244">В диалоговом окне Открытие проекта найдите **Source\Ex02 CreatingAController\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="45271-244">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="45271-245">Кроме того вы можете продолжить с решением, полученный после завершения работы в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="45271-245">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="45271-246">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="45271-246">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="45271-247">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="45271-247">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="45271-248">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="45271-248">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="45271-249">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="45271-249">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-250">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-250">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="45271-251">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-251">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="45271-252">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-252">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="45271-253">Добавьте новый контроллер.</span><span class="sxs-lookup"><span data-stu-id="45271-253">Add the new controller.</span></span> <span data-ttu-id="45271-254">Для этого щелкните правой кнопкой мыши **контроллеров** папки в обозревателе решений выберите **добавить** и затем **контроллера** команды.</span><span class="sxs-lookup"><span data-stu-id="45271-254">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="45271-255">Изменение **имя контроллера** для *StoreController*и нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-255">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="45271-256">![Диалоговое окно добавления контроллера](aspnet-mvc-4-fundamentals/_static/image8.png "диалоговое окно добавления контроллера")</span><span class="sxs-lookup"><span data-stu-id="45271-256">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="45271-257">*Диалоговое окно добавления контроллера*</span><span class="sxs-lookup"><span data-stu-id="45271-257">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="45271-258">Задача 2 - изменение StoreController действия</span><span class="sxs-lookup"><span data-stu-id="45271-258">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="45271-259">В этой задаче предстоит изменить методы контроллера, которые вызываются **действия**.</span><span class="sxs-lookup"><span data-stu-id="45271-259">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="45271-260">Операции отвечают за обработку запросов URL-адрес и определение содержимого, который будет отправлен обратно в браузер или пользователь, вызвавший URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="45271-260">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="45271-261">**StoreController** класс уже имеет **индекс** метод.</span><span class="sxs-lookup"><span data-stu-id="45271-261">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="45271-262">Он будет использовать далее в этой лаборатории для реализации страницу, где перечислены все жанров музыкальном магазине.</span><span class="sxs-lookup"><span data-stu-id="45271-262">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="45271-263">Теперь, просто замените **индекс** метод следующим кодом, который возвращает строку, &quot;Hello из Store.Index()&quot;:</span><span class="sxs-lookup"><span data-stu-id="45271-263">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="45271-264">(Фрагмент - кода *основы ASP.NET MVC 4 - индекс StoreController Ex2*)</span><span class="sxs-lookup"><span data-stu-id="45271-264">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="45271-265">Добавить **Обзор** и **сведения** методы.</span><span class="sxs-lookup"><span data-stu-id="45271-265">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="45271-266">Чтобы сделать это, добавьте следующий код в **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="45271-266">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="45271-267">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="45271-267">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="45271-268">Задача 3. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="45271-268">Task 3 - Running the Application</span></span>

<span data-ttu-id="45271-269">В этой задаче будет опробовать приложение в веб-браузере.</span><span class="sxs-lookup"><span data-stu-id="45271-269">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="45271-270">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-270">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="45271-271">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-271">The project starts in the **Home** page.</span></span> <span data-ttu-id="45271-272">Измените URL-адрес, чтобы проверить реализацию каждого действия.</span><span class="sxs-lookup"><span data-stu-id="45271-272">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="45271-273">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="45271-273">**/Store**.</span></span> <span data-ttu-id="45271-274">Вы увидите  **&quot;Hello из Store.Index()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="45271-274">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="45271-275">**/ Store/обзора**.</span><span class="sxs-lookup"><span data-stu-id="45271-275">**/Store/Browse**.</span></span> <span data-ttu-id="45271-276">Вы увидите  **&quot;Hello из Store.Browse()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="45271-276">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="45271-277">**/ / Сведения о хранилище**.</span><span class="sxs-lookup"><span data-stu-id="45271-277">**/Store/Details**.</span></span> <span data-ttu-id="45271-278">Вы увидите  **&quot;Hello из Store.Details()&quot;**.</span><span class="sxs-lookup"><span data-stu-id="45271-278">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="45271-279">![Просмотр StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "просмотра StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="45271-279">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="45271-280">*Просмотр /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="45271-280">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="45271-281">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="45271-281">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="45271-282">Упражнение 3: Передача параметров контроллера</span><span class="sxs-lookup"><span data-stu-id="45271-282">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="45271-283">До этого момента в на контроллерах возврат константных строк.</span><span class="sxs-lookup"><span data-stu-id="45271-283">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="45271-284">В этом упражнении вы узнаете, как для передачи параметров в контроллере с помощью URL-адреса и строки запроса, а затем выполнить метод действия отвечает с текстом в браузере.</span><span class="sxs-lookup"><span data-stu-id="45271-284">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="45271-285">Задача 1. Добавление параметра жанр StoreController</span><span class="sxs-lookup"><span data-stu-id="45271-285">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="45271-286">В этой задаче будет использоваться **querystring** для отправки параметров **Обзор** метода действия в **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="45271-286">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="45271-287">Если это еще не открыто, запустите **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="45271-287">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="45271-288">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="45271-288">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="45271-289">В диалоговом окне Открытие проекта найдите **Source\Ex03 PassingParametersToAController\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="45271-289">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="45271-290">Кроме того вы можете продолжить с решением, полученный после завершения работы в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="45271-290">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="45271-291">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="45271-291">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="45271-292">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="45271-292">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="45271-293">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="45271-293">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="45271-294">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="45271-294">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-295">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-295">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="45271-296">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-296">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="45271-297">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-297">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="45271-298">Откройте **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-298">Open **StoreController** class.</span></span> <span data-ttu-id="45271-299">Для этого в **обозревателе решений**, разверните **контроллеров** папку и дважды щелкните **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="45271-299">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="45271-300">Изменение **Обзор** метод, добавление строковый параметр для запроса для конкретных жанра.</span><span class="sxs-lookup"><span data-stu-id="45271-300">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="45271-301">ASP.NET MVC будет автоматически передавать все строки запроса или отправки формы параметры с именем **жанр** этого метода действие при вызове.</span><span class="sxs-lookup"><span data-stu-id="45271-301">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="45271-302">Для этого замените **Обзор** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="45271-302">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="45271-303">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="45271-303">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="45271-304">Вы используете **HttpUtility.HtmlEncode** вспомогательный метод для добавления Javascript в представление с помощью ссылки, например не позволяет пользователям   **/Store/обзора? Жанр =&lt;сценарий&gt;window.location= "[http://hackersite.com](http://hackersite.com)"&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="45271-304">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
    > 
    > <span data-ttu-id="45271-305">Дополнительные пояснения посетите [в этой статье msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="45271-305">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="45271-306">Задача 2 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="45271-306">Task 2 - Running the Application</span></span>

<span data-ttu-id="45271-307">В этой задаче будет испытать приложение в веб-браузере и использовать **жанр** параметра.</span><span class="sxs-lookup"><span data-stu-id="45271-307">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="45271-308">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-308">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="45271-309">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-309">The project starts in the **Home** page.</span></span> <span data-ttu-id="45271-310">Изменить URL-адрес *, хранения, Обзор? Жанр = Disco* чтобы убедиться, что действие получает параметр жанра.</span><span class="sxs-lookup"><span data-stu-id="45271-310">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="45271-311">![Просмотр StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "просмотра StoreBrowseGenre = Disco")</span><span class="sxs-lookup"><span data-stu-id="45271-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="45271-312">*Просмотр /Store/Browse? Жанр = Disco*</span><span class="sxs-lookup"><span data-stu-id="45271-312">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="45271-313">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="45271-313">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="45271-314">Задача 3. Добавление параметра Id, URL-адрес</span><span class="sxs-lookup"><span data-stu-id="45271-314">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="45271-315">В этой задаче будет использоваться **URL-адрес** для передачи **идентификатор** параметр **сведения** метод действия **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="45271-315">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="45271-316">ASP.NET MVC по умолчанию используется соглашение о маршрутизации обрабатывать сегмента URL-адрес после имени метода действия, как параметр с именем **идентификатор**. Если метод действия имеет параметр с именем Id, затем ASP.NET MVC будет автоматически передавать сегмента URL-адреса вам как параметр.</span><span class="sxs-lookup"><span data-stu-id="45271-316">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="45271-317">В URL-АДРЕСЕ **хранилища, сведения и 5**, **идентификатор** будет интерпретироваться как **5**.</span><span class="sxs-lookup"><span data-stu-id="45271-317">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="45271-318">Изменение **сведения** метод **StoreController**, добавление **int** параметр с именем **идентификатор**. Для этого замените **сведения** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="45271-318">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="45271-319">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="45271-319">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="45271-320">Задача 4. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="45271-320">Task 4 - Running the Application</span></span>

<span data-ttu-id="45271-321">В этой задаче будет испытать приложение в веб-браузере и использовать **идентификатор** параметра.</span><span class="sxs-lookup"><span data-stu-id="45271-321">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="45271-322">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-322">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="45271-323">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-323">The project starts in the **Home** page.</span></span> <span data-ttu-id="45271-324">Изменить URL-адрес */Store/Details/5* чтобы убедиться, что действие получает идентификатор параметра.</span><span class="sxs-lookup"><span data-stu-id="45271-324">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="45271-325">![Просмотр StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "просмотра StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="45271-325">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="45271-326">*Просмотр /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="45271-326">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="45271-327">Упражнение 4: Создание представления</span><span class="sxs-lookup"><span data-stu-id="45271-327">Exercise 4: Creating a View</span></span>

<span data-ttu-id="45271-328">Пока возврат строк из действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="45271-328">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="45271-329">Несмотря на то, что это удобный способ понимать работу контроллеров, это не как к реальной веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="45271-329">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="45271-330">Представления — компоненты, которые больше подходят для создания HTML обратно в браузер с использованием файлов шаблонов.</span><span class="sxs-lookup"><span data-stu-id="45271-330">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="45271-331">В этом упражнении вы узнаете, как добавить главную страницу макета для настройки шаблонов для распространенных HTML-содержимого, таблицы стилей, чтобы улучшить внешний вид сайта, и наконец Просмотр шаблона для включения HomeController для возврата HTML.</span><span class="sxs-lookup"><span data-stu-id="45271-331">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="45271-332">Задача 1. Изменение файла \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="45271-332">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="45271-333">Файл **~/Views/Shared/\_layout.cshtml** позволяет настроить шаблон для HTML, общие для всего веб-сайта.</span><span class="sxs-lookup"><span data-stu-id="45271-333">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="45271-334">В этой задаче вы добавите Главная страница макета с заголовком общих со ссылками в область домашней страницы и хранилища.</span><span class="sxs-lookup"><span data-stu-id="45271-334">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="45271-335">Если это еще не открыто, запустите **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="45271-335">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="45271-336">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="45271-336">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="45271-337">В диалоговом окне Открытие проекта найдите **Source\Ex04 CreatingAView\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="45271-337">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="45271-338">Кроме того вы можете продолжить с решением, полученный после завершения работы в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="45271-338">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="45271-339">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="45271-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="45271-340">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="45271-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="45271-341">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="45271-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="45271-342">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="45271-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-343">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="45271-344">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="45271-345">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="45271-346">Файл  **\_layout.cshtml** содержит контейнер HTML-разметку для всех страниц на сайте.</span><span class="sxs-lookup"><span data-stu-id="45271-346">The file **\_layout.cshtml** contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="45271-347">Он включает  **&lt;html&gt;**  элемент для HTML-ответа, а также  **&lt;head&gt;**  и  **&lt;текста&gt;**  элементы.</span><span class="sxs-lookup"><span data-stu-id="45271-347">It includes the **&lt;html&gt;** element for the HTML response, as well as the **&lt;head&gt;** and **&lt;body&gt;** elements.</span></span> <span data-ttu-id="45271-348">**@RenderBody()** в HTML-код текста определить областей представление шаблоны смогут наполнить динамического содержимого.</span><span class="sxs-lookup"><span data-stu-id="45271-348">**@RenderBody()** within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
<span data-ttu-id="45271-349">(C#)</span><span class="sxs-lookup"><span data-stu-id="45271-349">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="45271-350">Добавьте общие заголовок со ссылками в область домашней страницы и хранилища на всех страницах на сайте.</span><span class="sxs-lookup"><span data-stu-id="45271-350">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="45271-351">Чтобы сделать это, добавьте следующий код ниже &lt;текст&gt; инструкции.</span><span class="sxs-lookup"><span data-stu-id="45271-351">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
<span data-ttu-id="45271-352">(C#)</span><span class="sxs-lookup"><span data-stu-id="45271-352">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="45271-353">Включить div для отрисовки текста части каждой страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-353">Include a div to render the body section of each page.</span></span> <span data-ttu-id="45271-354">Замените  **@RenderBody()** higlighted следующим кодом: (C#)</span><span class="sxs-lookup"><span data-stu-id="45271-354">Replace **@RenderBody()** with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="45271-355">Знаете ли вы?</span><span class="sxs-lookup"><span data-stu-id="45271-355">Did you know?</span></span> <span data-ttu-id="45271-356">Visual Studio 2012 имеет фрагменты, которые позволяют легко добавлять часто используемый код в HTML, файлы кода и многое другое!</span><span class="sxs-lookup"><span data-stu-id="45271-356">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="45271-357">Проверим, введя  **&lt;div&gt;**  и нажав клавишу **ВКЛАДКЕ** дважды, чтобы вставить полный **div** тег.</span><span class="sxs-lookup"><span data-stu-id="45271-357">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="45271-358">Задача 2 - Добавление таблицы стилей CSS</span><span class="sxs-lookup"><span data-stu-id="45271-358">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="45271-359">Шаблон пустого проекта содержит очень упрощенный CSS-файл, включающий только стили, используемые для отображения базовых форм и проверки сообщений.</span><span class="sxs-lookup"><span data-stu-id="45271-359">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="45271-360">Используется дополнительных CSS и изображений (потенциально предоставляется конструктором), чтобы улучшить внешний вид веб-узла.</span><span class="sxs-lookup"><span data-stu-id="45271-360">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="45271-361">В этой задаче вы добавите в таблице стилей CSS, определять стили сайта.</span><span class="sxs-lookup"><span data-stu-id="45271-361">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="45271-362">CSS-файл и изображения включены в **Source\Assets\Content** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-362">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="45271-363">Чтобы добавить их в приложение, перетащите их содержимое из **Проводник** в окно **обозревателе решений** в Visual Studio Express для Web, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="45271-363">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="45271-364">![Перетаскивание содержимое стиль](aspnet-mvc-4-fundamentals/_static/image12.png "перетаскивания содержимое стиля")</span><span class="sxs-lookup"><span data-stu-id="45271-364">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="45271-365">*Перетаскивание содержимое стиля*</span><span class="sxs-lookup"><span data-stu-id="45271-365">*Dragging style contents*</span></span>
2. <span data-ttu-id="45271-366">Предупреждение появится диалоговое окно, запрашивающее подтверждение для замены **Site.css** файлов и некоторых существующих образов.</span><span class="sxs-lookup"><span data-stu-id="45271-366">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="45271-367">Проверьте **применить ко всем элементам** и нажмите кнопку **Да**.</span><span class="sxs-lookup"><span data-stu-id="45271-367">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="45271-368">Задача 3. Добавление шаблона представления</span><span class="sxs-lookup"><span data-stu-id="45271-368">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="45271-369">В этой задаче вы добавите Просмотр шаблона для создания HTML-ответа, который будет использовать главную страницу макета и добавлены CSS в этом упражнении.</span><span class="sxs-lookup"><span data-stu-id="45271-369">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="45271-370">Чтобы использовать шаблон представления при просмотре на домашнюю страницу, сначала необходимо указать, что вместо возвращения строки, **индекс HomeController** метод будет возвращать **представление**.</span><span class="sxs-lookup"><span data-stu-id="45271-370">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="45271-371">Откройте **HomeController** и измените его **индекс** метод для возврата **ActionResult**, и вернуть его **View()**.</span><span class="sxs-lookup"><span data-stu-id="45271-371">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="45271-372">(Фрагмент - кода *основы ASP.NET MVC 4 - индекс HomeController Ex4*)</span><span class="sxs-lookup"><span data-stu-id="45271-372">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="45271-373">Теперь необходимо добавить соответствующий шаблон представления.</span><span class="sxs-lookup"><span data-stu-id="45271-373">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="45271-374">Чтобы сделать это, **щелкните правой кнопкой мыши** внутри **индекс** метода действия и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="45271-374">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="45271-375">Данная команда откроет **добавить представление** диалогового окна.</span><span class="sxs-lookup"><span data-stu-id="45271-375">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="45271-376">![Добавление представления из метода индекс](aspnet-mvc-4-fundamentals/_static/image13.png "Добавление представления из метода индекса")</span><span class="sxs-lookup"><span data-stu-id="45271-376">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="45271-377">*Добавление представления из метода индекса*</span><span class="sxs-lookup"><span data-stu-id="45271-377">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="45271-378">**Добавить представление** появится диалоговое окно для создания файла шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-378">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="45271-379">По умолчанию это диалоговое окно предварительно определяет имя шаблона представления, чтобы он соответствовал метод действия, который будет его использовать.</span><span class="sxs-lookup"><span data-stu-id="45271-379">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="45271-380">Поскольку использовался **добавить представление** контекстного меню в **индекс** метода действия в пределах HomeController **добавить представление** диалогового окна есть индекс в качестве имени представления по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="45271-380">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="45271-381">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-381">Click **Add**.</span></span>

    <span data-ttu-id="45271-382">![Диалоговое окно добавления представления](aspnet-mvc-4-fundamentals/_static/image14.png "диалоговое окно добавления представления")</span><span class="sxs-lookup"><span data-stu-id="45271-382">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="45271-383">*Диалоговое окно добавления представления*</span><span class="sxs-lookup"><span data-stu-id="45271-383">*Add View Dialog*</span></span>
4. <span data-ttu-id="45271-384">Visual Studio создает **Index.cshtml** Просмотр шаблона внутри **Views\Home** папки, а затем открывает его.</span><span class="sxs-lookup"><span data-stu-id="45271-384">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="45271-385">![Home индекс представления, созданного](aspnet-mvc-4-fundamentals/_static/image15.png "созданного Главная индекса представления")</span><span class="sxs-lookup"><span data-stu-id="45271-385">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="45271-386">*Домашняя созданного индекса представления*</span><span class="sxs-lookup"><span data-stu-id="45271-386">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-387">имя и расположение **Index.cshtml** файл действителен и следует соглашения об именовании ASP.NET MVC по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="45271-387">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="45271-388">Папка \Views\**Главная** соответствует имени контроллера (**Главная** контроллера).</span><span class="sxs-lookup"><span data-stu-id="45271-388">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="45271-389">Имя шаблона представления (**индекс**), соответствующий метод действия контроллера, который будет применяться для отображения представления.</span><span class="sxs-lookup"><span data-stu-id="45271-389">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="45271-390">Таким образом, ASP.NET MVC позволяет избежать необходимости явно указывать имя или расположение шаблона представления при использовании это соглашение об именовании, чтобы вернуть представление.</span><span class="sxs-lookup"><span data-stu-id="45271-390">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="45271-391">Созданный шаблон представления основана на  **\_layout.cshtml** шаблоном, определенным ранее.</span><span class="sxs-lookup"><span data-stu-id="45271-391">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="45271-392">Обновить свойство ViewBag.Title для **Главная**и измените основного содержимого для **это домашняя страница**, как показано в следующем коде:</span><span class="sxs-lookup"><span data-stu-id="45271-392">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="45271-393">Выберите **MvcMusicStore** проекта в обозревателе решений и нажать клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-393">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="45271-394">Задача 4: проверка</span><span class="sxs-lookup"><span data-stu-id="45271-394">Task 4: Verification</span></span>

<span data-ttu-id="45271-395">Чтобы проверить, правильно выполнить все эти шаги в предыдущем упражнении, выполните следующие действия:</span><span class="sxs-lookup"><span data-stu-id="45271-395">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="45271-396">Приложения, открывается в браузере следует отметить, что:</span><span class="sxs-lookup"><span data-stu-id="45271-396">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="45271-397">Метод действия HomeController индекс найден и отображаются **\Views\Home\Index.cshtml** Просмотр шаблона, несмотря на то, что код вызвал **возвращают View()**, так как шаблон представления, а затем соглашение об именовании стандартного.</span><span class="sxs-lookup"><span data-stu-id="45271-397">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="45271-398">Домашняя страница отображает приветственное сообщение, определенные в **\Views\Home\Index.cshtml** шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-398">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="45271-399">Использование домашней странице  **\_layout.cshtml** шаблона, и поэтому приветственное сообщение, содержащихся в макет узла стандартный HTML.</span><span class="sxs-lookup"><span data-stu-id="45271-399">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="45271-400">![Главном представлении индекса с помощью определенных LayoutPage и стиль](aspnet-mvc-4-fundamentals/_static/image16.png "Главная представление индекса, используя заданное LayoutPage и стиля")</span><span class="sxs-lookup"><span data-stu-id="45271-400">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="45271-401">*Домашняя индекс представления, с помощью определенных LayoutPage и стиля*</span><span class="sxs-lookup"><span data-stu-id="45271-401">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="45271-402">Упражнение 5: Создание модели представления</span><span class="sxs-lookup"><span data-stu-id="45271-402">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="45271-403">Таким образом внесенные отображения жестко закодировано HTML представления, но, чтобы создавать динамические веб-приложения, Просмотр шаблона должен получать сведения из контроллера.</span><span class="sxs-lookup"><span data-stu-id="45271-403">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="45271-404">Имеет один обычный способ, используемый для этой цели **ViewModel** шаблон, который позволяет контроллеру запаковать все сведения, необходимые для создания соответствующих HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="45271-404">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="45271-405">В этом упражнении вы узнаете, как создать класс ViewModel и добавить необходимые свойства: число жанров в хранилище и список этих жанров.</span><span class="sxs-lookup"><span data-stu-id="45271-405">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="45271-406">Обновит StoreController использовать созданный ViewModel, и наконец, вы создадите новый шаблон представления, упомянутых свойства отобразятся на странице.</span><span class="sxs-lookup"><span data-stu-id="45271-406">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="45271-407">Задача 1. Создание класса ViewModel</span><span class="sxs-lookup"><span data-stu-id="45271-407">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="45271-408">В этой задаче вы создадите ViewModel класс, который будет реализовать сценарий листинг жанр хранилища.</span><span class="sxs-lookup"><span data-stu-id="45271-408">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="45271-409">Если это еще не открыто, запустите **VS Express для Web**.</span><span class="sxs-lookup"><span data-stu-id="45271-409">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="45271-410">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="45271-410">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="45271-411">В диалоговом окне Открытие проекта найдите **Source\Ex05 CreatingAViewModel\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="45271-411">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="45271-412">Кроме того вы можете продолжить с решением, полученный после завершения работы в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="45271-412">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="45271-413">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="45271-413">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="45271-414">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="45271-414">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="45271-415">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="45271-415">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="45271-416">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="45271-416">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-417">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-417">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="45271-418">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-418">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="45271-419">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-419">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="45271-420">Создание **ViewModels** папку для хранения ViewModel.</span><span class="sxs-lookup"><span data-stu-id="45271-420">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="45271-421">Для этого щелкните правой кнопкой мыши верхнего уровня **MvcMusicStore** проекта, выберите **добавить** и затем **новую папку**.</span><span class="sxs-lookup"><span data-stu-id="45271-421">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="45271-422">![Добавление новой папки](aspnet-mvc-4-fundamentals/_static/image17.png "добавления новой папки")</span><span class="sxs-lookup"><span data-stu-id="45271-422">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="45271-423">*Добавление новой папки*</span><span class="sxs-lookup"><span data-stu-id="45271-423">*Adding a new folder*</span></span>
4. <span data-ttu-id="45271-424">Назовите папку *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="45271-424">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="45271-425">![ViewModels папку в обозревателе решений](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels папку в обозревателе решений")</span><span class="sxs-lookup"><span data-stu-id="45271-425">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="45271-426">*ViewModels папку в обозревателе решений*</span><span class="sxs-lookup"><span data-stu-id="45271-426">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="45271-427">Создание **ViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-427">Create a **ViewModel** class.</span></span> <span data-ttu-id="45271-428">Для этого щелкните правой кнопкой мыши **ViewModels** папку недавно создан, выберите **добавить** и затем **новый элемент**.</span><span class="sxs-lookup"><span data-stu-id="45271-428">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="45271-429">В разделе **кода**, выберите **класса** элемента и присвойте файлу имя *StoreIndexViewModel.cs*, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-429">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="45271-430">![Добавление нового класса](aspnet-mvc-4-fundamentals/_static/image19.png "Добавление нового класса")</span><span class="sxs-lookup"><span data-stu-id="45271-430">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="45271-431">*Добавление нового класса*</span><span class="sxs-lookup"><span data-stu-id="45271-431">*Adding a new Class*</span></span>

    <span data-ttu-id="45271-432">![Создание класса StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel создание класса")</span><span class="sxs-lookup"><span data-stu-id="45271-432">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="45271-433">*Создание класса StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="45271-433">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="45271-434">Задача 2 - Добавление свойства в класс ViewModel</span><span class="sxs-lookup"><span data-stu-id="45271-434">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="45271-435">Существует два параметра должны быть переданы из StoreController Просмотр шаблона для создания ожидаемый ответ в виде HTML: число жанров в хранилище и список этих жанров.</span><span class="sxs-lookup"><span data-stu-id="45271-435">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="45271-436">В этой задаче вы добавите эти свойства 2 **StoreIndexViewModel** класса: **NumberOfGenres** (целое число) и **жанров** (список строк).</span><span class="sxs-lookup"><span data-stu-id="45271-436">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="45271-437">Добавить **NumberOfGenres** и **жанров** свойства **StoreIndexViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-437">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="45271-438">Чтобы сделать это, добавьте следующие строки 2 в определение класса:</span><span class="sxs-lookup"><span data-stu-id="45271-438">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="45271-439">(Фрагмент - кода *MVC 4 ASP.NET основы - свойства Ex5 StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="45271-439">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="45271-440">**{Get; задать;}**  нотации задействует возможности C# автоматически реализуемые свойства компонента.</span><span class="sxs-lookup"><span data-stu-id="45271-440">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="45271-441">Предоставляет преимущества свойства без необходимости нами для объявления резервным полем.</span><span class="sxs-lookup"><span data-stu-id="45271-441">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="45271-442">Задача 3 - обновления StoreController использовать StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="45271-442">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="45271-443">**StoreIndexViewModel** класс инкапсулирует сведения, необходимые для передачи из **StoreController** **индекс** метод Просмотр шаблона для создания ответа .</span><span class="sxs-lookup"><span data-stu-id="45271-443">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="45271-444">В этой задаче будет обновлена **StoreController** использовать **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="45271-444">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="45271-445">Откройте **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-445">Open **StoreController** class.</span></span>

    <span data-ttu-id="45271-446">![Открытие класса StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "StoreController открытия класса")</span><span class="sxs-lookup"><span data-stu-id="45271-446">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="45271-447">*Открытие StoreController класса*</span><span class="sxs-lookup"><span data-stu-id="45271-447">*Opening StoreController class*</span></span>
2. <span data-ttu-id="45271-448">Чтобы использовать **StoreIndexViewModel** класса из **StoreController**, добавьте следующее пространство имен в верхней части **StoreController** кода:</span><span class="sxs-lookup"><span data-stu-id="45271-448">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="45271-449">(Фрагмент - кода *MVC 4 ASP.NET основы - с помощью ViewModels StoreIndexViewModel Ex5*)</span><span class="sxs-lookup"><span data-stu-id="45271-449">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="45271-450">Изменение **StoreController** **индекс** метод действия, чтобы создает и заполняет **StoreIndexViewModel** объекта, а затем передает его шаблон, представление Создайте HTML-ответа с ним.</span><span class="sxs-lookup"><span data-stu-id="45271-450">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-451">В лаборатории &quot;модели ASP.NET MVC и доступа к данным&quot; будет написан код, который извлекает из базы данных список жанров хранилища.</span><span class="sxs-lookup"><span data-stu-id="45271-451">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="45271-452">В следующем коде создается **списка** жанров фиктивные данные, которые будут заполнять **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="45271-452">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="45271-453">После создания и настройки **StoreIndexViewModel** объекта, он будет передана в качестве аргумента для **представление** метод.</span><span class="sxs-lookup"><span data-stu-id="45271-453">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="45271-454">Это означает, что шаблон представления будет использовать этот объект для создания HTML-ответа с ним.</span><span class="sxs-lookup"><span data-stu-id="45271-454">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="45271-455">Замените **индекс** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="45271-455">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="45271-456">(Фрагмент - кода *MVC 4 ASP.NET основы - метод индекс StoreController Ex5*)</span><span class="sxs-lookup"><span data-stu-id="45271-456">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="45271-457">Если вы знакомы с C#, могут предположить, используя **var** означает, что **viewModel** переменной позднего связывания.</span><span class="sxs-lookup"><span data-stu-id="45271-457">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="45271-458">Неправильный - компилятор C# используется для определения того, что определение зависимости от того, что можно присвоить переменной типа **viewModel** относится к типу **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="45271-458">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="45271-459">Кроме того, при компиляции локальной **viewModel** переменной как **StoreIndexViewModel** тип проверки во время компиляции get и поддержка редактора кода Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-459">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="45271-460">Задача 4. Создание шаблона представления, который использует StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="45271-460">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="45271-461">В этой задаче будет создан шаблон представления, который будет использовать StoreIndexViewModel объекта, переданного из контроллера, чтобы отобразить список жанров.</span><span class="sxs-lookup"><span data-stu-id="45271-461">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="45271-462">Перед созданием нового шаблона представление, нужно построить проект, чтобы **добавить диалоговое окно представления** знает о **StoreIndexViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-462">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="45271-463">Постройте проект, выбрав **построения** пункта меню и затем **построения MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="45271-463">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="45271-464">![Построение проекта](aspnet-mvc-4-fundamentals/_static/image22.png "построение проекта")</span><span class="sxs-lookup"><span data-stu-id="45271-464">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="45271-465">*Построение проекта*</span><span class="sxs-lookup"><span data-stu-id="45271-465">*Building the project*</span></span>
2. <span data-ttu-id="45271-466">Создание шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-466">Create a new View template.</span></span> <span data-ttu-id="45271-467">Чтобы сделать это, щелкните правой кнопкой мыши внутри **индекс** метод и выберите **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="45271-467">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="45271-468">![Добавление представления](aspnet-mvc-4-fundamentals/_static/image23.png "Добавление представления")</span><span class="sxs-lookup"><span data-stu-id="45271-468">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="45271-469">*Добавление представления*</span><span class="sxs-lookup"><span data-stu-id="45271-469">*Adding a View*</span></span>
3. <span data-ttu-id="45271-470">Поскольку **добавить диалоговое окно представления** был вызван из **StoreController**, добавит Просмотр шаблона по умолчанию в **\Views\Store\Index.cshtml** файла.</span><span class="sxs-lookup"><span data-stu-id="45271-470">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="45271-471">Проверьте **создавать строго типизированные представления** флажок, а затем выберите **StoreIndexViewModel** как **класс модели**.</span><span class="sxs-lookup"><span data-stu-id="45271-471">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="45271-472">Кроме того, убедитесь в том, что выбран обработчик представлений **Razor**.</span><span class="sxs-lookup"><span data-stu-id="45271-472">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="45271-473">Нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-473">Click **Add**.</span></span>

    <span data-ttu-id="45271-474">![Диалоговое окно добавления представления](aspnet-mvc-4-fundamentals/_static/image24.png "диалоговое окно добавления представления")</span><span class="sxs-lookup"><span data-stu-id="45271-474">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="45271-475">*Диалоговое окно добавления представления*</span><span class="sxs-lookup"><span data-stu-id="45271-475">*Add View Dialog*</span></span>

    <span data-ttu-id="45271-476">**\Views\Store\Index.cshtml** создается и открывается файл шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-476">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="45271-477">На основе сведений, предоставленных для **добавить представление** диалоговое окно на предыдущем шаге, представление будет ожидать шаблона **StoreIndexViewModel** экземпляр в качестве данных, используемый для создания HTML-ответа.</span><span class="sxs-lookup"><span data-stu-id="45271-477">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="45271-478">Обратите внимание, что шаблон наследует `ViewPage<musicstore.viewmodels.storeindexviewmodel>` в C#.</span><span class="sxs-lookup"><span data-stu-id="45271-478">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="45271-479">Задача 5 - Обновление шаблона представления</span><span class="sxs-lookup"><span data-stu-id="45271-479">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="45271-480">В этой задаче будет обновить представление шаблон, созданный в последней задаче для получения количества жанров и их имена в пределах страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-480">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="45271-481">Будет использовать @ синтаксис (часто обозначается как &quot;фрагменты кода&quot;) для выполнения кода в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="45271-481">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="45271-482">В **Index.cshtml** в файл **хранилища** папка, замените его код следующим:</span><span class="sxs-lookup"><span data-stu-id="45271-482">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="45271-483">Как только вы закончите вводить периода после слова **модели**, Intellisense в Visual Studio отображается список возможных свойств и методов для выбора.</span><span class="sxs-lookup"><span data-stu-id="45271-483">As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.</span></span>
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > <span data-ttu-id="45271-484">*Получение свойств модели и методы с IntelliSense в Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="45271-484">*Getting Model properties and methods with Visual Studio's IntelliSense*</span></span>
    > 
    > <span data-ttu-id="45271-485">**Модель** ссылок на свойства **StoreIndexViewModel** который передан контроллер для шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-485">The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template.</span></span> <span data-ttu-id="45271-486">Это означает, что можно получить доступ ко всем данных, передаваемых из контроллера шаблон представления через **модель** свойство и отформатируйте его в соответствующий ответ HTML в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="45271-486">This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.</span></span>
    > 
    > <span data-ttu-id="45271-487">Нужно просто выбрать **NumberOfGenres** список свойств в IntelliSense, а не вводить его и затем она будет Автозаполнение его, нажав клавишу **клавиши tab**.</span><span class="sxs-lookup"><span data-stu-id="45271-487">You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.</span></span>
2. <span data-ttu-id="45271-488">Цикл по списку genre в **StoreIndexViewModel** и создать HTML  **&lt;ul&gt;**  список с помощью **foreach** цикла.</span><span class="sxs-lookup"><span data-stu-id="45271-488">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
<span data-ttu-id="45271-489">(C#)</span><span class="sxs-lookup"><span data-stu-id="45271-489">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="45271-490">Нажмите клавишу **F5** для запуска приложения и Обзор **или сохранений**.</span><span class="sxs-lookup"><span data-stu-id="45271-490">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="45271-491">Появится список жанров переданный **StoreIndexViewModel** объекта из **StoreController** для шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-491">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="45271-492">![Представление, содержащее список жанров](aspnet-mvc-4-fundamentals/_static/image26.png "представление, отображающее список жанров")</span><span class="sxs-lookup"><span data-stu-id="45271-492">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="45271-493">*Представление, содержащее список жанров*</span><span class="sxs-lookup"><span data-stu-id="45271-493">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="45271-494">Закройте браузер.</span><span class="sxs-lookup"><span data-stu-id="45271-494">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="45271-495">Упражнение 6: Использование параметров в представлении</span><span class="sxs-lookup"><span data-stu-id="45271-495">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="45271-496">В упражнении 3 вы узнали, как передавать параметры контроллера.</span><span class="sxs-lookup"><span data-stu-id="45271-496">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="45271-497">В этом упражнении вы узнаете, как использовать эти параметры в шаблоне представления.</span><span class="sxs-lookup"><span data-stu-id="45271-497">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="45271-498">Для этой цели будут представлены сначала для классов модели, которые помогают управлять логику данных и домена.</span><span class="sxs-lookup"><span data-stu-id="45271-498">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="45271-499">Кроме того вы узнаете, как создавать ссылки на страницы в приложении ASP.NET MVC позволяет не задумываться вещей, как кодирование URL-пути.</span><span class="sxs-lookup"><span data-stu-id="45271-499">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="45271-500">Задача 1. Добавление классов модели</span><span class="sxs-lookup"><span data-stu-id="45271-500">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="45271-501">В отличие от ViewModels, в которой создаются только для передачи данных из контроллера в представление классов модели создаются для хранения и управления логику данных и домена.</span><span class="sxs-lookup"><span data-stu-id="45271-501">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="45271-502">В этой задаче вы добавите двух классов модели для представления этих концепций: **жанр** и **альбом**.</span><span class="sxs-lookup"><span data-stu-id="45271-502">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="45271-503">Если это еще не открыто, запустите **VS Express для Web**</span><span class="sxs-lookup"><span data-stu-id="45271-503">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="45271-504">В **файл** меню, выберите **Открытие проекта**.</span><span class="sxs-lookup"><span data-stu-id="45271-504">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="45271-505">В диалоговом окне Открытие проекта найдите **Source\Ex06 UsingParametersInView\Begin**выберите **Begin.sln** и нажмите кнопку **откройте**.</span><span class="sxs-lookup"><span data-stu-id="45271-505">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="45271-506">Кроме того вы можете продолжить с решением, полученный после завершения работы в предыдущем упражнении.</span><span class="sxs-lookup"><span data-stu-id="45271-506">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="45271-507">Если вы открыли указанных **начать** решение, необходимо будет загрузить несколько отсутствующих пакетов NuGet прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="45271-507">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="45271-508">Чтобы сделать это, нажмите кнопку **проекта** и выбрать пункт **управление пакетами NuGet**.</span><span class="sxs-lookup"><span data-stu-id="45271-508">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="45271-509">В **управление пакетами NuGet** диалоговое окно, нажмите кнопку **восстановить** чтобы скачивать отсутствующие пакеты.</span><span class="sxs-lookup"><span data-stu-id="45271-509">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="45271-510">Наконец, постройте решение, нажав кнопку **построения** | **построить решение**.</span><span class="sxs-lookup"><span data-stu-id="45271-510">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-511">Одним из преимуществ использования NuGet является у вас не указан для отправки всех библиотек в вашем проекте, уменьшив размер проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-511">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="45271-512">С помощью NuGet Power Tools указав версий пакета в файле Packages.config можно для загрузки всех необходимых библиотек первый раз, при запуске проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-512">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="45271-513">Вот почему необходимо будет выполнить эти шаги после открытия существующего решения в этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-513">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="45271-514">Добавить **жанр** класс модели.</span><span class="sxs-lookup"><span data-stu-id="45271-514">Add a **Genre** Model class.</span></span> <span data-ttu-id="45271-515">Для этого щелкните правой кнопкой мыши **моделей** папки в **обозревателе решений**выберите **добавить** и затем **новый элемент** параметр.</span><span class="sxs-lookup"><span data-stu-id="45271-515">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="45271-516">В разделе **кода**, выберите **класса** элемента и присвойте файлу имя *Genre.cs*, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-516">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="45271-517">![Добавление класса](aspnet-mvc-4-fundamentals/_static/image27.png "Добавление класса")</span><span class="sxs-lookup"><span data-stu-id="45271-517">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="45271-518">*Добавление нового элемента*</span><span class="sxs-lookup"><span data-stu-id="45271-518">*Adding a new item*</span></span>

    <span data-ttu-id="45271-519">![Добавьте класс модели жанр](aspnet-mvc-4-fundamentals/_static/image28.png "добавьте класс модели жанр")</span><span class="sxs-lookup"><span data-stu-id="45271-519">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="45271-520">*Добавьте класс модели жанр*</span><span class="sxs-lookup"><span data-stu-id="45271-520">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="45271-521">Добавить **имя** свойство жанр классу.</span><span class="sxs-lookup"><span data-stu-id="45271-521">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="45271-522">Чтобы сделать это, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="45271-522">To do this, add the following code:</span></span>

    <span data-ttu-id="45271-523">(Фрагмент - кода *основы ASP.NET MVC 4 - жанр Ex6*)</span><span class="sxs-lookup"><span data-stu-id="45271-523">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="45271-524">Таким же способом, как и прежде, добавьте **альбом** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-524">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="45271-525">Для этого щелкните правой кнопкой мыши **моделей** папки в **обозревателе решений**выберите **добавить** и затем **новый элемент** параметр.</span><span class="sxs-lookup"><span data-stu-id="45271-525">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="45271-526">В разделе **кода**, выберите **класса** элемента и присвойте файлу имя *Album.cs*, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-526">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="45271-527">Добавьте в класс альбом два свойства: **жанр** и **заголовка**.</span><span class="sxs-lookup"><span data-stu-id="45271-527">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="45271-528">Чтобы сделать это, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="45271-528">To do this, add the following code:</span></span>

    <span data-ttu-id="45271-529">(Фрагмент - кода *основы ASP.NET MVC 4 - альбом Ex6*)</span><span class="sxs-lookup"><span data-stu-id="45271-529">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="45271-530">Задача 2 - Добавление StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="45271-530">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="45271-531">Объект **StoreBrowseViewModel** будет использоваться в этой задаче для отображения альбомы, которые соответствуют выбранной жанра.</span><span class="sxs-lookup"><span data-stu-id="45271-531">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="45271-532">В этой задаче будет создайте этот класс и добавьте два свойства для обработки **жанр** и его **альбом**в списке.</span><span class="sxs-lookup"><span data-stu-id="45271-532">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="45271-533">Добавить **StoreBrowseViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-533">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="45271-534">Для этого щелкните правой кнопкой мыши **ViewModels** папки в **обозревателе решений**выберите **добавить** и затем **новый элемент** параметр.</span><span class="sxs-lookup"><span data-stu-id="45271-534">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="45271-535">В разделе **кода**, выберите **класса** элемента и присвойте файлу имя *StoreBrowseViewModel.cs*, нажмите кнопку **добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-535">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="45271-536">Добавьте ссылку в моделях в **StoreBrowseViewModel** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-536">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="45271-537">Чтобы сделать это, добавьте следующие пространства имен с помощью:</span><span class="sxs-lookup"><span data-stu-id="45271-537">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="45271-538">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="45271-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="45271-539">Добавьте два свойства для **StoreBrowseViewModel** класса: **жанр** и **альбомы**.</span><span class="sxs-lookup"><span data-stu-id="45271-539">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="45271-540">Чтобы сделать это, добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="45271-540">To do this, add the following code:</span></span>

    <span data-ttu-id="45271-541">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="45271-541">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="45271-542">Что такое **списка&lt;альбом&gt;**  ?: использует это определение **списка&lt;T&gt;**  типа, где **T** ограничивает тип, для каких элементов этого **списка** принадлежат, в этом случае **альбом** (или один из его потомков).</span><span class="sxs-lookup"><span data-stu-id="45271-542">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
    > 
    > <span data-ttu-id="45271-543">Эта возможность создавать классы и методы, которые отложить спецификации один или несколько типов, пока класс или метод объявляется и создавать экземпляры в клиентском коде функции языка C# называется **универсальные шаблоны**.</span><span class="sxs-lookup"><span data-stu-id="45271-543">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
    > 
    > <span data-ttu-id="45271-544">**Список&lt;T&gt;**  универсального эквивалентно **ArrayList** тип, доступный в **System.Collections.Generic** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="45271-544">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="45271-545">Одно из преимуществ использования **универсальных шаблонов** что, поскольку задан тип, не требуется выполнять проверку операции, такие как элементы в приведении типа **альбом** как в случае с **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="45271-545">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="45271-546">Задача 3. Использование нового ViewModel в StoreController</span><span class="sxs-lookup"><span data-stu-id="45271-546">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="45271-547">В этой задаче предстоит изменить **StoreController** **Обзор** и **сведения** методы действий для использования новых **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="45271-547">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="45271-548">Добавьте ссылку на папку модели в **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-548">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="45271-549">Для этого разверните **контроллеров** папки в **обозревателе решений** и откройте **StoreController** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-549">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="45271-550">Затем добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="45271-550">Then add the following code:</span></span>

    <span data-ttu-id="45271-551">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="45271-551">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="45271-552">Замените **Обзор** метода действия для использования **StoreViewBrowseController** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-552">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="45271-553">Вы создадите жанра, а два новых альбомы фиктивными данными (в следующей практической работе будут использовать реальные данные из базы данных).</span><span class="sxs-lookup"><span data-stu-id="45271-553">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="45271-554">Для этого замените **Обзор** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="45271-554">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="45271-555">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="45271-555">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="45271-556">Замените **сведения** метода действия для использования **StoreViewBrowseController** класса.</span><span class="sxs-lookup"><span data-stu-id="45271-556">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="45271-557">Будет создана новая **альбом** объекта, возвращаемого в **представление**.</span><span class="sxs-lookup"><span data-stu-id="45271-557">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="45271-558">Для этого замените **сведения** метод следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="45271-558">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="45271-559">(Фрагмент - кода *основы ASP.NET MVC 4 - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="45271-559">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="45271-560">Задача 4. Добавление шаблона представления обзора</span><span class="sxs-lookup"><span data-stu-id="45271-560">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="45271-561">В этой задаче вы добавите **Обзор** представления для отображения альбомы, найдено для определенного жанра.</span><span class="sxs-lookup"><span data-stu-id="45271-561">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="45271-562">Прежде чем создавать новый шаблон представления, при построении проекта, чтобы **добавить представление** диалоговое окно знает о **ViewModel** класс использовать.</span><span class="sxs-lookup"><span data-stu-id="45271-562">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="45271-563">Постройте проект, выбрав **построения** пункта меню и затем **построения MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="45271-563">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="45271-564">Добавить **Обзор** представления.</span><span class="sxs-lookup"><span data-stu-id="45271-564">Add a **Browse** View.</span></span> <span data-ttu-id="45271-565">Для этого щелкните правой кнопкой мыши в **Обзор** метод действия **StoreController** и нажмите кнопку **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="45271-565">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="45271-566">В **добавить представление** диалогового окна убедитесь, что имя представления **Обзор**.</span><span class="sxs-lookup"><span data-stu-id="45271-566">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="45271-567">Проверьте **создать строго типизированное представление** флажок и выбрать **StoreBrowseViewModel** из **класс модели** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="45271-567">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="45271-568">Остальные поля оставьте значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="45271-568">Leave the other fields with their default value.</span></span> <span data-ttu-id="45271-569">Затем нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-569">Then click **Add**.</span></span>

    <span data-ttu-id="45271-570">![Добавление представления обзора](aspnet-mvc-4-fundamentals/_static/image29.png "Добавление представления обзора")</span><span class="sxs-lookup"><span data-stu-id="45271-570">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="45271-571">*Добавление представления обзора*</span><span class="sxs-lookup"><span data-stu-id="45271-571">*Adding a Browse View*</span></span>
4. <span data-ttu-id="45271-572">Изменить **Browse.cshtml** для отображения информации жанра, доступ к **StoreBrowseViewModel** объекта, передаваемого шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-572">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="45271-573">Для этого замените содержимое следующим: (C#)</span><span class="sxs-lookup"><span data-stu-id="45271-573">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="45271-574">Задача 5 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="45271-574">Task 5 - Running the Application</span></span>

<span data-ttu-id="45271-575">В этой задаче вы проверите, **Обзор** метод извлекает альбомы из **Обзор** метода действия.</span><span class="sxs-lookup"><span data-stu-id="45271-575">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="45271-576">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-576">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="45271-577">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-577">The project starts in the Home page.</span></span> <span data-ttu-id="45271-578">Изменить URL-адрес **, хранения, Обзор? Жанр = Disco** чтобы убедиться, что действие возвращает два альбомов.</span><span class="sxs-lookup"><span data-stu-id="45271-578">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="45271-579">![Просмотр альбомов Disco хранилища](aspnet-mvc-4-fundamentals/_static/image30.png "просмотра Disco альбомы хранилища")</span><span class="sxs-lookup"><span data-stu-id="45271-579">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="45271-580">*Просмотр альбомов Disco хранилища*</span><span class="sxs-lookup"><span data-stu-id="45271-580">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="45271-581">Задача 6 - Отображение сведений о конкретных альбом</span><span class="sxs-lookup"><span data-stu-id="45271-581">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="45271-582">В этой задаче будет реализовывать **сведения о хранилище** представление для отображения сведений о конкретных альбома.</span><span class="sxs-lookup"><span data-stu-id="45271-582">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="45271-583">В этой лаборатории практических все данные будут отображаться альбома уже содержится в **представление** шаблона.</span><span class="sxs-lookup"><span data-stu-id="45271-583">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="45271-584">В этом случае вместо создания **StoreDetailsViewModel** класса, будет использовать текущий **StoreBrowseViewModel** шаблона, передав ему альбома.</span><span class="sxs-lookup"><span data-stu-id="45271-584">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="45271-585">Закройте браузер, если необходимо, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-585">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="45271-586">Добавьте новый **сведения** просмотра для **StoreController** **сведения** метода действия.</span><span class="sxs-lookup"><span data-stu-id="45271-586">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="45271-587">Для этого щелкните правой кнопкой мыши **сведения** метод в **StoreController** класса и нажмите кнопку **добавить представление**.</span><span class="sxs-lookup"><span data-stu-id="45271-587">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="45271-588">В **добавить представление** диалогового окна, убедитесь, что **имя представления** — **сведения**.</span><span class="sxs-lookup"><span data-stu-id="45271-588">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="45271-589">Проверьте **создать строго типизированное представление** флажок и выбрать **альбом** из **класс модели** раскрывающегося списка.</span><span class="sxs-lookup"><span data-stu-id="45271-589">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="45271-590">Остальные поля оставьте значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="45271-590">Leave the other fields with their default value.</span></span> <span data-ttu-id="45271-591">Затем нажмите кнопку **Добавить**.</span><span class="sxs-lookup"><span data-stu-id="45271-591">Then click **Add**.</span></span> <span data-ttu-id="45271-592">Это создает и открывает **\Views\Store\Details.cshtml** файла.</span><span class="sxs-lookup"><span data-stu-id="45271-592">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="45271-593">![Добавление представления сведений](aspnet-mvc-4-fundamentals/_static/image31.png "Добавление представления сведений")</span><span class="sxs-lookup"><span data-stu-id="45271-593">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="45271-594">*Добавление представления сведений*</span><span class="sxs-lookup"><span data-stu-id="45271-594">*Adding a Details View*</span></span>
3. <span data-ttu-id="45271-595">Изменить **Details.cshtml** файл для отображения сведений о дисках, доступ к **альбом** объекта, передаваемого шаблона представления.</span><span class="sxs-lookup"><span data-stu-id="45271-595">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="45271-596">Для этого замените содержимое следующим: (C#)</span><span class="sxs-lookup"><span data-stu-id="45271-596">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="45271-597">Задача 7. выполнение приложения</span><span class="sxs-lookup"><span data-stu-id="45271-597">Task 7 - Running the Application</span></span>

<span data-ttu-id="45271-598">В этой задаче вы проверите, **сведения** представление получает данные альбома из **подробное описание действий** метод.</span><span class="sxs-lookup"><span data-stu-id="45271-598">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="45271-599">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-599">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="45271-600">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-600">The project starts in the **Home** page.</span></span> <span data-ttu-id="45271-601">Изменить URL-адрес **/Store/Details/5** для проверки сведений о диске.</span><span class="sxs-lookup"><span data-stu-id="45271-601">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="45271-602">![Просмотр сведений альбомы](aspnet-mvc-4-fundamentals/_static/image32.png "просмотра альбомы детализации")</span><span class="sxs-lookup"><span data-stu-id="45271-602">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="45271-603">*Подробности обозревателя альбом*</span><span class="sxs-lookup"><span data-stu-id="45271-603">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="45271-604">Задача 8. Добавление ссылки между страницами</span><span class="sxs-lookup"><span data-stu-id="45271-604">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="45271-605">В этой задаче будет добавить в представление хранилища должна иметься ссылка в имени каждой жанр в соответствующую ссылку **/Store/обзора** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="45271-605">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="45271-606">Таким образом, при щелчке жанра, например **Disco**, произойдет переход **/Store/обзора? жанр = Disco** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="45271-606">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="45271-607">Закройте браузер, если необходимо, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-607">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="45271-608">Обновление **индекс** страницу, чтобы добавить ссылку на **Обзор** страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-608">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="45271-609">Для этого в **обозревателе решений** разверните **представления** папки, то **хранилища** папку и дважды щелкните **Index.cshtml** страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-609">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="45271-610">Добавьте ссылку на представление обзора, указывающее, выбран жанр.</span><span class="sxs-lookup"><span data-stu-id="45271-610">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="45271-611">Для этого замените следующий выделенный код в  **&lt;li&gt;**  теги: (C#)</span><span class="sxs-lookup"><span data-stu-id="45271-611">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="45271-612">другой подход будет связывания непосредственно на страницу с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="45271-612">another approach would be linking directly to the page, with a code like the following:</span></span>
    > 
    > <span data-ttu-id="45271-613">&lt;href =&quot;/Store/обзора? жанр =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="45271-613">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
    > 
    > <span data-ttu-id="45271-614">Несмотря на то, что такой подход работает, он зависит от строки жестко задано.</span><span class="sxs-lookup"><span data-stu-id="45271-614">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="45271-615">При переименовании контроллера в более поздней версии необходимо вручную изменить эту инструкцию.</span><span class="sxs-lookup"><span data-stu-id="45271-615">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="45271-616">Лучшим вариантом является использование **вспомогательный метод HTML** метод.</span><span class="sxs-lookup"><span data-stu-id="45271-616">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="45271-617">ASP.NET MVC включает метод вспомогательный метод HTML, который доступен для выполнения таких задач.</span><span class="sxs-lookup"><span data-stu-id="45271-617">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="45271-618">**Html.ActionLink()** вспомогательный метод позволяет легко построить HTML  **&lt;&gt;**  ссылки, всегда убеждаться в том пути URL-адрес правильно URL-кодированием.</span><span class="sxs-lookup"><span data-stu-id="45271-618">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
    > 
    > <span data-ttu-id="45271-619">Htlm.ActionLink имеет несколько перегрузок.</span><span class="sxs-lookup"><span data-stu-id="45271-619">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="45271-620">В этом упражнении используется одна из которых принимает три параметра:</span><span class="sxs-lookup"><span data-stu-id="45271-620">In this exercise you will use one that takes three parameters:</span></span>
    > 
    > 1. <span data-ttu-id="45271-621">Текст ссылки, который будет отображаться название жанра</span><span class="sxs-lookup"><span data-stu-id="45271-621">Link text, which will display the Genre name</span></span>
    > 2. <span data-ttu-id="45271-622">Имя действия контроллера (**Обзор**)</span><span class="sxs-lookup"><span data-stu-id="45271-622">Controller action name (**Browse**)</span></span>
    > 3. <span data-ttu-id="45271-623">Значения параметра, указав и имя маршрута (**жанр**) и значение (**название жанра**)</span><span class="sxs-lookup"><span data-stu-id="45271-623">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="45271-624">Задача 9. запуск приложения</span><span class="sxs-lookup"><span data-stu-id="45271-624">Task 9 - Running the Application</span></span>

<span data-ttu-id="45271-625">В этой задаче вы проверите отображения со ссылками на соответствующие каждом жанре **/Store/обзора** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="45271-625">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="45271-626">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-626">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="45271-627">На домашней странице запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-627">The project starts in the Home page.</span></span> <span data-ttu-id="45271-628">Изменить URL-адрес **или сохранений** чтобы убедиться, что каждом жанре ссылки на соответствующие **/Store/обзора** URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="45271-628">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="45271-629">![Просмотр жанров со ссылками на странице «Обзор»](aspnet-mvc-4-fundamentals/_static/image33.png "просмотра жанров со ссылками на странице «Обзор»")</span><span class="sxs-lookup"><span data-stu-id="45271-629">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="45271-630">*Просмотр жанров со ссылками на странице «Обзор»*</span><span class="sxs-lookup"><span data-stu-id="45271-630">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="45271-631">Задача 10 — с помощью динамического ViewModel коллекции для передачи значений</span><span class="sxs-lookup"><span data-stu-id="45271-631">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="45271-632">В этой задаче будет изучено простой и эффективный способ передачи значений между контроллером и представлением без внесения изменений в модели.</span><span class="sxs-lookup"><span data-stu-id="45271-632">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="45271-633">ASP.NET MVC 4 предоставляет коллекцию &quot;ViewModel&quot;, которой назначены все динамические значения и получить доступ в контроллеры и представления также.</span><span class="sxs-lookup"><span data-stu-id="45271-633">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="45271-634">Вы сможете использовать динамическую коллекцию ViewBag для передачи списка &quot; **звезду жанров** &quot; из контроллера в представление.</span><span class="sxs-lookup"><span data-stu-id="45271-634">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="45271-635">Просмотр индекса хранилища будет доступ к **ViewModel** и отображения информации.</span><span class="sxs-lookup"><span data-stu-id="45271-635">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="45271-636">Закройте браузер, если необходимо, чтобы вернуться в окно Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-636">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="45271-637">Откройте **StoreController.cs** и изменения **индекс** метод для создания списка или звездообразную жанров в коллекцию ViewModel:</span><span class="sxs-lookup"><span data-stu-id="45271-637">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="45271-638">Можно также использовать синтаксис **ViewBag [&quot;Starred&quot;]** доступа к свойствам.</span><span class="sxs-lookup"><span data-stu-id="45271-638">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="45271-639">Значок звезды  **&quot;starred.png&quot;**  включается в **Source\Assets\Images** папку части этой лаборатории.</span><span class="sxs-lookup"><span data-stu-id="45271-639">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="45271-640">Чтобы добавить его в приложение, перетащите их содержимое из **Проводник** в окно **обозревателе решений** в Visual Web Developer Express, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="45271-640">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="45271-641">![Добавление типа «звезда» образ для решения](aspnet-mvc-4-fundamentals/_static/image34.png "изображение решения, добавление типа «звезда»")</span><span class="sxs-lookup"><span data-stu-id="45271-641">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="45271-642">*Добавление типа «звезда» изображения в решение*</span><span class="sxs-lookup"><span data-stu-id="45271-642">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="45271-643">Откройте представление **Store/Index.cshtml** и изменения содержимого.</span><span class="sxs-lookup"><span data-stu-id="45271-643">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="45271-644">Будет считывать &quot;звезду&quot; свойство в **ViewBag** коллекции и узнайте имя текущего жанра, в списке.</span><span class="sxs-lookup"><span data-stu-id="45271-644">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="45271-645">В этом случае будут показаны значка звездочки право жанр ссылку.</span><span class="sxs-lookup"><span data-stu-id="45271-645">In that case you will show a star icon right to the genre link.</span></span>
<span data-ttu-id="45271-646">(C#)</span><span class="sxs-lookup"><span data-stu-id="45271-646">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="45271-647">Задача 11 - запуск приложения</span><span class="sxs-lookup"><span data-stu-id="45271-647">Task 11 - Running the Application</span></span>

<span data-ttu-id="45271-648">В этой задаче вы проверите, что отмечено жанров отображать значок.</span><span class="sxs-lookup"><span data-stu-id="45271-648">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="45271-649">Нажмите клавишу **F5** для запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-649">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="45271-650">Запускает проект в **Главная** страницы.</span><span class="sxs-lookup"><span data-stu-id="45271-650">The project starts in the **Home** page.</span></span> <span data-ttu-id="45271-651">Изменить URL-адрес **или сохранений** для убедитесь, что каждый важная жанр уважение метку:</span><span class="sxs-lookup"><span data-stu-id="45271-651">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="45271-652">![Просмотр жанров с элементами отмечено](aspnet-mvc-4-fundamentals/_static/image35.png "просмотра жанров отмечено элементами")</span><span class="sxs-lookup"><span data-stu-id="45271-652">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="45271-653">*Просмотр жанров отмечено элементами*</span><span class="sxs-lookup"><span data-stu-id="45271-653">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="45271-654">Упражнение 7. Знакомство с функциями новый шаблон ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="45271-654">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="45271-655">В этом упражнении вы исследуете улучшения, внесенные в шаблоны проектов ASP.NET MVC 4, изучать наиболее соответствующие компоненты нового шаблона.</span><span class="sxs-lookup"><span data-stu-id="45271-655">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="45271-656">Задача 1: Изучение шаблона приложения ASP.NET MVC 4 Интернета</span><span class="sxs-lookup"><span data-stu-id="45271-656">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="45271-657">Если это еще не открыто, запустите **VS Express для Web**</span><span class="sxs-lookup"><span data-stu-id="45271-657">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="45271-658">Выберите **файл | Новые | Проект** команду меню.</span><span class="sxs-lookup"><span data-stu-id="45271-658">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="45271-659">В **новый проект** диалогового окна выберите **Visual C# | Web** шаблона на левой панели дерева, а затем выберите **веб-приложение ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="45271-659">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="45271-660">**Имя** проекта *MusicStore* и обновить **имя решения** для *начать*, затем выберите расположение (или оставьте значение по умолчанию) и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="45271-660">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="45271-661">![Создание нового проекта ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Создание нового проекта ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="45271-661">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="45271-662">*Создание нового проекта ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="45271-662">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="45271-663">В **нового проекта ASP.NET MVC 4** диалогового окна выберите **веб-приложение** шаблон проекта и нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="45271-663">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="45271-664">Обратите внимание, что можно выбрать как обработчик представлений Razor или ASPX.</span><span class="sxs-lookup"><span data-stu-id="45271-664">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="45271-665">![Создание нового приложения ASP.NET MVC 4 Интернет](aspnet-mvc-4-fundamentals/_static/image37.png "Создание нового приложения ASP.NET MVC 4 Интернета")</span><span class="sxs-lookup"><span data-stu-id="45271-665">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="45271-666">*Создание нового приложения ASP.NET MVC 4 Интернета*</span><span class="sxs-lookup"><span data-stu-id="45271-666">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-667">Синтаксис Razor была введена в ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="45271-667">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="45271-668">Его задача — чтобы свести к минимуму число знаков и нажатий клавиш в файл, включение fast и жидкости кодирования рабочего процесса.</span><span class="sxs-lookup"><span data-stu-id="45271-668">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="45271-669">Razor использует существующие C# и Visual Basic (или другое) языковые навыки и доставляет синтаксиса разметки шаблона, который включает это здорово рабочий процесс построения HTML.</span><span class="sxs-lookup"><span data-stu-id="45271-669">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="45271-670">Нажмите клавишу **F5** для запуска решения и отображения обновленного шаблона.</span><span class="sxs-lookup"><span data-stu-id="45271-670">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="45271-671">Проверьте работу следующих функций:</span><span class="sxs-lookup"><span data-stu-id="45271-671">You can check out the following features:</span></span>

    1. <span data-ttu-id="45271-672">**Шаблоны в современном стиле**</span><span class="sxs-lookup"><span data-stu-id="45271-672">**Modern-style templates**</span></span>

        <span data-ttu-id="45271-673">Шаблоны были обновлены, предоставляя дополнительные стили, современный вид.</span><span class="sxs-lookup"><span data-stu-id="45271-673">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="45271-674">![Шаблоны ASP.NET MVC 4 приведен](aspnet-mvc-4-fundamentals/_static/image38.png "приведен шаблоны ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="45271-674">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="45271-675">*Шаблоны ASP.NET MVC 4 приведен*</span><span class="sxs-lookup"><span data-stu-id="45271-675">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="45271-676">**Адаптивной отрисовки**</span><span class="sxs-lookup"><span data-stu-id="45271-676">**Adaptive Rendering**</span></span>

        <span data-ttu-id="45271-677">Извлеките изменения размера окна браузера и обратите внимание на то, как динамически адаптируется макет страницы и новый размер окна.</span><span class="sxs-lookup"><span data-stu-id="45271-677">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="45271-678">Эти шаблоны использовать методика адаптивной отрисовки к просмотру правильно в настольных и мобильных платформ без настройки.</span><span class="sxs-lookup"><span data-stu-id="45271-678">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="45271-679">![Шаблон проекта ASP.NET MVC 4 в другом браузере размеры](aspnet-mvc-4-fundamentals/_static/image39.png "шаблон проекта ASP.NET MVC 4 в другом браузере размеры")</span><span class="sxs-lookup"><span data-stu-id="45271-679">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="45271-680">*Шаблон проекта ASP.NET MVC 4 в другом браузере размеры*</span><span class="sxs-lookup"><span data-stu-id="45271-680">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="45271-681">Закройте браузер, чтобы остановить отладчик и вернуться в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-681">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="45271-682">Теперь имеется возможность просматривать решения и посмотрите, какие новые функции ASP.NET MVC 4 в шаблоне проекта.</span><span class="sxs-lookup"><span data-stu-id="45271-682">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="45271-683">![MVC4-Интернет приложения шаблон проекта ASP.NET-](aspnet-mvc-4-fundamentals/_static/image40.png "шаблон проекта ASP.NET MVC 4 Интернет приложений")</span><span class="sxs-lookup"><span data-stu-id="45271-683">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="45271-684">*Шаблон проекта приложения Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="45271-684">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    1. <span data-ttu-id="45271-685">**Разметку HTML5**</span><span class="sxs-lookup"><span data-stu-id="45271-685">**HTML5 markup**</span></span>

        <span data-ttu-id="45271-686">Обзор представлений шаблона, чтобы определить новый разметку тему, например открыть **About.cshtml** просматривать внутри **Главная** папки.</span><span class="sxs-lookup"><span data-stu-id="45271-686">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

        <span data-ttu-id="45271-687">![Новый шаблон, с помощью Razor и HTML5 разметки](aspnet-mvc-4-fundamentals/_static/image41.png "новый шаблон, с помощью Razor и HTML5 разметки")</span><span class="sxs-lookup"><span data-stu-id="45271-687">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

        <span data-ttu-id="45271-688">*Новый шаблон, с помощью Razor и HTML5 разметки*</span><span class="sxs-lookup"><span data-stu-id="45271-688">*New template, using Razor and HTML5 markup*</span></span>
    2. <span data-ttu-id="45271-689">**Включены библиотеки JavaScript**</span><span class="sxs-lookup"><span data-stu-id="45271-689">**JavaScript libraries included**</span></span>

        1. <span data-ttu-id="45271-690">**jQuery**: jQuery упрощает обход документа HTML, обработка событий, анимации и взаимодействий Ajax.</span><span class="sxs-lookup"><span data-stu-id="45271-690">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
        2. <span data-ttu-id="45271-691">**jQuery UI**: Эта библиотека предоставляет абстракции для низкоуровневого взаимодействия, а также дополнительные эффекты анимации и тематизации мини-приложений построена на базе библиотека JavaScript jQuery.</span><span class="sxs-lookup"><span data-stu-id="45271-691">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

            > [!NOTE]
            > <span data-ttu-id="45271-692">Вы можете узнать о jQuery и jQuery UI в [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="45271-692">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
        3. <span data-ttu-id="45271-693">**Использованием KnockoutJS**: шаблон по умолчанию ASP.NET MVC 4 теперь включает **использованием KnockoutJS**, платформа JavaScript MVVM, которая позволяет создавать широкие возможности и с высокой скоростью отклика веб-приложения с помощью JavaScript и HTML.</span><span class="sxs-lookup"><span data-stu-id="45271-693">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="45271-694">Как и в ASP.NET MVC 3, jQuery и jQuery библиотеки пользовательского интерфейса также включаются в ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="45271-694">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

            > [!NOTE]
            > <span data-ttu-id="45271-695">Можно получить дополнительные сведения о библиотеке с использованием KnockOutJS в этой связи: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="45271-695">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
        4. <span data-ttu-id="45271-696">**Modernizr**: Эта библиотека запускается автоматически, создание сайтов, совместимые с старых браузерах при использовании технологии HTML5 и CSS3.</span><span class="sxs-lookup"><span data-stu-id="45271-696">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

            > [!NOTE]
            > <span data-ttu-id="45271-697">Можно получить дополнительные сведения о библиотеке Modernizr в этой связи: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="45271-697">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
    3. <span data-ttu-id="45271-698">**SimpleMembership, включенные в решение**</span><span class="sxs-lookup"><span data-stu-id="45271-698">**SimpleMembership included in the solution**</span></span>

        <span data-ttu-id="45271-699">SimpleMembership была разработана для замены предыдущей системы поставщика ролей ASP.NET и членства.</span><span class="sxs-lookup"><span data-stu-id="45271-699">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="45271-700">Он имеет много новых возможностей, которые упрощают для разработчиков на безопасное веб-страницы в более гибкий способ.</span><span class="sxs-lookup"><span data-stu-id="45271-700">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

        <span data-ttu-id="45271-701">Шаблон Интернета уже настроил несколько моментов, которые интегрируются SimpleMembership, например, AccountController подготовлена для использования OAuthWebSecurity (для регистрации учетной записи OAuth, имя входа, управления, т. д.) и веб-безопасности.</span><span class="sxs-lookup"><span data-stu-id="45271-701">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

        <span data-ttu-id="45271-702">![SimpleMembership, включенные в решение](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership, включенные в решение")</span><span class="sxs-lookup"><span data-stu-id="45271-702">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

        <span data-ttu-id="45271-703">*SimpleMembership, включенные в решение*</span><span class="sxs-lookup"><span data-stu-id="45271-703">*SimpleMembership Included in the solution*</span></span>

        > [!NOTE]
        > <span data-ttu-id="45271-704">Получите дополнительную информацию о [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) в библиотеке MSDN.</span><span class="sxs-lookup"><span data-stu-id="45271-704">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="45271-705">Кроме того, можно развернуть это приложение на веб-сайтов Windows Azure ниже [приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="45271-705">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="45271-706">Сводка</span><span class="sxs-lookup"><span data-stu-id="45271-706">Summary</span></span>

<span data-ttu-id="45271-707">Выполнив этот Практическое лабораторное занятие вы изучили основы ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="45271-707">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="45271-708">Основными компонентами приложения MVC и их взаимодействие</span><span class="sxs-lookup"><span data-stu-id="45271-708">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="45271-709">Создание приложения ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="45271-709">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="45271-710">Добавление и настройка контроллеров для обработки параметров, передаваемые через URL-адрес и строки запроса</span><span class="sxs-lookup"><span data-stu-id="45271-710">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="45271-711">Добавление главной страницы макета для настройки шаблонов для распространенных HTML-содержимого, в таблице стилей, улучшить внешний вид и представление шаблон для отображения HTML-содержимого</span><span class="sxs-lookup"><span data-stu-id="45271-711">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="45271-712">Как использовать шаблон ViewModel для передачи свойства в шаблоне представления для отображения динамических данных</span><span class="sxs-lookup"><span data-stu-id="45271-712">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="45271-713">Как использовать параметры, передаваемые контроллеров в шаблоне представления</span><span class="sxs-lookup"><span data-stu-id="45271-713">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="45271-714">Добавление ссылки на страницы в приложении ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="45271-714">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="45271-715">Как добавить и использовать динамические свойства в представлении</span><span class="sxs-lookup"><span data-stu-id="45271-715">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="45271-716">Усовершенствования в шаблоны проектов ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="45271-716">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="45271-717">Приложение а. Установка Visual Studio Express 2012 для Web</span><span class="sxs-lookup"><span data-stu-id="45271-717">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="45271-718">Можно установить **Microsoft Visual Studio Express 2012 для Web** или другой &quot;Express&quot; версию, используя  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="45271-718">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="45271-719">Следующие инструкции описывают действия, необходимые для установки *Visual studio Express 2012 для Web* с помощью *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="45271-719">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="45271-720">Последовательно выберите пункты [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="45271-720">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="45271-721">Кроме того, если вы уже установили установщика веб-платформы, можно открыть его и выполните поиск продукта &quot; *Visual Studio Express 2012 для Web с пакетом Windows Azure SDK*&quot;.</span><span class="sxs-lookup"><span data-stu-id="45271-721">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="45271-722">Щелкните **установить сейчас**.</span><span class="sxs-lookup"><span data-stu-id="45271-722">Click on **Install Now**.</span></span> <span data-ttu-id="45271-723">Если у вас **Web Platform Installer** вы будете перенаправлены, чтобы загрузить и установить ее сначала.</span><span class="sxs-lookup"><span data-stu-id="45271-723">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="45271-724">Один раз **Web Platform Installer** открыто, нажмите кнопку **установить** для запуска программы установки.</span><span class="sxs-lookup"><span data-stu-id="45271-724">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="45271-725">![Установка Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "установка Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="45271-725">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="45271-726">*Установка Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="45271-726">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="45271-727">Чтение всех продуктов лицензий и условия и нажмите кнопку **принимаю** для продолжения.</span><span class="sxs-lookup"><span data-stu-id="45271-727">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Принятие условий лицензионного соглашения](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="45271-729">*Принятие условий лицензионного соглашения*</span><span class="sxs-lookup"><span data-stu-id="45271-729">*Accepting the license terms*</span></span>
5. <span data-ttu-id="45271-730">Дождитесь завершения процесса загрузки и установки.</span><span class="sxs-lookup"><span data-stu-id="45271-730">Wait until the downloading and installation process completes.</span></span>

    ![Ход установки](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="45271-732">*Ход выполнения установки*</span><span class="sxs-lookup"><span data-stu-id="45271-732">*Installation progress*</span></span>
6. <span data-ttu-id="45271-733">По завершении установки нажмите кнопку **Готово**.</span><span class="sxs-lookup"><span data-stu-id="45271-733">When the installation completes, click **Finish**.</span></span>

    ![Установка завершена](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="45271-735">*Установка завершена*</span><span class="sxs-lookup"><span data-stu-id="45271-735">*Installation completed*</span></span>
7. <span data-ttu-id="45271-736">Нажмите кнопку **выхода** закрыть установщик веб-платформы.</span><span class="sxs-lookup"><span data-stu-id="45271-736">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="45271-737">Чтобы открыть Visual Studio Express для Web, перейдите к **запустить** экрана и начинается запись &quot; **VS Express**&quot;, выберите команду **VS Express для Web** Плитка.</span><span class="sxs-lookup"><span data-stu-id="45271-737">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express для Web плитки](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="45271-739">*VS Express для Web плитки*</span><span class="sxs-lookup"><span data-stu-id="45271-739">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="45271-740">Приложение б. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="45271-740">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="45271-741">В этом приложении будет показано, как создать новый веб-сайт на портале управления Windows Azure и публикация приложения, получить, выполнив в лаборатории преимуществами средство публикации веб-развертывания, предоставленного Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="45271-741">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="45271-742">Задача 1. Создание нового веб-узла с Windows Azure Portal</span><span class="sxs-lookup"><span data-stu-id="45271-742">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="45271-743">Последовательно выберите пункты [портала управления Windows Azure](https://manage.windowsazure.com/) и войдите с использованием учетных данных Майкрософт, связанную с вашей подпиской.</span><span class="sxs-lookup"><span data-stu-id="45271-743">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-744">С помощью Windows Azure можно бесплатно размещения 10 веб-сайтов ASP.NET и затем масштабирование по мере увеличения трафика.</span><span class="sxs-lookup"><span data-stu-id="45271-744">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="45271-745">Вы можете зарегистрироваться [здесь](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="45271-745">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="45271-746">![Войдите на портал Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "входа на портал Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="45271-746">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="45271-747">*Выполните вход на портале управления Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="45271-747">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="45271-748">Нажмите кнопку **New** на панели команд.</span><span class="sxs-lookup"><span data-stu-id="45271-748">Click **New** on the command bar.</span></span>

    <span data-ttu-id="45271-749">![Создание нового веб-сайта](aspnet-mvc-4-fundamentals/_static/image49.png "создания нового веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="45271-749">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="45271-750">*Создание нового веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="45271-750">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="45271-751">Нажмите кнопку **вычислений** | **веб-сайт**.</span><span class="sxs-lookup"><span data-stu-id="45271-751">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="45271-752">Выберите **быстрое создание** параметр.</span><span class="sxs-lookup"><span data-stu-id="45271-752">Then select **Quick Create** option.</span></span> <span data-ttu-id="45271-753">Укажите URL-адрес доступен для нового веб-сайта и нажмите кнопку **Создание веб-сайта**.</span><span class="sxs-lookup"><span data-stu-id="45271-753">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-754">Веб-приложение, работающее в облаке, вы можете управлять размещается веб-сайта Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="45271-754">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="45271-755">Параметр быстрого создания позволяет развернуть завершенное веб-приложение для Windows Azure веб-сайта из вне портала.</span><span class="sxs-lookup"><span data-stu-id="45271-755">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="45271-756">Он не включает действия по настройке базы данных.</span><span class="sxs-lookup"><span data-stu-id="45271-756">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="45271-757">![Создание нового веб-сайта с помощью быстрого создания](aspnet-mvc-4-fundamentals/_static/image50.png "создания нового веб-сайта с помощью быстрого создания")</span><span class="sxs-lookup"><span data-stu-id="45271-757">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="45271-758">*Создание нового веб-сайта с помощью быстрого создания*</span><span class="sxs-lookup"><span data-stu-id="45271-758">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="45271-759">Подождите, пока новый **веб-сайт** создается.</span><span class="sxs-lookup"><span data-stu-id="45271-759">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="45271-760">После создания веб-сайта по ссылке **URL-адрес** столбца.</span><span class="sxs-lookup"><span data-stu-id="45271-760">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="45271-761">Проверьте, что новый веб-сайт работает.</span><span class="sxs-lookup"><span data-stu-id="45271-761">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="45271-762">![Просмотр новых веб-сайт](aspnet-mvc-4-fundamentals/_static/image51.png "просмотра на новый веб-сайт")</span><span class="sxs-lookup"><span data-stu-id="45271-762">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="45271-763">*Просмотр новых веб-сайт*</span><span class="sxs-lookup"><span data-stu-id="45271-763">*Browsing to the new web site*</span></span>

    <span data-ttu-id="45271-764">![Веб-сайта под управлением](aspnet-mvc-4-fundamentals/_static/image52.png "веб-сайта под управлением")</span><span class="sxs-lookup"><span data-stu-id="45271-764">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="45271-765">*Запуск веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="45271-765">*Web site running*</span></span>
6. <span data-ttu-id="45271-766">Вернитесь на портал и щелкните имя веб-сайта в разделе **имя** столбец для отображения страницы управления.</span><span class="sxs-lookup"><span data-stu-id="45271-766">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="45271-767">![Открытие страницы управления веб-сайт](aspnet-mvc-4-fundamentals/_static/image53.png "Открытие страниц управления веб-сайта")</span><span class="sxs-lookup"><span data-stu-id="45271-767">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="45271-768">*Открытие страницы управления веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="45271-768">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="45271-769">В **панели мониторинга** в разделе **беглого** щелкните **загрузить профиль публикации** ссылку.</span><span class="sxs-lookup"><span data-stu-id="45271-769">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-770">*Профиль публикации* содержит все сведения, необходимые для публикации веб-приложения на веб-сайте Windows Azure для каждого разрешенного метода публикации.</span><span class="sxs-lookup"><span data-stu-id="45271-770">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="45271-771">Профиль публикации содержит URL-адреса, учетные данные пользователя и строк базы данных, необходимых для подключения и проверки подлинности с каждой из конечных точек, для которых включен метода публикации.</span><span class="sxs-lookup"><span data-stu-id="45271-771">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="45271-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express для Web** и **Microsoft Visual Studio 2012** поддерживают чтение профилей публикации для автоматизации настройки этих программ для Публикация веб-приложений на веб-сайтов Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="45271-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="45271-773">![Профиль публикации веб-сайт загрузки](aspnet-mvc-4-fundamentals/_static/image54.png "профиль публикации веб-сайта загрузки")</span><span class="sxs-lookup"><span data-stu-id="45271-773">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="45271-774">*Профиль публикации веб-сайта загрузки*</span><span class="sxs-lookup"><span data-stu-id="45271-774">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="45271-775">Загрузите файл профиля публикации в известном расположении.</span><span class="sxs-lookup"><span data-stu-id="45271-775">Download the publish profile file to a known location.</span></span> <span data-ttu-id="45271-776">Далее в этом упражнении вы увидите как использовать этот файл для публикации веб-приложения для веб-сайтов, Windows Azure из Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45271-776">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="45271-777">![Сохранить файл профиля публикации](aspnet-mvc-4-fundamentals/_static/image55.png "Сохранение профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="45271-777">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="45271-778">*Сохранить файл профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="45271-778">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="45271-779">Задача 2 - Настройка сервера базы данных</span><span class="sxs-lookup"><span data-stu-id="45271-779">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="45271-780">Если приложение использует SQL Server базы данных, необходимо будет создать сервер базы данных SQL.</span><span class="sxs-lookup"><span data-stu-id="45271-780">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="45271-781">Если вы хотите развернуть простое приложение, которое не использует SQL Server может пропустить эту задачу.</span><span class="sxs-lookup"><span data-stu-id="45271-781">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="45271-782">Сервер базы данных SQL необходимо для хранения базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="45271-782">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="45271-783">Можно просматривать серверы базы данных SQL из подписки на портале управления Windows Azure в **баз данных Sql** | **серверы** | **сервера Панель мониторинга**.</span><span class="sxs-lookup"><span data-stu-id="45271-783">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="45271-784">Если у вас на сервер, созданный, можно создать ее, используя **добавить** кнопки на панели команд.</span><span class="sxs-lookup"><span data-stu-id="45271-784">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="45271-785">Обратите внимание **имя сервера и URL-адрес, имя входа администратора и пароль**, как будет использовать их в следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="45271-785">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="45271-786">Не следует создавать базу данных, как он будет создан в более поздней стадии.</span><span class="sxs-lookup"><span data-stu-id="45271-786">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="45271-787">![Панель мониторинга базы данных SQL Server](aspnet-mvc-4-fundamentals/_static/image56.png "панель мониторинга базы данных SQL Server")</span><span class="sxs-lookup"><span data-stu-id="45271-787">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="45271-788">*Панель мониторинга базы данных SQL Server*</span><span class="sxs-lookup"><span data-stu-id="45271-788">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="45271-789">В следующей задаче требуется проверить подключение к базе данных из Visual Studio, по этой причине необходимо включить локальный IP-адреса в список серверов **разрешенные IP-адреса**.</span><span class="sxs-lookup"><span data-stu-id="45271-789">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="45271-790">Чтобы сделать это, нажмите кнопку **Настройка**, выберите IP-адрес из **текущий IP-адрес клиента** и вставьте его на **начальный IP-адрес** и **конечный IP-адрес** текстовые поля и нажмите кнопку ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) кнопки.</span><span class="sxs-lookup"><span data-stu-id="45271-790">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Добавление IP-адрес клиента](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="45271-792">*Добавление IP-адрес клиента*</span><span class="sxs-lookup"><span data-stu-id="45271-792">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="45271-793">Один раз **IP-адрес клиента** добавляется разрешенных IP-адресов выберите на **Сохранить** для подтверждения изменения.</span><span class="sxs-lookup"><span data-stu-id="45271-793">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Подтверждение изменений](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="45271-795">*Подтверждение изменений*</span><span class="sxs-lookup"><span data-stu-id="45271-795">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="45271-796">Задача 3. публикация приложения ASP.NET MVC 4 с помощью веб-развертывания</span><span class="sxs-lookup"><span data-stu-id="45271-796">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="45271-797">Вернитесь в решении ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="45271-797">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="45271-798">В **обозревателе решений**, щелкните правой кнопкой мыши проект веб-сайта и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="45271-798">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="45271-799">![Публикация приложения](aspnet-mvc-4-fundamentals/_static/image60.png "публикация приложения")</span><span class="sxs-lookup"><span data-stu-id="45271-799">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="45271-800">*Публикация веб-сайта*</span><span class="sxs-lookup"><span data-stu-id="45271-800">*Publishing the web site*</span></span>
2. <span data-ttu-id="45271-801">Импортируйте профиль публикации, сохраненный в первой задаче.</span><span class="sxs-lookup"><span data-stu-id="45271-801">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="45271-802">![Импорт профиля публикации](aspnet-mvc-4-fundamentals/_static/image61.png "Импорт профиля публикации")</span><span class="sxs-lookup"><span data-stu-id="45271-802">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="45271-803">*Импорт профиля публикации*</span><span class="sxs-lookup"><span data-stu-id="45271-803">*Importing publish profile*</span></span>
3. <span data-ttu-id="45271-804">Нажмите кнопку **Проверка подключения**.</span><span class="sxs-lookup"><span data-stu-id="45271-804">Click **Validate Connection**.</span></span> <span data-ttu-id="45271-805">После завершения проверки нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="45271-805">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="45271-806">Проверка завершена, как только вы увидите зеленый флажок рядом с "Проверить подключение".</span><span class="sxs-lookup"><span data-stu-id="45271-806">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="45271-807">![Проверка подключения](aspnet-mvc-4-fundamentals/_static/image62.png "Проверка подключения")</span><span class="sxs-lookup"><span data-stu-id="45271-807">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="45271-808">*Проверка подключения*</span><span class="sxs-lookup"><span data-stu-id="45271-808">*Validating connection*</span></span>
4. <span data-ttu-id="45271-809">В **параметры** в разделе **баз данных** статьи, нажмите кнопку рядом с текстового поля для подключения к базе данных (т. е. **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="45271-809">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="45271-810">![Конфигурация веб-развертывания](aspnet-mvc-4-fundamentals/_static/image63.png "конфигурация веб-развертывания")</span><span class="sxs-lookup"><span data-stu-id="45271-810">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="45271-811">*Конфигурация веб-развертывания*</span><span class="sxs-lookup"><span data-stu-id="45271-811">*Web deploy configuration*</span></span>
5. <span data-ttu-id="45271-812">Настройте подключение к базе данных следующим образом.</span><span class="sxs-lookup"><span data-stu-id="45271-812">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="45271-813">В **имя сервера** введите ваш URL-адрес сервера базы данных SQL с помощью *tcp:* префикс.</span><span class="sxs-lookup"><span data-stu-id="45271-813">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="45271-814">В **имя пользователя** введите имя входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="45271-814">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="45271-815">В **пароль** введите пароль входа администратора сервера.</span><span class="sxs-lookup"><span data-stu-id="45271-815">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="45271-816">Введите имя новой базы данных, например: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="45271-816">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="45271-817">![Настройка строки подключения назначения](aspnet-mvc-4-fundamentals/_static/image64.png "Настройка целевой строки подключения")</span><span class="sxs-lookup"><span data-stu-id="45271-817">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="45271-818">*Настройка целевой строки подключения*</span><span class="sxs-lookup"><span data-stu-id="45271-818">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="45271-819">Затем нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="45271-819">Then click **OK**.</span></span> <span data-ttu-id="45271-820">Когда появится запрос на создание базы данных щелкните **Да**.</span><span class="sxs-lookup"><span data-stu-id="45271-820">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="45271-821">![Создание базы данных](aspnet-mvc-4-fundamentals/_static/image65.png "Создание строки базы данных")</span><span class="sxs-lookup"><span data-stu-id="45271-821">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="45271-822">*Создание базы данных*</span><span class="sxs-lookup"><span data-stu-id="45271-822">*Creating the database*</span></span>
7. <span data-ttu-id="45271-823">Строка подключения, который будет использоваться для подключения к базе данных SQL в Windows Azure отображается внутри текстового поля по умолчанию соединения.</span><span class="sxs-lookup"><span data-stu-id="45271-823">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="45271-824">Затем нажмите кнопку **Далее**.</span><span class="sxs-lookup"><span data-stu-id="45271-824">Then click **Next**.</span></span>

    <span data-ttu-id="45271-825">![Строка подключения, указывающий базу данных SQL](aspnet-mvc-4-fundamentals/_static/image66.png "строку соединения, указывающий базу данных SQL")</span><span class="sxs-lookup"><span data-stu-id="45271-825">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="45271-826">*Строка подключения, указывающий базу данных SQL*</span><span class="sxs-lookup"><span data-stu-id="45271-826">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="45271-827">В **предварительного просмотра** щелкните **публикации**.</span><span class="sxs-lookup"><span data-stu-id="45271-827">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="45271-828">![Публикация веб-приложения](aspnet-mvc-4-fundamentals/_static/image67.png "публикации веб-приложения")</span><span class="sxs-lookup"><span data-stu-id="45271-828">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="45271-829">*Публикация веб-приложения*</span><span class="sxs-lookup"><span data-stu-id="45271-829">*Publishing the web application*</span></span>
9. <span data-ttu-id="45271-830">После завершения процесса публикации в браузере по умолчанию откроется опубликованного веб-узла.</span><span class="sxs-lookup"><span data-stu-id="45271-830">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="45271-831">![Публикации приложения в Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "публикации приложения в Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="45271-831">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="45271-832">*Приложения, опубликованные в Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="45271-832">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="45271-833">Приложение в. фрагменты кода</span><span class="sxs-lookup"><span data-stu-id="45271-833">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="45271-834">С помощью фрагментов кода у вас есть весь код, который требуется под рукой.</span><span class="sxs-lookup"><span data-stu-id="45271-834">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="45271-835">Документ лаборатории сообщает только при их использовании, как показано на следующем рисунке.</span><span class="sxs-lookup"><span data-stu-id="45271-835">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="45271-836">![Фрагменты кода Visual Studio для вставки кода в проекте](aspnet-mvc-4-fundamentals/_static/image69.png "фрагменты кода с помощью Visual Studio вставьте код в проект")</span><span class="sxs-lookup"><span data-stu-id="45271-836">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="45271-837">*Фрагменты кода Visual Studio для вставки кода в проекте*</span><span class="sxs-lookup"><span data-stu-id="45271-837">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="45271-838">***Добавление фрагмента кода, с помощью клавиатуры (только C#)***</span><span class="sxs-lookup"><span data-stu-id="45271-838">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="45271-839">Поместите курсор в место вставки кода.</span><span class="sxs-lookup"><span data-stu-id="45271-839">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="45271-840">Начните вводить имя фрагмента (без пробелов и дефисы).</span><span class="sxs-lookup"><span data-stu-id="45271-840">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="45271-841">Смотрите, как IntelliSense отображает, совпадающие с именами фрагменты.</span><span class="sxs-lookup"><span data-stu-id="45271-841">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="45271-842">Выберите правильный фрагмент (или продолжите ввод, пока не будет выделен весь фрагмент имени).</span><span class="sxs-lookup"><span data-stu-id="45271-842">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="45271-843">Нажмите клавишу Tab дважды, чтобы вставить фрагмент в положении курсора.</span><span class="sxs-lookup"><span data-stu-id="45271-843">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="45271-844">![Начните вводить имя фрагмента](aspnet-mvc-4-fundamentals/_static/image70.png "начните вводить имя фрагмента")</span><span class="sxs-lookup"><span data-stu-id="45271-844">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="45271-845">*Начните вводить имя фрагмента*</span><span class="sxs-lookup"><span data-stu-id="45271-845">*Start typing the snippet name*</span></span>

<span data-ttu-id="45271-846">![Нажмите клавишу Tab, чтобы выделить выделенный фрагмент](aspnet-mvc-4-fundamentals/_static/image71.png "нажмите клавишу Tab, чтобы выделить выделенный фрагмент")</span><span class="sxs-lookup"><span data-stu-id="45271-846">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="45271-847">*Нажмите клавишу Tab, чтобы выделить выделенный фрагмент*</span><span class="sxs-lookup"><span data-stu-id="45271-847">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="45271-848">![Снова нажмите клавишу Tab и фрагмент будет расширен](aspnet-mvc-4-fundamentals/_static/image72.png "будет расширен, снова нажмите клавишу Tab и фрагмента кода")</span><span class="sxs-lookup"><span data-stu-id="45271-848">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="45271-849">*Снова нажмите клавишу Tab и фрагмент будет расширен*</span><span class="sxs-lookup"><span data-stu-id="45271-849">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="45271-850">***Добавление фрагмента кода, с помощью мыши (C#, Visual Basic и XML)*** 1.</span><span class="sxs-lookup"><span data-stu-id="45271-850">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="45271-851">Щелкните правой кнопкой мыши место вставки фрагмента кода.</span><span class="sxs-lookup"><span data-stu-id="45271-851">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="45271-852">Выберите **вставить фрагмент** следуют **Мои фрагменты кода**.</span><span class="sxs-lookup"><span data-stu-id="45271-852">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="45271-853">Выберите соответствующий фрагмент из списка, щелкнув по ней.</span><span class="sxs-lookup"><span data-stu-id="45271-853">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="45271-854">![Щелкните правой кнопкой мыши место для вставки фрагмента кода и выбора вставить фрагмент](aspnet-mvc-4-fundamentals/_static/image73.png "место для вставки фрагмента кода и выбора вставить фрагмент кода щелкните правой кнопкой мыши")</span><span class="sxs-lookup"><span data-stu-id="45271-854">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="45271-855">*Щелкните правой кнопкой мыши в место вставки фрагмента кода и выберите Вставить фрагмент*</span><span class="sxs-lookup"><span data-stu-id="45271-855">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="45271-856">![Выберите из списка, соответствующего фрагмента, щелкнув его](aspnet-mvc-4-fundamentals/_static/image74.png "выбрать соответствующий фрагмент из списка, щелкнув по ней")</span><span class="sxs-lookup"><span data-stu-id="45271-856">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="45271-857">*Выберите соответствующий фрагмент из списка, щелкнув по ней*</span><span class="sxs-lookup"><span data-stu-id="45271-857">*Pick the relevant snippet from the list, by clicking on it*</span></span>
